### Create a project with a pricelist

You can define pricelists in every single project. This is useful if you want to sell NFTs for different prices. \

You can define a pricelist with the following call:

```shell
curl --request POST \
  --url https://<apiaddress>/v2/CreateProject \
  --header 'Content-Type: application/json' \
  --header 'authorization: <apikey>' \
  --data '{
   "projectname":"ProjectName",
   "description":"Test Collectible",
   "payoutWalletaddress":"addr_test1qqcc3smxujsxj8qcmuz8uncfavwh4wgkwkyztcwy78vnank645uv25ew03p69h2acvapem95kfajm4lcrzfuatuc093qvyxyh5",
   "maxNftSupply":1,
   "addressExpiretime":60,
   "aptoscollectionname":"TestCollectionAptos",
   "aptoscollectionimageurl":"https://c-ipfs-gw.nmkr.io/ipfs/QmXHBX67PrmXabzJ66VxLQKt25kaeeoJBdC3ytbSaTPFuQ",
   "aptoscollectionimagemimetype":"image/jpeg",
   "payoutwalletaddressaptos":"0x76b98eb82b78134fb039dc22bde0f189c9e77ca4d6175f14627f0ae5163db49e",
   "priceList":[
      {
         "countNft":1,
         "currency":"ADA",
         "price":10,
         "isActive":true
      },
      {
         "countNft":2,
         "currency":"ADA",
         "price":20,
         "isActive":true
      },
      {
         "countNft":3,
         "currency":"ADA",
         "price":30,
         "isActive":true
      },
      {
         "countNft":1,
         "currency":"APT",
         "price":1,
         "isActive":true
      },
      {
         "countNft":2,
         "currency":"APT",
         "price":2,
         "isActive":true
      },
      {
         "countNft":3,
         "currency":"APT",
         "price":3,
         "isActive":true
      },
      {
         "countNft":5,
         "currency":"USD",
         "price":70,
         "isActive":true
      }
   ],
   "enableDecentralPayments":false,
   "enableCrossSaleOnPaymentgateway":true,
   "activatePayinAddress":true,
   "enableFiat":false,
   "enableaptos":true,
   "enablecardano":true,
   "enablesolana":false
}
'
```

You can define a price for every blockchain. So in this example, i have defined a price for 1 or 2 or 3 NFT in Aptos and Cardano and 5 NFTs in USD.\
If you define a price in USD, EUR or JPY, the amount will be calculated in the current exchange rate.\

If you want now an payin address where the user can buy a random NFT, you can call the following function:

```shell
curl --request GET \
  --url 'https://<apiaddress>/v2/GetPaymentAddressForRandomNftSale/<projectuid>/<countnft>?blockchain=Aptos' \
  --header 'authorization: <apikey>'
  
```

The countnft is the number of NFTs you want to sell. If you have defined a price with this amount of nfts, the call will return you an address. If you have not defined a price for this amount of nfts, you will receive an error.\

Please note that it is not possible to define a price for an amount of NFT in FIAT and Crypto. So you can not define a price for 1 NFT in USD/EUR/JPY and for 1 NFTs in ADA/APT/SOL.\

