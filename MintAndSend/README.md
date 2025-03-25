# Mint Aptos NFT with NMKR Studio
### This is a very simple example how to mint an NFT with NMKR Studio on the Aptos blockchain. You can find more samples in the corresponding folders.

## Prerequisites
First create an Account with NMKR Studio and create an API Key.

Second, Login and buy some Mint Coupons. Since your Aptos NFT needs a collection, you have to fill up your account with at least one mint coupon.


## URLs for the API
The API Addresses for NMKR Studio are:

- Testnet: https://studio-api.preprod.nmkr.io
- Mainnet: https://studio-api.nmkr.io


## Step 1: Create a Project
Upload a Collection Image to IPFS or any other Decentral Filesystem.\
Alternatively, you can also create the project in NMKR Studio and upload the collection image there. We save this automatically in IPFS.

Create an Aptos only Project:
```shell
curl --request POST \
  --url https://<apiaddress>/v2/CreateProject \
  --header 'Content-Type: application/json' \
  --header 'authorization: <apikey>' \
  --data '{
      "projectname":"TestProjectAptos",
      "description":"This is the description of the Aptos Project",
      "aptoscollectionname":"TestCollectionAptos",
      "aptoscollectionimageurl":"https://c-ipfs-gw.nmkr.io/ipfs/QmXHBX67PrmXabzJ66VxLQKt25kaeeoJBdC3ytbSaTPFuQ",
      "aptoscollectionimagemimetype":"image/jpeg",
      "payoutwalletaddressaptos":"0x76b98eb82b78134fb039dc22bde0f189c9e77ca4d6175f14627f0ae5163db49e",
      "maxNftSupply":1,
      "addressExpiretime":60,
    	"enableaptos":true,
    	"enablecardano":false,
    	"enablesolana":false
}
'
```
The AptosCollectionName is required, because the collection is the main object for the NFTs. The AptosCollectionImage is the image of the collection. The PayoutWalletAddressAptos is the address where the payments for the NFTs will be sent (the sellers wallet).\
The MaxNftSupply must be 1, otherwise the Project is no NFT, but a Fungible Token project. At the moment we do not support FT Projects in the Aptos Blockchain.\
The AddressExpireTime is the time in minutes, how long the payment address is valid.


If you want to enable the Aptos Blockchain, set the EnableAptos to true. If you want to enable the Cardano Blockchain, set the EnableCardano to true. If you want to enable the Solana Blockchain, set the EnableSolana to true.



You will receive an JSON Result (or an errormessage) with some information:

```json
{
	"projectId": 0,
	"metadata": null,
	"policyId": null,
	"policyScript": null,
	"policyExpiration": null,
	"uid": "89b066c3-67ae-40b2-acc9-93cfb8fc8689",
	"metadataTemplateAptos": null,
	"metadataTemplateSolana": "{\r\n  \"name\": \"<asset_name>\",\r\n  \"image\": \"https://c-ipfs-gw.nmkr.io/ipfs/<ipfs_link>\",\r\n  \"properties\": {\r\n    \"files\": [\r\n      {\r\n        \"type\": \"<mime_type>\",\r\n        \"uri\": \"https://c-ipfs-gw.nmkr.io/ipfs/<ipfs_link>\"\r\n      }\r\n    ]\r\n  },\r\n  \"description\": \"<project_description>\",\r\n  \"attributes\": [\r\n    {\r\n      \"trait_type\": \"description\",\r\n      \"value\": \"<project_description>\"\r\n    }\r\n  ]\r\n}"
}
```

