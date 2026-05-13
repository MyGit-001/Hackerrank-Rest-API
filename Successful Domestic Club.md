# REST API: Most Successful Domestic Club

## Problem Statement

A local sports publication aims to highlight the most successful domestic football clubs beyond just their titles. Use HTTP GET requests to access a football club database via the URL: \
`https://jsonmock.hackerrank.com/api/football_teams?league={league_name}`

*(replace {league_name} with the league name)*

The database details various clubs' statistics, and the query result is paginated. You can access additional pages by appending `&page={num}` to the query string *(replace {num} with page number)*.

---

## The Query Response Includes

- **page** — the current page
- **per_page** — maximum results per page
- **total** — total number of records
- **total_pages** — total number of pages
- **data** — an array of JSON objects containing club information

---

## Each Object in the Data Field Includes

- **name** — the club's name
- **league** — the league in which the club plays
- **total_silverware_count** — count of silverware won
- **number_of_champions_league_won** — number of Champions League titles won
- **league_top_three_finishes** — count of top-three finishes
- other details not relevant to this question

---

## Task

Given a weight multiplier, use this API to fetch data and compute a **"success point"** for each club based on this formula:\
`success_points = total_silverware_count - number_of_champions_league_won + (weight × league_top_three_finishes)`

Return the **name of the club** with the highest success points.

---

## Example

For Arsenal FC with data:

```json
{
  "name": "Arsenal FC",
  "total_silverware_count": 30,
  "number_of_champions_league_won": 0,
  "league_top_three_finishes": 18
}
```

Given a weight of **0.50**, the success points are: \
30 - 0 + (0.50 × 18) = 39 success points

---

## Function Description

Complete the function `mostSuccessfulDomesticClub` with the following parameters:

| Parameter | Type   | Description                                      |
|-----------|--------|--------------------------------------------------|
| `league`  | STRING | The league name                                  |
| `weight`  | DOUBLE | The weight multiplier for top-three league finishes |

**Returns:** `STRING` — the name of the most successful domestic club

---

## Sample Case

### Input
> English Premier League (EPL) \
> 0.37

### Output
> Manchester United FC
### Explanation

Within the English Premier League (EPL):

| Club | Formula | Score |
|------|---------|-------|
| Manchester United FC | 66 - 3 + (0.37 × 27) | 72.99 ✅ |
| Liverpool | 48 - 6 + (0.37 × 27) | 51.99 |

Manchester United FC has the highest score of **72.99** and is returned as the answer.

> Note: Data is found on page 2 of the query result.

---

## Code Editor
## Key Concepts Tested

This question demands us to do:
### Core Skills

```
FETCH      → Hit the API using HttpClient
    +
PARSE      → Extract fields from JSON response
    +
PAGINATE   → Loop through all pages to get all clubs
```

---
### On Top of That
```
LOGIC      → Apply the formula and track highest score
```

Which is the **easiest part** of the whole question.

---

### Marks Breakdown

> For every HackerRank REST API question:

```
90% of the marks → fetch + parse + paginate (boilerplate)
10% of the marks → actual logic/formula
```

Once the boilerplate becomes muscle memory, these questions become very easy.
The logic itself is always straightforward. 🎯

```Java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;
import java.net.*;
import org.json.simple.*;
import org.json.simple.parser.*;
import java.net.http.*;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import com.google.gson.*;

class Result {

    /*
     * Complete the 'mostSuccessfulDomesticClub' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. STRING league
     *  2. DOUBLE weight
     * API URL: https://jsonmock.hackerrank.com/api/football_teams?league=<league>
     */

    public static String mostSuccessfulDomesticClub(String league, double weight) {
      try {
        String encodedLeague = league.replace(" ", "%20");
        String baseUrl = "https://jsonmock.hackerrank.com/api/football_teams?league=" + encodedLeague;
        String url = baseUrl + "&page=1";

        JSONParser parser = new JSONParser();
        JSONObject obj1 = (JSONObject) parser.parse(fetch(url));
        Long totalPages = (Long) obj1.get("total_pages");

        String bestClub = "";
        Double bestScore = Double.NEGATIVE_INFINITY;

        for (int page = 1; page <= totalPages; page++) {
            String pageUrl = baseUrl + "&page=" + page;
            JSONObject obj = (JSONObject) parser.parse(fetch(pageUrl));
            JSONArray arr = (JSONArray) obj.get("data");

            for (JSONObject item : arr) {
                Long totalSilverware = (Long) item.get("total_silverware_count");
                Long championsLeagueWon = (Long) item.get("number_of_champions_league_won");
                Long topThreeFinishes = (Long) item.get("league_top_three_finishes");

                Double highestScore = totalSilverware - championsLeagueWon + (weight * topThreeFinishes);

                if (bestScore < highestScore) {
                    bestScore = highestScore;
                    bestClub = (String) item.get("name");
                }
            }
        }
        return bestClub;

    } catch (Exception e) {
        throw new RuntimeException(e);
    }
    }

    public static String fetch(String url) throws Exception {
      HttpClient client = HttpClient.newHttpClient();
      HttpRequest req = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .build();
      return client.send(req, HttpResponse.BodyHandlers.ofString()).body();
}

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String league = bufferedReader.readLine();

        double weight = Double.parseDouble(bufferedReader.readLine().trim());

        String result = Result.mostSuccessfulDomesticClub(league, weight);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
