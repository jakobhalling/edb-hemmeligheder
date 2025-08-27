# Værktøjer til fejlfinding af driver- og systemnedbrud

## Display Driver Uninstaller (DDU)

**Hvad det er:**\
Display Driver Uninstaller er et værktøj til fuldstændigt at fjerne
grafikdrivere (NVIDIA, AMD, Intel) fra systemet.\
Det bruges typisk, når almindelig afinstallation ikke løser problemer,
eller hvis gamle driverrester skaber konflikter.

**Hvordan man bruger det:**\
1. Download DDU fra det officielle site (Wagnardsoft).\
2. Start Windows i **Fejlsikret tilstand** for bedst resultat.\
3. Kør DDU og vælg producenten af din nuværende driver (f.eks. NVIDIA).\
4. Vælg "Clean and restart" for at afinstallere driveren helt og
genstarte systemet.\
5. Installer derefter den nyeste driver fra producentens officielle
hjemmeside.

------------------------------------------------------------------------

## WhoCrashed

**Hvad det er:**\
WhoCrashed analyserer **Windows crash dump-filer** (minidumps), som
bliver genereret ved blå skærm (BSOD).\
Programmet identificerer mulige årsager til nedbruddet, ofte relateret
til drivere eller hardwareproblemer.

**Hvordan man bruger det:**\
1. Download og installer WhoCrashed fra Resplendence Software.\
2. Start programmet og klik på **Analyze**.\
3. Programmet scanner dump-filer og genererer en **rapport** med
detaljer om årsagen til nedbruddet.\
4. Rapporten kan kopieres eller gemmes og sendes til support (f.eks. MM
Vision) for videre fejlsøgning.

------------------------------------------------------------------------

## Typisk workflow

1.  Brug **DDU** til at afinstallere eksisterende grafikdrivere
    fuldstændigt.\
2.  Installer den nyeste driver fra din GPU-producent.\
3.  Hvis nedbrud fortsætter, kør **WhoCrashed** for at analysere
    dump-filer.\
4.  Gem rapporten og send den til support for videre hjælp.
