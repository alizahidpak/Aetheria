FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /app

# copy .csproj file and install dependencies
COPY *.csproj ./
RUN dotnet restore

# copy source code
COPY . ./
WORKDIR /app/

# publish application
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app
COPY --from=build /app/out ./
ENTRYPOINT ["dotnet", "Backend.dll"]