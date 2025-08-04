# Hassio Addon for Backing up to S3 Bucket

DEPRECATED - use this custom integration instead: [S3-Compatible](https://github.com/PhantomPhoton/S3-Compatible)

Add-on for uploading hass.io backups to AWS S3.

## Installation

Add this add-on by clicking on this link:

[![Open your Home Assistant instance and show the dashboard of a Supervisor add-on.](https://my.home-assistant.io/badges/supervisor_addon.svg)](https://my.home-assistant.io/redirect/supervisor_addon/?addon=63120367_backup-s3&repository_url=https%3A%2F%2Fgithub.com%2FAlexanderBabel%2Fhassio-addons)

After installation, set the config variables that you obtained from setting up the AWS account, user and bucket (see below):
awskey: `access key id`
awssecret: `secret access key`
bucketname: `AWS S3 bucket name`

Steps to setting up an Amazon AWS account:

1. Go to https://portal.aws.amazon.com/billing/signup#/start

2. Create a bucket following the standard settings (any name will do):
   https://s3.console.aws.amazon.com/s3/

Note the AWS S3 Bucket name

3. Create a specific user with AmazonS3FullAccess rights
   https://console.aws.amazon.com/iam

Make sure that after completing the user creation wizard you note down the Access key ID and Secret Access Key. Especially the Secret Access Key will only be displayed once.

The policy should look something like:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:DeleteObject",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::MY_BUCKET_HERE/*"
            ]
        }
    ]
}
```

## Usage

To sync your HASSIO backup folder with AWS just click START in this add-on. It will keep a synced cloud-copy, so any purged backup files will not be kept in your bucket either.

You could automate this using Automation:

```yaml
- alias: Make backup
  trigger:
    - platform: time
      at: "03:30:00"
  condition:
    - condition: time
      weekday:
        - mon
  action:
    - service: hassio.backup_full
      data_template:
        name: "WeeklyBackup: ha-{{ now().strftime('%Y-%m-%d') }}"
  mode: single

- alias: Upload to S3
  trigger:
    - platform: time
      at: "03:30:00"
  condition:
    - condition: time
      weekday:
        - mon
  action:
    - service: hassio.addon_start
      data:
        addon: 63120367_backup-s3
```

The automation above first makes a backup at 3am, and then at 3.30am uploads to S3.

## Help and Debug

Please post an issue on this repo with your full log.

## Alternative Backup solution

I really like this community integration too:
[hass-auto-backup](https://github.com/jcwillox/hass-auto-backup)

Once installed, it can be easily adapted to run alongside this addon.

Credits: Based on jperquin/hassio-backup-s3 based on rrostt/hassio-backup-s3

Advanced automation with auto backup:

```yaml
- alias: "[Backup] Create Backup"
  description: ""
  trigger:
    - platform: time
      at: "02:30:00"
  condition:
    - condition: time
      weekday:
        - mon
  action:
    - service: auto_backup.backup
      data:
        name: "WeeklyBackup: ha-{{ now().strftime('%Y-%m-%d-%H-%M-%S') }}"
        exclude_folders:
          - share
        password: PASSWORD_HERE
        keep_days: 1
  mode: single

- alias: "[Backup] Handle Events"
  description: ""
  trigger:
    - platform: event
      event_type: auto_backup.backup_start
      id: start
    - platform: event
      event_type: auto_backup.backup_successful
      id: successful
    - platform: event
      event_type: auto_backup.backup_failed
      id: failed
  condition: []
  action:
    - choose:
        - conditions:
            - condition: trigger
              id: start
          sequence:
            - service: rest_command.backup_start
              data: {}
        - conditions:
            - condition: trigger
              id: successful
          sequence:
            - service: hassio.addon_start
              data:
                addon: 63120367_backup-s3
        - conditions:
            - condition: trigger
              id: failed
          sequence:
            - service: rest_command.backup_failed
              data: {}
            - service: persistent_notification.create
              data:
                title: Snapshot Failed.
                message: |-
                  Name: {{ trigger.event.data.name }}
                  Error: {{ trigger.event.data.error }}
      default: []
  mode: single
```

The first automation starts the backup process. Afterwards the second automation handles events issued during the backup process. As soon as the backup starts, a call is made to healthchecks.io to track the starting point of a backup. If the backups fails another ping is made. If the backup succeeds, then the backup-s3 addon is started.

The following rest commands are set in the configuration.yaml:
```yaml
rest_command:
  backup_start:
    url: !secret https://hc-ping.com/HEALTH_CHECK_UUID/start
    method: GET
    headers:
      accept: "application/json, text/html"
      user-agent: "HomeAssistant/{{ state_attr('update.home_assistant_core_update', 'installed_version') }}"
    verify_ssl: true
  backup_failed:
    url: https://hc-ping.com/HEALTH_CHECK_UUID/fail
    method: GET
    headers:
      accept: "application/json, text/html"
      user-agent: "HomeAssistant/{{ state_attr('update.home_assistant_core_update', 'installed_version') }}"
    verify_ssl: true
```
