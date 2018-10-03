Úvod do Pygame Zero
===========================

.. highlight:: python
    :linenothreshold: 5

Vytvorenie okna 
-----------------

Najprv vytvorte prázdny súbor s názvom ``intro.py``.

Uistite sa, že sa po spustení nasledujúceho príkazu vytvorí prázdne okno ::

    pgzrun intro.py

Všetko v knižnici Pygame Zero je voliteľné; prázdny súbor je valídnym skriptom Pygame Zero!

Hru môžete ukončiť kliknutím na ikonu pre zatvorenie okna alebo stlačením kláves
``Ctrl-Q`` (``⌘-Q`` na Mac-u). Ak z nejakého dôvodu prestane hra reagovať, môžete 
ju ukončiť stlačením ``Ctrl-C`` v okne vášho terminálu.


Vykreslenie pozadia
--------------------

Teraz pridáme funkciu :func:`draw` a nastavíme rozmery okna. Pygame Zero
bude túto funkciu volať zakaždým, keď bude potrebovať kresliť na obrazovku.

Do súboru ``intro.py`` pridajte tieto riadky::

    WIDTH = 300
    HEIGHT = 300

    def draw():
        screen.fill((128, 0, 0))

Znovu spustite príkaz ``pgzrun intro.py`` čím sa obrazovka zmení na červený štvorec!

Ako tento kód pracuje?

Premenné ``WIDTH`` a ``HEIGHT`` nastavujú šírku a výšku okna. Tento kód teda nastaví 
šírku aj výšku okna na 300 pixelov.

``screen`` je zabudovaný objekt, ktorý reprezentuje plochu okna. Obsahuje 
:ref:`množstvo metód pre vykreslovanie sprite-ov a tvarov <screen>`. Zavolaním metódy
``screen.fill()`` dôjde k vyfarbeniu obrazovky jednou farbou, ktorá je definovaná
ako trojica ``(červená, zelená, modrá)``. ``(128, 0, 0)`` bude stredne
tmavočervená. Vyskúšajte si meniť hodnoty týchto čísiel v rozsahu od 0 do 255
a sledujte, aké farby dokážete vytvoriť.

Poďme nastaviť sprite, ktorý môžeme animovať.


Kreslíme sprite
---------------

Ešte predtým, ako čokoľvek nakreslíme, potrebujeme stiahnuť sprite s mimozemšťanom.
Kliknite na neho pravým tlačidlom a uložte ho (Vyberte "Uložit obrázok ako..." alebo niečo podobné).

.. image:: _static/alien.png

(Tento sprite je priesvitný (má "alpha" kanál), čo je pre hry skvelé!
Je ale navrhnutý pre tmavé pozadie, takže pokiaľ sa v hre nezobrazí, 
nemusíte vidieť mimozemšťanovu prilbu).

.. tip::

    Množstvo voľných spritov, vrátane tohto, môžete nájsť na stránke `kenney.nl
    <https://kenney.nl/assets?q=2d>`_. Tento sprite pochádza z balíka 
    `Platformer Art Deluxe pack
    <https://kenney.nl/assets/platformer-art-deluxe>`_.

Súbor potrebujete uložiť na správne miesto, aby ho knižnica Pygame Zero vedela nájsť.
Vytvorte priečinok s názvom ``images`` a obrázok do neho ulože pod názvom ``alien.png``.
Názvy oboch musia byť malými písmenami. V opačnom prípade sa bude knižnica sťažovať,
aby vás upozornila na potenciálny problém v multiplatformnej kompatibilite.

Ak ste to urobili, váš projekt by mal vyzerať takto:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    └── intro.py

``images/`` je štandardný priečinok, ktorý bude Pygame Zero používať na hľadanie
vašich obrázkov.

V knižnici sa nachádza zabudovaná trieda s názvom :class:`Actor`, ktorú môžete použiť
na reprezentovanie grafiky, ktorá sa má vykresliť na obrazovku.

Poďme jednu vytvoriť. Upravte súbor ``intro.py`` takto::

    alien = Actor('alien')
    alien.pos = 100, 56

    WIDTH = 500
    HEIGHT = alien.height + 20

    def draw():
        screen.clear()
        alien.draw()

Váš mimozemšťan by sa mal teraz zobraziť na obrazovke! Odovzdaním reťazca ``'alien'``
do konštruktora triedy ``Actor`` sa automaticky nahrá sprite a má k dispozícii 
atribúty ako pozícia a rozmer. To nám umožní nastaviť ``HEIGHT`` okna na 
základe výšky mimozemšťana.

Metóda ``alien.draw()`` vykreslí sprite na obrazovku na jeho aktuálnu pozíciu.


Hýbeme s mimozemšťanom
----------------------

Let's set the alien off-screen; zmeňte riadok obsahujúci ``alien.pos`` na::

    alien.topright = 0, 10

Všimnite si, ako sa priradením hodnoty do ``topright`` zmení poloha mimozemšťana
vzhľadom na jeho pravý horný roh. If the right-hand edge of the alien is at ``0``, the
alien is just offscreen to the left. Teraz ho poďme rozpohybovať. Pridajte tieto 
riadky na koniec súboru::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero zavolá vašu funkciu :func:`update` raz za každý snímok. Posunutím
mimozemšťana o niekoľko pixelov počas každého snímku spôsobí, že sa bude po obrazovke presúvať.
Keď dosiahne pravý okraj obrazovky, znovu ho spustíme od ľavého okraja.

