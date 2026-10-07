# Tennis Stats Scraper - ATP & WTA Scores, Rankings

ATP and WTA tennis match results with set and tiebreak scores, seeds, rankings and player profiles for any date range.

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-0f9f74)](https://apify.com/datagrit/tennis-stats-scraper) [![Docs](https://img.shields.io/badge/docs-getdatagrit.github.io-0e1726)](https://getdatagrit.github.io/tennis-stats-scraper/)

**from $4.20 per 1,000 results + $10 per run (pay per result; the rate depends on your Apify plan).** Export as JSON, CSV or Excel, call it through the API, or schedule it on Apify.

## What it does

Tennis Stats Scraper returns ATP and WTA tennis matches for any date range as clean, flat rows: tournament, round, players with country and seed, status, winner, the games of every set and the tiebreak points of every tiebreak. The same run can add the current ATP and WTA singles rankings with points and movement, and player profiles with height, playing hand and season record, titles and prize money. It reads the public ESPN tennis JSON API, needs no proxy and no browser, and exports to JSON, CSV or Excel, the Apify API, n8n, Make or AI agents through MCP.

## Quick start

1. Open [Tennis Stats Scraper - ATP & WTA Scores, Rankings on Apify Store](https://apify.com/datagrit/tennis-stats-scraper) and click **Try for free**.
2. Fill in the input form (or paste the JSON below) and run it.
3. Download the dataset, or fetch it from the API.

```json
{
  "dataTypes": [
    "matches"
  ],
  "lastDays": 3,
  "maxItems": 50
}
```

## Input

| Field | Type | What it does |
|---|---|---|
| `dataTypes` | array | What to return: matches (results and schedule with set and tiebreak scores), rankings (current ATP and WTA singles rankings, top 150 per tour) and players (player profiles with season record and prize money; needs Players). One run can combine several types. Default: matches. |
| `dateFrom` | string | First match date to return, YYYY-MM-DD (UTC). Leave empty to use Last days. Match data is available from about 2010; for older seasons ESPN lists tournaments without matches. Future dates return the order of play that is already published. |
| `dateTo` | string | Last match date to return, YYYY-MM-DD (UTC). Leave empty for today (or for Date from, when Date from is in the future). |
| `lastDays` | integer | When Date from is empty the run covers this many days ending on Date to (today by default). 3 means today and the two days before. |
| `tours` | array | Optional. ATP (men), WTA (women) and/or Mixed (mixed doubles at the Grand Slams). Leave empty for all. The tour of a match follows its draw, so the women's matches of a combined event such as the China Open are WTA. Rankings exist for ATP and WTA; player profiles are filtered by the tour of the player. |
| `players` | array | Optional. Player names or ESPN player IDs. Matches: keeps matches in which any listed player plays (the name only has to contain the text, accents and case are ignored, so Zverev matches both Alexander and Mischa Zverev). Rankings: keeps those players. Players data type: returns the profile of every player whose name contains the text among the current ATP/WTA top 150 and the matches read in the run, or of the given ESPN ID (for example 3623). |
| `headToHeadOnly` | boolean | Matches only: keep just the matches in which two different listed players face each other (for example Players = Sinner, Alcaraz returns their meetings). Needs at least two Players. |
| `tournaments` | array | Optional. Keep only matches of tournaments whose name contains one of these texts (for example Wimbledon, Open) or whose ESPN tournament ID equals the value (for example 188 for Wimbledon). |
| `matchTypes` | array | Optional. singles and/or doubles. Leave empty for both. |
| `matchStatuses` | array | Optional. Keep only matches with these statuses: scheduled, in_progress, finished, retired, walkover, postponed, cancelled, suspended. Leave empty for all. Use finished, retired and walkover for completed results only. |
| `includeQualifying` | boolean | Include qualifying-round matches. Turn off for main-draw matches only. |
| `maxRank` | integer | Rankings only: keep players ranked this high or better, for example 10 for the top 10. 0 keeps the whole list (ESPN publishes the top 150 per tour). |
| `onlyNewSinceLastRun` | boolean | Return only rows that earlier runs with the same filters have not delivered to you. The Actor remembers the rows it actually returned, per filter combination (dates excluded, so a scheduled run with Last days keeps one feed), in a storage on your account. A match delivered as scheduled or in progress is delivered again once it is finished; a ranking row again when a new ranking is released. Rows dropped by your filters or cut off by Maximum results are not remembered and can still come later. |
| `oldestFirst` | boolean | Return matches in chronological order (oldest first) instead of the default newest first. Useful for building a history archive in date order. |
| `maxItems` | integer | Stop after this many rows in total. Matches come newest first (see Oldest first), so the limit never cuts off today's results; raise it for long date ranges (a busy month of both tours is 1,300 to 3,000 matches). |
| `proxyConfiguration` | object | Optional proxy. Leave disabled: the ESPN JSON API is public and answers requests from data-center addresses. |

## Output

| Field | Type | Description |
|---|---|---|
| `recordType` | string | What the row describes: match, ranking, player, or status (the single row written when a run returns nothing). |
| `id` | string | ESPN identifier of the match. Stable across runs and identical in the ATP and WTA feeds. |
| `tour` | string | ATP for men's matches and men's rankings, WTA for women's, Mixed for mixed doubles. Derived from the draw of the match, so a women's match at a combined event is WTA. |
| `matchType` | string | singles or doubles (mixed doubles have matchType doubles and tour Mixed). |
| `drawName` | string | Draw name as ESPN labels it. |
| `tournamentId` | string | ESPN tournament identifier, the same every year (959 is the China Open). |
| `tournamentEventId` | string | ESPN identifier of this edition of the tournament: tournament ID and season. |
| `tournamentName` | string | Tournament name as ESPN shows it today; for older seasons ESPN uses the current sponsor name. |
| `season` | integer | Season year of the tournament edition. |
| `grandSlam` | boolean | True for the Australian Open, Roland Garros, Wimbledon and the US Open. |
| `tournamentStartDate` | string | ISO 8601 start of the tournament edition as ESPN lists it. |
| `tournamentEndDate` | string | ISO 8601 end of the tournament edition as ESPN lists it. |
| `location` | string | City and country of the tournament. |
| `court` | string | Court the match was played or is scheduled on, when ESPN publishes it. |
| `round` | string | Round of the draw, for example Qualifying 1st Round, Round 1, Quarterfinal, Final. |
| `qualifying` | boolean | True for qualifying rounds. |
| `startTime` | string | ISO 8601 start time in UTC. The date range filter compares the UTC date of this time. |
| `startTimeConfirmed` | boolean | False when ESPN only has a placeholder time for a scheduled match. |
| `status` | string | scheduled, in_progress, finished, retired, walkover, postponed, cancelled or suspended. |
| `statusDetail` | string | Status text from ESPN, for example Final, Retired or 3rd Set. |
| `completed` | boolean | True when the match is over (finished, retired or walkover). |
| `player1Name` | string | Player 1 as ordered by ESPN; for doubles both names joined with " / ". Null while the player is not yet known (TBD). |
| `player1Id` | string | ESPN ID of player 1; for doubles the ESPN pair ID (two player IDs joined with a hyphen). Use a singles ID in the players input to get the profile. |
| `player1PlayerIds` | string | ESPN player IDs of player 1, comma-separated for doubles. |
| `player1Country` | string | Country name of player 1 (both partners for doubles, joined with " / " when they differ). |
| `player1CountryCode` | string | Three-letter country code ESPN uses for player 1 (ESPN codes, for example SER for Serbia). |
| `player1Seed` | integer | Seed of player 1 in this draw; null when unseeded. |
| `player2Name` | string | Player 2 as ordered by ESPN; for doubles both names joined with " / ". Null while the player is not yet known (TBD). |
| `player2Id` | string | ESPN ID of player 2; for doubles the ESPN pair ID. |
| `player2PlayerIds` | string | ESPN player IDs of player 2, comma-separated for doubles. |
| `player2Country` | string | Country name of player 2. |
| `player2CountryCode` | string | Three-letter country code ESPN uses for player 2. |
| `player2Seed` | integer | Seed of player 2 in this draw; null when unseeded. |
| `winner` | integer | 1 or 2: which player won. Null while the match is not decided. |
| `winnerName` | string | Name of the winner (pair for doubles). |
| `loserName` | string | Name of the loser (pair for doubles). |
| `setsPlayed` | integer | Number of sets with a score, including an unfinished set of a retired or live match. |
| `player1SetsWon` | integer | Sets won by player 1. An unfinished set counts for nobody. |
| `player2SetsWon` | integer | Sets won by player 2. |
| `score` | string | Score from the winner's point of view (player 1 first while undecided), tiebreak points in brackets, a deciding match tiebreak in square brackets such as [10-4]; "ret." marks a retirement and "w/o" a walkover. In the set fields ESPN stores a match tiebreak as a 1-0 set with the tiebreak points. |
| `scoreConsistent` | boolean | For finished matches: true when the recorded winner won more sets. False flags a match whose published set scores are incomplete. Null for other statuses. |
| `retired` | boolean | True when a player retired during the match. |
| `walkover` | boolean | True when the match was not played (walkover). |
| `tiebreaks` | integer | Number of sets decided by a tiebreak with published tiebreak points. |
| `set1Player1` | integer | Games won by player 1 in set 1; null when the set was not played. |
| `set1Player2` | integer | Games won by player 2 in set 1; null when the set was not played. |
| `set1TiebreakPlayer1` | integer | Tiebreak points of player 1 in set 1; null when set 1 had no tiebreak. |
| `set1TiebreakPlayer2` | integer | Tiebreak points of player 2 in set 1; null when set 1 had no tiebreak. |
| `set2Player1` | integer | Games won by player 1 in set 2; null when the set was not played. |
| `set2Player2` | integer | Games won by player 2 in set 2; null when the set was not played. |
| `set2TiebreakPlayer1` | integer | Tiebreak points of player 1 in set 2; null when set 2 had no tiebreak. |
| `set2TiebreakPlayer2` | integer | Tiebreak points of player 2 in set 2; null when set 2 had no tiebreak. |
| `set3Player1` | integer | Games won by player 1 in set 3; null when the set was not played. |
| `set3Player2` | integer | Games won by player 2 in set 3; null when the set was not played. |
| `set3TiebreakPlayer1` | integer | Tiebreak points of player 1 in set 3; null when set 3 had no tiebreak. |
| `set3TiebreakPlayer2` | integer | Tiebreak points of player 2 in set 3; null when set 3 had no tiebreak. |
| `set4Player1` | integer | Games won by player 1 in set 4; null when the set was not played. |
| `set4Player2` | integer | Games won by player 2 in set 4; null when the set was not played. |
| `set4TiebreakPlayer1` | integer | Tiebreak points of player 1 in set 4; null when set 4 had no tiebreak. |
| `set4TiebreakPlayer2` | integer | Tiebreak points of player 2 in set 4; null when set 4 had no tiebreak. |
| `set5Player1` | integer | Games won by player 1 in set 5; null when the set was not played. |
| `set5Player2` | integer | Games won by player 2 in set 5; null when the set was not played. |
| `set5TiebreakPlayer1` | integer | Tiebreak points of player 1 in set 5; null when set 5 had no tiebreak. |
| `set5TiebreakPlayer2` | integer | Tiebreak points of player 2 in set 5; null when set 5 had no tiebreak. |
| `resultNote` | string | ESPN one-line summary of the result with seeds and country codes. |
| `rank` | integer | Ranking rows: current singles rank. Player rows: current rank when the player is in the ESPN top 150 list, otherwise null. |
| `previousRank` | integer | Rank in the previous ranking release. |
| `rankChange` | integer | Places gained since the previous release (negative = places lost). |
| `rankingPoints` | integer | Ranking points. |
| `rankingDate` | string | ISO 8601 date of the ranking release. |
| `playerId` | string | ESPN player ID (ranking and player rows). |
| `playerName` | string | Player name (ranking and player rows). |
| `firstName` | string | First name. |
| `lastName` | string | Last name. |
| `country` | string | Citizenship country (ranking and player rows). |
| `countryCode` | string | Three-letter citizenship code as ESPN publishes it. |
| `age` | integer | Age in years as ESPN publishes it. |
| `birthPlace` | string | Place of birth. |
| `dateOfBirth` | string | Date of birth, YYYY-MM-DD. |
| `heightCm` | integer | Height in centimetres, converted from the inches ESPN publishes. |
| `weightKg` | integer | Weight in kilograms, converted from the pounds ESPN publishes. |
| `hand` | string | Playing hand: Right or Left. |
| `debutYear` | integer | Year of the professional debut. |
| `active` | boolean | True when ESPN lists the player as active. |
| `seasonYear` | integer | Season the season statistics refer to. |
| `seasonStatsAvailable` | boolean | False when ESPN has no season statistics for this player (the season fields are then null). |
| `seasonSinglesWon` | integer | Singles matches won this season. |
| `seasonSinglesLost` | integer | Singles matches lost this season. |
| `seasonSinglesTitles` | integer | Singles titles won this season. |
| `seasonDoublesTitles` | integer | Doubles titles won this season. |
| `seasonPrizeMoneyUsd` | integer | Prize money earned this season in US dollars. |
| `sourceUrl` | string | ESPN page of the tournament, ranking list or player. |
| `found` | boolean | True on data rows; false on the single status row written when a run returns nothing. |
| `scrapedAt` | string | ISO 8601 time of the run. |

Sample record:

```json
{
  "recordType": "match",
  "id": "186257",
  "tour": "ATP",
  "matchType": "singles",
  "drawName": "Men's Singles",
  "tournamentId": "959",
  "tournamentEventId": "959-2026",
  "tournamentName": "China Open",
  "season": 2026,
  "grandSlam": false,
  "tournamentStartDate": "2026-09-27T04:00:00.000Z",
  "tournamentEndDate": "2026-10-12T03:59:00.000Z",
  "location": "Beijing, China PR",
  "court": "Court 7",
  "round": "Qualifying 1st Round",
  "qualifying": true,
  "startTime": "2026-09-28T03:00:00.000Z",
  "startTimeConfirmed": true,
  "status": "finished",
  "statusDetail": "Final",
  "completed": true,
  "player1Name": "Yannick Hanfmann",
  "player1Id": "3322",
  "player1PlayerIds": "3322",
  "player1Country": "Germany",
  "player1CountryCode": "GER",
  "player1Seed": null,
  "player2Name": "Tomas Machac",
  "player2Id": "3811",
  "player2PlayerIds": "3811",
  "player2Country": "Czechia",
  "player2CountryCode": "CZE",
  "player2Seed": null,
  "winner": 2,
  "winnerName": "Tomas Machac",
  "loserName": "Yannick Hanfmann",
  "setsPlayed": 3,
  "player1SetsWon": 1,
  "player2SetsWon": 2,
  "score": "7-6(9-7) 6-7(6-8) 6-4",
  "scoreConsistent": true,
  "retired": false,
  "walkover": false,
  "tiebreaks": 2,
  "set1Player1": 6,
  "set1Player2": 7,
  "set1TiebreakPlayer1": 7,
  "set1TiebreakPlayer2": 9,
  "set2Player1": 7,
  "set2Player2": 6,
  "set2TiebreakPlayer1": 8,
  "set2TiebreakPlayer2": 6,
  "set3Player1": 4,
  "set3Player2": 6,
  "set3TiebreakPlayer1": null,
  "set3TiebreakPlayer2": null,
  "set4Player1": null,
  "set4Player2": null,
  "set4TiebreakPlayer1": null,
  "set4TiebreakPlayer2": null,
  "set5Player1": null,
  "set5Player2": null,
  "set5TiebreakPlayer1": null,
  "set5TiebreakPlayer2": null,
  "resultNote": "Tomas Machac (CZE) bt Yannick Hanfmann (GER) 7-6 (9-7) 6-7 (6-8) 6-4",
  "rank": 1,
  "previousRank": 1,
  "rankChange": 0,
  "rankingPoints": 11000,
  "rankingDate": "2026-09-24T07:00:00.000Z",
  "playerId": "3623",
  "playerName": "Jannik Sinner",
  "firstName": "Jannik",
  "lastName": "Sinner",
  "country": "Italy",
  "countryCode": "ITA",
  "age": 25,
  "birthPlace": "San Candido, Italy",
  "dateOfBirth": "2001-08-16",
  "heightCm": 191,
  "weightKg": 77,
  "hand": "Right",
  "debutYear": 2018,
  "active": true,
  "seasonYear": 2026,
  "seasonStatsAvailable": true,
  "seasonSinglesWon": 44,
  "seasonSinglesLost": 3,
  "seasonSinglesTitles": 6,
  "seasonDoublesTitles": 0,
  "seasonPrizeMoneyUsd": 11577761,
  "sourceUrl": "https://www.espn.com/tennis/scoreboard/tournament/_/eventId/959-2026/competitionType/1",
  "found": true,
  "scrapedAt": "2026-09-30T18:00:00.000Z"
}
```

## Call it from code

Runnable examples are in [`examples/`](examples). Replace `YOUR_APIFY_TOKEN` with the token from your Apify account settings.

```bash
curl -X POST "https://api.apify.com/v2/acts/datagrit~tennis-stats-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"dataTypes":["matches"],"lastDays":3,"maxItems":50}'
```

## FAQ

**Which tournaments are covered?**  
The ATP and WTA tour events that ESPN lists, from the Grand Slams down to the tour's smaller events, including qualifying and doubles. Challenger and ITF events are not in these feeds. Match data is available from about 2010; for 2005 ESPN lists the tournaments but no matches, and the run status reports tournaments without match data. Tournament names are the names ESPN uses today, also for older editions.

**Is court surface, match duration or point-by-point data included?**  
No. The source publishes scores, sets, tiebreaks, seeds and statuses, not surface, serve statistics, odds or point-by-point data.

**How long does a run take?**  
ESPN returns only the tournaments that start or end inside the requested dates, so the Actor starts its first request 23 days before Date from; that way a tournament already under way, such as the second week of a Grand Slam, is always included, and matches outside your dates are dropped. It asks for one calendar month per tour in a single request and waits about 0.7 seconds between requests. A daily feed of the last three days is two requests; January 2026 for both tours (1,298 matches, including the Australian Open) is two requests; a full season is about 24.

**How does the only-new mode work?**  
The Actor remembers, in a storage on your account, the rows it actually returned to you, separately for each combination of filters. Dates are not part of that combination, so a scheduled run with Last days keeps one feed. A match returned while scheduled or in progress is returned again once it is finished, and a ranking row again when a new ranking is released. Rows dropped by your filters or cut off by Maximum results are not remembered.

**How often should I schedule it?**  
Every few hours during tournaments for a results feed, daily for a history archive. A run that finds nothing new returns one free status row.

**Which player IDs can I use?**  
The ESPN IDs from the `player1Id`, `player2Id` or `playerId` fields, for example 3623 for Jannik Sinner. Names are looked up among the current top 150 of each tour and among the matches read in the same run.

**What happens when the source changes?**  
If the scoreboard changes shape, or set scores, winners, ranking points or season statistics disappear, the run fails with a message instead of returning rows with empty values.

**Something looks wrong.**  
Open an issue with the input you used; changes at the source are fixed quickly.

## More from datagrit

- [TED Contract Expiry Radar - Recompete Leads](https://github.com/getdatagrit/ted-contract-expiry-radar) - Find EU public contracts approaching expiry from TED award notices: incumbent, buyer, value, end date and renewal options.
- [UK Contract Expiry Radar - Recompete Leads](https://github.com/getdatagrit/uk-contract-expiry-radar) - UK public contracts ending soon with incumbent supplier, buyer, value and contact - recompete leads from Contracts Finder award notices.
- [French Company Finder - Sirene Financials](https://github.com/getdatagrit/french-company-finder) - French company lead lists from Sirene screened by net result and revenue, with net margin, size, matching establishment and optional directors.
- [IRS 990 Nonprofit Officers and Compensation](https://github.com/getdatagrit/irs-990-officer-compensation) - Named officers, directors and key employees with pay, hours and titles from IRS e-filed 990, 990-EZ and 990-PF returns.
- [Poland KRS New Company Registrations Feed](https://github.com/getdatagrit/poland-krs-new-companies) - Newly registered Polish companies, foundations and associations from the official KRS court register: NIP, address, PKD, capital, email, with filters and change detection.

All Actors: [https://getdatagrit.github.io/](https://getdatagrit.github.io/) · [Apify Store](https://apify.com/datagrit)

---

This repository holds documentation and usage examples. Questions, bug reports and feature requests: use the **Issues** tab of the Actor page on [Apify Store](https://apify.com/datagrit/tennis-stats-scraper). Examples are MIT licensed.
