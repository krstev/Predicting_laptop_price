# O projektu

Osnovna ideja projekta je prikupljanje podataka i cena novih laptopov računara sa sajta www.ebay.com i korišćenje tih podataka za pravljenje predikcionog modela koji bi predvideli cenu laptop računara sa određenom specifikacijom. 
U projektu su korišćena četiri vrste predikcionih metoda. Metode koje su korišćene su k-najbližih susesa (KNearestNeigbours), linearna regresija (LinearRegression), M5P i REPTree. Rezultati ovih metoda su upoređeni kako bi se došlo do utvrđivanja koja od ovih metoda je najpodobnija za ovaj tip predikcije.

# Korišćene metode

### Metoda k-najbližih suseda
Metoda k-najbližih suseda predstavlja neparametrsku klasifikacionu tehniku koja klasifikuje zadati vektor atributa na osnovu skupa od k najbližih suseda tog vektora. Pri tom se pod najbližim susedima misli na podatke iz trening skupa podataka koji imaju najviši stepen sličnosti vektora atributa sa posmatranim vektorom.<br>
### Linearna regresija
Linearna regresija (LinearRegression) je popularna tehnika koja se često koristi u procesu rudarenje podataka (Data Mining). Metoda je veoma prosta kada se koristi za predviđanje jedne izlazne promenljive sa jednom ulaznom promenjljivom. Uopšteno, model koristi jednu ili više nezavisnih promenljivih i predviđa nezavisnu promeljivu kao rezultat. Regresioni model se koristi za predviđanje vrednosti nepoznate promenljive.<br>
### M5P
M5P implementira bazne rutine za generisanje M5 Model stabla i pravila. Originalan R. Quinlan-ov i Yong Wang-ov algoritam M5 je unapređen. M5P kombinuje konvencionalno drvo odlučivanja sa funkcijom linearne regresije u čvorovima. Algoritam koji generise drvo odlučivanja je korišćen za pravljenje stabla, ali umesto entropije za svaki unutrašnji čvor, kriterijum podele koji se koristi je minimizacija varijanse u svakom podstablu.<br> 
### REPTree
REPTree koristi regresiono stablo kako bi  kreirao različita stabla u više iteracija. Nakon toga bira najbolje stablo od svih generisanih koje će se smatrati reprezentativnim.

# Skup podataka

