---
title: Datumy
demand: 2
---

1. Z odkazu na začátku lekce si stáhněte základ pro Webpack project s Reactem, nainstalujte závislostí pomocí a spusťte projekt.
1. Vytvořte komponentu `Today`, která bude očekávat tři props:

   - `day` - řetězec s číslem dne, například `'07'`,
   - `month` - řetězec s číslem měsíce, například `'12'`,
   - `year` - řetězec s číslem roku, například `'2020'`.

   Tato komponenta by měla zobrazit datum ve formátu 07.12.2020. Zabalte každou položku datumu do zvláštního `span` elementu a dejte každé vlastní CSS třídu, abychom mohli den měsíc i rok nastylovat zvlášť.

1. Vytvořte komponentu `App`, která na stránce zobrazí tři různé datumy pomocí `Today`.
1. Pro komponentu `Today` vytvořte soubor se styly a nastylujte číslo pro den tak, aby bylo zobrazeno tučně a číslo pro rok tak, aby bylo zobrazeno o 20% menším fontem.
