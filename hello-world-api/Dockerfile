# Используем образ с .NET SDK для сборки приложения
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src/hello-world-api

# Копируем файлы проекта и восстанавливаем зависимости 
COPY ["hello-world-api.csproj", "./"]
RUN dotnet restore "./hello-world-api.csproj"

# Копируем остальные файлы и собираем проект
COPY . .
RUN dotnet build "./hello-world-api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish

# Публикуем проект 
RUN dotnet publish "./hello-world-api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
# Копируем результаты публикации
COPY --from=publish /app/publish .

# Expose порт 80 для внешних соединений
EXPOSE 80
# Expose порт 443 для внешних соединений
#EXPOSE 443

# Запускаем приложение
ENTRYPOINT ["dotnet", "hello-world-api.dll"]