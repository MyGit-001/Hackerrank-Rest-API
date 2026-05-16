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
    try{
        String url1 = "https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team1=<team>";
        String url2 = "https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team2=<team>";
      
        String team1Url = url1.replace("<year>",String.valueOf(year)).replace("<team>",team);
        String team2Url = url2.replace("<year>",String.valueOf(year)).replace("<team>",team);
      
        JSONParser parser = new JSONParser();

      
        Long totalGoals = 0;
      
        JSONObject obj1 = (JSONObject) parser.parse(fetch(team1Url+"&page=1"));
        Long team1pages = (Long) obj1.get("total_pages");
    
      
        for(int page = 1; page<=team1pages ; page++){
          JSONObject team1Obj = (JSONObject) parser.parse(fetch(team1Url+"&page="+page));
          JSONArray team1arr = (JSONArray) team1Obj.get("data");
        
          for(JSONObject item : team1arr){
            totalGoals += Long.parseLong((String)item.get("team1goals"));
          }
        }
      
        JSONObject obj2 = (JSONObject) parser.parse(fetch(team2Url+"&page=1"));
        Long team2pages = (Long)obj2.get("total_pages");
      
        for(int page = 1; page<=team2pages ; page++){
          JSONObject team2Obj = (JSONObject) parser.parse(fetch(team2Url+"&page="+page));
          JSONArray team2arr = (JSONArray) team2Obj.get("data");
        
          for(JSONObject item : team2arr){
            totalGoals += Long.parseLong((String)item.get("team2goals"));
          }
        }

      return totalGoals.intValue(); 
      
      }catch(Exception e){
        throw new RuntimeException(e);
      }
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
