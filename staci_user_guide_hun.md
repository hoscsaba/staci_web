<header>
<center>
<h1>STACI kézikönyv - Hidraulikai modell</h1>
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

## Nyíltfelszínű ág

TODO

## Megoldási módszer

Az ágegyenletek és a kontinuitási egyenletek együtt egy nagyméretű nemlineáris algebrai egyenletrendszert alkotnak, mely

$$0=f_i(x),\quad i=1...M+N$$

alakú, ahol $x=(p_1,p_2,...,p_N,\dot{m}_1,\dot{m}_2,...,\dot{m}_M)^T$ az ismeretleneket tartalmazó vektor, $N$ a csomópontok száma, $M$ pedig az ágak száma. A megoldáshoz a relaxált Newton-módszert alkalmazzuk:

$$x_{j+1} = (1-\omega)\, x_j + \omega\, \tilde{x}_{j+1}\quad \text{ahol}\quad \tilde{x}_{j+1}=x_{j} + (J(x_j))^{-1} f(x_j)$$

ahol $J(x_j)$ az egyenletrendszer Jacobi mátrixa az előző, $x_j$ pontban kiértékelve:

$$J_{kl}(x_j)=\left.\frac{\partial f_k}{\partial x_l}\right|_{x=x_j}$$


# Tartózkodási idő

A $\tau$ tartózkodási idő (vízkor) iteratív úton történik. Ágelemek esetén az elem $L$ hossza és a $v$ áramlási sebesség segítségével könnyen kiszámítható, hogy az elemen végighaladva mennyit "öregszik" a víz: $\Delta \tau = L/v$. Természetesen, ha a közeg $\tau_{be}$ korral lép be az elembe, kilépéskor $\tau_{ki}=\tau_{be}+\Delta \tau$ lesz a vízkor. Csomópontokba érkező víz tömegáramai és a vízkorok legyenek $\dot{m}_{be,i}$ és $\tau_{be,i}$, a távozó ágakon pedig egységesen $\tau_{ki}$. A programban választatunk 

* átlagos vízkor (tökéletes keveredés a csomópontban): $\tau_{ki}=\frac{\sum_{i} \dot{m}_i \tau_{be,i}}{\sum_{i} \dot{m}_i}$,
* vagy (konzervatív becslés esetén) választhatjuk a legnagyobb vízkort is: $\tau{ki}=\max_i \tau_{be,i}$ 

mindkét esetben $i$ azokra az ágakra vonatkozik, melyeken keresztül a közeg a csomópontba áramlik. A számítás során az ágelemek vízkorát és a csomóponti keveredési törvényt felváltva léptetjük egészen addig, míg az egész hálózatban mért átlagos vízkor változása a megadott hibahatár alá nem csökken.

# Klór koncentráció

TODO

# Érzékenység

TODO

</main>