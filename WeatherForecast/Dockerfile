# Use the .NET 9 SDK (Preview) for building the app
FROM mcr.microsoft.com/dotnet/sdk:9.0-preview AS build
WORKDIR /src

# Copy project file and restore dependencies
COPY ["WeatherForecast.csproj", "./"]
RUN dotnet restore "WeatherForecast.csproj"

# Copy the rest of the source code
COPY . .

# Build the project
RUN dotnet build "WeatherForecast.csproj" -c Release -o /app/build

# Publish the app to a directory
RUN dotnet publish "WeatherForecast.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Use the ASP.NET Core runtime image (ARM64 compatible)
FROM mcr.microsoft.com/dotnet/aspnet:9.0-preview AS runtime
WORKDIR /app

# Copy published app from build container
COPY --from=build /app/publish .

# Expose ports
EXPOSE 80
EXPOSE 443

# Start the application
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]
