# Set the base image
FROM mcr.microsoft.com/dotnet/aspnet:6.0-nanoserver-ltsc2022 AS base
WORKDIR /app
EXPOSE 80

# Add the SDK so you can run the dotnet restore and build commands
FROM mcr.microsoft.com/dotnet/sdk:6.0-nanoserver-ltsc2022 AS build
WORKDIR /src
COPY *.csproj ./
RUN dotnet restore "taskapp.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "taskapp.csproj" -c Release -o /app/build

# Create the publish files
FROM build AS publish
RUN dotnet publish "taskapp.csproj" -c Release -o /app/publish

# Copy the publish files into the container
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "taskapp.dll"]
