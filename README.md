# Felles kartdataserver

ADB har et behov for å en felles kartdataserver for å håndtere geodataflyt innad i ADB og utad. Applikasjoner som Artskart, Artsobservasjoner og Økologisk grunnkart trenger eksterne kartdata som bør lastes ned og røktes i fellesskap. Vi er orginaldataforvalter på en del datasett som vi ønsker at det skal være god tilgang på i GeoNorge og i egne systemer. Dette kommer i tillegg til selve Artskart og Artobservasjoner som er egne kartapplikasjoner med orginaldata.

Vi kan med fordel se på om data som i dag er tilgjengelig på IPT fra excelark kan ligge i kartdatabase istedet. Med en god database i bunn kan vi serve OGC-api for både Maps og Features. Vi vil kunne ha et hjem for fremtidig lagrin av NIN-data. Vi vil ha et godt utgangspunkt for røkting av data i QGIS på fagansvarlignivå. Og vi kan via API legge grunnlaget for Plugins i QGIS både for oss selv og eksterne.

Hvilken løsning man velger er enda ikke klart, men PostGreSQL med PostGIS-geometri vs MSSQL er vel de reelle alternativene. En hybridløsning mellom Open Source og Azure er vel fort uansett fasiten. [Her er side generert i CoPilot for å belyse mulighetene](https://artsdatabankenno.sharepoint.com/:fl:/g/contentstorage/x8FNO-xtskuCRX2_fMTHLe9nnyTZSFZPrlyradMCMSI/IQC0YcUqZpyFTboDriLYuvV7AZDnrznJvunA2q4X4nnWn1g?e=I0ccKg&nav=cz0lMkZjb250ZW50c3RvcmFnZSUyRng4Rk5PLXh0c2t1Q1JYMl9mTVRITGU5bm55VFpTRlpQcmx5cmFkTUNNU0kmZD1iJTIxNHN4OXo0Y1ZqVS10S0o4VHJiSS1PSllGZ3VHbUlOdExxblRsMVZGWUxvT1EzNW15cDM4cFRhbjkzUGh6T2tVNyZmPTAxVkZXT01ZTlVNSENTVVpVNFFWRzNVQTVPRUxNTFY1TDMmYz0lMkYmcD0lNDBmbHVpZHglMkZsb29wLXBhZ2UtY29udGFpbmVy "Gå til Copliot" )

Vi bør bygge opp våre egne script, mulig med utgangspunkt i massivnedlasteren til Kartverket for å sikre at eksterne data, som vi trenger å ha lokalt, til en hver tid er oppdatert.

Dette kan fort bli flere databaser, og ikke alt trenger å realiseres med en gang, men radig å få på plass et rammeverk og pipelines. Vi har gjort tester på PostGreSQL med PostGIS i docker. Så  dette vil være det første som legges til i repoet. Husk .gitingore når vi holder på med store datamengder.
