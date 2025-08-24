FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# --- NETFREE CERT INSTALL ---
ADD https://netfree.link/dl/unix-ca.sh /home/netfree-unix-ca.sh
RUN cat /home/netfree-unix-ca.sh | sh
ENV NODE_EXTRA_CA_CERTS=/etc/ca-bundle.crt
ENV REQUESTS_CA_BUNDLE=/etc/ca-bundle.crt
ENV SSL_CERT_FILE=/etc/ca-bundle.crt
# --- END NETFREE CERT INSTALL ---

WORKDIR /DataEntities
COPY "DataEntities/DataEntities.csproj" .
RUN dotnet restore
COPY "DataEntities" .
RUN dotnet publish -c release -o /app

WORKDIR /src
COPY Products/Products.csproj .
RUN dotnet restore
COPY Products .
RUN dotnet publish -c release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
EXPOSE 80
EXPOSE 443
COPY --from=build /app .
ENTRYPOINT ["dotnet", "Products.dll"]
