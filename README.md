# WeatherService
import org.json.JSONObject;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class WeatherService {

    private static final String API_KEY = "YOUR_API_KEY"; 
    private static final String API_URL = "http://api.weatherstack.com/current?access_key=";

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter city or location:");
        String location = scanner.nextLine();
        scanner.close();

        try {
            String response = fetchWeatherData(location);
            displayWeatherInfo(response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String fetchWeatherData(String location) throws Exception {
        String urlString = API_URL + API_KEY + "&query=" + location;
        URL url = new URL(urlString);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");

        BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        StringBuilder result = new StringBuilder();
        String line;

        while ((line = reader.readLine()) != null) {
            result.append(line);
        }
        reader.close();
        return result.toString();
    }

    private static void displayWeatherInfo(String response) {
        JSONObject jsonObject = new JSONObject(response);
        JSONObject current = jsonObject.getJSONObject("current");
        String temperature = current.getString("temperature");
        String weatherDescription = current.getJSONArray("weather_descriptions").getString(0);
        String location = jsonObject.getJSONObject("location").getString("name");

        System.out.println("Weather information for " + location + ":");
        System.out.println("Temperature: " + temperature + "°C");
        System.out.println("Description: " + weatherDescription);
    }
}
