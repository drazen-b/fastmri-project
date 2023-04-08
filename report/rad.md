```
Zadatak:
Istražiti i opisati način dobivanja dinamičkih MRI slika te problematiku
rekonstrukcije MRI slika. Istražiti i opisati postupak i važnost rekonstrukcije
MRI medicinskih slika u kliničkoj praksi te dati kratak pregled prethodnih
istraživanja. Opisati fastMRI biblioteku za rekonstrukciju medicinskih slika.
Odabrati metodu implementiranu unutar fastMRI biblioteke za rekonstrukciju
MRI medicinskih slika, opisati je te prilagoditi za zadatak rekonstrukcije MRI
slika mozga. Prikazati i komentirati dobivene rezultate.
```

# Rekonstrukcija slika magnetske rezonance putem fastMRI biblioteke

## Uvod

Medicinska dijagnostika je područje medicine koje obuhvaća načine identifikacije zdravstvenih problema. Zbog svojih sposobnosti stvaranja detaljnih slika mekog tkiva magnetska rezonanca je nezamjenjiva metoda dobivanja medicinskih slika. Potpuno je bezbolna metoda koja za stvaranje slike koristi jako magnetsko polje i radiovalove, a time nema štetnog ionizirajućeg zračenja i nuspojava. Postupak dobivanja slike traje od 15 do 90 minuta, a tokom postupka osoba mora mirno ležati unutar stroja. Najmanje kretnje uzrokuju šum i artefakte. S obzirom na trajanje postupka, iskustvo može biti neudobno i klaustrofobično. Dugo trajanje postupka ima i posljedice na povećanje cijene snimanja jer je potrebno više vremena po osobi. No, sposobnost detaljnog slikanja mekog tkiva nije zamjenjiva metodama poput CT snimanja i rendgenskog snimanja. Time, smanjenje trajanja slikanja postupkom magnetske rezonancije je poželjno.

### Magnetska rezonanca

Magnetska rezonancija se temelji na fizikalnoj pojavi nuklearne magnetske rezonancije. U toj pojavi jezgre određenih atoma pokazuju sposobnost upijanja i odašiljanja elektromagnetske energije prilikom stavljanja u magnetsko polje. Ponašaju se kao mali štapičasti magneti, te ukoliko se nalaze u magnetskom polju oni će se poravnati s njim. Zatim, stroj za magnetsku rezonanciju odašilje u njih elektromagnetski puls radio frekvencije, te ih time izbija iz poravnanja s magnetskim poljem. Ovdje djelovanje nuklearne magnetske rezonancije dolazi u utjecaj. Jezgre atoma će se rotirati i time odašiljati rf signal. No, nakon određenog vremena njihova će rotacija stati, te će se ponovno poravnati s magnetskim poljem. Vrijeme da se jezgra atoma prestane rotirati, vrijeme da se ponovno poravna s magnetskim poljem i količina energije koja je ispuštena ovise o kemijskom sastavu molekula. S obzirom na to je moguće razaznati razliku između različitih vrsta tkiva.

Postupak magnetske rezonance uzorkuje područje koje snimamo u Fourierovom prostoru, koji je također poznat kao k-prostor. Uporabom inverzne Diskretne Fourierove Transformacije (DFT) nad k-prostorom slike *y* dobivamo rekonstruiranu sliku *m*. **(Ubaciti formulu (1))**

Kvaliteta slike se određuje trima karakteristikama. Omjerom snage signala i snage šuma (SNR). oblikom piksela (point spread function) i artifaktima. Ukoliko je snaga šuma visoka, dolazi do diskoloracije konačne slike. Oblik piksela bi trebao biti samo jedan piksel, no ukoliko funkcija širenja sadrži više frekvencija doći će do zamućenja piksela. Artifakti nastaju kada su signali povezani uz piksele kojima ne pripadaju. To stvara preklapanja slike i nastanak nepostojećih oblika na slikama. Ukoliko je kvaliteta slike niska, može doći do skrivanja bitnih obilježja na slici što može dovesti do pogrešne dijagnoze, ili u najgorem slučaju slike koju nije moguće iskoristiti.

### Tehnike rekonstrukcije

Uvođenjem paralelnog slikanja u 1990ima skraćena je duljina trajanja postupka. Svaka dodana zavojnica slika zasebnu sliku u k-prostoru. Rekonstruirana slika y_i, zavojnice i od nc dobiva se iz **(Ubaciti formulu (2))** gdje je S_i osjetljivost zavojnice, a g_i njena Fourierova transformacija. Uvođenjem koprimirajuće osjetljivosti (compressed sensing (CS)) doprinijelo je napretku u smanjenju vremena snimanja magnetske rezonance. Tehnike CS ubrzavaju slikanje uzorkovanjem manje količine podataka nego klasičnim metodama. No zbog kršenja Nyquist-Shannonovog teorema uzorkovanja, ono dovodi do nastanka artefakta. Artefakti se prilikom rekonstrukcije moraju ukloniti, a to se postiže uporabom prethodnog znanja tokom rekonstrukcije. ESPIRiT je pristup koji povezuje paralelno slikanje i CS. U slikanju s više zavojnica služi za procjenu osjetljivosti zavojnica i za izvođenje rekonstrukcije slika u kombinaciji s CS korištenjem regularizacije ukupne varijacije.

Najnoviji pokušaji rekonstrukcije se izvode metodama dubokog učenja modela. Kretanje u tom smjeru je potaknuto objavljivanjem javno dostupnog fastMRI skupa podataka.

