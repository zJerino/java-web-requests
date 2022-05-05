import com.google.gson.Gson;
import com.google.gson.JsonObject;
import org.jetbrains.annotations.Nullable;

import java.io.IOException;
import java.io.OutputStream;
import java.net.Authenticator;
import java.net.URL;
import java.net.HttpURLConnection;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.Map;

public class Request {
    public static String get(
            String url,
            @Nullable Map<String, String> headers,
            @Nullable String Authorization
    )  {
        return make(url, null, headers, Authorization, "GET");
    }
    public static String post(
            String uri,
            JsonObject body,
            @Nullable Map<String, String> headers,
            @Nullable String Authorization
    ) {
        return make(uri, body, headers, Authorization, "POST");
    }

    public static String put(
                             String uri,
                             JsonObject body,
                             @Nullable Map<String, String> headers,
                             @Nullable String Authorization
    ) {
        return make(uri, body, headers, Authorization, "PUT");
    }


    private static String make(
            String uri,
            @Nullable  JsonObject body,
            @Nullable Map<String, String> headers,
            @Nullable String Authorization,
            @Nullable String Method
    ) {
        try {

            URL url = new URL(uri);
            HttpURLConnection http = (HttpURLConnection) url.openConnection();

            if (Method != null && !Method.equals("GET")) {
                http.setRequestMethod(Method);
            }

            http.setDoOutput(true);
            if (headers != null) {
                headers.forEach(http::setRequestProperty);
            }

            if (Authorization != null) {
                http.setRequestProperty("Authorization", "Bearer " + Authorization);
            }


            if (body != null) {
                http.setRequestProperty("Content-Type", "application/json");

                OutputStream stream = http.getOutputStream();
                String data = new Gson().toJson(body);
                byte[] out = data.getBytes(StandardCharsets.UTF_8);
                stream.write(out);
            }

            BufferedReader br = null;

            if (100 <= http.getResponseCode() && http.getResponseCode() <= 399) {
                br = new BufferedReader(new InputStreamReader(http.getInputStream()));
            } else {
                br = new BufferedReader(new InputStreamReader(http.getErrorStream()));
            }

            StringBuilder response = new StringBuilder();
            String currentLine;
            while ((currentLine = br.readLine()) != null) {
                response.append(currentLine);
            }

            http.disconnect();
            br.close();

            return response.toString();
        } catch (IOException e) {
            System.out.println("Something happened when sending a web request: " + e.getMessage());
            System.out.println(e);
        }
        return null;
    }
}