You can also create the project in NMKR Studio (https://studio.nmkr.io - or https://studio.preprod.nmkr.io for Testnet)



## Step 2: Upload one (or more) NFTs to the Project
Notice the UID, because this is necessary to upload the NFT into the project. If you want to have different metadata, you can have a look into the samples for traits and fixed fields in the metadata.

Now you can add some NFTs to your project with the following call:

```shell
curl --request POST \
  --url https://<apiaddress>/v2/UploadNft/<projectuid> \
  --header 'Content-Type: application/json' \
  --header 'authorization: <apikey>' \
  --data '{
   "tokenname": "Nft1",
   "displayname": "Nft #1",
   "description": "This is the first NFT in the new collection",
   "previewImageNft": {
     "mimetype": "image/jpeg",
     "fileFromsUrl": "https://picsum.photos/640/480"
  }

}'
```
After that, you receive an JSON Result (or an error)

```json
{
	"nftId": 2511295,
	"nftUid": "9ee88c31-1ae2-41e1-99b3-4fc19f83c8d8",
	"ipfsHashMainnft": "QmVuXZVkpKsH9oQrdEQ4wzCzHq5hJMEdDskxYAaiuAe6FB",
	"ipfsHashSubfiles": [],
	"metadata": null,
	"assetId": "565f3a76a2bffab0dcb5745e4a8409275dd9203c2e68bf0b6e72fdf44e667434",
	"metadataAptos": "{\r\n  \"name\": \"Nft1\",\r\n  \"image\": \"https://c-ipfs-gw.nmkr.io/ipfs/QmVuXZVkpKsH9oQrdEQ4wzCzHq5hJMEdDskxYAaiuAe6FB\",\r\n  \"properties\": {\r\n    \"files\": [\r\n      {\r\n        \"type\": \"image/jpeg\",\r\n        \"uri\": \"https://c-ipfs-gw.nmkr.io/ipfs/QmVuXZVkpKsH9oQrdEQ4wzCzHq5hJMEdDskxYAaiuAe6FB\"\r\n      }\r\n    ]\r\n  },\r\n  \"description\": \"This is the description of the Aptos Project\",\r\n  \"attributes\": [\r\n    {\r\n      \"trait_type\": \"description\",\r\n      \"value\": \"This is the description of the Aptos Project\"\r\n    }\r\n  ]\r\n}",
	"metadataSolana": null
}
```

## Step 3: Send a random NFT to a receiver

```shell

curl --request GET \
  --url https://<apiaddress>/v2/MintAndSendRandom/<projectuid>/<countnft>/<receiveraddress>&blockchain=Aptos \
  --header 'authorization: <apikey>'
```

If there are still nfts available you will receive a JSON Result like this

```json
{
  "mintAndSendId": 26532,
  "sendedNft": [
    {
      "id": 2384360,
      "uid": "2c2e1420-a7a7-497b-9e2a-32e73a513944",
      "name": "test0018",
      "displayname": null,
      "detaildata": null,
      "ipfsLink": "ipfs://QmYpckCWQGzJ3wuFbXYcm6caAqLX5TqMxooS7SXD2aCE8r",
      "gatewayLink": "https://c-ipfs-gw.nmkr.io/ipfs/QmYpckCWQGzJ3wuFbXYcm6caAqLX5TqMxooS7SXD2aCE8r",
      "state": "reserved",
      "minted": false,
      "policyId": null,
      "assetId": null,
      "assetname": null,
      "fingerprint": null,
      "initialMintTxHash": null,
      "series": null,
      "tokenamount": 0,
      "price": null,
      "selldate": null,
      "paymentGatewayLinkForSpecificSale": null,
      "priceSolana": null,
      "priceAptos": null,
    }
  ]
}
```

The NFT is now queued for minting and sending. The receiver will receive the NFT in the next minting cycle, which will be in the next few seconds.

## Step 4: Check the state of the NFT
To check the state of the NFT you can use the following command:

```shell
curl --request GET \
  --url https://<apiaddress>/v2/GetNftDetailsById/<nftuid> \
  --header 'authorization: <apikey>'
```

You will receive a JSON Result like this:

```json
{
  "id": 2511296,
  "ipfshash": "QmYQTHmWAAGAGX3jeffzZPcocAXhAuKSnZofMCy1kKM4Lt",
  "state": "sold",
  "name": "Nft1",
  "displayname": "Nft #1",
  "detaildata": "This is the first NFT in the new collection",
  "minted": true,
  "receiveraddress": "0x76b98eb82b78134fb039dc22bde0f189c9e77ca4d6175f14627f0ae5163db49e",
  "selldate": "2025-03-21T15:05:59",
  "soldby": null,
  "reserveduntil": null,
  "policyid": null,
  "assetid": null,
  "assetname": "4e667431",
  "fingerprint": null,
  "initialminttxhash": null,
  "title": null,
  "series": null,
  "ipfsGatewayAddress": "https://c-ipfs-gw.nmkr.io/ipfs/QmYQTHmWAAGAGX3jeffzZPcocAXhAuKSnZofMCy1kKM4Lt",
  "metadata": "{\r\n  \"name\": \"Nft1\",\r\n  \"image\": \"https://c-ipfs-gw.nmkr.io/ipfs/QmYQTHmWAAGAGX3jeffzZPcocAXhAuKSnZofMCy1kKM4Lt\",\r\n  \"properties\": {\r\n    \"files\": [\r\n      {\r\n        \"type\": \"image/jpeg\",\r\n        \"uri\": \"https://c-ipfs-gw.nmkr.io/ipfs/QmYQTHmWAAGAGX3jeffzZPcocAXhAuKSnZofMCy1kKM4Lt\"\r\n      }\r\n    ]\r\n  },\r\n  \"description\": \"This is the description of the Aptos Project3\",\r\n  \"attributes\": [\r\n    {\r\n      \"trait_type\": \"description\",\r\n      \"value\": \"This is the description of the Aptos Project3\"\r\n    }\r\n  ]\r\n}",
  "singlePrice": null,
  "uid": "390e242b-33e6-416b-ae5a-a2fbd9b92553",
  "paymentGatewayLinkForSpecificSale": "https://pay.preprod.nmkr.io/?p=24fc0cb0ba104034ad4d1fea9c9cb547&n=390e242b33e6416bae5aa2fbd9b92553",
  "sendBackCentralPaymentInLovelace": null,
  "priceInLovelaceCentralPayments": null,
  "uploadSource": "API",
  "priceInLamportCentralPayments": null,
  "singlePriceSolana": null,
  "priceInOctsCentralPayments": null,
  "mintedOnBlockchain": "Aptos",
  "mintingfees": null
}

```

Here you see, that the NFT is in state "sold", which means, the NFT is minted and send to the receiver. The receiver can now see the NFT in his wallet.

## Mint a specific NFT
If you want to mint a specific NFT, you can use the following command:

```shell
curl --request GET \
  --url https://<apicall>/v2/MintAndSendSpecific/<projectuid>/<nftuid>/<tokencount>/<receiveraddress>?blockchain=Aptos \
  --header 'authorization: <apikey>'
```
Please note, that the NFT must be in state "free" to be minted and send to a receiver. If the NFT is in state "sold" it is already minted and send to the receiver. If the state is "reserved" the NFT is reserved for another receiver and can not be minted unless the nft state is released.

The Tokencount must always be 1 on NFT Projects. If you have a FT (Fungible Token) Project, you can mint more than one token.

