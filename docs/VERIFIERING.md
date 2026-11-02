# Verifiering

Kontrollerat 2026-11-02 i ett lokalt labb. Det här är inte en verifierad eller
certifierad företagsinstallation i OpenShift.

| Kontroll | Resultat |
| --- | --- |
| Go-tester | 39 testfunktioner godkända med race detector. |
| Statisk analys | `go vet ./...` godkänd. |
| Container | Go-image byggd. |
| Manifestgenerering | Riktigt byggskript och den byggda imagen provade i två namespaces: intern callback och imageadress, ingen Route som standard. Uttryckligt HTTPS-undantag kontrollerat. |
| Befintlig PVC | Tester verifierar att rätt claim monteras på /data utan att PV, PVC eller lagrings-Secret genereras. Ogiltiga namn och motstridiga lagringsinställningar nekas. Riktigt hjälpskript och ny image verifierade med två claim-namn; Deployment accepterad av labbets API med server-side dry-run. |
| Nätverkslagring | Tester godkända för UNC/SMB och NFS, statisk PV/PVC-bindning, Retain, separata SMB-Secrets och felaktiga inställningar. Riktigt hjälpskript och ny image verifierade för SMB, UNC, NFS och standardlagring. Nätverks-PV/PVC accepterade med server-side dry-run i lokala labbet. SQLite-journal och bevarad state testade lokalt. |
| Registryflöde | Simulerade `oc`- och containerkommandon verifierar routeuppslagning, namespace-kontroll, token på stdin och uttrycklig intern push-adress. |
| Riktig integration | 11 tidigare kontroller mot lokala Mattermost och ACS: manuell skanning, CVE-sökning, formulär och provkörning av sparat schema. |

Den riktiga manuella skanningen omfattade en image. Schemakörningen omfattade två
images och 189 råa fyndrader, med noll misslyckade images. Åtgärd och JSON-bilagor,
nya skanntider och bevarat schema kontrollerades. Befintliga kritiska CVE:er gav
ingen upprepad kanalping. Automatisk verifiering av botidentiteten provades också.

En tidigare Go-image verifierades med read-only rotfilsystem och samma beständiga
SQLite-lagring under två olika icke-root-UID:n. Tidigare Kubernetes-resurser
accepterades av labbets API med server-side dry-run; den kontrollen omfattade
inte OpenShift Route-resurser.

Push till företagets registry och installation i företagets OpenShift-kluster
har inte utförts. Nätverk, TLS, credentials, behörigheter och lagring behöver
verifieras i den avsedda miljön. Ingen riktig SMB- eller NFS-montering har provats;
lagringsserverns låsning, synkronisering och skrivrättigheter återstår att verifiera där.
