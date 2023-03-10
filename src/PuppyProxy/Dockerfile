#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

EXPOSE 80
EXPOSE 443
EXPOSE 8000

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["PuppyProxy/PuppyProxy.csproj", "HttpProxy/"]
RUN dotnet restore "PuppyProxy/PuppyProxy.csproj"
COPY . .
WORKDIR "/src/PuppyProxy"
RUN dotnet build "PuppyProxy.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PuppyProxy.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PuppyProxy.dll"]