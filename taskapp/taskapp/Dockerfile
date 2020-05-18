FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1809 AS build
WORKDIR /src
#COPY ["dotnetcorewc/dotnetcorewc.csproj", "dotnetcorewc/"]
#RUN dotnet restore "dotnetcorewc/dotnetcorewc.csproj"
COPY *.csproj ./
RUN dotnet restore "taskapp.csproj"

COPY . .
#WORKDIR "/src/dotnetcorewc"
WORKDIR "/src/"

RUN dotnet build "taskapp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "taskapp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "taskapp.dll"]
