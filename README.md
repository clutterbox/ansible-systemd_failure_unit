# clutterbox.systemd_failure_unit

Adds a systemd unit notify-failure@.service to be used with OnFailure to mail the output of journald.

## Role Variables

Variable | Function | Default
--- | --- | ---
systemd_failure_unit | List with environment variables to override sender/recipient | {}
systemd_failure_unit_manage_env | If yes, unneeded env-files in /etc/systemd-notify will be deleted | no

## Using the systemd service

In an existing systemd unit or an override, the following has to be added:

```
[Unit]
OnFailure=notify-failure@%i.service
```

Be sure to to an `systemd daemon-reload` afterwards to load the changes.

## Overriding recipients

Additional Environment variables will be loaded from /etc/systemd-notify/%i.env, where %i is the name of the service with the OnFailure directive.

There are two environment variables to change sender and recipient of the mail

Variable | Function | Default
--- | --- | ---
MAILFROM | Override sender | systemd <root@$HOSTNAME>
MAILTO | Override recipient| root

This can be done per unit with the systemd_failure_unit variable. The format is as follows:

```yaml
systemd_failure_unit:
    unit_name:
        mailfrom: from@example.com
        mailto: recipient@example.com
```

Keys below the unit_name will simply be converted to uppercase and written to the environment file.

If your unit with the OnFailure-Statement is called "example" and you want to sent an E-Mail to hostmaster@example.com, the configuration looks like:

```yaml
systemd_failure_unit:
    unit_name:
        mailto: hostmaster@example.com
```

## License

MIT License

## Author Information

Alexander Zielke - mail@alexander.zielke.name
