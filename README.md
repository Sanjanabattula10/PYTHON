#PYTHON
import requests
import matplotlib.pyplot as plt
import seaborn as sns

# OpenWeatherMap API Configuration
API_KEY = "your_api_key_here"  # Replace with your OpenWeatherMap API key
CITY = "New York"  # Change to any city you prefer
URL = f"http://api.openweathermap.org/data/2.5/forecast?q={CITY}&appid={API_KEY}&units=metric"

def fetch_weather_data():
    """Fetch weather forecast data from OpenWeatherMap API."""
    response = requests.get(URL)
    if response.status_code == 200:
        return response.json()
    else:
        print("Error fetching data:", response.status_code)
        return None

def process_weather_data(data):
    """Extract relevant weather data for visualization."""
    dates, temps, humidity = [], [], []
    for entry in data['list']:
        dates.append(entry['dt_txt'])  # Timestamp
        temps.append(entry['main']['temp'])  # Temperature in Celsius
        humidity.append(entry['main']['humidity'])  # Humidity in %

    return dates, temps, humidity

def visualize_weather(dates, temps, humidity):
    """Create weather data visualizations using Matplotlib and Seaborn."""
    plt.figure(figsize=(12, 6))

    # Temperature Plot
    sns.lineplot(x=dates, y=temps, marker="o", label="Temperature (°C)", color="r")
    
    # Humidity Plot
    sns.lineplot(x=dates, y=humidity, marker="s", label="Humidity (%)", color="b")

    plt.xticks(rotation=45, ha='right')  # Rotate date labels
    plt.xlabel("Date & Time")
    plt.ylabel("Value")
    plt.title(f"Weather Forecast for {CITY}")
    plt.legend()
    plt.grid()
    plt.show()

if __name__ == "__main__":
    weather_data = fetch_weather_data()
    if weather_data:
        dates, temps, humidity = process_weather_data(weather_data)
        visualize_weather(dates, temps, humidity)
