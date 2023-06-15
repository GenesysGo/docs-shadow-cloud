# Python

* [**Install Python SDK**](sdk-python.md#installing-shadow-drive)
* [**Examples**](sdk-python.md#example-end-to-end-script)
* [**Github**](sdk-python.md#about-the-github-repo)
* [**Methods**](sdk-python.md#methods)

## **Installing Shadow Drive**

`pip install shadow-drive`

Also for running the examples:

`pip install solders`

Check out the [`examples`](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py) directory for a demonstration of the functionality.

https://shdw-drive.genesysgo.net/\[STORAGE\_ACCOUNT\_ADDRESS]

### **Example -** Shadow Drive Storage Management: Account Creation, File Upload, and Storage Modification in Python

```python
from shadow_drive import ShadowDriveClient
from solders.keypair import Keypair
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--keypair', metavar='keypair', type=str, required=True,
                    help='The keypair file to use (e.g. keypair.json, dev.json)')
args = parser.parse_args()

# Initialize client
client = ShadowDriveClient(args.keypair)
print("Initialized client")

# Create account
size = 2 ** 20
account, tx = client.create_account("full_test", size, use_account=True)
print(f"Created storage account {account}")

# Upload files
files = ["./files/alpha.txt", "./files/not_alpha.txt"]
urls = client.upload_files(files)
print("Uploaded files")

# Add and Reduce Storage
client.add_storage(2**20)
client.reduce_storage(2**20)

# Get file
current_files = client.list_files()
file = client.get_file(current_files[0])
print(f"got file {file}")

# Delete files
client.delete_files(urls)
print("Deleted files")

# Delete account
client.delete_account(account)
print("Closed account")
```

#### **About the** [**Github Repo**](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py)

This package uses PyO3 to build a wrapper around the official Shadow Drive Rust SDK. For more information, see the [Rust SDK documentation](https://github.com/GenesysGo/shadow-drive-rust/tree/main/sdk).

#### **Methods**

Section under development.
