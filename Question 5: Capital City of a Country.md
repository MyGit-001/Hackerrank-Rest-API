```Java
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
      try{
        
        String encodedCountry = country.replace(" ","%20");
        String baseUrl = "https://jsonmock.hackerrank.com/api/countries?name="+encodedCountry;
        JSONParser parser = new JSONParser();
        
        JSONObject obj1 = (JSONObject) parser.parse(fetch(baseUrl));
        JSONArray arr = (JSONArray) obj1.get("data");
        
        String capital = "-1";
        
        for(JSONObject item : arr){
          String name = (String) item.get("name");
          if(name.equals(country)){
            capital = (String) item.get("capital");
          }
        }
        
        return capital;
      
      }catch(Exception e){
        throw new RuntimeException(e); 
      }
    }

    public static String fetch(String url) throws Exception {
      
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest req = HttpRequest.newBuilder().uri(URI.create(url)).build();
        String res = client.send(req , HttpResponse.BodyHandlers.ofString()).body();
        return res;
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
