FROM mcr.microsoft.com/dotnet/sdk:10.0
WORKDIR /src
COPY src/TaskApi/ .
RUN dotnet publish -c Release -o /app
WORKDIR /app
ENTRYPOINT ["dotnet", "TaskApi.dll"]