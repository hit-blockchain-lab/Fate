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

- `Address`: the account address on blockchain
- `Game_ID`: the unique identification for game
- `Game`: data structure for a game about its information, such as `struct{Game_ID, Game_Name, Game_Asset_Contracts}`, `Game_Asset_Contracts` is a list of address of `Game_Asset_Contract`
- `Games_Map`: A map for all registered games on the platform indexed by `Game_ID`,  such as `map{Game_ID_1:Game1, Game_ID_2:Game2, Game_ID_3:Game3 ...}`
- `Account`: data structure for a user about his game accounts., such as `struct{Address, Game_Accounts}`, `Game_Accounts` is `map{Game_ID:list[Game_Account]}` 
- `Account_Map`: A map for all registered user on the platform from `Address` to `Account`, such as `map{Address1:Account1, Address2:Account2, Address3:Account3}` 

#### Bind Function

Binding a game account to blockchain address. Append `Account` to `Account_Map`

```
def bind(address, game_id, game_account):
	if address not in Account_Map:
		account = map{game_id:[game_account]}
    	Account_Map[address] = account
    else:
    	if game_id not in old_account:
    		Account_Map[address].Game_Accounts[game_id] = [game_account]
    	else:
    		Account_Map[address].Game_Accounts[game_id]].append(game_account)
    return true
```

#### Unbind Function

Unbinding a game account from blockchain address. 

```
def unbind(address, game_id, game_account):
	if address not in Account_Map or game_id not in Account_Map[address] or game_account not in Account_Map[address].Game_Accounts[game_id]:
    	return error
    else:
    	Account_Map[address].Game_Accounts[game_id].remove(game_account)
    	if not len(Account_Map[address].Game_Accounts[game_id]):  
    		Account_Map[address].Game_Accounts.remove(game_id)
    	if not len(Account_Map[address].Game_Accounts):
    		Account_Map.remove[address]
    return ture
```

#### Register Function

Register a game or a game asset contract to blockchain.  

```
def register(is_new_game, game_id, game_name, game_asset_contract_address):
	if is_new_game:
		game_id = len(Games_Map)
		game = Game{game_id, game_name, [game_asset_contract_address]}
		Games_Map[game_id] = game
	else:
		Games_Map[game_id].Game_Asset_Contracts.append(game_asset_contract_address)
	return true
		
```

#### Unregister Function

Unregister a game or a game asset  contract from blockchain. 

```
def unregister(is_game, game_id, game_asset_contract_address):
	if is_game:
		Game_Map.remove(game_id)
	else:
		Game_Map[game_id].Game_Asset_Contracts.remove(game_asset_contract_address)
		if not len(Game_Map[game_id].Game_Asset_Contracts):
			Game_Map.remove(game_id)
	return true
```

#### Query Function

TODO 

## Game Asset Contract(Need design)

The purpose of this contract is to store one game's assets state. One game could have more contracts that mapping different type of assets. Of course, one contract can have many assets for the same game. This contract would be built by game developers, and we could offer a standard template. 

#### State Data Structure

- `Asset_ID`: the unique identification for a type of asset in one contract

- `Asset_Type`: a data structure for one asset item. `Struct{Asset_ID, Asset_Type_Name, Asset_Type_Info}`
- `Asset_Types`:  A map for one game's assets information(`Asset_Type`), such as Skins,  Equipment. Like `map{asset_id_1:Asset_Type_1, asset_id_2:Asset_Type_2, asset_id_3:Asset_Type_3}`
- `Asset_Data_Item`: For each type of asset,  game developers must define the corresponding data storage structure `Asset_Data`.   Such as `struct{asset_data_item_id, asset_data_item_owner, asset_data_item_address}`
- `Asset_Data`:  `list[Asset_Data_Item}`
- `Asset_Datas`: A map for one game's assets data,  like `map{asset_id_1:Asset_Data_1, asset_id_2:Asset_Data_2, asset_id_3:Asset_Data_3}`

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

- `trade_data`: data used trading. `struct{Game_ID, Game_Asset_Address, Asset_ID, Asset_Data_Item}`

- `trade_src`:  trade data for the initiator. `strcut{address_src, trade_data_src}`
- `trade_tar`: trade data for the receiver. `strcut{address_tar, trade_data_tar}`

#### Trade Function

Call Game Asset Contract's Add/Sub function to update asset state.



#### 



## Asset Rent Contract

The purpose of this contract is to handle rent. 

#### State Data Structure

- `rent_data`: data used trading. `struct{Game_ID, Game_Asset_Address, Asset_ID, Asset_Data_Item}`

- `rent_src`:  trade data for the initiator. `strcut{address_src, rent_data_src}`
- `rent_tar`: trade data for the receiver. `strcut{address_tar, rent_data_tar}`
- `time`

#### Rent Function





## Store Contract

The purpose of this contract is to save store state for players. One player only build one store and maintain that. 

#### State Data Structure

- `store`: `struct{id, store_infos, store_orders}`

- `store_map`: `map{store_id_1: store_1, store_id_2:store_2, store_id_3:store_3}`

- `store_infos`: `struct{name, owner_address, open_date, success_orders}`

- `store_orders`:`struct{orders_count, orders: List[order1,order2,order3...]}`

- `order`:`struct{name, id, description, time, game_id, asset_address, asset_info, asset_data, price, isrent, rent_time}`

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

- `Wallet_Account`: user account. `struct{userid, username, password, address, money}`
- `Wallet_Accounts`: all accounts. `map{userid: Wallet_Account}`

#### Register&Bind Function

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