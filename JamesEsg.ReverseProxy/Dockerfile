FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8090

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["JamesEsg.ReverseProxy.csproj", "JamesEsg.ReverseProxy/"]
RUN dotnet restore "JamesEsg.ReverseProxy/JamesEsg.ReverseProxy.csproj"
COPY . .
WORKDIR "/src/JamesEsg.ReverseProxy"
RUN dotnet build "JamesEsg.ReverseProxy.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "JamesEsg.ReverseProxy.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JamesEsg.ReverseProxy.dll"]
