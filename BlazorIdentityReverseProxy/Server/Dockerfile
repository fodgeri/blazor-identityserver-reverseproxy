#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["BlazorIdentityReverseProxy/Server/BlazorIdentityReverseProxy.Server.csproj", "BlazorIdentityReverseProxy/Server/"]
COPY ["BlazorIdentityReverseProxy/Shared/BlazorIdentityReverseProxy.Shared.csproj", "BlazorIdentityReverseProxy/Shared/"]
COPY ["BlazorIdentityReverseProxy/Client/BlazorIdentityReverseProxy.Client.csproj", "BlazorIdentityReverseProxy/Client/"]
RUN dotnet restore "BlazorIdentityReverseProxy/Server/BlazorIdentityReverseProxy.Server.csproj"
COPY . .
WORKDIR "/src/BlazorIdentityReverseProxy/Server"
RUN dotnet build "BlazorIdentityReverseProxy.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorIdentityReverseProxy.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorIdentityReverseProxy.Server.dll"]