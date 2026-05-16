```Java
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
   
    }

    public static String fetch(String url) throws Exception {
      HttpClient client = HttpClient.newHttpClient();
      HttpRequest req = HttpRequest.newBuilder().uri(URI.create(url)).build();
      String res = client.send( req , HttpResponse.BodyHandlers.ofString()).body();
      return res;
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
