FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /source

COPY *.csproj .
RUN dotnet restore

COPY . .
RUN dotnet publish -c Release -o /app

FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app

RUN useradd -m -u 1000 appuser

COPY --from=build /app .
RUN chown -R appuser:appuser /app
USER appuser

CMD ["dotnet", "Worker.dll"]