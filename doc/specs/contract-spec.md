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

`map[game_id, [Game Asset Contract address]]`: a mapping from game_id to Game Asset contract address list `[contract address1, contract address2, contract address3...]` 

`map[address,[(game_id, game_id_account)]]`:a mapping from address to game information list `[(game_id_1, game_id_1_account), (game_id_2, game_id_2_account),...]`

#### Bind Function

Binding a game account to blockchain address. Append account information `(game_id, game_id_account)` to `map[address,[(game_id, game_id_account)]]`

#### Unbind Function

Unbinding a game account from blockchain address. Remove account information `(game_id, game_id_account)` from `map[address,[(game_id, game_id_account)]]`

#### Register Function

Register a game or a game asset to blockchain.  Append game information `Game Asset Contract address` to `map[game_id, [Game Asset Contract address]]`. If `game_id` is not in map, must create a entry in map.

#### Unregister Function

Unregister a game or a game asset from blockchain.  Remove game information `Game Asset Contract address` from `map[game_id, [Game Asset Contract address]]`. If `game_id` has not any value in map, must delete `game_id` from map.

#### Query Function

TODO

## Game Asset Contract

The purpose of this contract is to store one game's assets state. One game could have more contracts that mapping different type of assets. Of course, one contract can have many assets for the same game.

#### State Data Structure

`List[asset_id_1, asset_id_2, asset_id_3...]`: a list for one game's assets

**TODO**: Need to design a structure to store assets data.

#### Query Function

TODO

#### Add Function

Adding asset data to above structure.

#### Sub Function

Subing asset data to above structure.

#### New Function

Append a new asset to list.

#### Delete Function

Remove a exist asset to list.

## Asset Trade Contract

The purpose of this contract is to handle trade

#### State Data Structure

TODO

#### Trade Function

TODO

## Asset Rent Contract

#### State Data Structure

#### Rent Function

## Smart Wallet Contract

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

## Store Contract

#### State Data Structure

#### Build Function

#### Destroy Function

#### NewOrder Function

#### RevokeOrder Function

## Mall Contract

#### State Data Structure

#### Append Function

#### Remove Function

#### Update Function

## 