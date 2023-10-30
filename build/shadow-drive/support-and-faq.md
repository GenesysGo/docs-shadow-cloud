# FAQ

If you've got questions, we've got answers.

## Technical

<details>

<summary>Where can I go to reach out for technical assistance?</summary>

Our [Discord server](https://discord.gg/genesysgo) is the best place to get in touch with us.\
We have a dedicated support section.

In addition to this FAQ, you might find the [Github Q\&A](https://github.com/GenesysGo/Shdw-drive/issues?q=is%3Aissue+is%3Aclosed) useful as deeper technical issues are discussed.

Discord Server: https://discord.gg/genesysgo

GitHub FAQ: https://github.com/GenesysGo/Shdw-drive/issues?q=is%3Aissue+is%3Aclosed

</details>

<details>

<summary>I'm getting a "410 Gone" error when trying to use the CLI. What should I do?</summary>

This error means the Solana RPC provider you're using with the CLI doesn't support a specific RPC method that's necessary for the CLI to function. This could be \`getProgramAccounts\` or some other method.

We'd recommend trying a more premium RPC provider like [Helius](https://www.helius.dev/), Hellomoon.io, or some other premium Solana RPC provider that allows for all Solana RPC methods to be used.

</details>

<details>

<summary>What should I do if creating a storage account is failing?</summary>

If creating a storage account is failing, make sure that you have appropriate amounts of both SOL and SHDW in your wallet. Creating a storage account requires a small amount of SOL to cover the transaction fee, as well as some SHDW to cover the initial storage allocation. Make sure that your wallet has enough funds to cover these requirements. Review the docs here: https://docs.shadow.cloud/build/the-cli#create-a-storage-account

If you have the correct amount of SOL and SHDW in your wallet but creating a storage account is still failing, there may be other factors at play that are causing the issue. Some possible causes could be network connectivity issues, problems with the ShdwDrive node, or bugs/issues with the SDK.

To troubleshoot the issue, you can try the following:

* Verify that the [ShdwDrive network](https://status.genesysgo.net/) is up and running. https://status.genesysgo.net/
* Check the ShdwDrive [Change Log](../../reference/change-logs.md) for any known issues or bugs that may be causing the problem. https://docs.shadow.cloud/reference/change-logs
* Contact ShdwDrive [support](https://discord.gg/genesysgo) for further assistance. https://discord.gg/genesysgo

</details>

<details>

<summary>How much storage space can I reserve?</summary>

A user can reserve 4kb at minimum.

There is an upper limit of one terabyte (1TB) per bucket.

Development is currently underway which will greatly increase this cap.

</details>

<details>

<summary>What's the smallest or largest file I can upload?</summary>

Currently, these are the limits:

* Minimum: 4kb. If you upload a 100 byte file, it will still take up 4kb of space. This is due to the replication overhead required.
* Maximum: 1gb.

With the [s3-compatible client access](../s3-compatible-client-access.md), you're able to upload files up to 1TiB.

There's ongoing development to increase the maximum file size.

</details>

<details>

<summary>What should I do if I think there is an issue with how I'm implementing the wallet, or my transactions are not working?</summary>

If you think there is an issue with how you're implementing the wallet, or your transactions are not working, you can try upgrading the wallet adapters. Check the Solana wallet adapter repositories for their examples, as the process for importing the adapters may have changed.

Additionally, you can refer to the ShdwDrive documentation and SDK for more information on how to properly implement the wallet and perform transactions. You can review the example here: https://docs.shadow.cloud/build/the-sdk/sdk-javascript#example-post-request-via-sdk-make-immutable

If you are using react to build a wallet using `const drive = await new ShdwDrive(connection, wallet).init();` and getting the error "Cannot read properties of undefined (reading 'toBytes')" then remember to make sure you must pass the entire wallet around and make sure to not deconstruct it.

If you're still having issues, contact ShdwDrive support for further assistance.

</details>

<details>

<summary>I can create a storage account using the phantom wallet through CLI, but when I try from the SDK in my app the transaction fails saying insufficient balance. Why is this?</summary>

For the purposes of utilizing the ShdwDrive, \~0.1 SOL in our experience will avoid insufficient balance errors. You can also examine the TXs to see if there's any differences in your spend when using the CLI versus the SDK methods.

</details>

<details>

<summary>Does ShdwDrive support Ledger wallet signing?</summary>

No, ShdwDrive does not currently support Ledger wallet signing. The reason we are currently unable to provide Ledger support is due to the absence of the message signing feature in the Solana app for Ledger, as our system relies on this functionality.

To expedite the implementation of Ledger support, kindly consider drawing attention to this GitHub issue by leaving a comment: https://github.com/solana-labs/wallet-adapter/pull/712

</details>

<details>

<summary>Are accounts returned in any specific order when calling the `getStorageAccounts` method?</summary>

Yes, accounts are returned in the order they are created when calling the `getStorageAccounts` method in GenesysGo ShdwDrive. This is because the system was designed and built in such a way to ensure that the accounts are returned in the order they were created. https://docs.shadow.cloud/build/the-sdk/sdk-javascript#getstorageaccounts

</details>

<details>

<summary>Is there a way to delete multiple files at the same time?</summary>

Currently, it is not possible to delete multiple files at once. However, we have added this feature to our roadmap and will be working on it in the near future. Thank you for your suggestion!

</details>

<details>

<summary>Is there a reason why `edit-file` works differently from `upload-file` when generating upload hashes server side?</summary>

The `edit-file` functionality works differently from `upload-file` because it is a remnant of the first iteration of ShdwDrive where every file had an associated account on-chain with some metadata that was crucial for tracking. However, we've made some changes that aren't documented yet and aren't implemented in the SDKs. If you add `overwrite: true` to the request body of an upload request that you make manually instead of through the SDK, it will do the same thing as editing a file.

</details>

<details>

<summary>Is it possible to ask the user to sign a Shdw transaction and another transaction at the same time on the frontend?</summary>

Currently, it is not possible to ask the user to sign a Shdw transaction and another transaction at the same time on the frontend. The Shdw network only allows ShdwDrive-specific transactions to have instructions related to the ShdwDrive on chain program. Any other instructions will cause the transaction to fail. This security feature is in place to prevent malicious transactions.

</details>

<details>

<summary>I'm trying to create a File object on my React app to upload it to Shdw but I keep getting an error.</summary>

The error you're getting may be due to the ShdwDrive instance being created before the wallet-provider is ready. In the latest example on the main branch, there is a slight change in the useEffect that creates the drive instance which may resolve your issue. Additionally, make sure that the file data buffer is converted to a Blob using `new Blob([Buffer.from("data")])`.

</details>

<details>

<summary>I'm using `createStorageAccount` on a node script and it works fine, but when I try to use it in my React app, I get a 403 error.</summary>

By default, the rpc used is the Solana mainnet rpc api.mainnet-beta.solana.com. If you're getting blocked by that, you'll have to sign up for a paid RPC as we cannot control how the Solana mainnet rpc endpoint is limited. It is possible that the endpoint is blocking requests from the browser due to security reasons.

For additional help, consider joining our [Discord](https://discord.gg/genesysgo) and asking in support channels.

</details>

<details>

<summary>Why does my method fail with "Blockhash not found" error?</summary>

This is an issue on the Solana RPC side and unfortunately, all you can do is retry the method. Consider implementing retry and/or error handling in your application.

</details>

<details>

<summary>How would I use the SDK to get file contents from the account?</summary>

You can send a normal GET request to https://shdw-drive.genesysgo.net// to get the file contents from the account. You can read more in API methods here: https://docs.shadow.cloud/build/the-api

</details>

<details>

<summary>Is there a way to get information about filetypes so I can then handle different types?</summary>

You can make a HEAD request or a GET request to get information about file types. If you make a GET request, the response headers should include the content type. Review the API methods here: https://docs.shadow.cloud/build/the-api

</details>

<details>

<summary>How can I get metadata for the file?</summary>

You can get metadata for the file by making a POST request to https://shdw-drive.genesysgo.net//. The response will include metadata for the file. Review the API methods here: https://docs.shadow.cloud/build/the-api

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

<summary>Is it possible to resume an upload from where it left off if it fails</summary>

No, unfortunately it is not possible to resume an upload from where it left off if it fails. However, the CLI checks files before uploading and skips them if they already exist. You also receive an output JSON file for each file upload, which will indicate if a file already exists.

</details>

<details>

<summary>Can I use the rust SDK in anchor programs?</summary>

No, the SDK requires internet access to send http requests. This is not allowed within Solana runtime because arbitrary http responses are not deterministic and may produce different Solana ledger state transitions

</details>

<details>

<summary>Is it possible for a user to sign a Shdw transaction and another unrelated transaction at the same time?</summary>

Currently, the Shdw network only allows ShdwDrive-specific transactions to include instructions related to the ShdwDrive on-chain program. Any other instructions will cause the transaction to fail as a security measure. This means that it is not possible for a user to sign a Shdw transaction and another unrelated transaction at the same time.

</details>

<details>

<summary>I'm getting a 400 error.</summary>

When getting 400 timeouts for transaction submissions, it is most likely due to congestion on the Solana network. While timing out and retrying is normal during Solana congestion, many are now using priority fees which may help solve congestion-related issues. Contact your RPC provider for further help.

If your 400 error is stating "Invalid transaction supplied" then you may need to join our support channel in [Discord](https://discord.gg/genesysgo) and provide more details on the specific method. To resolve the typical causes of this error do the following:

1. Check announcements in Discord (https://discord.gg/genesysgo) or the network status (https://status.genesysgo.net/) to make sure there is no platform-wide problem.
2. Check all of your versions and dependencies. You Solana wallet adapter dependencies and the version of the JavaScript SDK must be up to date.
3. Double check the wallet you have chosen to work with is not having issues. You may need to reach out to them directly.

</details>

<details>

<summary>What should I do if I encounter an ENOTFOUND error when using the ShdwDrive CLI?</summary>

If you encounter an ENOTFOUND error when using the ShdwDrive CLI, it is likely a local DNS issue on your side. ENOTFOUND is a DNS resolver problem, which means you will need to check with your Internet Service Provider (ISP) to resolve the issue. Alternatively, you can try using a Virtual Private Network (VPN) to see if that resolves the issue.

</details>

<details>

<summary>What are some things I should check when getting errors?</summary>

You can try setting --log-level debug with your command that is getting an error. Make sure to confirm you have installed the latest versions and dependencies and that your keypair file is being accessed properly. Make sure you wallet is funded properly with both SOL and SHDW, that you are handling Solana connection objects properly, and that you are not having Solana RPC related errors. For further help you can capture logs and share relevant code in the technical support channels of our [Discord](https://discord.gg/genesysgo).

</details>

<details>

<summary>What does "Internal Server Error" mean when calling the ShdwDrive API?</summary>

There are a few reasons for this error but the most common is the file that have not migrated from the original version 1 format storage account to the newer version 2 format. For users that have created legacy style ShdwDrive accounts, please finish the migration steps.

For additional help please reach out to to us in Discord (https://discord.gg/genesysgo).

</details>

<details>

<summary>How do I submit a bug or a security issue?</summary>

**https://github.com/GenesysGo/shdw-drive-bug-reports**

We adhere to a responsible disclosure process for security related issues. To ensure the responsible disclosure and handling of security vulnerabilities, we ask that you follow the process outlined below.

**Bug Reporting Process**

1. Submit a new bug report by creating a [new issue](https://github.com/GenesysGo/shdw-drive-bug-reports/issues/new/choose) in this repository. https://github.com/GenesysGo/shdw-drive-bug-reports/issues/new/choose
2. Please provide a clear and concise description of the issue, steps to reproduce it, and any relevant screenshots or logs.
3. Label your issue as a 'bug' or 'security' accordingly.

**Important**: For security-related issues, do not include sensitive information in the issue description. Instead, submit a pull request to our repository, containing the necessary details, so that the information remains concealed until the issue is resolved.

**Security related issues should only be reported through this repository.**

While we strongly encourage the use of this repository for bug reports and security issues, you may also reach out to us via our [**Discord**](https://discord.gg/genesysgo) server. Join the #shdw-drive-technical-support channel for assistance. However, please note that we will redirect you to submit the bug report through this GitHub repository for proper handling and tracking.

</details>

<details>

<summary>Is there a way to monitor the network so I know if there are issues or downtime?</summary>

Yes, you can subscribe to the Shdw Network status here: https://status.genesysgo.net/

Also follow us on twitter https://twitter.com/GenesysGo or join our tech support Discord: https://discord.gg/genesysgo

</details>

<details>

<summary>Is there a grace period if my storage account runs out of <a href="https://docs.shadow.cloud/reference/shdw-token">$SHDW</a> to cover mutable fees?</summary>

Yes. Your storage account will be kept for 6 months. After that, it is up for cleanup and a storage node may delete your storage account and all data in it.

</details>

<details>

<summary>How can I add more <a href="https://docs.shadow.cloud/reference/shdw-token">$SHDW</a> to my storage account for mutable fees?</summary>

1. Either use the \`topUp\` method in one of the [SDKs](the-sdk.md) or send $SHDW directly to the storage account's token address
2. Use the \`refreshStake\` method in one of the SDKs to refresh your storage account's stake status. This is not done for you, you must do this step manually.

</details>

<details>

<summary>How much are the Mutable storage fees?</summary>

Mutable storage fees target a specific USD price. Currently, that is $0.05 USD per gibibyte per year. This comes out to $0.0002739726 USD per gib per Solana Epoch (interval for which mutable storage fees are collected. This price target is converted to $SHDW/$USDC at the time of fee collection.

Mutable storage fees are collected for bytes stored.

</details>

<details>

<summary>Where should I send PRs if I have a good example to share? Is there a way to give feedback on the technical documents?</summary>

We welcome any feedback and examples you can provide to our documentation. You can submit a PR to our technical documents repository here - https://github.com/GenesysGo/docs-Shdw-cloud/tree/main - and we will find a good place for it.

</details>

### General

<details>

<summary>What makes ShdwDrive unique?</summary>

ShdwDrive is a commodity cloud network that offers multiple service options, leveraging distributed ledger technology, and offering vertically integrated, L1-specific storage and compute. It is the only cloud network designed to democratize the earnings of traditional cloud platforms without sacrificing performance. Being S3-compatible, ShdwDrive maintains an open-source SDK and interoperability standards that make it easy to access through popular builder tools and SDKs. Its objective is to support popular tools that make building easier, regardless of the application you are building.

</details>

<details>

<summary>How does ShdwDrive ensure data privacy and security?</summary>

ShdwDrive ensures data privacy and security by encrypting and erasure coding the data, and then algorithmically distributing the fragments across the distributed network. This is done trustlessly via smart contracts and requires signed Solana transactions, creating a publicly verifiable on-chain log. Additionally, ShdwDrive provides developers with the tools they need to comply with GDPR and can show records that prove that they have deleted a user's personal data.

</details>

<details>

<summary>How is GDPR handled?</summary>

ShdwDrive provides developers with tools to comply with GDPR and can provide records to prove the deletion of a user's personal data. All records for GDPR compliance are stored on-chain and have been verified by the Solana validator network. The data is then encrypted and algorithmically distributed across the network in triplicate. All transactions are signed and publicly verifiable on-chain.

</details>

<details>

<summary>Is ShdwDrive supported on mobile?</summary>

Yes, ShdwDrive is supported on mobile through our ecosystem partners who are actively building on mobile. Please check out our Shdw Ecosystem page for more details. https://docs.shadow.cloud/build/community-mainted-uis

Additionally, in the future, our _D.A.G.G.E.R._ distributed ledger technology will enable Solana Saga powered storage solutions for those seeking low cost decentralized mobile clouds. Please check out the Learn section for more information. You can read more here: https://docs.shadow.cloud/learn#compute

</details>

<details>

<summary>Is ShdwDrive S3-compatible?</summary>

Yes, ShdwDrive is S3-compatible. S3-compatibility is a widely adopted standard in the cloud storage industry, and many providers offer S3-compatible APIs and protocols, which gives builders greater flexibility in choosing a cloud storage provider. This means developers can easily move data between different services without worrying about compatibility issues. Additionally, S3-compatibility offers robust APIs that enable fast and reliable query, along with virtual mount capability, making it important for Web2, Web3, and the frontiers of distributed ledger tech and AI. ShdwDrive aims to empower developers to integrate it directly into their builds, and to support the talented community of designers who will create innovative platforms for ShdwDrive. You can read more here: https://docs.shadow.cloud/learn/design#s3-compatibility

</details>

<details>

<summary>What physical infrastructure powers ShdwDrive?</summary>

ShdwDrive runs on a global network of bare metal infrastructure, with all compute and storage existing on bare metal. There is no dependency on cloud providers for ShdwDrive operations. For more details on the design of ShdwDrive, please see the "Design" section under the "Learn" category: https://docs.shadow.cloud/learn/design

</details>

<details>

<summary>What is GenesysGo?</summary>

GenesysGo (GG) is a company that was founded in April 2021 as a Solana validator. Since then, GG has expanded its offerings to focus on a large ecosystem of tools and infrastructure for Solana. More details about the scope of our offerings can be found under the "Learn" category. GG has a team of talented developers and coders who are dedicated to building innovative solutions for the Solana community. For more information, you can visit our website at https://Shdw.cloud.

</details>

<details>

<summary>Can I advertise my project if I use D.A.G.G.E.R./ShdwDrive?</summary>

Yes, the ShdwDrive team would love to hear about your project if you are building on top of the Drive or using _D.A.G.G.E.R_. The best way to gain visibility is to submit a PR directly to the docs-Shdw-cloud repo adding your project/business, details, and image to the Shdw Ecosystem list: https://github.com/GenesysGo/docs-Shdw-cloud

Submit a PR to edit the file located here: https://github.com/GenesysGo/docs-Shdw-cloud/blob/main/build/Shdw-drive/community-mainted-uis.md

You can also share your work in the [ShdwDrive Discord](https://discord.com/invite/genesysgo). We will soon release an automated process to be added to the Shdw Ecosystem page.

</details>
