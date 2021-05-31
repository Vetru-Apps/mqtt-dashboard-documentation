# Privacy

### Privacy disclaimer
This app does not collect, store, share or sell any personal information about you, your actions inside the app or any detail about your connections options and details. 

Databases are stored locally and never exported, unless requested by the user; in this case the exported file is encrypted. Note that when exporting the database over MQTT, the security of the whole transation depends upon the security of your MQTT broker.

The app makes use of Firebase Analytics to retrieve informations about crashes or bugs.

### Sensitive permissions
**MqttDashboard** requests the following sensitive permissions in order to work properly:

- `android.permission.INTERNET`: internet communication is used to reach the MQTT broker.
- `android.permission.ACCESS_NETWORK_STATE`: used to determine whether to use public or private addresses..
- `android.permission.ACCESS_WIFI_STATE`: used to determine if the phone is currently connected to a wifi network.
- `android.permission.WRITE_EXTERNAL_STORAGE` & `android.permission.READ_EXTERNAL_STORAGE`: used to backup and restore the database.
- `android.permission.FOREGROUND_SERVICE`: used to mantain the connection to the broker(s) in background.
- `com.android.vending.BILLING`: offers the user the ability to purchase additional services and to perform donations.
- `android.permission.RECEIVE_BOOT_COMPLETED`: enables the app to start the mqtt connections in background when the phone is turned on - if the option has been selected in settings.