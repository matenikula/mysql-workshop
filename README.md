# Adatbázis, távoli elérés és MySQL

## Távoli szerver

Egy web alkalmazás esetén szükség van egy `futtató környezetre`, mely biztosítja a számítási kapacitást, a memóriát, a tárhelyet, ...stb ami ahhoz kell ,hogy az alkalmazás működőképes legyen. Ez a kiszolgáló környezet lehet akár a saját számítógépünk, mely esetben egy `lokális` környezetről beszélünk, lehet azonban egy fizikailag távol, akár a felhőben lévő környezet. Ezt szokás `távoli` szervernek nevezni. 

### AWS

A tesztelendő alkalmazás az `Amazon Web Services` által biztosított felhő alapu környezetben fut. Ez egy virtuális számítógép, mely biztosítja hogy az app számára folyamatosan rendelkezésre állnak a szükséges erőforrások és a hálózati kapcsolat. 

## Hálózati alapfogalmak 

Az AWS környezetében futó alkalmazás számára elengedhetetlen a kapcsolat az internettel, hiszen a hálózaton keresztül, a megfelelő IP cím és port szám segítségével tudjuk azt elérni a böngészőből. 

### IP cím

Az IP-cím (Internet Protocol-cím) egy egyedi `hálózati azonosító`, amelyet az Internet Protokoll (__IP__) segítségével kommunikáló számítógépek egymás azonosítására használnak. 

- Minden internetre kapcsolódott eszköznek van IP címe
- Egy-egy cím nem kötődik feltétlenül egy géphez (pl. dinamikus IP címek a lakossági internet szolgáltatók esetén)
- IPv4: négy darab, 0-255 közötti, ponttal elválaszott szám. Pl.: `172.16.254.1`

### Port

Számítógépes hálózatok esetében egy port a hálózati kommunikáció végpontja, egy `bemeneti-kimeneti kapu` melyen keresztül a adatátvitel történhet.

- A portok logikai konstrukciók, melyek lehetővé teszik, hogy a számítógépen futó alkalmazások a beérkező csomagokból csak a nekik szólót kapják meg
- A portokat számokkal azonosítjuk
- A webalkalmazásokat a szerver általában a 80-as porton teszi elérhetővé

### SSH

Az alkalmazás mögött lévő adatbázis egy meghatározott port-számon kommunikál, mely a MySQL esetében konvenció szerint: 3306. Ez a port nem elérhető kívűlről. így szükség van egy biztonságos csatornára

### Port forwarding

## MySQL

### MySQL Workbench




