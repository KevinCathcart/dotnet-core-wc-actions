
# Set the base image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Add the SDK so you can run the following restore and build commands
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1809 AS build
WORKDIR /src
COPY ["taskapp/taskapp.csproj", "taskapp/"]
RUN dotnet restore "taskapp/taskapp.csproj"
COPY . .
WORKDIR "/src/taskapp"
RUN dotnet build "taskapp.csproj" -c Release -o /app/build

# Create the publish files
FROM build AS publish
RUN dotnet publish "taskapp.csproj" -c Release -o /app/publish

# Copy the publish ready files into the container
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "taskapp.dll"]