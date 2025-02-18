# Steam gems

## getting gems for item type

```
https://steamcommunity.com/auction/ajaxgetgoovalueforitemtype/?appid=239350&item_type=2
```

## Getting item type for an item

1. Go to page of any item like
   [https://steamcommunity.com/market/listings/753/239350-Hardcore](https://steamcommunity.com/market/listings/753/239350-Hardcore)
2. View page source
3. Search for something like `javascript:GetGooValue( '%contextid%',
'%assetid%', 239350, 21, 0 )` here `21` is the item type

[[steam]]

## Item gem correlations

- foil card gem value is always (normal card gem value) \* 10
- emoticon gem value is always the same as background gem value => enough to
  get one emoticon (or background) gem value for app
