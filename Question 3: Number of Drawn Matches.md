```Java
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
    try{
        String baseUrl = "https://jsonmock.hackerrank.com/api/football_matches?year=<year>&team1goals=<goals>&team2goals=<goals>&page=1";
        String encodedUrl = baseUrl.replace("<year>",year);
        
        JSONParser parser = new JSONParser();
        Long totalDraws = 0;
        
        for(int i=0 ; i<10 ; i++){
            String newUrl = encodedUrl.replace("<goals>",i);
            JSONObject obj = (JSONObject) parser.parse(fetch(newUrl));
            totalDraws += (Long) obj.get("total");
        }
        return totalDraws;
    }catch(Exception e){
        throw new RuntimeException(e);
    }
        
    }

    public static String fetch(String url) throws Exception {
        HttpClient client = HttpClient.newClient();
        HttpRequest req = HttpRequest.newBuilder().uri(URI.create(url)).build();
        String res = client.send(client , HttpResponse.bodyHandler.ofStrings()).body();
        
        return res;
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
