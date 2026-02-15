# Weather API Setup Guide

This guide will help you set up the OpenWeatherMap API integration for the Green Hydrogen InfraVision application.

## 1. Get OpenWeatherMap API Key

1. Visit [OpenWeatherMap](https://openweathermap.org/api)
2. Sign up for a free account
3. Go to your API keys section
4. Copy your API key

## 2. Set Environment Variable

Add your API key to your environment variables:

### For Development (.env file)
```bash
OPENWEATHER_API_KEY=your_api_key_here
```

### For Production
Set the environment variable in your deployment platform:
- **Vercel**: Add in Project Settings > Environment Variables
- **Netlify**: Add in Site Settings > Environment Variables
- **Railway**: Add in Project Settings > Variables

## 3. API Usage Limits

**Free Tier:**
- 1,000 API calls per day
- 60 calls per minute
- Perfect for development and small-scale production

**Paid Plans:**
- Higher limits available if needed
- More features like historical data

## 4. Features Included

The weather integration provides:

### Weather Data
- Current temperature (°C)
- Weather conditions (sunny, cloudy, rainy, etc.)
- Humidity percentage
- Wind speed (m/s)
- Atmospheric pressure (hPa)
- Visibility (km)

### Air Quality Data
- Air Quality Index (AQI) 1-5 scale
- PM2.5 and PM10 particulate matter
- CO, NO₂, O₃, SO₂, NH₃ levels
- Pollution level assessment

## 5. Caching

The implementation includes:
- 1-hour cache for weather data
- 30-minute cache for pollution data
- Automatic cache invalidation
- Fallback to cached data if API fails

## 6. Error Handling

The system handles:
- API rate limits
- Network failures
- Invalid coordinates
- Missing API key

## 7. Testing

To test the integration:

1. Set up your API key
2. Start the development server
3. Click on the map to place a marker
4. Open the Site Assessment popup
5. Click on the "Weather" tab
6. Verify weather and pollution data loads

## 8. Troubleshooting

**Common Issues:**

1. **"OPENWEATHER_API_KEY is not set"**
   - Make sure you've added the environment variable
   - Restart your development server after adding the variable

2. **"Failed to fetch weather data"**
   - Check your API key is valid
   - Verify you haven't exceeded rate limits
   - Check network connectivity

3. **No data showing**
   - Ensure coordinates are valid (lat/lon within valid ranges)
   - Check browser console for error messages

## 9. Cost Optimization

To minimize API usage:
- Data is cached for 1 hour
- Only fetches when weather tab is opened
- Uses efficient API endpoints
- Implements proper error handling

## 10. Security

- API key is stored server-side only
- Never exposed to client-side code
- All API calls go through your backend
- Rate limiting prevents abuse

---

**Need Help?**
- Check the [OpenWeatherMap API Documentation](https://openweathermap.org/api)
- Review the server logs for detailed error messages
- Ensure your API key has the correct permissions
