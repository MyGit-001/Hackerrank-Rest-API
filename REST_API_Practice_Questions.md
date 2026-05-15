# REST API Practice Questions (Java)
### Asked in Real Coding Rounds | HackerRank Style
### API Base: `https://jsonmock.hackerrank.com`

---

> ## How to Use This File
> - Each question has a **Problem Statement**, **Code Template** and **Sample Case**
> - You only need to implement the **asked method** and the **fetch() method**
> - All questions follow the same pattern: **Fetch + Parse + Paginate + Logic**

---

---

# Question 1: Most Successful Domestic Club
> **Difficulty:** Intermediate | **Topic:** Pagination + Formula

## Problem Statement

A local sports publication aims to highlight the most successful domestic football clubs beyond
just their titles. Use HTTP GET requests to access a football club database via the URL:

```
https://jsonmock.hackerrank.com/api/football_teams?league={league_name}
```

*(replace {league_name} with the league name)*

The database details various clubs' statistics, and the query result is paginated.
You can access additional pages by appending `&page={num}` to the query string.

### The Query Response Includes:
- **page** — the current page
- **per_page** — maximum results per page
- **total** — total number of records
- **total_pages** — total number of pages
- **data** — an array of JSON objects containing club information

### Each Object in the Data Field Includes:
- **name** — the club's name
- **league** — the league in which the club plays
- **total_silverware_count** — count of silverware won
- **number_of_champions_league_won** — number of Champions League titles won
- **league_top_three_finishes** — count of top-three finishes

### Task
Given a weight multiplier, compute a **"success point"** for each club:

```
success_points = total_silverware_count
               - number_of_champions_league_won
               + (weight × league_top_three_finishes)
```

Return the **name of the club** with the highest success points.

### Sample Case

**Input:**
```
English Premier League (EPL)
0.37
```

**Output:**
```
Manchester United FC
```

**Explanation:**
| Club | Formula | Score |
|------|---------|-------|
| Manchester United FC | 66 - 3 + (0.37 × 27) | 72.99 ✅ |
| Liverpool | 48 - 6 + (0.37 × 27) | 51.99 |

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'mostSuccessfulDomesticClub' function below.
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. STRING league
     *  2. DOUBLE weight
     * API URL: https://jsonmock.hackerrank.com/api/football_teams?league=<league>
     */

    public static String mostSuccessfulDomesticClub(String league, double weight) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
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

---

---

# Question 2: Total Goals Scored by a Team
> **Difficulty:** Intermediate | **Topic:** Pagination + Accumulation

## Problem Statement

Use HTTP GET requests to access football match data via the URL:

```
https://jsonmock.hackerrank.com/api/football_matches?year={year}&team1={team}&page={page}
https://jsonmock.hackerrank.com/api/football_matches?year={year}&team2={team}&page={page}
```

A team can appear as either `team1` (home) or `team2` (away) in a match.
The goals scored are stored in `team1goals` and `team2goals` fields respectively.

### The Query Response Includes:
- **page** — the current page
- **total_pages** — total number of pages
- **data** — an array of match objects

### Each Match Object Includes:
- **team1** — home team name
- **team2** — away team name
- **team1goals** — goals scored by home team (String)
- **team2goals** — goals scored by away team (String)

### Task
Given a team name and a year, return the **total number of goals** scored by that team
across ALL matches (both home and away) in that year.

### Sample Case

**Input:**
```
Barcelona
2011
```

**Output:**
```
45
```

**Explanation:**
- Fetch all matches where `team1=Barcelona` → sum all `team1goals`
- Fetch all matches where `team2=Barcelona` → sum all `team2goals`
- Total = combined sum of both

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'getTotalGoals' function below.
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. STRING team
     *  2. INTEGER year
     * API URL: https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team1=<team>&page=<page>
     *          https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team2=<team>&page=<page>
     */

    public static int getTotalGoals(String team, int year) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String team = bufferedReader.readLine().trim();
        int year = Integer.parseInt(bufferedReader.readLine().trim());

        int result = Result.getTotalGoals(team, year);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

---

---

# Question 3: Number of Drawn Matches
> **Difficulty:** Intermediate | **Topic:** Pagination + Filtering

## Problem Statement

Use HTTP GET requests to access football match data via the URL:

```
https://jsonmock.hackerrank.com/api/football_matches?year={year}&team1goals={goals}&team2goals={goals}&page={page}
```

A match is a **draw** when both teams scored the same number of goals.

### Task
Given a year, return the **total number of matches** that ended in a draw.

> **Hint:** You can safely assume no team ever scored more than **10 goals** in a match.
> Use this to limit your API calls — query each goal value (0 to 10) separately
> instead of fetching all pages.

### Sample Case

**Input:**
```
2011
```

**Output:**
```
516
```

**Explanation:**
- Query matches where `team1goals=0&team2goals=0`, get total
- Query matches where `team1goals=1&team2goals=1`, get total
- ... repeat up to 10
- Sum all totals = 516

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'getNumDraws' function below.
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER year
     * API URL: https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team1goals=<goals>&team2goals=<goals>&page=<page>
     */

    public static int getNumDraws(int year) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int year = Integer.parseInt(bufferedReader.readLine().trim());

        int result = Result.getNumDraws(year);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

---

---

# Question 4: Total Goals Scored by Competition Winner
> **Difficulty:** Hard | **Topic:** Multiple API Calls + Pagination

## Problem Statement

Use HTTP GET requests to:

