# Retroarch_Mame_JPAC
Cílem je zaznamena a uchovat pro pzdější použití vše co jsem se naučil při konfiguraci emulátoru herního automatu.
## Hardware
- arcade automat libovolné konfigurace. V mém případě to je JAMMA konektor, 2xjoystick, 2x šest tlačítek, P1 Start, P2 Start a jedno tlačítko navíc, namapované jako P1SW7.
- LCD 4:3 *doplnit model LCD*. Můžete zvolit jiný model, ale dobré je dodržet rozměr 4:3, kvůli zachování stejného poměru stran jako má původní arcade monitor. 
- Raspberry PI3. Má dostatečný výkon pro většinu her. Dá se použít i libovolné pc, jedno jestli Linux nebo Windows.
- Ultimarc J-PAC, mám starší verzi z PS/2 konektorem. Dá se samožrejmě použít jakýkoliv převodník. Doporučuji ale převodník typu USB2JAMMA, nepřijdete tak o možnost zahrát si původní hru z pcb.
## Software
Pokud to s emulací myslíte vážně, tak dříve nebo později narazíte na projekty [libretro](https://www.libretro.com/) a [retroarch](https://www.retroarch.com/). Libretro je backend, retroarch je frontend a potří k sobě. Jejich kvalita a možnost konfigurace výrazně převyšuje vše co jsem zatím poznal, včetně neuvěřitelných vychytávek které zpříjemní hru. Umožňují i instalaci více emulátorů, takzvaných jader,  které díky retroarch sdílejí stejnou konfiguraci ovladače. Tento virtuální ovladač se nazývá Retropad a dá se k němu vše potřebné vyhledat na internetu.

Pro instalaci se pak nabízí trojice připravených distribucí Retropie, Lakka, Recalbox nebo Batocera. Nejlépe mi vychází Retropie, zejména kvůli těmto vlastnostem:
- lze instalovat na libovolný hardware, pokud není připravena binárka, proveden se instalace ze zdrojového kódu
- umožňuje pokročilou editaci konfiguračních souborů, bez které se neobejdeme
- aktualizace všeho :)
- frontend Emulationstation, jednoduchý a funkční 

Je pravděpodobné, že ostatní distribuce taky umožní ruční editaci konfiguráků, ale s instalací jinak než pomocí předpřipravených obrazů je to horší. 
## Instalace
Zvykněte si všechny příkazy spouštět přes `sudo`. Po rozbalení instalačního obrazu na SD kartu nezapoměňte přes `raspiconfig`:
- rozšíření ext4 partice na celou sd kartu, jedině tak bude možné použít její plnou kapacitu
- nastavit automatické přihlášení uživatele `pi`
Po restartu by vás měla přivítat příkazová řádka a distribuce přes celou kapacitu sd karty.
Teď je na čase se podívat na na Retropie, konfiguraci spustíte z příkazové řádky z adresáře `RetroPie-Setup` příkazem `sudo ./retropie_setup.sh`:
1. aktualizaci včetně všech balíčků systému
2. odinstalace  Retropie, potvrdíme smazání adresáře s ROM a BIOS, ale zakážeme odinstalova balíčky systému!

![IMAGE](1_retropie_setup.PNG)

Aktualizace bo potřebujeme, aby se nainstalovaly správně všechny závislosti a odinstalace, abychom se zbavili veškerého balastu. Mezi aktualizací a odinstalací doporučuji provést restart, možná se nainstalovalo nové jádro.
Všechny tyto adresáře by měly být prázdné nebo by neměly po odinstalaci existovat:
- `/opt/retropie`
- `~/.config/~retroarch`
- `~/RetroPie`
Pokud exsistují, smazat. Nakonec smažte i adresář s instalačním skriptem `~/RetroPie-Setup`.

