# ---- Stage 1 : build ----
FROM mcr.microsoft.com/dotnet/sdk:7.0-bookworm-slim AS build
WORKDIR /source

COPY *.csproj .
RUN dotnet restore

COPY . .
RUN dotnet publish -c Release -o /app

# ---- Stage 2 : runtime ----
FROM mcr.microsoft.com/dotnet/runtime:7.0-bookworm-slim
WORKDIR /app

# Force la mise à jour des paquets système pour les derniers correctifs de sécurité
RUN apt-get update && \
    apt-get upgrade -y && \
    rm -rf /var/lib/apt/lists/*

RUN useradd -m -u 1000 appuser

COPY --from=build /app .
RUN chown -R appuser:appuser /app
USER appuser

CMD ["dotnet", "Worker.dll"]