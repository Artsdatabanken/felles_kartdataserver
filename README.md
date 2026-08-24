# Felles kartdataserver

[Oversikt over behov i Powerpoint](https://artsdatabankenno.sharepoint.com/:p:/s/Artskart/IQBll5gjleQdS5AL_Kv_fmpDAXBNtvL9emExMFDWsNKOGaE?e=4m5GB0 "Powerpoint lagret i kanalen til Artskart på Teams")

Artsdatabanken har et behov for å en felles kartdataserver for å håndtere geodataflyt innad i Artsdatabanken og utad. Applikasjoner som Artskart, Artsobservasjoner og Økologisk grunnkart trenger eksterne kartdata som bør lastes ned og røktes i fellesskap. Vi er orginaldataforvalter på en del datasett som vi ønsker at det skal være god tilgang på i GeoNorge og i egne systemer. Dette kommer i tillegg til selve Artskart og Artobservasjoner som er egne kartapplikasjoner med orginaldata.

Vi kan med fordel se på om data som i dag er tilgjengelig på IPT fra excelark kan ligge i kartdatabase istedet. Med en god database i bunn kan vi serve [OGC-api](https://ogcapi.ogc.org/ "Gå til OGC-api" ) for både Maps og Features. Vi vil kunne ha et hjem for fremtidig lagrin av NIN-data. Vi vil ha et godt utgangspunkt for røkting av data i QGIS på fagansvarlignivå. Og vi kan via API legge grunnlaget for Plugins i QGIS både for oss selv og eksterne.

Hvilken løsning man velger er enda ikke klart, men PostGreSQL med PostGIS-geometri vs MSSQL er vel de reelle alternativene. En hybridløsning mellom Open Source og Azure er vel fort uansett fasiten. [Her er side generert i CoPilot for å belyse mulighetene.](https://artsdatabankenno.sharepoint.com/:fl:/g/contentstorage/x8FNO-xtskuCRX2_fMTHLe9nnyTZSFZPrlyradMCMSI/IQC0YcUqZpyFTboDriLYuvV7AZDnrznJvunA2q4X4nnWn1g?e=I0ccKg&nav=cz0lMkZjb250ZW50c3RvcmFnZSUyRng4Rk5PLXh0c2t1Q1JYMl9mTVRITGU5bm55VFpTRlpQcmx5cmFkTUNNU0kmZD1iJTIxNHN4OXo0Y1ZqVS10S0o4VHJiSS1PSllGZ3VHbUlOdExxblRsMVZGWUxvT1EzNW15cDM4cFRhbjkzUGh6T2tVNyZmPTAxVkZXT01ZTlVNSENTVVpVNFFWRzNVQTVPRUxNTFY1TDMmYz0lMkYmcD0lNDBmbHVpZHglMkZsb29wLXBhZ2UtY29udGFpbmVy "Gå til Copliot" ) De fleste av våre applikasjoner er basert på Open Source og det vil være naturlig med en sterk forankring mot det.

Vi bør bygge opp våre egne script, mulig med utgangspunkt i [massivnedlasteren til Kartverket](https://github.com/kartverket/Geonorge.NedlastingKlient/ "Klient for massiv nedlasting fra Kartverket på GitHUB" ) for å sikre at eksterne data, som vi trenger å ha lokalt, til en hver tid er oppdatert.

Her er noen eksempler på data som bør inn.
- Verneområder
- Administrative områder kommuner/fylker
- Vår egne administrative inndelinger for liste arbeid og Artskartområde
- Varig verna vassdrag
- Vannområder
- Marin grense
- Nasjonale laksefjorder
- Anadrom strekning
- MIS
- Naturtyper (både gamle og nye)
- Bioklimatiske soner (ADB-orginaldatavert)
- IBA important bird area (Birdlife)
-  Artskart (en kopi her kan avlaste hovedprosjektet for de som ønsker å speile hele Artskart eller lage plugins i QGIS)

(Noen er allerede i bruk i interne system, andre er planlagt brukt i interne system. I tillegg kan vi sørge for å ha datasett som er etterspurt fra våre egen QGIS-brukere)
Dette kan fort bli flere databaser, og ikke alt trenger å realiseres med en gang, men radig å få på plass et rammeverk og pipelines. Vi har gjort tester på PostGreSQL med PostGIS i docker. Så  dette vil være det første som legges til i repoet. Husk .gitingore når vi holder på med store datamengder.

Vi må også tenke litt autentisering, kan det være aktuelt med windows login for direkte tilgang til databasen internt på nettverket eller har vi andre løsninger ev. kun databasen sitt autentiseringersystem?

I utgangspunktet er dette tenkt som en vektordatabase, men via EcoMapprosjektet vil vi også få større rasterdatasett som vi skal forvalte. Det vil være naturlig å begynne å tenke på rasterdata i dette prosjektet, da mye er felles med vektordata, men muligens litt annen teknisk løsning.

- PostGIS Raster:Lagrer raster direkte i databasen.Best for mindre rasetere eller når du trenger transaksjonssikkerhet sammen med vektordata.
- Cloud Optimized GeoTIFF (COG) + Objektlager (S3/MinIO):Moderne standard for store rastermengder. Dataene lagres som optimaliserte filer i skyen fremfor i en tradisjonell database, og leses ved behov via verktøy som GDAL.
- DuckDB (med Spatial-utvidelse):Rask, filbasert analytisk database som egner seg godt for raske spørringer på romlige data og raster/vektorkombinasjoner.
