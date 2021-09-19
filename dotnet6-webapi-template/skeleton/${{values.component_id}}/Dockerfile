FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["${{values.component_id}}/${{values.component_id}}.csproj", "${{values.component_id}}/"]
RUN dotnet restore "${{values.component_id}}/${{values.component_id}}.csproj"
COPY . .
WORKDIR "/src/${{values.component_id}}"
RUN dotnet build "${{values.component_id}}.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "${{values.component_id}}.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "${{values.component_id}}.dll"]
