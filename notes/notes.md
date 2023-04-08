# Rekonstrukcija MRI slika mozga pomoću fastMRI biblioteke

Zadatak:

- Istražiti i opisati način dobivanja dinamičkih MRI slika te problematiku
rekonstrukcije MRI slika. 
- Istražiti i opisati postupak i važnost rekonstrukcije MRI medicinskih 
slika u kliničkoj praksi.
- Dati kratak pregled prethodnih istraživanja. 
- Opisati fastMRI biblioteku za rekonstrukciju medicinskih slika.
- Odabrati metodu implementiranu unutar fastMRI biblioteke za rekonstrukciju
MRI medicinskih slika, opisati je te prilagoditi za zadatak rekonstrukcije MRI
slika mozga.
- Prikazati i komentirati dobivene rezultate.

---

## Izvori:

- https://www.nibib.nih.gov/science-education/science-topics/magnetic-resonance-imaging-mri
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3097694/

---

## Istražiti i opisati način dobivanja dinamičkih MRI slika te problematiku rekonstrukcije MRI slika. 

### Uvod

Magnetska rezonancija je često korištena tehnika dobivanja medicinskih slika. Koristi magnetsko polje i radio valove kako bi stvorila detaljne slike mekog tkiva. Dinamička rezonanca (dMRI) snima više slika u nekom vremenskom periodu i te slike se kombiniraju kako bi stvorile filmski prikaz koji prikazuje pokret dijela tijela koji se snima. Ovaj oblik se često koristi za vizualizaciju pokretnih struktura. S druge strane statički MRI (sMRI) snima smo jednu sliku nepomičnog dijela tijela. Služi za pružanje detaljnijih slika Strojevi koji provode slikanje magnetskom rezonancijom su zapravo veliki magneti. Ne emitiraju ionizirajuće zračenje koje se pronalazi kod CT i rendgenskih slika što ga čini sigurnijim. Ali, emitiraju jako magnetsko zračenje te se u okolini stroja ne smiju nalaziti feromagnetici. `UMETNI SLIKU`. Postupak dobivanja slike može trajati od 15 do 90 minuta. Prilikom postupka, osoba koje se nalazi u stroju mora biti potpuno mirna. Najmanje kretnje i pokreti stvaraju šum i mutnu sliku. S obzirom na dugo trajanje postupka, iskustvo može biti neudobno i klaustrofobično. Također, dugo trajanje postupka znači da nije moguće brzo slikati velik broj ljudi, čime se i cijena postupka povečava. No, zbog sposobnosti prikaza mekog tkiva, magnetska rezonancija nije zamjenjiva drugim metodama poput CT snimanja i rendgenskih snimanja. TIme, smanjenje trajanja magnetske rezonancije je poželjno. Snimanje je moguće obaviti brže, no posljedica toga su mutne slike pune šuma.

Bitna razlika između magnetske rezonancije i ostalih metoda prikupljanja medicinskih slika je ta što je prilikom izvođenja snimanja moguće upravljati prikupljanjem podataka, te time utjecati na konačnu dobivenu sliku. Moguće je prilagoditi prostornu rezolucije, polje pregleda (FOV), kontrast slike, brzinu prikupljanja, artifakte i još dodatne parametre koji pridonose konačnoj slici.

### Način rada magnetske rezonancije

Magnetska rezonancija se temelji na fizikalnoj pojavi nuklearne magnetske rezonancije. U toj pojavi jezgre određenih atoma pokazuju sposobnost upijanja i odašiljanja elektromagnetske energije prilikom stavljanja u magnetsko polje. Ponašaju se kao mali štapičasti magneti, te ukoliko se nalaze u magnetskom polju oni će se poravnati s njim. Zatim, stroj za magnetsku rezonanciju odašilje u njih elektromagnetski puls radio frekvencije, te ih time izbija iz poravnanja s magnetskim poljem. Ovdje djelovanje nuklearne magnetske rezonancije dolazi u utjecaj. Jezgre atoma će se rotirati i time odašiljati rf signal. No, nakon određenog vremena njihova će rotacija stati, te će se ponovno poravnati s magnetskim poljem. Vrijeme da se jezgra atoma prestane rotirati, vrijeme da se ponovno poravna s magnetskim poljem i količina energije koja je ispuštena ovise o kemijskom sastavu molekula. S obzirom na to je moguće razaznati razliku između različitih vrsta tkiva. 

Prethodno navedene mogućnosti prilagodbe prikupljanja podataka tokom snimanja omogućuje k-prostor. K-prostor je proširenje koncepta Fourierovog prostora. Matrica je koja sadrži sirove podatke prikupljene prilikom snimanja magnetskom rezonancijom. Kako bi se dobila slika iz k-prostora potrebno je obaviti više koraka procesiranja. Proces transformiranja k-prostora u konačne slike se naziva rekonstrukcija slike. Najčešći koraci rekonstrukcije slike su: Fourierova transformacija, pretvaranje u bijeli signal (noise prewhitening), filteri nad k-prostorom i kombiniranje zavojnica u sustavima s više zavojnica. 

Kvaliteta slike se određuje trima karakteristikama. Omjerom snage signala i snage šuma (SNR). oblikom piksela (point spread function) i artifaktima. Ukoliko je snaga šuma visoka, dolazi do diskoloracije konačne slike. Oblik piksela bi trebao biti samo jedan piksel, no ukoliko funkcija širenja sadrži više frekvencija doći će do zamućenja piksela. Artifakti nastaju kada su signali povezani uz piksele kojima ne pripadaju. To stvara preklapanja slike i nastanak nepostojećih oblika na slikama. Ukoliko je kvaliteta slike niska, može doći do skrivanja bitnih obilježja na slici što može dovesti do pogrešne dijagnoze, ili u najgorem slučaju slike koju nije moguće iskoristiti.

Sliku iz k-prostora dobivamo Fourierovom tranformacijom. `POGLEDATI ONAJ VIDEO I OPISATI MAL KAK TO SVE IZGLEDA`

