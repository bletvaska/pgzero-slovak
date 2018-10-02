Inštalácia knižnice Pygame Zero
===============================

Dodávané s Mu
----------------

`Mu IDE <https://codewith.mu>`_, ktoré sa zameriava na začiatočníkov, je 
dodávané s knižnicou Pygame Zero.

You will need to `switch mode <https://codewith.mu/en/tutorials/1.0/modes>`_
into Pygame Zero mode to use it. Then type in a program and
`use the Play button <https://codewith.mu/en/tutorials/1.0/pgzero>`_ to run it
with Pygame Zero.

.. note::

    Verzia dodávanej knižnice Pygame Zero v editore Mu však nemusí byť najnovšia!
    Verziu knižnice môžete zistiť spustením tohto kódu v editore Mu::

        import pgzero
        print(pgzero.__version__)


Samostatná inštalácia
----------------------

Najprv potrebujete mať nainštalovaný **Python 3**! Ten je obyčajne predinštalovaný, 
ak používate **Linux** alebo **Raspberry Pi**. Pre iné systémy si ho môžete stiahnuť
zo stránky `python.org <https://www.python.org/>`.


Windows
'''''''

Pre nainštalovanie knižnice Pygame Zero použite **pip**. Do `príkazového riadku`__ zadajte

.. __: https://www.lifewire.com/how-to-open-command-prompt-2618089

::

    pip install pgzero


Mac
'''

Do okna terminálu zadajte

::

   pip install pgzero


Note that there are currently no Wheels for Pygame that support python 3.4 for Mac,
so you may need to you will need to upgrade Python to >=3.6 (or use python 2.7) in
order to be able to install pygame. For a list of available Wheels, please visit
`pyPI_`

.. _pyPI: https://pypi.org/project/Pygame/#files

Linux
'''''

Do okna terminálu zadajte

::

   sudo pip install pgzero


Niektoré linuxové systémy však používajú ``pip3``; ak predchádzajúci príkaz vypíše niečo ako
``sudo: pip: príkaz nenájdený`` potom zadajte::

    sudo pip3 install pgzero

Občas pip nie je nainštalovaný, takže ho treba doinštalovať. Ak je to váš prípad, 
zadajte najprv nasledujúci príkaz pred opätovným spustením predchádzajúcich::


    sudo python3 -m ensurepip


.. _install-repl:

Inštalácia REPL
-------------------

:doc:`Pygame Zero's REPL <repl>` is an optional feature. This can be enabled
when installing with ``pip`` by adding ``pgzero[repl]`` to the pip command
line::

    pip install pgzero[repl]

If you aren't sure if you have the REPL installed, you can still run this
command (it won't break anything if it is installed!).

