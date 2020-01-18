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
- Csak a `nyitott` portok érhetőek el hálózati kapcsolaton keresztül. 
- A tesztelendő alkalmazás mögött lévő adatbázis a 3306-os port számon kommunikál, ami főleg biztonsági okok miatt `zárva` van a hálózat felé. 

### SSH

Mivel a 3306-os port nem érhető el kívülről, ezért szükség van egy módszerre, amivel `biztonságos csatornán`, távolról be tudunk jelentkezni a szerver környezetbe. Ebben segít az `SSH` (Secure Shell) protokoll, ami egy lokális és egy távoli számítógép közötti biztonságos csatorna kiépítését teszi lehetővé.

### Port forwarding

### Windows 10 SSH Client

A 2017-es Fall Creators Update előtt a Windows 10 nem rendelkezett beépített SSH klienssel. Olyan külső programok használatára volt szükség mint például a [PuTTY](https://www.putty.org/)

A frissítés óta azonban szerencsére rendelkezésre áll a beépített kliens, amivel egyszerűen, a parancssorból tudunk SSH kapcsolatot létesíteni. Lássuk hogyan:

1. SSH kliens ellenőrzése: Gépház -> Alkalmazások -> Opcionális szolgáltatások
2. Windows PowerShell megnyitása
3. Csatlakozás a távoli számítógépre, port forwardinggal együtt:

```bash
ssh -i path/to/key/2019augPrivateAWSKey.pem -L 3307:localhost:3306 ubuntu@example.progmasters.hu
```



## MySQL

### MySQL Workbench




