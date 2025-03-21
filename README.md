# Mint Aptos NFT With NMKR Studio

First create an Account with NMKR Studio and create an API Key.

Second, Login and buy some Mint Coupons. Since your Aptos NFT needs a collection, you have to fill up your account with at least one mint coupon.



The API Addresses for NMKR Studio are:

- Testnet: https://studio-api.preprod.nmkr.io
- Mainnet: https://studio-api.nmkr.io

  
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

Notice the UID, because this is neccessary to upload the NFT into the project. If you want to have different metadata, you can have a look into the samples for traits and fixed fields in the metadata.

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
