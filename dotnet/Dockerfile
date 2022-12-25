#######################
##      BUILD        ##
#######################
FROM mcr.microsoft.com/dotnet/sdk:6.0 as build
WORKDIR /app

COPY *.csproj ./

RUN dotnet restore

EXPOSE 5000

COPY . ./
# RUN dotnet publish -c Release -o out

#######################
##      IMAGE        ##
#######################

FROM mcr.microsoft.com/dotnet/sdk:6.0 as production
WORKDIR /app
COPY --from=build /app/out .
EXPOSE 5000
ENTRYPOINT ["dotnet", "ms-interview.dll", "--launch-profile production"]
