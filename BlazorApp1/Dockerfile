FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

RUN apt-get update && \
    apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs

COPY ["BlazorApp1/BlazorApp1.csproj", "BlazorApp1/"]
COPY ["BlazorApp1/package.json", "BlazorApp1/"]

WORKDIR /src/BlazorApp1
RUN dotnet restore
RUN npm install

COPY BlazorApp1/. ./

RUN npm run css:build

RUN dotnet build "BlazorApp1.csproj" -c Release -o /app/build

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/build .
ENTRYPOINT ["dotnet", "BlazorApp1.dll"] 