Vaše funkcie ``draw()`` a ``update()`` pracujú veľmi podobne, ale sú navrhnuté pre dva rozličné účely.
Funkcia ``draw()`` zobrazí mimozemšťana na jeho aktuálnu pozíciu, zatiaľ čo
funkcia ``update()`` sa použije na aktualizovanie pohybu mimozemšťana po obrazovke.


Ošetrenie kliknutí
------------------

Teraz poďme zabezpečiť, aby hra urobila niečo po kliknutí na mimozemšťana. Aby sme 
to mohli spraviť, potrebujeme zadefinovať funkciu s názvom :func:`on_mouse_down`. 
Pridajte do kódu tieto riadky::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Au!")
        else:
            print("Netrafil si!")

Spustite hru a skúste na mimozemšťana klikať.

Knižnica Pygame Zero je pomerne chytrá čo sa týka volania vašich funkcií. Ak v
definícii vašej funkcie nepoužijete parameter ``pos``, Pygame Zero ju bude volať
bez pozície. Funkcia ``on_mouse_down`` taktiež obsahuje parameter ``button``.
Takže ju môžete napísať takto::

    def on_mouse_down():
        print("Klikol si!")

alebo takto::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Au!")



Zvuky a obrázky
-----------------

Teraz poďme zabezpečiť, aby mimozemšťan vyzeral zranene. Uložte si tieto súbory:

* `alien_hurt.png <_static/alien_hurt.png>`_ - súbor uložte ako ``alien_hurt.png``
  do priečinku ``images``.
* `eep.wav <_static/eep.wav>`_ - vytvorte priečinok s názvom ``sounds`` a uložte 
  do neho tento súbor ako ``eep.wav``.

Váš projekt by mal teraz vyzerať takto:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/`` je štandardný priečinok, v ktorom bude knižnica Pygame Zero hľadať
vaše zvukové súbory.

Teraz zmeňme funkciu ``on_mouse_down`` tak, aby použila tieto nové zdroje::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()

Ak teraz kliknete na mimozemšťana, mali by ste počuť zvuk a sprite sa zmení
na nešťastného mimozemšťana.

V hre sa však nachádza chyba; mimozemšťan sa už nikdy nezmení späť na šťastného
(zvuk sa však vždy po kliknutí prehrá). Túto chybu preto hneď opravíme.


Clock
-----

Ak poznáte Python mimo programovania počítačových hier, určite poznáte metódu
``time.sleep()``, ktorá pozastaví vykonávanie programu. To vás môže zvádzať k
napísaniu takéhoto programu::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()
            time.sleep(1)
            alien.image = 'alien'

Žiaľ, tento prístup nie je vôbec vhodný pre použitie v počítačových hrách.
Metóda ``time.sleep()`` blokuje všetky činnosti a my chceme, aby hra pokračovala
v behu a veci sa hýbali. Vlastne sa len potrebujeme vrátiť z funkcie ``on_mouse_down``
a nechať hru rozhodnúť, kedy resetuje mimozemšťana ako súčasť jej bežnej práce počas
vykonávania vašich metód ``draw()`` a ``update()``.

To je však nie je zložité vyriešiť s knižnicou Pygame Zero, pretože obsahuje 
zabudovanú triedu class:`Clock`, ktorá umožňuje naplánovať spúšťanie funkcií na neskôr.

Najprv "refaktorujme" (to znamená reorganizujme) kód. Môžeme vytvoriť funkcie, 
ktoré nastavia mimozemšťana na zraneného a rovnako ho vrátia naspäť do normálu::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

Zatiaľ však k žiadnej zmene nedôjde, pretože funkcia ``set_alien_normal()`` sa 
nikde nevolá. Ale zmeňme funkciu ``set_alien_hurt()`` tak, aby používala objekt
``clock``, takže funkcia ``set_alien_normal()`` sa bude volať o chvíľu neskôr.::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 0.5)

Metóda ``clock.schedule_unique()`` spôsobí, že funkcia ``set_alien_normal()`` 
sa zavolá po uplynutí ``0.5`` sekundy. Metóda ``clock.schedule_unique()`` 
taktiež zabráni tomu, aby rovnaká funkcia bola naplánovaná viackrát, ako napríklad
keď budete klikať veľmi rýchlo.

Vyskúšajte to a uvidíte, že sa mimozemšťan vráti do normálu po uplynutí 0.5 sekundy. 
Skúste rýchlo klikať a overte, že sa mimozemšťan nevráti po 0.5 sekunde od posledného 
kliknutia.
rapidly and verify that the alien doesn't revert until 0.5 second after the last
click.

``clock.schedule_unique()`` akceptuje pre zadanie časového intervalu ako celé 
tak aj desatinné čísla. V tomto návode sme použili desatinné čísla, aby to bolo vidieť,
ale neváhajte použiť obe, aby ste videli rozdiel a dopad, ktorý majú rozličné hodnoty.


Zhrnutie
-------

Ukázali sme si, ako nahrať a zobraziť sprity, prehrávať zvuky, ošetriť vstupné udalosti
a ako sa používajú zabudované hodiny.

Môžete hru rozšíriť o skóre alebo môžete upraviť mimozemšťana tak, aby sa pohyboval 
viac náhodne.

Knižnica Pygame Zero obsahuje množstvo ďalších zabudovaných vlastností, ktoré
robia prácu s ňou jednoduchou. Ak sa chcete naučiť aj zvyšné API, navštívte 
stránku o :doc:`zabudovaných objektoch <builtins>`.
