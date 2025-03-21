# Mint Aptos NFT With NMKR Studio
### This is a very simple example how to mint an NFT with NMKR Studio on the Aptos blockchain. You can find other samples in the corresponding folders.

## Prerequisites
First create an Account with NMKR Studio and create an API Key.

Second, Login and buy some Mint Coupons. Since your Aptos NFT needs a collection, you have to fill up your account with at least one mint coupon.


## URLs for the API
The API Addresses for NMKR Studio are:

- Testnet: https://studio-api.preprod.nmkr.io
- Mainnet: https://studio-api.nmkr.io

  
## Step 1: Create a Project
Upload a Collection Image to IPFS or any other Decentral Filesystem

Create an Aptos only Project:
```shell
curl --request POST \
  --url https://<apiaddress>/v2/CreateProject \
  --header 'Content-Type: application/json' \
  --header 'authorization: <apikey>' \
  --data '{
      "projectname":"TestProjectAptos",
      "description":"This is the description of the Aptos Project",
      "payoutwalletaddressaptos":"0x76b98eb82b78134fb039dc22bde0f189c9e77ca4d6175f14627f0ae5163db49e",
      "maxNftSupply":1,
      "addressExpiretime":60,
    	"enableaptos":true,
    	"enablecardano":false,
    	"enablesolana":false
}
'
```
You will receive an JSON Result (or an errormessage) with some informations:

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

## Step 2: Upload an NFT
Notice the UID, because this is neccessary to upload the NFT into the project. If you want to have different metadata, you can have a look into the samples for traits and fixed fields in the metadata.

Now you can add some NFTs to your project with the following call:

```shell
curl --request POST \
  --url https://<apiaddress>/v2/UploadNft/apiaddress> \
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

## Step 3: Catch a payment address for an random NFT
```shell
 curl --request GET \
  --url 'https://apiaddress>/v2/GetPaymentAddressForRandomNftSale/apiaddress>/<countnft>/<priceinoctas>?blockchain=Aptos' \
  --header 'authorization: <apikey>'
  
```

If no error occurs, you will receive an JSON like this:

```json
{
	"paymentAddress": "0xb6aa8844961de0e7416008288eb9ac4bea207d99f54a048d3ecf5ee526e0f13c",
	"paymentAddressId": 87769,
	"expires": "2025-03-21T14:33:18.9652512+00:00",
	"adaToSend": null,
	"solToSend": null,
	"aptToSend": "1",
	"debug": "",
	"priceInEur": 5.06,
	"priceInUsd": 5.47,
	"priceInJpy": 816.5,
	"priceInBtc": 0,
	"effectivedate": "2025-03-21T13:33:19.2424227+00:00",
	"priceInLovelace": 0,
	"additionalPriceInTokens": null,
	"sendbackToUser": 0,
	"revervationtype": "random",
	"currency": "APT",
	"priceInLamport": 0,
	"priceInOcta": 100000000
}
```
Here you find an aptos payment address (paymentAddress) where the buyer has to send 1 APT. The payment address is valid until the date in expires. The price is also shown in EUR, USD, JPY and BTC.
After the buyer has made the payment, the NFT will be minted and transfered to the buyer.


