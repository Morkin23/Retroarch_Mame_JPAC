# Retroarch_Mame_JPAC
Cílem je zaznamena a uchovat pro pzdější použití vše co jsem se naučil při konfiguraci emulátoru herního automatu.
## Hardware
- arcade automat libovolné konfigurace. V mém případě to je JAMMA konektor, 2xjoystick, 2x šest tlačítek, P1 Start, P2 Start a jedno tlačítko navíc.
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
2. odinstalace  Retropie
![IMAGE](1_retropie_setup.PNG)