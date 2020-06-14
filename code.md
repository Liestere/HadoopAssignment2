# For a 6
```
loadedOrders = LOAD '/user/maria_dev/Diplomacy/orders.csv'  USING PigStorage(',') 
AS (game_id:chararray, unit_id:chararray, unit_order:chararray, location:chararray, target:chararray, target_dest:chararray, success:chararray, reason:chararray, turn_num:chararray);

filteredHolland = FILTER loadedOrders BY (target matches '.*Holland.*');
countedTarget = FOREACH (GROUP filteredHolland BY location) GENERATE FLATTEN (group), filteredHolland.target, COUNT($1);
distinctTarget = DISTINCT countedTarget;
sortedTarget = ORDER distinctTarget BY $0 ASC;

DUMP sortedTarget;
```

# For a 7
```
loadedPlayers = LOAD '/user/maria_dev/Diplomacy/players.csv'  USING PigStorage(',') 
AS (game_id:chararray, country:chararray, won:chararray, num_supply_centers:chararray, eliminated:chararray, start_turn:chararray, end_turn:chararray);

filteredWins = FILTER loadedPlayers BY (won matches '.*1.*');
countedWins = FOREACH (GROUP filteredWins BY country) GENERATE FLATTEN (group), COUNT($1);
sortedWins = ORDER countedWins BY $0 ASC;

DUMP sortedWins;
```
