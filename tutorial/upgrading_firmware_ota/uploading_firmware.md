# Unit 3: Uploading the Firmware File

In this unit, upload the prepared device firmware file on EnOS Console by take the following steps:

1. Log into the EnOS Console, and select **Device Management > OTA**.

2. Click **Add Firmware** and provide the following information:
   - **Product**: Select *RPi_Product* from the drop-down list.
   - **Firmware Name**: Enter *Firmware_2.0* for the firmware file to be uploaded.
   - **Firmware Version**: Enter *2.0* as the firmware version.
   - **Signature Algorithm**: By default, *MD5* is supported currently.
   - **Select Firmware**: Click **Select File**, browse and upload the *firmware_2.0.zip* firmware file.
   - **Version Description**: Enter a short description of the firmware.

   .. image:: media/ota_add_firmware.png

3. Click **Confirm** to upload the firmware file.

When the firmware file is uploaded, its status will be **Not verified**.

   .. image:: media/firmware_not_verified.png

## Next Unit

[Verifying the Firmware and Device Performance](verifying_firmware)

<!-- end -->
