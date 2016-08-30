<header>
<center>
<h1>STACI kézikönyv</h1>
</header>
<main>

[toc]

# Bevezetés

A **Staci** egy általános hidraulikai megoldó, mely nyomottvizes (teltszelvényű) csővezetékrendszerek és csatornahálózatok (nyíltfelszínű áramlás) állandósult állapotának vizsgálatát tesz lehetővé. A program képes

* a csomóponti nyomások,
* az ág-térfogatáramok (tömegáramok), az áramlási sebességek,
* tartózkodási idő (vízkor),
* jelleggörbével megadott ágelemek (pl. szivattyú, szabályozószelep) munkapontjának,
* nyíltfelszínű ágak esetén a vízszint-görbe,
* a hálózatba bejuttatott klór egyensúlyi eloszlásának meghatározására,
* valamint adott időszakra vonatkozó üzemvitel-követés (sorozatszámítás) elvégzésére.

A program grafikus felhasználói felülete (GUI - Graphical User Interface) lehetővé teszi a hidraulikai modellek gyors felépítését, a számítások könnyű követését, valamint az eredmények átlátható, könnyen értelmezhető megjelenítését.

# Hidraulikai modell

## Bevezetés

A hidraulikai modellezés során ismeretlennek tekintjük

* a csomóponti nyomásokat ill.
* az ágelemek térfogatáramát (vagy, ezzel egyenértékűen a tömegáramot vagy áramlási sebességet).

## Csomóponti egyenlet

Mivel a program lehetővé teszi minden ágelemhez külön-külön sűrűségérték hozzárendelését, a kontinuitási egyenleteket tömegáramok segítségével fogalmazzuk meg:

$$ \sum_{i(j)} \delta_{ij}{\dot m}_i = d_j $$

ahol $j$ jelöli az aktuális csomópontot, ${\dot m}_i\,(kg/s)$ az $i$-edik ágelem tömegáramát, $d_j$ pedig a csomópontbeli fogyasztást (ami lehet negatív is, ez esetben a rendszerbe betáplált tömegáramról van szó). Mivel minden ágelem *irányított*, azaz pontosan definiáljuk, hogy melyik csomópontot tekintjük az ágelem 'elejének' és 'végének', ezért a tömegáram is irányított (pozitív tömegáram esetén a közeg az 'eleje' csomóponttól a 'vége' csomópont felé áramlik, negatív érték esetén fordítva). Így $\delta_{ij}=+1$, ha a j-edik csomópont végcsomópontja az i-edik ágelemnek, míg $\delta_{ij}=-1$, ha a j-edik csomópont induló csomópontja az i-edik ágelemnek. 

## Ágegyenletek

Minden ágegyenlet közös jellemzője, hogy az ág elején lévő $p_e$ nyomást, a végén található $p_v$ nyomást és az ágelem térfogatáramát kapcsolja össze, azaz $p_e-p_v=f(\dot{m})$ alakú.

### Csővezeték

Csővezetékek esetén a Bernoulli egyenlet írja le közeg viselkedését, azaz

$$\frac{p_e}{\rho g}+z_e = \frac{p_v}{\rho g}+z_v +\lambda(Re)\frac{L}{D}\frac{1}{2g} \frac{\dot{m} |\dot{m}|}{A^2 \rho}$$

TODO: lambda részletesen

### Jelleggörbés fojtás

A fojtás (tolózár) ellenállását a $\zeta$ ellenállástényezővel vesszük figyelembe:

$$\frac{p_e}{\rho g}+z_e = \frac{p_v}{\rho g}+z_v +\zeta(e) \dot{m} |\dot{m}|$$

A fojtás $\zeta$ ellenállástényezője az $e$ nyitás függvénye, ezt a függvénykapcsolatot a felhasználó megadhatja.

### Szivattyú

A szivattyú szállítómagassága definíció szerint a Bernoulli-összeg megváltozása a szívó- és nyomócsonk között:

$$\frac{p_{ny} - p_{sz}}{\rho g}+\frac{v_{ny}^2-v_{sz}^2}{2 g} = H_{sz}(Q)$$

Itt $v_{ny}=\frac{\dot{m}}{A_{ny}rho}$ és $v_{sz}=\frac{\dot{m}}{A_{sz}rho}$ a nyomó- és szívóoldali sebességet jelölik, H_{sz} pedig a szivattyú jelleggörbéjét.

### Medence

Medence esetén a csatlakozó nyomópontbeli $p$ nyomást a $H_f$ fenékmagasság és a $H_v$ vízszint-magasság összegéből számíthatjuk:

$$\frac{p}{\rho g} +z = H_f + H_v$$

### Nyíltfelszínű ág

TODO

</main>