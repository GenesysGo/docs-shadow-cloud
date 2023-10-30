---
description: >-
  ShdwDrive now provides a S3-Compatible method of accessing the ShdwDrive
  network. This is powered by shadow.cloud.
---

# S3-Compatible Client Access

In order to use the S3-Compatible gateway for the ShdwDrive network, head over to [https://portal.shadow.cloud](https://portal.shadow.cloud), connect your wallet and sign in, and then navigate to the storage account you'd like to generate an access key/secret key pair. Select the "Addons" tab and then you'll be able to enable & add s3-compatible key/secret pairs.

## Obtaining your credentials

Once you've enabled the S3 Access addon for a given storage account, you can view and add more s3-compatible key/secret pairs by navigating to the storage account of your choosing on [https://portal.shadow.cloud](https://portal.shadow.cloud).

To get your credentials, simply click "View Credentials".

## Permissions

Once enabled, you can manage individual permissions of each key/secret pair you generate for a given storage account. The list of permissions are configured to be compatible with existing s3 clients. Some examples of compatible s3 clients are rclone, s3cmd, and even the aws s3 cli. There's a plethora of tooling that exists for s3-compatible gateways, which is why we chose to build this into the ShdwDrive network.

In general, if you want to enable uploads with one of your keys, you'll need to enable the following permissions:

```
Get Object, Put Object, List Multipart Upload Parts, Abort Multipart upload
```

You can also control if a key is able to read, which allows you to have read, write, and read+write keys for various use cases.

## Gateway endpoints

In order to access the s3 compatible gateways on the ShdwDrive network, you'll need to configure your s3 client to use one of the following gateways:

1. https://us.shadow.cloud
2. https://eu.shadow.cloud

These endpoints proxy requests to the ShdwDrive network and allows you to have the best network connectivity possible given your geographical preference. All uploads are synced and available globally, so you can use either endpoint.

## Example client configurations

### RCLONE

The following is an example that you can add to your rclone configuration file. Typically, this is located in \`\~/.config/rclone/rclone.conf\`.

```
[shdw-cloud]
type = s3
provider = Other
access_key_id = [redacted]
secret_access_key = [redacted]
endpoint = https://us.shadow.cloud
acl = public-read
bucket_acl = public-read
```

## Speed

In order to ensure the stability of the ShdwDrive network, speeds are initially limited to 20 MiB/s. You can opt to upgrade the bandwidth rate limit for each individual key/secret pair you generate. The upgraded rate limit is 40 MiB/s

## Security

In the event a s3 key has been compromised, you can easily rotate the key. Simply navigate to [https://portal.shadow.cloud](https://portal.shadow.cloud), connect your wallet, select a storage account, and then click on the "Addons" tab. From there, you can click "Rotate" button to rotate a given key/secret pair to generate a fresh pair.

