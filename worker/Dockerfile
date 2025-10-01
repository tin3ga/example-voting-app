FROM mcr.microsoft.com/dotnet/sdk:9.0 AS builder

WORKDIR /source

COPY *.csproj  .

RUN dotnet restore

COPY . .

RUN dotnet publish -c Release -o /src

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS final

WORKDIR /src

COPY --from=builder /src .

ENTRYPOINT ["dotnet", "Worker.dll"]
