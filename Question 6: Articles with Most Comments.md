```Java
// Online Java Compiler
// Use this editor to write, compile and run your Java code online

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
     
class Post {
    String title;
    long numOfComments;

    public Post(String title, long numOfComments) {
        this.title = title;
        this.numOfComments = numOfComments;
    }

    public String getTitle() {
        return title;
    }

    public long getNumOfComments() {
        return numOfComments;
    }
}

    public static List<String> topArticles(int n) {
        // Write your code here
        String url = "https://jsonmock.hackerrank.com/api/articles?page=1";
        JSONParser parser = new JSONParser();
        JSONObject obj = (JSONObject) parser.parse(fetch(url));
        long total_pages = obj.get("total_pages");
        for(long page =1; page<=total_pages; page++){
            String newUrl = "https://jsonmock.hackerrank.com/api/articles?page="+page;
            JSONObject obj2 = (JSONObject) parser.parse(fetch(newUrl));
            JSONArray arr = (JSONArray) obj2.get("data");
            for(JSONObject item : arr){
                List<Post> post = new ArrayList<>();
                if(item.get("title")){
                    map.add(new post(item.get("story_title"),item.get("num_comments")));
                else
                    map.add(new post(item.get("title"),item.get("num_comments")));
                }
            }
            
        }
    }

    public static String fetch(String url) throws Exception {
        // Write your code here
        HttpClient client = HttpClent.newHttpClient();
        HttpRequest req = HttpRequest
        .newBuilders()
        .uri(URI.create(url))
        .build();
        
        String res = client.send(req , HttpResponse.BodyHandlers.ofStrings()).body();
        return res;
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
