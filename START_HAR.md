# Bygg och utforska CVEBridge

Använd Go 1.27.3, samma version som i CI och Dockerfile.

```bash
go mod download
go test -race ./...
go vet ./...
go build -o bin/cvebridge ./cmd/cvebridge
docker build -t cvebridge:dev .
docker run --rm cvebridge:dev version
```

Testerna använder lokala HTTP-servrar och tillfälliga SQLite-databaser. De behöver
varken företagsuppgifter eller tillgång till ett kluster.

## Hitta i projektet

| Sökväg | Innehåll |
| --- | --- |
| `cmd/cvebridge` | Start av boten och konfigurationsgenerator. |
| `internal/bridge` | ACS- och Mattermost-klienter, åtkomstkontroll, rapporter, formulär och jobb. |
| `internal/workflow` | Tester för container- och registryflödet. |
| `.env.example` | Kort, generell inställningsmall. |
| `.env.advanced.example` | Valfria undantag från standardinställningarna. |
| `cvebridge.sh` | Hjälpskript för imagebygge, push och manifestgenerering. |

En ansluten installation behöver Mattermost, ACS Central och godkända
Kubernetes-behörigheter. Sensorn skickar klusterdata till Central, och boten
hämtar dessa via Centrals API. Central kan stå i ett annat kluster; då måste
adressen vara nåbar från botens kluster.

De interna serviceadresserna i mallen är exempel. Kontrollera service, namespace
och port för er installation. Håll `.env`, CA-filer och genererade Secret-manifest privata.

Se [arkitektur](docs/ARKITEKTUR.md), [åtkomstmodell](docs/ACCESS.md) och
[verifiering](docs/VERIFIERING.md). Runtime är Go; `legacy/python/` innehåller den
tidigare implementationen och labbverktyg som historisk referens.
