# Utveckla Go-versionen

```sh
go mod download
go test -race ./...
go vet ./...
go build -o bin/cvebridge ./cmd/cvebridge
docker build -t cvebridge:go .
docker run --rm cvebridge:go version
```

Go 1.27.3 används i CI och Dockerfile. Tester körs med lokala HTTP-testservrar och
en tillfällig SQLite-databas; de behöver inga företagsuppgifter eller ett kluster.

`cmd/cvebridge` startar boten. `internal/bridge` innehåller API-klienter,
behörighetskontroller, Mattermost-dialoger, rapporter, schemaläggning, state och
konfigurationsgenerator. `.env.example` är en generisk inställningsmall.
Den riktiga `.env` och genererade filer ska hållas privata.

För en verklig anslutning behövs Mattermost, ACS/StackRox och Kubernetes RBAC.
Källkodstesterna ersätter inte ett installationstest i ett kluster.
Den detaljerade OpenShift-installationsguiden finns i projektets privata leverans.
Se README för användning och referensbilder från labbet.
