# 🌤️ Weather API Integration Guide

This document provides comprehensive information about the OpenWeatherMap API integration in the Green Hydrogen InfraVision application.

## 📋 Table of Contents

- [Overview](#overview)
- [API Setup](#api-setup)
- [Data Structure](#data-structure)
- [Implementation Details](#implementation-details)
- [Usage Examples](#usage-examples)
- [Error Handling](#error-handling)
- [Performance Optimization](#performance-optimization)
- [Troubleshooting](#troubleshooting)
- [API Limits & Costs](#api-limits--costs)

---

## 🎯 Overview

The weather API integration provides real-time environmental data for hydrogen plant site assessment, including:

- **Current Weather Conditions**: Temperature, humidity, wind speed, pressure
- **Air Quality Data**: AQI levels, particulate matter, pollutant concentrations
- **Environmental Impact**: Weather data for site planning and sustainability analysis

### Key Benefits

- **Enhanced Site Assessment**: Weather conditions affect hydrogen production efficiency
- **Air Quality Monitoring**: Important for environmental compliance and health
- **Real-time Data**: Up-to-date environmental conditions for accurate planning
- **Global Coverage**: Works worldwide with precise coordinates

---

## 🔧 API Setup

### 1. OpenWeatherMap Account Setup

1. **Sign Up**: Visit [OpenWeatherMap](https://openweathermap.org/api)
2. **Get API Key**: Navigate to API keys section
3. **Choose Plan**: Free tier provides 1000 calls/day (sufficient for development)

### 2. Environment Configuration

Add to your `.env` file:

```bash
OPENWEATHER_API_KEY=your_api_key_here
```

### 3. API Endpoints Used

```typescript
// Current Weather
GET https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={API_KEY}&units=metric

// Air Pollution
GET https://api.openweathermap.org/data/2.5/air_pollution?lat={lat}&lon={lon}&appid={API_KEY}
```

---

## 📊 Data Structure

### Weather Data Response

```typescript
interface WeatherData {
  temperature: number;           // °C
  weatherCondition: string;      // "Clear", "Clouds", "Rain", etc.
  weatherDescription: string;    // "clear sky", "few clouds", etc.
  humidity: number;             // Percentage
  windSpeed: number;            // m/s
  pressure: number;             // hPa
  visibility: number;           // km
  icon: string;                 // Weather icon code
}
```

### Pollution Data Response

```typescript
interface PollutionData {
  aqi: number;                  // 1-5 scale (Good to Hazardous)
  aqiLevel: string;             // "Good", "Fair", "Moderate", "Poor", "Very Poor"
  pm25: number;                 // μg/m³
  pm10: number;                 // μg/m³
  co: number;                   // μg/m³
  no2: number;                  // μg/m³
  o3: number;                   // μg/m³
  so2: number;                  // μg/m³
  nh3: number;                  // μg/m³
}
```

### Complete Response Structure

```typescript
interface WeatherResponse {
  weather: WeatherData;
  pollution: PollutionData;
  timestamp: number;            // Unix timestamp
  location: {
    lat: number;
    lon: number;
    name: string;               // City/location name
  };
}
```

---

## 🛠️ Implementation Details

### Server-Side Service (`server/weather-service.ts`)

```typescript
// Key functions
export async function getWeatherData(lat: number, lon: number): Promise<WeatherResponse>
export function getWeatherIcon(condition: string): string
export function getAQIColor(aqi: number): string
export function getPollutionLevel(pm25: number): string
```

### API Route (`/api/weather`)

```typescript
GET /api/weather?lat={latitude}&lon={longitude}
```

**Response Example:**
```json
{
  "weather": {
    "temperature": 25,
    "weatherCondition": "Clear",
    "weatherDescription": "clear sky",
    "humidity": 65,
    "windSpeed": 3.2,
    "pressure": 1013,
    "visibility": 10,
    "icon": "01d"
  },
  "pollution": {
    "aqi": 2,
    "aqiLevel": "Fair",
    "pm25": 15.3,
    "pm10": 22.1,
    "co": 0.4,
    "no2": 12.5,
    "o3": 45.2,
    "so2": 3.1,
    "nh3": 1.8
  },
  "timestamp": 1703123456789,
  "location": {
    "lat": 28.6139,
    "lon": 77.2090,
    "name": "New Delhi"
  }
}
```

### Frontend Integration

The weather data is integrated into the Site Assessment Panel with three tabs:

1. **Suitability**: Site scoring and metrics
2. **Analysis**: Key factors and recommendations  
3. **Weather**: Real-time environmental data

---

## 💡 Usage Examples

### Basic Weather Fetch

```typescript
// Fetch weather data for coordinates
const weatherData = await fetch('/api/weather?lat=28.6139&lon=77.2090');
const data = await weatherData.json();

console.log(`Temperature: ${data.weather.temperature}°C`);
console.log(`AQI Level: ${data.pollution.aqiLevel}`);
```

### React Component Usage

```typescript
const [weatherData, setWeatherData] = useState(null);

useEffect(() => {
  if (coordinates) {
    fetchWeatherData();
  }
}, [coordinates]);

const fetchWeatherData = async () => {
  const response = await fetch(`/api/weather?lat=${coordinates.lat}&lon=${coordinates.lon}`);
  const data = await response.json();
  setWeatherData(data);
};
```

---

## ⚠️ Error Handling

### Common Error Scenarios

1. **Missing API Key**
   ```json
   {
     "error": "OPENWEATHER_API_KEY is not set in environment variables"
   }
   ```

2. **Invalid Coordinates**
   ```json
   {
     "error": "Invalid latitude or longitude"
   }
   ```

3. **API Rate Limit Exceeded**
   ```json
   {
     "error": "API rate limit exceeded"
   }
   ```

4. **Network Issues**
   ```json
   {
     "error": "Failed to fetch weather data: Network error"
   }
   ```

### Error Handling Implementation

```typescript
try {
  const weatherData = await getWeatherData(lat, lon);
  // Process data
} catch (error) {
  console.error('Weather API Error:', error);
  // Show user-friendly error message
  // Fall back to cached data if available
}
```

---

## 🚀 Performance Optimization

### Caching Strategy

- **Weather Data**: 1-hour cache (weather changes slowly)
- **Pollution Data**: 30-minute cache (more dynamic)
- **In-Memory Cache**: Reduces API calls and improves response time

### Cache Implementation

```typescript
const weatherCache = new Map<string, { data: WeatherResponse; timestamp: number }>();
const CACHE_DURATION = 60 * 60 * 1000; // 1 hour

// Check cache before API call
const cached = weatherCache.get(cacheKey);
if (cached && (Date.now() - cached.timestamp) < CACHE_DURATION) {
  return cached.data;
}
```

### Lazy Loading

- Weather data is only fetched when the Weather tab is opened
- Reduces unnecessary API calls
- Improves initial page load performance

---

## 🔍 Troubleshooting

### Common Issues & Solutions

#### 1. "API Key Not Set" Error

**Problem**: `OPENWEATHER_API_KEY is not set`

**Solution**:
```bash
# Add to .env file
OPENWEATHER_API_KEY=your_actual_api_key

# Restart development server
npm run dev
```

#### 2. No Weather Data Showing

**Problem**: Weather tab shows loading but no data

**Solutions**:
- Check browser console for errors
- Verify API key is valid
- Ensure coordinates are within valid ranges (-90 to 90 for lat, -180 to 180 for lon)
- Check network connectivity

#### 3. Rate Limit Exceeded

**Problem**: "API rate limit exceeded" error

**Solutions**:
- Wait for rate limit reset (usually 1 minute)
- Implement exponential backoff
- Consider upgrading to paid plan for higher limits

#### 4. Invalid Coordinates

**Problem**: "Invalid latitude or longitude" error

**Solution**:
- Ensure coordinates are numbers, not strings
- Check coordinate ranges:
  - Latitude: -90 to 90
  - Longitude: -180 to 180

### Debug Mode

Enable debug logging by adding to your environment:

```bash
DEBUG_WEATHER=true
```

This will log detailed information about API calls and responses.

---

## 💰 API Limits & Costs

### Free Tier Limits

- **Calls per day**: 1,000
- **Calls per minute**: 60
- **Data retention**: 5 days
- **Support**: Community only

### Usage Estimation

For typical usage:
- **Development**: ~50-100 calls/day
- **Small Production**: ~200-500 calls/day
- **Medium Production**: Consider paid plans

### Cost Optimization Tips

1. **Implement Caching**: Reduces API calls by 80-90%
2. **Lazy Loading**: Only fetch when needed
3. **Batch Requests**: Combine multiple locations if possible
4. **Monitor Usage**: Track API calls to avoid overages

### Paid Plans

- **Startup**: $40/month - 100,000 calls/day
- **Professional**: $150/month - 1,000,000 calls/day
- **Enterprise**: Custom pricing

---

## 🔮 Future Enhancements

### Planned Features

1. **Historical Weather Data**: 5-day weather history
2. **Weather Forecasts**: 3-day weather predictions
3. **Weather Alerts**: Severe weather notifications
4. **Climate Data**: Long-term climate patterns
5. **Multiple Providers**: Fallback to other weather APIs

### Integration Opportunities

1. **Site Suitability**: Factor weather into scoring algorithm
2. **Seasonal Analysis**: Consider seasonal weather patterns
3. **Climate Risk Assessment**: Evaluate climate change impacts
4. **Energy Production**: Correlate weather with renewable energy potential

---

## 📚 Additional Resources

- [OpenWeatherMap API Documentation](https://openweathermap.org/api)
- [Weather API Best Practices](https://openweathermap.org/faq)
- [Air Quality Index Guide](https://www.airnow.gov/aqi/aqi-basics/)
- [Weather Data Standards](https://www.wmo.int/pages/prog/www/DPFS/documents/485_Vol_I_en.pdf)

---

## 🤝 Support

For issues related to the weather API integration:

1. Check this documentation first
2. Review server logs for detailed error messages
3. Verify API key and environment setup
4. Test with known coordinates (e.g., New Delhi: 28.6139, 77.2090)

---

*Last updated: January 2024*
