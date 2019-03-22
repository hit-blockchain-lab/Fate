# Phase 0 -- contract specification

**NOTICE**:This document is a work-in-progress for researcher and implementers. It reflects recent sepc changes.

## Table of contents

## Introduction

This document represents the specification for Phase 0 of Fate -- The contract.

At the core of Fate is contracts. These contracts store and manage the virtual assets of game players on  MaYi blockchain. We design several different smart contracts to support services of the Fate.

## Notation

Code snippets appearing in `this style` are to be interpreted as Python code.

## Constants

## Account Contract

The purpose of this contract is to link the blockchain address to the game player's accounts. One blockchain address could bind different accounts for multiple games.

#### State Data Structure

`game_asset_address`: `map[game_id, [Game Asset Contract address]]`. A mapping from game_id to Game Asset contract address list `[contract address1, contract address2, contract address3...]` 

```
int game_id
uint64 Game_Asset_Contract_address
map[int,list[uint64]] game_asset_address
```

`account_asset`: `map[address,[(game_id, game_id_account)]]`. A mapping from address to game information list `[(game_id_1, game_id_1_account), (game_id_2, game_id_2_account),...]`

```
int game_id
uint64 address
gameAccount game_id_account
map[uint64,list[(int,gameAccount)]] account_asset
```

#### Bind Function

Binding a game account to blockchain address. Append account information `(game_id, game_id_account)` to `map[address,[(game_id, game_id_account)]]`

```
def bind(address, game_id, game_id_account):
	if address not in account_asset:
    	account_asset[address] = [(game_id, game_id_account)]
    else:
    	account_asset[address].append((game_id, game_id_account))
    return true
```

#### Unbind Function

Unbinding a game account from blockchain address. Remove account information `(game_id, game_id_account)` from `map[address,[(game_id, game_id_account)]]`

```
def unbind(address, game_id, game_id_account):
	if address not in account_asset:
    	return false
    else:
    	account_asset[address].remove((game_id, game_id_account))
    	if not len(account_asset[address]):
    		account_asset.remove(address)
    return ture
```

#### Register Function

Register a game or a game asset to blockchain.  Append game information `Game Asset Contract address` to `map[game_id, [Game Asset Contract address]]`. If `game_id` is not in map, must create a entry in map.

```
def register(is_new_game, game_id, game_asset_contract_address):
	if is_new_game:
		game_id = len(game_asset_address)
		game_asset_address[game_id] = [game_asset_contract_address]
	else:
		game_asset_address[game_id].append(game_asset_contract_address)
	return game_id, len(game_asset_address[game_id])-1
		
```

#### Unregister Function

Unregister a game or a game asset from blockchain.  Remove game information `Game Asset Contract address` from `map[game_id, [Game Asset Contract address]]`. If `game_id` has not any value in map, must delete `game_id` from map.

```
def unregister(game_id, game_asset_contract_address):
	if game_id not in game_asset_address:
		return false
	else:
		game_asset_address[game_id].remove(game_asset_contract_address)
		if not len(game_asset_address[game_id]):
			gamse_asset_address.remove(game_id)
	return true
```

#### Query Function

TODO

## Game Asset Contract

The purpose of this contract is to store one game's assets state. One game could have more contracts that mapping different type of assets. Of course, one contract can have many assets for the same game.

#### State Data Structure

`asset_list`: `List[asset_info_1, asset_info_2, asset_info_3...]`: a list for one game's assets information

`asset_data`: `List[asset_data_1, asset_data_2, asset_data_3...]`: asset data for one game

#### Create Function

```
def create():
	asset_list = []
	asset_data = []
	return success
```

#### Query Function

```
def query(asset_info, condition):
	asset_id = asset_list.get(asset_info)
	data = asset_data[asset_id]
	//the fllowing is selecting data which satisfy condition
	...
	
	return data
```

#### Add Function

Adding asset data to above structure.

```
def add(asset_info, condition, add_data):
	asset_id = asset_list.get(asset_info)
	data = asset_data[asset_id].add
	
	//the fllowing is adding data which satisfy condition
	...
	return success
```

#### Sub Function

Subbing asset data from above structure.

```
def sub(asset_info, condition, sub_data):
	asset_id = asset_list.get(asset_info)
	data = asset_data[asset_id].add
	
	//the fllowing is subing data which satisfy condition
	...
	return success
```

#### New Function

Append a new asset to list.

```
def new(asset_info,asset_data):
	asset_list[len(asset_list)] = asset_info
	asset_data[len(asset_data)] = asset_data
	return success
```

#### Delete Function

Remove a exist asset from list.

```
def delete(asset_info,asset_data):
	asset_id = asset_list.get(asset_info)
	del asset_list[asset_info]
	del asset_data[asset_id]
	return success
```



## Asset Trade Contract

The purpose of this contract is to handle trade. 

#### State Data Structure

None

#### Trade Function

Call Game Asset Contract's Add/Sub function to update asset state.

```
def trade(address_src, address_tar, game_id_src, game_id_tar, game_asset_address_src, game_asset_address_tar, data_src, data_tar)
	 TODO
	
```



## Asset Rent Contract

The purpose of this contract is to handle rent. 

#### State Data Structure

None

#### Rent Function

Call Asset Trade function twice and set rent condition.

```
def rent(address_src, address_tar, game_id_src, game_id_tar, game_asset_address_src, game_asset_address_tar, data_src, data_tar, rent_time)
	TODO
```



## Store Contract

The purpose of this contract is to save store state for players. One player only build one store and maintain that. 

#### State Data Structure

`store`: `struct{`

​			` id`

​			`store_infos`

​			`store_orders`

​		`}`

`store_list`: `List[store1,store2,store3...]`

`store_infos`: `struct{`

​			`name`

​			`owner_address`

​			`open_date`

​			`success_orders`

​			`}`

`store_orders`:`struct{`

​			`orders_count`

​			`orders`: `List[order1,order2,order3...]`

​			`}`

`order`:`struct{`

​			`name`

​			`id`

​			`description`

​			`time`

​			`game_id`

​			`asset_address`

​			`asset_info`

​			`asset_data`			

​			`price`

​			`isrent`

​			`rent_time`

`}`

#### Build Function

create `store`, and then append `store` to `store_list`

```
def build()
```



#### Destroy Function

remove `store` from `store_list`, and then delete `store`

```
def destory()
```



#### NewOrder Function

create `order`, and then append `order` to `store_orders`

```
def neworder()
```



#### RevokeOrder Function

remove `order` from `store_orders`, and then delete `order`

```
def revokeorder()
```



## Mall Contract(abandon)

#### State Data Structure

#### Append Function

#### Remove Function

#### Update Function

## Smart Wallet Contract(Need design)

#### State Data Structure

#### Login Function

#### Logout Function

#### BindAsset Function

#### UnbindAsset Function

#### ShowAsset Function

#### BuildStore Function

#### DestroyStore Function

#### SellAsset Function

#### BuyAsset Function

#### ExchangeAsset Function

## 