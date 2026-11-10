FROM golang:alpine AS build
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 go build -ldflags='-s -w' -o /out/ . ./galenectl

FROM alpine
RUN adduser -D -h /galene galene
WORKDIR /galene
COPY --from=build /out/ /usr/local/bin/
COPY --from=build /src/static static
USER galene
RUN mkdir data groups recordings
EXPOSE 8443 1194 1194/udp
ENTRYPOINT ["galene"]
