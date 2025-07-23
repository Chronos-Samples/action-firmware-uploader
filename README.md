# Firmware uploader action

This GitHub Action is designed to upload Over-The-Air (OTA) firmware to an updater API. 
It authenticates via Keycloak, handles firmware file uploads, and provides metadata about the uploaded firmware.

---

## Inputs

| Name                   | Description                                                                                                      | Required | Default                              |
|------------------------|------------------------------------------------------------------------------------------------------------------|----------|--------------------------------------|
| `KEYCLOAK_URL`         | Keycloak URL                                                                                                    | No       | `https://auth.chronosai.xyz`         |
| `KEYCLOAK_REALM`       | The realm in Keycloak for authentication.                                                                       | No       | `master`                             |
| `KEYCLOAK_CLIENT_ID`   | The client ID for Keycloak access.                                                                              | Yes      |                                      |
| `KEYCLOAK_CLIENT_SECRET`| The client secret associated with the Keycloak client.                                                        | Yes      |                                      |
| `KEYCLOAK_USERNAME`    | The username for Keycloak authentication.                                                                       | Yes      |                                      |
| `KEYCLOAK_PASSWORD`    | The password for Keycloak authentication.                                                                       | Yes      |                                      |
| `KEYCLOAK_GRANT_TYPE`  | The grant type for Keycloak authentication (commonly `password`).                                               | No       | `password`                           |
| `API_URL`              | The URL of the API to upload the firmware to.                                                                  | No       | `https://wss.dev.chronosai.xyz`      |
| `DEVICE_KIND`          | The type of device. Allowed values: `nrf` or `esp`.                                                             | Yes      |                                      |
| `FIRMWARE_VERSION`     | The version of the firmware to be uploaded.                                                                    | Yes      |                                      |
| `DESCRIPTION`          | A description of the firmware.                                                                                 | No       |                                      |
| `DEVICE_TYPE`          | The type of the device. Allowed values: `node`.                      | Yes      |                                      |
| `FIRMWARE_PATH`        | The file path to the firmware zip file for upload.                                                             | Yes      |                                      |
| `MODEL`              | The model of the device. Allowed values: `SPECTRE`, `LocatorLite` or `xLocator`. | Yes      |                                      |

---

## Outputs

| Name         | Description                                              |
|--------------|----------------------------------------------------------|
| `ID`         | Unique identifier of the uploaded file.                  |
| `URL`        | URL of the uploaded file.                                |
| `Checksum`   | Checksum of the uploaded file for verification.           |
| `Size`       | Size of the uploaded file in bytes.                      |
| `UploadedBy` | The username of the individual who uploaded the file.     |
| `UploadedAt` | Timestamp of when the file was uploaded.                 |

---

## Usage

Below is an example of how to use this GitHub Action:

```yaml
      - name: Upload Firmware
        uses: Chronos-Samples/action-firmware-uploader@v1.0.0
        with:
          KEYCLOAK_CLIENT_ID: ${{ secrets.KEYCLOAK_CLIENT_ID }}
          KEYCLOAK_CLIENT_SECRET: ${{ secrets.KEYCLOAK_CLIENT_SECRET }}
          KEYCLOAK_USERNAME: ${{ secrets.KEYCLOAK_USERNAME }}
          KEYCLOAK_PASSWORD: ${{ secrets.KEYCLOAK_PASSWORD }}
          DEVICE_KIND: "nrf"
          FIRMWARE_VERSION: "1.0.0"
          DEVICE_TYPE: "node"
          FIRMWARE_PATH: "./firmware.zip"

      - name: Use Upload Metadata
        run: |
          echo "Firmware ID: ${{ steps.upload_firmware.outputs.ID }}"
          echo "Firmware URL: ${{ steps.upload_firmware.outputs.URL }}"
          echo "Checksum: ${{ steps.upload_firmware.outputs.Checksum }}"
          echo "Size: ${{ steps.upload_firmware.outputs.Size }}"
          echo "Uploaded By: ${{ steps.upload_firmware.outputs.UploadedBy }}"
          echo "Uploaded At: ${{ steps.upload_firmware.outputs.UploadedAt }}"
```
