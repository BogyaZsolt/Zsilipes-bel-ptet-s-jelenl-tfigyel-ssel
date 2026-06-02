# Zsilipes-beléptetés-jelenlétfigyeléssel

Ez a Project egy Arduino alapú zsilipvezérlő rendszer működését mutatja be. 

TinkerCad elérhetőség: https://www.tinkercad.com/things/fgj0YUyewr7-zsilipes-beleptetes-jelenletszamolassal-es-riasztokezelessel?sharecode=iQvcjAV0DgIMQSb-Zdnnwvowykz98aeRw-UrZ0dSfg4


1. Bevezetés
Ez a dokumentum egy Arduino alapú zsilipvezérlő rendszer működését mutatja be. A rendszer célja, hogy két ajtó között biztonságos, szabályozott átjárást biztosítson, megakadályozva, hogy egyszerre mindkét ajtó nyitva legyen. Emellett a rendszer nyilvántartja a bent tartózkodók számát, automatikusan élesíti vagy kikapcsolja a riasztást, és mozgásérzékelő segítségével felügyeli a helyiséget.
A dokumentum tartalmazza:
-	a rendszer működésének részletes leírását,
-	a zsilip logikáját,
-	a riasztás működését,
-	a létszámkezelést,
-	a LED ek és az LCD működését,
-	a fejlesztés során felmerült problémákat,
-	a további fejlesztési lehetőségeket,
2. A rendszer felépítése
A rendszer az alábbi fő komponensekből áll:
- Arduino UNO mikrokontroller
- Két ajtónyiitás érzékelőt (sajnos nyitásérzékelő nem volt ezért gomb formájában)
- Két LED az ajtók nyitott állapotának jelzésére
- PIR mozgásérzékelő
- Buzzer a riasztáshoz
- Riasztó LED
- 16×2 karakteres LCD kijelző
A rendszer két fő funkciót lát el:
- Zsilipvezérlés – egyszerre csak egy ajtó lehet nyitva.
- Riasztás és létszámkezelés, a bent tartózkodók számának nyilvántartása és a riasztórendszer automatikus élesítése, és kikapcsolása.
 


3. A zsilip működési elve
A zsilip célja, hogy egyszerre csak egy ajtó legyen nyitva, így biztosítva a helyiség védelmét és a szabályozott beléptetést.


3/1 Ajtónyitás logikája
•	Ha a külső ajtó nyitva van, a belső ajtó nem nyitható.
•	Ha a belső ajtó nyitva van, a külső ajtó nem nyitható.
•	Ha mindkét ajtót egyszerre próbálják nyitni, a rendszer „Zsilip foglalt”” üzenetet jelenít meg.

3/2 Ajtóállapot visszajelzés
Mindkét ajtóhoz tartozik egy LED:
•	Világít, ha az adott ajtó nyitva van.
•	Nem világít, ha az ajtó zárva van.

4. Létszámkezelés
A rendszer nyilvántartja, hány személy tartózkodik a helyiségben.

4/1 Belépés
A belépés folyamata:
1.	A külső ajtó kinyílik.
2.	A külső ajtó bezárul.
3.	A rendszer ellenőrzi, hogy az ajtó nyitása engedélyezett volt e.
4.	Ha igen, a létszám eggyel nő.
5.	A riasztás kikapcsol.


4/2 Kilépés
A kilépés folyamata:
1.	A belső ajtó kinyílik.
2.	A belső ajtó bezárul.
3.	A rendszer ellenőrzi, hogy az ajtó nyitása engedélyezett volt e.
4.	Ha igen, a létszám eggyel csökken.
5.	Ha a létszám eléri a nullát, a riasztó élesedik.

5. Riasztás működése
A riasztás két állapotot ismer: élesített és kikapcsolt.

5/1 Riasztás élesítve
A riasztás akkor éles, ha:
-	a bent tartózkodók száma 0,
-	nincs ajtónyitás,
Az LCD kijelző ilyenkor ezt mutatja: „Rendszer kikapcs”

5/2 Riasztás aktiválása
Ha:a bent tartózkodók száma 0, és a PIR mozgást érzékel,
akkor a rendszer riasztást indít:
-a riasztó LED világít,
-a buzzer hangot ad,
-az LCD kijelzőn megjelenik:
Riasztás!
Mozgas van!



5/3 Riasztás kikapcsolása
A riasztás automatikusan kikapcsol, ha valaki belép a helyiségbe, vagy ha a mozgás abbamarad(eggyenlőre).

6. LCD kijelző működése
A kijelző két soron jeleníti meg a rendszer állapotát.
Normál állapot
Benti letszam: X
Élesítve, vagy Rendszer kikapcs

Zsilip foglalt jelzés a kijelzőn:
Zsilip foglalt
Varakozz.

Riasztás
RIASZTAS!
Mozgas van!


7. A rendszer működésének logikai blokkjai
A program öt fő logikai egységből áll:
1.	Ajtóállapotok beolvasása
2.	LED-ek vezérlése
3.	Zsilip tiltás
4.	Létszámkezelés
5.	Riasztáskezelés


8. A fejlesztés során felmerült problémák
A projekt fejlesztése során több olyan helyzet adódott, amely megoldást igényelt. Ezek közül a legfontosabbak:
-A kijelző villogása
A kijelző folyamatos frissítése villogást okozott. Megoldás: csak akkor frissül, ha a tartalom változik.
-Az  LCD kijelzőnél: - a háttérvilágításra nem került ellenállás,a kijelző felrobbant a szimulációban
-Több kódrész hibás volt:
-rossz zárójelezés
-hibás változónév
-rossz LCD szöveg
-logikai sorrend felcserélése
Ezeket mind javítottam.
- Létszám ugrálása
A rendszer kezdetben minden ajtónyitást belépésnek vagy kilépésnek érzékelt. Megoldás: csak akkor számol, ha az ajtó nyitása engedélyezett volt.

Sajnos nem találtam nyitásérzékelőt a TinkerCad-ben, ezért csak nyomógombok imitáljék az ajtónyitást,ezért az ajtó nyitva tartását, csak egy átkötéssel tudtam próbálni, így lehet kipróbálni a zsilipelés funkciót.

-Zsilip logika hibái
Előfordult, hogy a rendszer túl korán vagy túl későn jelezte a zsilip foglaltságát. Megoldás: a zsilip foglalt állapot csak akkor jelenik meg, ha mindkét ajtót egyszerre próbálják nyitni.
- Riasztás téves aktiválása
A riasztás néha akkor is aktiválódott, amikor nem kellett volna. Megoldás: pontosított logika, amely csak akkor indít riasztást, ha valóban nincs bent senki.





9. Fejlesztési lehetőségek
A rendszer továbbfejleszthető az alábbi irányokban:
-RFID kártyás beléptetés
A gombok helyett RFID olvasóval lehetne azonosítani a belépő személyeket.
-Automatikus ajtóvezérlés
A rendszer elektromos zárakat is vezérelhetne, nem csak LED-eket.
-Hálózati kapcsolat
WiFi vagy Bluetooth segítségével távolról is ellenőrizhető lenne a rendszer állapota.

10. Összegzés
A zsilipvezérlő rendszer egy jól átgondolt, stabilan működő megoldás, amely valós rendszerek logikáját követi. A fejlesztés során több problémát is sikerült megoldani, és a rendszer további bővítési lehetőségeket is kínál.



– A teljes programkód a TinkerCad szimulációban megtekinthető.
