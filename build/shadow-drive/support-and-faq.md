# FAQ

If you've got questions, we've got answers.

## Technical

<details>

<summary>Where can I go to reach out for technical assistance?</summary>

Our [Discord server](https://discord.gg/genesysgo) is the best place to get in touch with us.\
We have a dedicated support section.

In addition to this FAQ, you might find the [Github Q\&A](https://github.com/GenesysGo/shadow-drive/issues?q=is%3Aissue+is%3Aclosed) useful as deeper technical issues are discussed.&#x20;

</details>

<details>

<summary>What should I do if creating a storage account is failing?</summary>

If creating a storage account is failing, make sure that you have appropriate amounts of both SOL and SHDW in your wallet. Creating a storage account requires a small amount of SOL to cover the transaction fee, as well as some SHDW to cover the initial storage allocation. Make sure that your wallet has enough funds to cover these requirements.

If you have the correct amount of SOL and SHDW in your wallet but creating a storage account is still failing, there may be other factors at play that are causing the issue. Some possible causes could be network connectivity issues, problems with the Shadow Drive node, or bugs/issues with the SDK.

To troubleshoot the issue, you can try the following:

* Verify that the [Shadow Drive network](https://status.genesysgo.net/) is up and running.
* Check the Shadow Drive [Change Log](../../reference/change-logs.md) for any known issues or bugs that may be causing the problem.
* Contact Shadow Drive [support](https://discord.gg/genesysgo) for further assistance.

</details>

<details>

<summary>How much storage space can I reserve?</summary>

There is an upper limit of 1GB per bucket. Development is currently underway which will greatly increase this cap.

</details>

<details>

<summary>What should I do if I think there is an issue with how I'm implementing the wallet, or my transactions are not working?</summary>

If you think there is an issue with how you're implementing the wallet, or your transactions are not working, you can try upgrading the wallet adapters. Check the Solana wallet adapter repositories for their examples, as the process for importing the adapters may have changed. Additionally, you can refer to the Shadow Drive documentation and SDK for more information on how to properly implement the wallet and perform transactions. If you're still having issues, contact Shadow Drive support for further assistance.

</details>

<details>

<summary>Does Shadow Drive support Ledger wallet signing?</summary>

No, Shadow Drive does not currently support Ledger wallet signing.

</details>

<details>

<summary>Are accounts returned in any specific order when calling the `getStorageAccounts` method?</summary>

Yes, accounts are returned in the order they are created when calling the `getStorageAccounts` method in GenesysGo Shadow Drive. This is because the system was designed and built in such a way to ensure that the accounts are returned in the order they were created.

</details>

<details>

<summary>Is there a way to delete multiple files at the same time ? Or is it in the roadmap ?</summary>

Currently, it is not possible to delete multiple files at once. However, we have added this feature to our roadmap and will be working on it in the near future. Thank you for your suggestion!

</details>

<details>

<summary>Is there a reason why `edit-file` works differently from `upload-file` when generating upload hashes server side?</summary>

The `edit-file` functionality works differently from `upload-file` because it is a remnant of the first iteration of Shadow Drive where every file had an associated account on-chain with some metadata that was crucial for tracking. However, we've made some changes that aren't documented yet and aren't implemented in the SDKs. If you add `overwrite: true` to the request body of an upload request that you make manually instead of through the SDK, it will do the same thing as editing a file.

</details>

<details>

<summary>Is it possible to ask the user to sign a Shadow transaction and another transaction at the same time on the frontend?</summary>

Currently, it is not possible to ask the user to sign a Shadow transaction and another transaction at the same time on the frontend. The Shadow network only allows shadow drive-specific transactions to have instructions related to the shadow drive on chain program. Any other instructions will cause the transaction to fail. This security feature is in place to prevent malicious transactions.

</details>

<details>

<summary>I'm trying to create a File object on my React app to upload it to Shadow but I keep getting an error.</summary>

The error you're getting may be due to the ShdwDrive instance being created before the wallet-provider is ready. In the latest example on the main branch, there is a slight change in the useEffect that creates the drive instance which may resolve your issue. Additionally, make sure that the file data buffer is converted to a Blob using `new Blob([Buffer.from("data")])`.

</details>

<details>

<summary>I'm using `createStorageAccount` on a node script and it works fine, but when I try to use it in my React app, I get a 403 error.</summary>

By default, the rpc used is the Solana mainnet rpc api.mainnet-beta.solana.com. If you're getting blocked by that, you'll have to sign up for a paid RPC as we cannot control how the Solana mainnet rpc endpoint is limited. It is possible that the endpoint is blocking requests from the browser due to security reasons.

</details>

<details>

<summary>Why does my method fail with "Blockhash not found" error?</summary>

This is an issue on the Solana RPC side and unfortunately, all you can do is retry the method.

</details>

<details>

<summary>How would I use the SDK to get file contents from the account?</summary>

You can send a normal GET request to https://shdw-drive.genesysgo.net// to get the file contents from the account.

</details>

<details>

<summary>Is there a way to get information about filetypes so I can then handle different types?</summary>

You can make a HEAD request or a GET request to get information about file types. If you make a GET request, the response headers should include the content type.

</details>

<details>

<summary>How can I get metadata for the file?</summary>

You can get metadata for the file by making a POST request to https://shdw-drive.genesysgo.net//. The response will include metadata for the file.

</details>

<details>

<summary>Is it possible to transfer ownership of a storage account to another wallet?</summary>

Currently, this is not an active feature in the CLI or SDK. However, it is a planned feature for future releases.

</details>

<details>

<summary>Can only the owner of the storage edit files?</summary>

Yes, currently only the owner of the storage account can edit the files.

</details>

<details>

<summary>Is it possible to resume an upload from where it left off if it fails?</summary>

No, unfortunately it is not possible to resume an upload from where it left off if it fails. However, the CLI checks files before uploading and skips them if they already exist. You also receive an output JSON file for each file upload, which will indicate if a file already exists.

</details>

<details>

<summary>Is it possible for a user to sign a Shadow transaction and another unrelated transaction at the same time?</summary>

Currently, the Shadow network only allows Shadow Drive-specific transactions to include instructions related to the Shadow Drive on-chain program. Any other instructions will cause the transaction to fail as a security measure. This means that it is not possible for a user to sign a Shadow transaction and another unrelated transaction at the same time.

</details>

<details>

<summary>I'm getting a 400 error.</summary>

When getting 400 timeouts for transaction submissions, it is most likely due to congestion on the Solana network. While timing out and retrying is normal during Solana congestion, many are now using priority fees may help solve congestion-related issues. Contact your RPC provider for further help.

</details>

<details>

<summary>What should I do if I encounter an ENOTFOUND error when using the Shadow Drive CLI?</summary>

If you encounter an ENOTFOUND error when using the Shadow Drive CLI, it is likely a local DNS issue on your side. ENOTFOUND is a DNS resolver problem, which means you will need to check with your Internet Service Provider (ISP) to resolve the issue. Alternatively, you can try using a Virtual Private Network (VPN) to see if that resolves the issue.

</details>

## General

<details>

<summary>What makes Shadow Drive unique?</summary>

Shadow Drive is a commodity cloud network that offers multiple service options, leveraging distributed ledger technology, and offering vertically integrated, L1-specific storage and compute. It is the only cloud network designed to democratize the earnings of traditional cloud platforms without sacrificing performance. Being S3-compatible, Shadow Drive maintains an open-source SDK and interoperability standards that make it easy to access through popular builder tools and SDKs. Its objective is to support popular tools that make building easier, regardless of the application you are building.

</details>

<details>

<summary>How is GDPR handled?</summary>

Shadow Drive provides developers with tools to comply with GDPR and can provide records to prove the deletion of a user's personal data. All records for GDPR compliance are stored on-chain and have been verified by the Solana validator network. The data is then encrypted and algorithmically distributed across the network in triplicate. All transactions are signed and publicly verifiable on-chain.

</details>

<details>

<summary>Is Shadow Drive supported on mobile?</summary>

Yes, Shadow Drive is supported on mobile through our ecosystem partners who are actively building on mobile. Please check out our Shadow Ecosystem page for more details. Additionally, in the future, our DAGGER distributed ledger technology will enable Solana Saga powered storage solutions for those seeking low cost decentralized mobile clouds. Please check out the Learn section for more information.

</details>

<details>

<summary>How much does it cost to store data on Shadow Drive?</summary>

Shadow Drive storage costs are driven by wholesale network costs and can be estimated through various front end UIs that capture moment-in-time estimates. One example is the front-end designed by an ecosystem partners, which provides detailed information on the network as well. Here is the link to the front-end: https://sdrive.app/stats

</details>

<details>

<summary>Is Shadow Drive S3-compatible?</summary>

Yes, Shadow Drive is S3-compatible. S3-compatibility is a widely adopted standard in the cloud storage industry, and many providers offer S3-compatible APIs and protocols, which gives builders greater flexibility in choosing a cloud storage provider. This means developers can easily move data between different services without worrying about compatibility issues. Additionally, S3-compatibility offers robust APIs that enable fast and reliable query, along with virtual mount capability, making it important for Web2, Web3, and the frontiers of distributed ledger tech and AI. Shadow Drive aims to empower developers to integrate it directly into their builds, and to support the talented community of designers who will create innovative platforms for Shadow Drive.

</details>

<details>

<summary>What physical infrastructure powers Shadow Drive?</summary>

Shadow Drive runs on a global network of bare metal infrastructure, with all compute and storage existing on bare metal. There is no dependency on cloud providers for Shadow Drive operations. For more details on the design of Shadow Drive, please see the "Design" section under the "Learn" category.

</details>

<details>

<summary>What is GenesysGo?</summary>

GenesysGo (GG) is a company that was founded in April 2021 as a Solana validator. Since then, GG has expanded its offerings to provide RPCs and build out a large ecosystem of tools and infrastructure for Solana. GG has a team of talented developers and coders who are dedicated to building innovative solutions for the Solana community. For more information, you can visit their website at http://shadow.cloud/.

</details>

<details>

<summary>Can I advertise my project if I use DAGGER/Shadow Drive?</summary>

Yes, the Shadow Drive team would love to hear about your project if you are building on top of the Drive or using DAGGER. You can share your work in the [Shadow Drive Discord](https://discord.com/invite/genesysgo) community or submit a pull request to get added to the [Shadow Ecosystem](community-mainted-uis.md) page, which showcases projects built on Shadow Drive.

</details>
