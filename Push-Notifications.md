# Push Notifications

Push Notifications are now an option in Grbl_ESP32. When events occur, the firmware can send messages to your phone or email. Currently you can send messages via the following services.

* [Line Notification](https://github.com/luc-github/ESP3D/wiki/Line) (https://line.me) Free
* [Pushover Notification](https://github.com/luc-github/ESP3D/wiki/Pushover) (https://pushover.net/) not Free
* [Email using SMTP and HTTPS](https://github.com/luc-github/ESP3D/wiki/Email_and_SMTP) Free

The feature is enabled with #define ENABLE_NOTIFICATIONS in the config.h file. Further setup is required via the WebUI or via the [ESP610] command.

In Grbl_ESP32 push notifications are only used when running a job from an SD card. You will be notified when the job completes or an error occurs. 

A custom message can be sent with the [ESP600] command.
Example (assuming password="admin"): [ESP600]Hello from Grbl pwd=admin



