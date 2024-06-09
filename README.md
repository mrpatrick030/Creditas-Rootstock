# Creditas Documentation [Web App](https://creditasdapp.vercel.app) / [Demo Video](https://youtu.be/JmK5-9mpDOk?si=caAhJLKhu2tL_ibK)
![File](https://ipfs.filebase.io/ipfs/QmRxLpsZSFTuZg6c6cNXxRnUFsyNA3QEPvpNQc8eRWuJUK)
Through its P2P Lending/Borrowing dApp, Creditas is a DeFi protocol that aims to address a number of lending and borrowing-related difficulties. Creditas provides platform users with an easy-to-use interface by combining front end, integration, and smart contract technologies in a powerful way. The Rootstock test network is the foundation of Creditas.
Yes, your data and assets on the platform are safe and only accessible by you with a wallet or email login, but the Creditas dashboard makes it easy to access the platform's dApp.


## Features of Creditas
This platform features a robust combination of smart contracts and front end and integration tools ranging from a P2P Lending/Borrowing dApp, a Dashboard, wallet and email login (using Web3Modal x Ethers.js), decentralised chat in the lending dApp, documentation, Filebase storage, search functionalities, paginations, flaticons library, and an intuitive interface with a slick responsive design and smooth user experience.

## Tools used to create Creditas
### Smart Contract Tools
Solidity, Foundry, OpenZeppelin

### Front end and Integration Tools
Web3Modal, Ethers.js, Rootstock testnet Token Tracker, Next.js, Node.js, JavaScript, Tailwind CSS, Trading View Live API, Filebase storage, PostgreSQL

## The Creditas User Flow

### Connect Wallet
![File](https://ipfs.filebase.io/ipfs/QmY5gXEQbqxiSFsdsg3qLLhjTQfGoboMH3cHkXnY9FqZyP)
To connect wallet, go to our [Web App](https://Creditasdapp.vercel.app). For desktop, click on the "Connect Wallet" button on the upper right of the screen and choose your login method either with wallet or email. If you don't have a wallet installed already, the wallet connect window has links to download a wallet of your choice on your device's browser (Brave browser, Google Chrome, Opera browser, or Yandex browser all work fine). On mobile, simply download your desired EVM-compatible wallet and use its inbuilt browser to connect to Creditas. Connect your wallet by clicking on the "Connect wallet" button on the upper right of the screen. Ensure to change network to Rootstock test network from the wallet connect window. The display on the screen will change after connecting (typically, it shows your tRBTC balance and a substring of your connected wallet address).

### Get testnet tRBTC for gas fees payments
![File](https://ipfs.filebase.io/ipfs/QmeTAgrea7RSrNT3siQuFW98TBsx1SVxpVEemYCwE6UEs4)
With Creditas being deployed currently on the testnet of Rootstock, testnet tRBTC is required to pay for gas fees during transactions. Testnet tRBTC can be obtained from the [Rootstock testnet faucet](https://faucet.rootstock.io/).

### Web app homepage
The homepage of the Creditas's web app is the first page you see when you visit the app. It is the gateway to Creditas's dashboard and features the wallet connect button as well as other important information about the platform including documentation.

 ### The Creditas Dashboard
This is the most functional part of Creditas. It houses the most important parts of the platform including User's Balances, Metrics and the P2P Lending/Borrowing dApp.

### User's Balances
![File](https://ipfs.filebase.io/ipfs/QmVEpoh4TWRAiEYB4STqQzNSTjrSR5fWtDwtBWsqNFx5L9)
This section of the dashboard shows all the balances of a user after logging in. It displays the following info for a user: ULT balance, ULT price, tRBTC balance, number of loans you created, number of loans you funded.

### Metrics
![File](https://ipfs.filebase.io/ipfs/QmNYspNFfUD1zb2xErbZFkdmn3wv8SV1sh38i9W1THkeGp)
The metrics section shows live updates to Creditas including the following: ULT Total Supply, ULT Price, Protocol Blockchain, Total number of Loans, Max Loan Amount, Min Loan Amount, contract addresses of all contracts used by the protocol and Live BTC market chart.


### P2P Lend/Borrow dApp
Creditas users are able to participate in unique P2P lending/borrowing activities of supported tokens. Firstly, a loan has to be created by the borrower using the "Approve/Create Loan" buttons in the Borrow section, then another user (the lender) funds the loan by lending to the borrower using the "Fund Loan" button in the Lend section. The loan has the following characteristics: loan amount, interest, expiry date, and collateral. Collateral provided by the borrower is locked up by the system until the given duration expires, and can be claimed back if there is no lender after the duration of the loan using the "Withdraw Collateral" button in the Lend section of the dApp. Borrowed tokens can also be claimed using the "Withdraw loan" button. The collateral can be claimed at anytime by the lender if the borrower fails to repay the loan during the specified time using the "Claim collateral" button in the Lend section. When you lend on Creditas, you get rewarded hugely according to the agreed terms of the loan (your funds are secured with our security algorithm). When you borrow from Creditas P2P, ensure to present a collateral and pay back with interest when you repay the amount using the "Repay loan" button in the Lend section. For a better user experience, a "view all available loans section" and "view your loans section" with search functionality and pagination have been integrated into the dApp for lenders to navigate between loans. A decentralised chat has also been integrated for lenders and borrowers to effectively communicate, all while maintaining privacy.  New tokens/collaterals will be added to the dApp in due time. 

### Borrowing
![File](https://ipfs.filebase.io/ipfs/QmQE8xG6K84q6Q3BGjAjm1nhHwxdaAno4Km2QrpgMmoZyz)
To borrow, follow the steps below (Your wallet will popup to sign the transactions):
* Fill the details above setting the duration, amount of tRBTC to borrow, collateral, collateral amount, "yes if collateral is ERC20 or no if collateral is ERC721 (an NFT)", and interest rate (which is the amount of interest you will pay to any user that funds your loan).
* Click on the "Approve" button to approve the loan transaction.
* Proceed to create the loan by clicking on the "Create Loan" button.
After creating the loan, you will be automatically navigated to the Lending section where you can monitor all your created loans.

### Lending
![File](https://ipfs.filebase.io/ipfs/QmP2r6Wftt8meiD1dgN3syNDVKYxToxmTFzQa8fFhW3DCB)
Lending on Creditas is easier than you think, with a smooth user experience, users can easily carry out lending activities. With a "View all loans" section, "Loans you funded" and "Loans you created" sections, users can easily track their loan activities. Search and pagination functionaliites also compliment this beauty of a Lending dApp. To lend, the following buttons are available to use on the dApp.
* #### Fund loan ####: 
This button is the button a user clicks on to fund a loan created by a borrower. No user can fund his own loan.
* #### Claim collateral ####: 
The claim collateral button is typically available for the lender to use in case a borrower fails to repay a loan during the specified duration in the loan. You can only claim collateral of a loan you funded.
* Withdraw loan: This button can be used by the borrower of a loan to withdraw the loan after it has been funded by a lender.
* Repay loan: This button is available to only the borrower of a loan such that before the end of the loan duration, the borrower uses it to repay the borrowed amount with interest.
* Withdraw collateral: If there is a scenario where there is no lender of a loan you created as a borrower, you can use this button to withdraw your collateral back. You can use this button only when your loan has not been funded.


### Creditas Token (ULT)
The Creditas token (ULT) is the native token of Creditas and is used as a collateral for lending. Other tokens available as collaterals currently are USDT and DAI. 

## Additional Notes
In future upgrades of this project, more dApp features/services will be integrated. We are also focussed on user experience. File sharing through IPFS and Filebase SDK will also be integrated in the decentralised chat system for effective communication.

## Team Members (Team Creditas)
* Patrick Ominisan - Blockchain developer (Web3Bridge)
* Kolapo Goodness - Smart contract developer (Web3Bridge)


# Contract Addresses with Rootstock Testnet Links [Rootstock Explorer](https://explorer.testnet.rootstock.io)
![File](https://ipfs.filebase.io/ipfs/QmUbcvrVF2FuDY5n8BDRSFJ3L2Raie2crQrUuwkFgGHaMg)

## P2P Lending/Borrowing Contract

✅ [P2PLending](https://explorer.testnet.rootstock.io/address/0xe9c49311b81545cced67ab3313c8f4f938ba1920) 🟢
- Address: `0xe9c49311b81545cced67ab3313c8f4f938ba1920`

## Creditas ERC20 (ULT) Contract

✅ [Creditas Token](https://explorer.testnet.rootstock.io/address/0x1d5cd5833f43c63f724ebb0f28c6aaed79bf5bf2) 🟢
- Address: `0x1d5cd5833f43c63f724ebb0f28c6aaed79bf5bf2`

# Other Tokens

## USDT

- **[USDT](https://explorer.testnet.rootstock.io/address/0xadd24771bcb47b28d85cefb641ef469bc9e52cf7)**: 0xadd24771bcb47b28d85cefb641ef469bc9e52cf7

## DAI

- **[DAI](https://explorer.testnet.rootstock.io/address/0x5816ab7fffd3f5c4d1897539240873b29c0f3aec)**: 0x5816ab7fffd3f5c4d1897539240873b29c0f3aec
# Creditas-DeFi-Protocol