**Step 1** — Find the winner of a competition:
```
https://jsonmock.hackerrank.com/api/football_competitions?name={name}&year={year}
```

**Step 2** — Find all matches played by that winner in that competition:
```
https://jsonmock.hackerrank.com/api/football_matches?competition={competition}&year={year}&team1={team}&page={page}
https://jsonmock.hackerrank.com/api/football_matches?competition={competition}&year={year}&team2={team}&page={page}
```

### Competition Response Includes:
- **data[0].winner** — name of the winning team

### Match Object Includes:
- **team1**, **team2** — team names
- **team1goals**, **team2goals** — goals scored (String)

### Task
Given a competition name and year:
1. Find the **winner** of that competition
2. Return the **total goals** scored by the winner across all matches in that competition

### Sample Case

**Input:**
```
UEFA Champions League
2011
```

**Output:**
```
27
```

**Explanation:**
- Find winner of UEFA Champions League 2011
- Fetch all matches where winner was team1 → sum team1goals
- Fetch all matches where winner was team2 → sum team2goals
- Return total

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'getTotalGoalsByWinner' function below.
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. STRING competition
     *  2. INTEGER year
     * API URL 1: https://jsonmock.hackerrank.com/api/football_competitions?name=<name>&year=<year>
     * API URL 2: https://jsonmock.hackerrank.com/api/football_matches?competition=<competition>&year=<year>&team1=<team>&page=<page>
     * API URL 3: https://jsonmock.hackerrank.com/api/football_matches?competition=<competition>&year=<year>&team2=<team>&page=<page>
     */

    public static int getTotalGoalsByWinner(String competition, int year) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String competition = bufferedReader.readLine().trim();
        int year = Integer.parseInt(bufferedReader.readLine().trim());

        int result = Result.getTotalGoalsByWinner(competition, year);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

---

---

# Question 5: Capital City of a Country
> **Difficulty:** Easy | **Topic:** Single API Call + Null Handling

## Problem Statement

Use HTTP GET requests to access country data via the URL:

```
https://jsonmock.hackerrank.com/api/countries?name={country}
```

### The Query Response Includes:
- **data** — array containing country record (empty if country not found)

### Each Country Object Includes:
- **name** — country name
- **capital** — capital city name
- other fields not relevant

### Task
Given a country name, return its **capital city**.
If the country is **not found**, return `"-1"`.

> **Note:** No pagination needed here — data array has exactly 1 element if found.

### Sample Case

**Input:**
```
Italy
```

**Output:**
```
Rome
```

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'getCapitalCity' function below.
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. STRING country
     * API URL: https://jsonmock.hackerrank.com/api/countries?name=<country>
     */

    public static String getCapitalCity(String country) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String country = bufferedReader.readLine().trim();

        String result = Result.getCapitalCity(country);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

---

---

# Question 6: Articles with Most Comments
> **Difficulty:** Intermediate | **Topic:** Pagination + Null Handling + Sorting

## Problem Statement

Use HTTP GET requests to access article data via the URL:

```
https://jsonmock.hackerrank.com/api/articles?page={page}
```

### The Query Response Includes:
- **total_pages** — total number of pages
- **data** — array of article objects

### Each Article Object Includes:
- **title** — article title (may be null or empty)
- **story_title** — alternate title (may be null or empty)
- **num_comments** — number of comments (may be null)
- other fields not relevant

### Task
Given a number `n`, return the **titles** of the top `n` articles by number of comments.

Rules:
- If `title` is null/empty, use `story_title` instead
- If both are null/empty, **ignore** that article
- If `num_comments` is null, treat it as **0**
- Sort by `num_comments` descending
- Return list of `n` titles

### Sample Case

**Input:**
```
5
```

**Output:**
```
["Title1", "Title2", "Title3", "Title4", "Title5"]
```

---

## Code Template

```java
import java.io.*;
import java.util.*;
import java.net.*;
import java.net.http.*;
import org.json.simple.*;
import org.json.simple.parser.*;

class Result {

    /*
     * Complete the 'topArticles' function below.
     * The function is expected to return a LIST OF STRINGS.
     * The function accepts following parameters:
     *  1. INTEGER n
     * API URL: https://jsonmock.hackerrank.com/api/articles?page=<page>
     */

    public static List<String> topArticles(int n) {
        // Write your code here
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> result = Result.topArticles(n);

        bufferedWriter.write(result.toString());
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

---

---

# Quick Reference

## Pattern for Every Question

```
STEP 1 → encode URL parameters (replace spaces with %20)
STEP 2 → fetch page 1 → get total_pages
STEP 3 → loop from page 1 to total_pages
STEP 4 → for each item in data → apply logic
STEP 5 → return result
```

## fetch() Method (Same for All Questions)

```java
public static String fetch(String url) throws Exception {
    HttpClient client = HttpClient.newHttpClient();
    HttpRequest req = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .build();
    return client.send(req, HttpResponse.BodyHandlers.ofString()).body();
}
```

## Casting Rules (org.json.simple)

| JSON Type | Java Cast |
|-----------|-----------|
| Number (integer) | `(Long)` |
| Number (decimal) | `(Double)` |
| String | `(String)` |
| Array | `(JSONArray)` |
| Object | `(JSONObject)` |

## Difficulty Order to Follow

```
Q5 (Easy)   → Q1 (Intermediate) → Q2 (Intermediate)
           → Q3 (Intermediate) → Q6 (Intermediate) → Q4 (Hard)
```
