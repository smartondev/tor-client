FROM alpine:latest

RUN apk add --no-cache curl

ENTRYPOINT ["curl", "https://ident.me"]
#ENTRYPOINT ["curl", "--socks5-hostname", "tor-client:9050", "https://ident.me"]