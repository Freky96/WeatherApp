# WeatherApp

WeatherApp is an Android application that provides real-time weather information for any city. The app fetches data from the OpenWeatherMap API and displays the current temperature, humidity, wind speed, and weather description in a user-friendly interface.

## Prerequisites

Before you begin, ensure you have met the following requirements:
- **Java Development Kit (JDK)**: Java 8 or higher.
- **Android Studio**: Version 4.0 or higher.
- **Android Device or Emulator**: Running Android 7.0 (Nougat) or higher.

## Installation

To install WeatherApp, follow these steps:1

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/your-username/WeatherApp.git
    ```

2. **Open the Project**:
    - Launch Android Studio.
    - Open the cloned project directory.

3. **Sync the Project with Gradle**:
    - Android Studio will prompt you to sync the project with Gradle files. Click on "Sync Now".

4. **Run the App**:
    - Connect your Android device or start an emulator.
    - Click on the "Run" button in Android Studio to build and run the app on your device.

## Usage

1. **Enter City Name**:
    - In the input field, type the name of the city you want to get weather information for.

2. **Fetch Weather Data**:
    - Press the "Fetch Weather" button or hit Enter to retrieve and display the weather data for the specified city.

## Key Features

- **Real-Time Weather Data**: Get up-to-date weather information for any city.
- **User-Friendly Interface**: Simple and intuitive design for easy navigation.
- **API Integration**: Utilizes the OpenWeatherMap API to fetch weather data.

## Important Code

### MainActivity.java

The `MainActivity.java` file contains the main logic for fetching and displaying weather data. Here are some key parts of the code:

- **Fetching Weather Data**:
    ```java
    private void FetchWeatherData(String cityName) {
        String url = "https://api.openweathermap.org/data/2.5/weather?q=" + cityName + "&appid=" + API_KEY + "&units=metric";
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        executorService.execute(() -> {
            OkHttpClient client = new OkHttpClient();
            Request request = new Request.Builder().url(url).build();
            try {
                Response response = client.newCall(request).execute();
                String result = response.body().string();
                runOnUiThread(() -> updateUI(result));
            } catch (IOException e) {
                e.printStackTrace();
            }
        });
    }
    ```

- **Updating the UI**:
    ```java
    private void updateUI(String result) {
        if (result != null) {
            try {
                JSONObject jsonObject = new JSONObject(result);
                JSONObject main = jsonObject.getJSONObject("main");
                double temperature = main.getDouble("temp");
                double humidity = main.getDouble("humidity");
                double windSpeed = jsonObject.getJSONObject("wind").getDouble("speed");

                String description = jsonObject.getJSONArray("weather").getJSONObject(0).getString("description");
                String iconCode = jsonObject.getJSONArray("weather").getJSONObject(0).getString("icon");
                String resourceName = "ic_" + iconCode;
                int resId = getResources().getIdentifier(resourceName, "drawable", getPackageName());
                weatherIcon.setImageResource(resId);

                cityNameText.setText(jsonObject.getString("name"));
                temperatureText.setText(String.format("%.0f°", temperature));
                humidityText.setText(String.format("%.0f%%", humidity));
                windText.setText(String.format("%.0f km/h", windSpeed));
                descriptionText.setText(description);
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    }
    ```

### AndroidManifest.xml

The `AndroidManifest.xml` file is crucial for defining the app's components and permissions. Here are some key points:

- **Permissions**:
    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    ```

- **Application Configuration**:
    ```xml
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="WeatherApp"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.WeatherApp"
        tools:targetApi="31"/>
    ```

- **Main Activity**:
    ```xml
    <activity
        android:name=".MainActivity"
        android:exported="true"
        android:label="@string/app_name">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    ```

### activity_main.xml

The `activity_main.xml` file defines the layout of the main activity. Here are some key elements:

- **City Name TextView**:
    ```xml
    <TextView
        android:id="@+id/cityNameText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="10dp"
        android:text="City"
        android:textColor="@color/white"
        android:textSize="36sp"
        android:textStyle="bold"
        android:fontFamily="@font/bruno_ace" />
    ```

- **Temperature TextView**:
    ```xml
    <TextView
        android:id="@+id/temperatureText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="10dp"
        android:layout_marginBottom="10dp"
        android:text="25°"
        android:textColor="#FFBF00"
        android:textSize="60sp"
        android:textStyle="bold"
        android:layout_below="@id/cityNameText"
        android:fontFamily="@font/bruno_ace" />
    ```

- **Weather Icon ImageView**:
    ```xml
    <ImageView
        android:id="@+id/weatherIcon"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:layout_below="@+id/detailsLayout"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:elevation="12dp"
        android:src="@drawable/sun_icon"
        android:contentDescription="Weather Icon" />
    ```

## API Implementation

WeatherApp uses the OpenWeatherMap API to fetch weather data. You need to sign up on the OpenWeatherMap website to get an API key and replace the placeholder in the code with your actual API key.

## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