Za prikupljanje podataka o novim laptop računarima korišćen je [eBay Finding API](https://go.developer.ebay.com/what-ebay-api). Servis je prvo korišćen radi prikupljanja ID-jeva proizvoda, da bi potom pomoću istog servisa korišćenjem prikupljenih ID-jeva prikupljani podaci za dva skupa podataka.
```
http://svcs.ebay.com/services/search/findingservice/v1?operation-name=finditemsbyproduct&service-version=
1.13.0&SECURITY-APPNAME=freelanc-xxxx-xxxx-xxxx-xxxxxxxxxxxx&RESPONSE-DATA-FORMAT=JSON
&REST-PAYLOAD&categoryId=175672&paginationInput.entriesPerPage=100&paginationInput.pageNumber=x
&itemFilter(0).name=Condition&itemFilter(0).value=1000&itemFilter(1).name=ListingType&itemFilter(1).
value=FixedPrice&aspectFilter(0).aspectName=Processor+Type&aspectFilter(0).aspectValueName(0)=Intel+Xeon
&aspectFilter(0).aspectValueName(1)=AMD+E-Series&aspectFilter(0).aspectValueName(2)=Intel+Atom&
aspectFilter(0).aspectValueName(3)=Intel+Celeron&aspectFilter(0).aspectValueName(4)=Intel+Core+2+Duo&
aspectFilter(0).aspectValueName(5)=Intel+Core+2+Quad&
aspectFilter(0).aspectValueName(6)=Intel+Core+i3+1st+Gen.&
aspectFilter(0).aspectValueName(7)=Intel+Core+i3+2nd+Gen.&
aspectFilter(0).aspectValueName(7)=Intel+Core+i3+3rd+Gen.&
aspectFilter(0).aspectValueName(8)=Intel+Core+i3+4th+Gen.&
aspectFilter(0).aspectValueName(9)=Intel+Core+i5+1st+Gen.&
aspectFilter(0).aspectValueName(10)=Intel+Core+i5+2nd+Gen.&
aspectFilter(0).aspectValueName(11)=Intel+Core+i5+3rd+Gen.&
aspectFilter(0).aspectValueName(12)=Intel+Core+i5+4th+Gen.&
aspectFilter(0).aspectValueName(13)=Intel+Core+i7+1st+Gen.&
aspectFilter(0).aspectValueName(14)=Intel+Core+i7+2nd+Gen.&
aspectFilter(0).aspectValueName(15)=Intel+Core+i7+3rd+Gen.&
aspectFilter(0).aspectValueName(16)=Intel+Core+i7+4th+Gen.&
aspectFilter(0).aspectValueName(17)=Intel+Pentium&
aspectFilter(1).aspectName=Graphics+Processing+Type&
aspectFilter(1).aspectValueName(0)=Dedicated+Graphics&
aspectFilter(1).aspectValueName(1)=Hybrid+Graphics&
aspectFilter(1).aspectValueName(2)=Integrated/On-Board+Graphics&
aspectFilter(2).aspectName=Memory&aspectFilter(0).aspectValueName=1+GB+or+more
```
 Kategorija 175672 na eBay sajtu predstavlja laptop i netbook računare (Laptops & Netbooks). Takođe pri pozivu servisa navedeno je da kod svakog rezultata trebaju biti deklarisane sledeće specifikacije koje su uzete kao relevantne za ova dva tipa podataka : tip procesora, RAM memorija, veličina ekrana, tip grafike, operacioni sistem, da li računar poseduje SSD i naravno cena, koja je naglašena da treba da bude fiksna kako pri prikupljanju podataka u obzir ne bi ulazili podaci o aukcijama koji bi zbog specifičnosti stvaranja cene mogli narušiti konzistentnost skupa podataka. Obradom prikupljenih podataka napravljena su dva skupa podataka u [.arff](https://weka.wikispaces.com/ARFF+(stable+version)) formatu koji izgledaju <br>
![arff](https://cloud.githubusercontent.com/assets/10245806/17696718/88c4d8ce-63b0-11e6-85a9-565c035e158a.PNG)<br>
![arff1](https://cloud.githubusercontent.com/assets/10245806/17696773/e3e381b0-63b0-11e6-80a9-aef9624e179a.PNG)

Podaci su obrađeni kako bi sve promenljive bile numerčkog ili nominalog tipa koji je potreban za prethodno navedene modele koji su koršćeni.

# Tehnička realizacija

Prikupljanje podataka i njihova obrada je urađena u programskom jeziku PHP. Realizicija upotrebljenih modela je urađena u programskom jeziku Java sa referenciranom bibliotekom Weka. 
Weka biblioteka je korišćena za primenu metoda mašinskog učenja nad skupovima podataka čiji je proces kreiranja ranije opisan. U pitanju je čitav niz algoritama mašinskog učenja koji se koriste za izvršavanje DataMining zadataka (procesiranje velike količine podataka sa ciljem dobijanja novih informacija). U pitanju je "open source" softver.
Klase koje su korišćene iz navedene biblioteke su IBk(KNearestNeigbours), LinearRegression , M5P i REPTree.

# Analiza

Detaljni rezultati se nalaze u dokumentu ["rezultati.txt"](https://gitlab.com/KrstevFilip/Predicting_laptop_price/blob/master/rezultati.txt).
+ Za upoređivanje predikcionih modela numeričke klase treba koristiti neke od sledećih parametara: srednja apsolutna greška, relativna apsolutna greška, srednja kvadratna greška i srednja relativna kvadratna greška.
Jedan od korišćenih modela ima svoju specifičnost koja ga najbolje opisuje a to Linearni model, pa ćemo uporediti dva dobijena modela na osnovu dva skupa podataka. 
![Linearni model 1](https://gitlab.com/KrstevFilip/Predicting_laptop_price/blob/master/images/linearniModel1.png "Linearni model 1")
![Linearni model 2](https://gitlab.com/KrstevFilip/Predicting_laptop_price/blob/master/images/linearniModel2.png "Linearni model 2")
Iz funkcija regresionih modela možemo uočiti da je ε, slučajna greška, odnosno skrivena promenljiva koja pokazuje neopisanost modela kod prvog modela 166,0519 a drugog 183,657 iz čega možemo zaključiti da što se linearne regresije tiče bolji model dobijamo treniranjem iz prvog skupa podataka.
Na osnovu sledeće tabele možemo uporediti sve korišćene modele: 
![tabela](https://gitlab.com/KrstevFilip/Predicting_laptop_price/blob/master/images/tabela.png "Uporedna tabela")
Uporedna analiza je izvršena na osnovu srednje kvadratne greške i možemo da vidimo poredići rezultate po modelu po svakom od dva skupa podataka da svi modeli trenirani nad prvim skupom podataka imaju manju srednje kvadratnu grešku, samim tim je i prvi skup podataka relevantniji za predikciju.
Poredići modele trenirane nad prvim skupom podataka uočavamo da najmanju srednje kvadratnu grešku ima Linearna regresija i sa veoma malom razlikom je sledi M5P model zatim REPTree. Poredići ova tri modela na drugom skupu podataka vidimo da je situacija malo drugačija, s toga možemo zaključiti da u zavisnosti od skupa podataka trebalo bi koristiti neki od tri prethodno navedena modela, gde bih prednost dao M5P modelu zbog konzistentnosti.

# Reference

[1] Michael Abernethy, "Data mining with WEKA, Part 1: Introduction and regression", 2010, link: http://www.ibm.com/developerworks/library/os-weka1/, 11.07.2016.
[2] Weka. link: https://weka.wikispaces.com/ , 11.07.2016
[3] OpenTox link: http://www.opentox.org/dev/documentation/components/m5p , 11.07.2016
[4] Sushilkumar Kalmegh "Analysis of WEKA Data Mining Algorithm REPTree, Simple Cart and RandomTree for Classification of Indian News " link: http://ijiset.com/vol2/v2s2/IJISET_V2_I2_63.pdf , 11.07.2016.
