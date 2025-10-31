# مرحله پایه برای اجرای اپلیکیشن
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

# مرحله ساخت پروژه
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# کپی فایل پروژه و restore
COPY ["IDP.Service.Api.csproj", "./"]
RUN dotnet restore "IDP.Service.Api.csproj"

# کپی کل پروژه و build
COPY . .
RUN dotnet build "IDP.Service.Api.csproj" -c Release -o /app/build

# مرحله publish
FROM build AS publish
RUN dotnet publish "IDP.Service.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

# مرحله نهایی برای اجرای اپلیکیشن
FROM base AS final
WORKDIR /app

# کپی فایل‌های منتشرشده
COPY --from=publish /app/publish .

# کپی فایل‌های تنظیمات از مسیر اصلی پروژه
COPY appsettings.json .
COPY appsettings.Development.json .

ENTRYPOINT ["dotnet", "IDP.Service.Api.dll"]