A můžeme začít "novou" instalací, z Githubu si stáhneme nejnovější verzi instalačních skriptů - (https://github.com/RetroPie/RetroPie-Setup) a spustíme Basic install. Pokud máme RPi nebo jinou podporovanou distribuci, nainstaluje se Retropie a emulátory z binárek, jinak se musí zkompilovat. Vše proběhne automaticky, jen kompilace je násobně pomalejší.

Pak už zbývá jen nainstalovat [lr-mame2003-plus](https://docs.libretro.com/library/mame2003_plus/) jádro, najdede ho v sekci experimentálních balíčků. 

*zde se dá stručně pohovořit o jádře lr-mame2003-plus*
## Konfigurace
Automatický konfigurátot dělá paseku v konfiguračních souborech a tak je lepší nastavení doplnit ručně, proto děláme ty zálohy! V tomto kroku by se ještě neměl Emulationstation spouštět automaticky po startu systému. Budeme ho zatím spouštět ručně příkazem `emulationstation`.

Zazálohujeme soubor s globální konfigurací `/opt/retropie/all/retroarch.cfg`.

Nyní můžeme spustit Emulationstation a provést prvotní nastavení virtuálního ovladače RetroPad, tato konfigurace se spustí automaticky. Při mém setupu nastavuju D-PAD na šipky a tlačítka A, B, X, Y, R, L na tlačítka 1-6. Start na P1start a Select na klávesu 5, použita funkce shift. Ostatní tlačítka včetně hotkey přeskočit. Jak je standardně nastavený JPAC a jak funguje shift se dá dohledat tady: (https://www.ultimarc.com/control-interfaces/j-pac-en/j-pac-heritage-ps-2-version/). 

Vygeneruje se soubor `/opt/reteropie/all/emulationstation/es_input.cfg` a konfigurace se propíše i do globálního konfiguračního souboru pro Retroarch - `/opt/retropie/all/retroarch.cfg`. Takto může pak vypadat soubor `es_input.cfg`:

```
<?xml version="1.0"?>
<inputList>
  <inputAction type="onfinish">
    <command>/opt/retropie/supplementary/emulationstation/scripts/inputconfiguration.sh</command>
  </inputAction>
  <inputConfig type="keyboard" deviceName="Keyboard" deviceGUID="-1">
    <input name="pageup" type="key" id="122" value="1"/>
    <input name="up" type="key" id="1073741906" value="1"/>
    <input name="left" type="key" id="1073741904" value="1"/>
    <input name="select" type="key" id="53" value="1"/>
    <input name="right" type="key" id="1073741903" value="1"/>
    <input name="pagedown" type="key" id="120" value="1"/>
    <input name="y" type="key" id="1073742049" value="1"/>
    <input name="x" type="key" id="32" value="1"/>
    <input name="down" type="key" id="1073741905" value="1"/>
    <input name="start" type="key" id="49" value="1"/>
    <input name="b" type="key" id="1073742048" value="1"/>
    <input name="a" type="key" id="1073742050" value="1"/>
  </inputConfig>
</inputList>
```

Vypneme Eulationstation a zkontrolujeme automaticky vygenerovaný soubor `/opt/retropie/all/retroarch.cfg`. Je pravděpodobné, že bude obsahovat spoustu balastu. Obnovte proto zálohu a ručně upravte soubor podle vzoru [vzory/retroarch.cfg.all](vzory/retroarch.cfg.all). S důrazem na:
- ovládání:
```
input_player1_a = "alt"
input_player1_b = "ctrl"
input_player1_y = "shift"
input_player1_x = "space"
input_player1_start = "num1"
input_player1_select = "num5"
input_player1_l = "z"
input_player1_r = "x"
input_player1_left = "left"
input_player1_right = "right"
input_player1_up = "up"
input_player1_down = "down"
```
- zapnutí funkce rewind:
```
rewind_enable = "true"
```
- vypnutí hotkey atd:
```
global_core_options = "true"
input_enable_hotkey = "nul"
auto_remaps_enable = "true"
rgui_aspect_ratio_lock = "2"
menu_show_restart_retroarch = "false"
xmb_show_add = "false"
xmb_show_history = "false"
xmb_show_images = "false"
xmb_show_music = "false"
xmb_shadows_enable = "false"
quit_press_twice = "false"
```
V dalším kroku pak nastavíme parametry pro samotné jádro `lr-mame2003-plus`. Tyto parametry se nastavují s souboru `/opt/retropie/arcade/retroarch.cfg`. Stačí upravit podle vzoru [vzory/retroarch.cfg.arcade](vzory/retroarch.cfg.arcade) nebo ručně doplnit tyto řádky:
```
input_remapping_directory = "/opt/retropie/configs/arcade/"

input_enable_hotkey = "c"
input_exit_emulator = "q"
input_menu_toggle = "f1"
input_pause_toggle = "right"
input_rewind = "alt"
input_toggle_slowmotion = "left"

#include "/opt/retropie/configs/all/retroarch.cfg"

```
Jak bylo psáno na začátku, zapojil jsem 1 tlačítko navíc, které mi slouží jako hotkey. Po stlačení kombinace hotkey a příslušné klávesy se provede jedna z akcí:
```
hotkey+q - ukončení hry
hotkey+F1 - konfigurační okno retroarch, zde se pak dají nastavit vychytávky pro jednotlivé hry zvlášť
hotkey+šipka vpravo - pauza
hotkey+šipka vlevo - zpomalení hry, dobré pro trénink obtížných pasáží
hotkey+alt - rewind, vrátí postup hrou v čase, geniální
```

Jen na ukázku balast který vytvoří konfigurátot, když ho necháte dělat svou práci - [vzory/retroarch.cfg.arcade.scrap](vzory/retroarch.cfg.arcade.scrap) :).

Poslední věc, než znovu spustíme emulationstation, je nastavení pro jádro, které umožní kromě vstupů od virtuálního ovladače Retropad i simultánní vstup z klávesnice. Není to sice nutné pokud pečlivě namapujete klávesy, ale pokud chceme naplno využít možnosti Retroarchu, tak je to myslím nezbytné. Vše se nastavuje v souboru `/opt/retropie/all/retroarch-core-options.cfg` a jedná se o přidání následujícího řádku:
```
mame2003-plus_input_interface = "simultaneous"
```
## ToDo
- mapování hotkeys bez pomoci klávesnice
- shaders
- 2 player
- doplnit odkazy na jednotlivé sekce v originální dokumentaci
## ToDo
26.5.2021 změna tlačítka pro EXIT z ESC na Q. Z důvodu, že J-PAC neposílá ESC sekvenci při aktivním hotkey tlačítku.