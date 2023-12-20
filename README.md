  require("httr")
  
  library(httr)
  
  # Create some empty vectors to hold data temporarily
  # City name column
  city <- c()
  # Weather column, rainy or cloudy, etc
  weather <- c()
  # Sky visibility column
  visibility <- c()
  # Current temperature column
  temp <- c()
  # Max temperature column
  temp_min <- c()
  # Min temperature column
  temp_max <- c()
  # Pressure column
  pressure <- c()
  # Humidity column
  humidity <- c()
  # Wind speed column
  wind_speed <- c()
  # Wind direction column
  wind_deg <- c()
  # Forecast timestamp
  forecast_datetime <- c()
  # Season column
  # Note that for season, you can hard code a season value from levels Spring, Summer, Autumn, and Winter based on your current month.
  season <- c()
  
  city <- character()
  
  weather <- character()
  
  visibility <- numeric()
  
  temp <- numeric()
  
  temp_min <- numeric()
  
  temp_max <- numeric()
  
  pressure <- numeric()
  
  humidity <- numeric()
  
  wind_speed <- numeric()
  
  wind_deg <- numeric()
  
  forecast_datetime <- character()
  
  season <- character()
  
  # Get forecast data for a given city list
  get_weather_forecaset_by_cities <- function(city_names){
    weather_data_frame <- data.frame()
    for (city_name in city_names){
      # Forecast API URL
      forecast_url <- 'https://api.openweathermap.org/data/2.5/forecast'
      # Create query parameters
      forecast_query <- list(q = city_name, appid = "f583418ea482464ac03ca3b78a2a8ed1", units="metric")
      # Make HTTP GET call for the given city
      response <- GET(forecast_url, query=forecast_query)
      # Note that the 5-day forecast JSON result is a list of lists. You can print the reponse to check the results
      json_list <- content(response, as="parsed")
      results <- json_list$list
      
      # Loop the json result
      for(result in results) {
        city <- c(city, city_name)
        weather <- c(weather, result$weather[[1]]$main)
        # Get Visibility
        visibility <- c(visibility, result$visibility)
        # Get current temperature
        temp <- c(temp,result$main$temp)
        # Get min temperature
        temp_min <- c(temp_min,result$main$temp_min)
        # Get max temperature
        temp_max <- c(temp_max, result$main$temp_max)
        # Get pressure
        pressure <- c(pressure, result$main$pressure)
        # Get humidity
        humidity <- c(humidity,result$main$humidity)
        # Get wind speed
        wind_speed <- c(wind_speed,result$wind$speed)
        # Get wind direction
        wind_deg <- c(wind_deg, result$wind$deg)
        forecast_datetime <- c(forecast_datetime, result$dt_txt)
        season <- c(season, "winter")
        
      }
      
      # Add the R Lists into a data frame
      weather_data_frame <- data.frame(city=city,
                                       weather=weather, 
                                       visibility=visibility, 
                                       temp=temp, 
                                       temp_min=temp_min, 
                                       temp_max=temp_max, 
                                       pressure=pressure, 
                                       humidity=humidity, 
                                       wind_speed=wind_speed, 
                                       wind_deg=wind_deg,
                                       forecast_datetime=forecast_datetime,
                                       season=season)
    }
    
    # Return a data frame
    return(weather_data_frame)
    
  }
  cities <- c("Seoul", "Washington, D.C.", "Paris", "Suzhou")
  cities_weather_df <- get_weather_forecaset_by_cities(cities)
  print(cities_weather_df)

  
