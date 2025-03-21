# Mint Aptos NFT With NMKR Studio

First create an Account with NMKR Studio and create an API Key.

Second, Login and buy some Mint Coupons. Since your Aptos NFT needs a collection, you have to fill up your account with at least one mint coupon.



The API Addresses for NMKR Studio are:

- Testnet: https://studio-api.preprod.nmkr.io
- Mainnet: https://studio-api.nmkr.io

  
Upload a Collection Image to IPFS or any other Decentral Filesystem

Create the Project:
```
curl --request POST \
  --url https://studio-api.preprod.nmkr.io/v2/CreateProject \
  --header 'Content-Type: application/json' \
  --header 'authorization: <apikey>' \
  --data '{
   "projectname":"TestProjectAptos",
   "description":"This is the description of the Aptos Project",
   "payoutwalletaddressaptos":"0x76b98eb82b78134fb039dc22bde0f189c9e77ca4d6175f14627f0ae5163db49e",
   "maxNftSupply":1,
   "addressExpiretime":60,
   "enableaptos":"true",
   "enablecardano":"false",
   "enablesolana":"false",
   "aptoscollectionimageurl":"",
   "aptoscollectionimagemimetype":"",
}
'
```
