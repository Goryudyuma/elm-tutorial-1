# Calculating bonuses

Now that we are fetching all the resources we can properly calculate the bonuses for the players instead of hardcoding 999.

## PerksPlayers Utils

Let's add a few functions to calculate the bonuses for a player. Create a new module in __src/PerksPlayers/Utils.elm__:

```elm
module PerksPlayers.Utils (..) where

import Players.Models exposing (PlayerId)
import Perks.Models exposing (PerkId, Perk)
import PerksPlayers.Models exposing (PerkPlayerId, PerkPlayer)


bonusesForPlayerId : List PerkPlayer -> List Perk -> PlayerId -> Int
bonusesForPlayerId perksPlayers perks playerId =
  perksForPlayerId perksPlayers perks playerId
    |> List.foldl (\perk acc -> acc + perk.bonus) 0


perksForPlayerId : List PerkPlayer -> List Perk -> PlayerId -> List Perk
perksForPlayerId perksPlayers perks playerId =
  let
    perkIds =
      perkIdsForPlayerId perksPlayers playerId

    included perk =
      List.any (\id -> id == perk.id) perkIds
  in
    perks
      |> List.filter included


perkIdsForPlayerId : List PerkPlayer -> PlayerId -> List Int
perkIdsForPlayerId perksPlayers playerId =
  perksPlayers
    |> List.filter (\perkPlayer -> perkPlayer.playerId == playerId)
    |> List.map (\perkPlayer -> perkPlayer.perkId)

```