## fastMRI dataset

fastMRI je zajednički istraživački projekt između *Facebook AI Research* i *NYU Langone Health*. Njihov javno objavljeni skup podataka sastoji se od od četri tipa snimaka mozga i koljena postukom magnetske rezonance:
- Neprocesiranih snimaka k-prostora za višestruke zavojnice
- Emuliranih podataka k-prostora za pojedinačne zavojnice
- (Ground-truth) rekonstruirane slike iz potpuno uzorkovanih snimaka višestrukih zavojnica
- DICOM slika

Podatci su predviđeni kako bi omogućili dva različita zadatka:
- Rekonstrukciju slika uzorkovanih jednom zavojnicom
- Rekonstrukciju slika uzorkovanih s više zavojnica

Za svaki izazov objavljeni su službeni podskupovi za trening, validaciju, test i izazova. Svi podatci su anonimizirani. Sveukupno je 1594 neprocesiranih snimaka k-prostora koljena dobivenih s više zavojnica, 6970 slika k-prostora mozga dobivenih s više zavojnica. Podatci za izazov rekonstrukcije slika uzorkovanih jednom zavojnicom dobiveni su emulacijom podataka iz k-prostora slika dobivenih metodom s više zavojnica. 10000 DICOM slika mozga i koljena dobivenih MRI skenom.


**Spomenuti ground truth**
**(Ubaciti sliku-tablicu s količinom podataka)**

U ovom radu se fokusiramo na dataset sa slikama mozga metodom više zavojnica. Prethodno navedenih 6970 MRI slika su dobivene korištenjem 11 magneta na 5 kliničkih lokacija. Kod uporabe ovog skupa podataka vrlo brzo dolazimo do problema. Naime, set za treniranje je veličine ~1.2 TB, set za validaciju je ~500GB, a set za testiranje je ~100GB. DICOM podatci se ne koriste jer ne pružaju neprocesirani k-prostor.

**((UBACITI NEKE SLIKICE OVO ONO))**

## fastMRI

Uz dataset, fastMRI također pruža open-source set alata za rad s podatcima iz dataseta i implementaciju modela dubokog učenja za rekonstrukciju slika. Oni sadrže Python biblioteke za učitavanje podataka, predprocesiranje i evaluaciju podataka, uz prije trenirane modele za rekonstrukciju. Prije trenirani modeli koji se nalaze u biblioteci su CS, Dino, Unet i Varnet modeli, uz njih sadrži i njihov kod. 


### Modeli


#### Unet

Kod klasifikacijskih zadataka u konvolucijskim mrežama na izlazu se očekuje jedna klasa slike. Ukoliko želimo postići lokalizaciju potrebno je svakom pikselu slike dodijeliti klasu.

**((UBACITI SLIKU LOKALIZACIJE))**

U-Net model koji se nalazi u fastMRI biblioteci je namijenjen za rekonstrukciju slika uzorkovanih s jednom zavojnicom. Za uporabu nad slikama uzorkovanih s više zavojnica potrebno izvršiti `zero-fill` metoda nad slikom svake zavojnice. `Zero-fill` metodom na mjesto svih neizmjerenih podataka u k-prostoru stavljamo nule, a zatim primjenjujemo Inverznu Fourierovu Transformaciju. Rezultat se centrirano reže kako bi se uklonilo prelijevanje očitanja i faze. Nakon primjene `Zero-fill` metode, slike zavojnica se povezuju koristeći algoritam korijena zbroja kvadrata. Model je treniran na smanjenje gubitaka srednje apsolutne greške. 

Model sadrži dva duboka konvolucijska mrežna puta. Prvi dio je dio sažimanja, a zatim ide dio širenja. Put sažimanja se sastoji od blokova s dvije 3x3 konvolucije, svaku konvoluciju prati normalizacija po instancama i ReLU funkcija. Blokovi se izmjenjuju down-sampling operacijama koje se sastoje od slojeva maksimalnog uzorkovanja (max-pooling) s korakom 2. Time prepolovljuju svaku prostornu dimenziju. Na putu povećavanja razlučivosti se nalaze blokovi slične arhitekture kao na putu sažimanja, a blokovi se izmjenjuju operacijama povećavanja rezolucije prethodnog bloka. Put povećavanja razlučivosti je povezan s putem sažimanja `skip` vezama. Put sažimanja omogućava hvatanje konteksta i otkriva što je na slici, dok put širenja otkriva gdje se to nalazi na slici. Kako bi lokalizacija bolje funkcionirala, značajke visoke rezolucije iz puta sažimanja se povezuju i kombiniraju s outputom puta širenja. S tom informacijom se dobiva precizniji izlaz iz bloka. Na kraju puta širenja nalaze se 1x1 konvolucije kako bi se smanjio broj kanala na jedan bez promjene prostorne reozolucije. Kako bi se predvidjeli rubni pikseli ulazne slike, primjenjuje se tehnika zrcaljenja za popunjavanje nedostajućih podataka.

<!-- Istrenirani model koji fastMRI biblioteka pruža je treniran na 973 slike iz trening seta uporabom RMSProp algoritma. Prvih 40 epoha stopa učenja je 0.001, a zatim se množi s 0.1 i trenira se još 10 epoha.  -->

**NAPISATI ONO S IZLAZA SLIKE**


#### VarNet

#### Dino


