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

Pygame Zero is smart about how it calls your functions. If you don't define
your function to take a ``pos`` parameter, Pygame Zero will call it without
a position. There's also a ``button`` parameter for ``on_mouse_down``. So we
could have written::

    def on_mouse_down():
        print("Klikol si!")

alebo::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Au!")



Zvuky a obrázky
-----------------

Now let's make the alien appear hurt. Save these files:

* `alien_hurt.png <_static/alien_hurt.png>`_ - save this as ``alien_hurt.png``
  in the ``images`` directory.
* `eep.wav <_static/eep.wav>`_ - create a directory called ``sounds`` and save
  this as ``eep.wav`` in that directory.

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

Now when you click on the alien, you should hear a sound, and the sprite will
change to an unhappy alien.

There's a bug in this game though; the alien doesn't ever change back to a
happy alien (but the sound will play on each click). Let's fix this next.


Clock
-----

If you're familiar with Python outside of games programming, you might know the
``time.sleep()`` method that inserts a delay. You might be tempted to write
code like this::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()
            time.sleep(1)
            alien.image = 'alien'

Unfortunately, this is not at all suitable for use in a game. ``time.sleep()``
blocks all activity; we want the game to go on running and animating. In fact
we need to return from ``on_mouse_down``, and let the game work out when to
reset the alien as part of its normal processing, all the while running your
``draw()`` and ``update()`` methods.

This is not difficult with Pygame Zero, because it has a built-in
:class:`Clock` that can schedule functions to be called later.

First, let's "refactor" (ie. re-organise the code). We can create functions to
set the alien as hurt and also to change it back to normal::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

This is not going to do anything different yet. ``set_alien_normal()`` won't be
called. But let's change ``set_alien_hurt()`` to use the clock, so that the
``set_alien_normal()`` will be called a little while after. ::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 0.5)

``clock.schedule_unique()`` will cause ``set_alien_normal()`` to be called
after ``0.5`` second. ``schedule_unique()`` also prevents the same function
being scheduled more than once, such as if you click very rapidly.

Try it, and you'll see the alien revert to normal after 0.5 second. Try clicking
rapidly and verify that the alien doesn't revert until 0.5 second after the last
click.

``clock.schedule_unique()`` accepts both integers and float numbers for the time interval. in the tutorial we are using
a float number to show this but feel free to use both to see the difference and effects the different values have.


Zhrnutie
-------

Ukázali sme si, ako nahrať a zobraziť sprity, prehrávať zvuky, ošetriť vstupné udalosti
a ako sa používajú zabudované hodiny.

You might like to expand the game to keep score, or make the alien move more
erratically.

There are lots more features built in to make Pygame Zero easy to use. Check
out the :doc:`built in objects <builtins>` to learn how to use the rest of the
API.
