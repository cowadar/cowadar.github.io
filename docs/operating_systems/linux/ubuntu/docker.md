# Docker Engine installeren op Ubuntu

Om aan de slag te gaan met Docker Engine op Ubuntu, moet u ervoor zorgen dat u [aan de vereisten voldoet](https://docs.docker.com/engine/install/ubuntu/#prerequisites) en vervolgens [Docker installeert](https://docs.docker.com/engine/install/ubuntu/#installation-methods) .

## Vereisten [🔗](https://docs.docker.com/engine/install/ubuntu/#prerequisites)

### OS-vereisten [🔗](https://docs.docker.com/engine/install/ubuntu/#os-requirements)

Om Docker Engine te installeren, hebt u de 64-bits versie van een van deze Ubuntu-versies nodig:

- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Impish 21.10
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)

Docker Engine wordt ondersteund op `x86_64`(of `amd64`), `armhf`, `arm64`, en `s390x`architecturen.

### Oude versies verwijderen [🔗](https://docs.docker.com/engine/install/ubuntu/#uninstall-old-versions)

Oudere versies van Docker werden `docker`, `docker.io`, of `docker-engine`. Als deze zijn geïnstalleerd, verwijder ze dan:

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Het is oké als `apt-get`wordt gemeld dat geen van deze pakketten is geïnstalleerd.

De inhoud van `/var/lib/docker/`, inclusief afbeeldingen, containers, volumes en netwerken, blijft behouden. Als u uw bestaande gegevens niet hoeft op te slaan en met een schone installatie wilt beginnen, raadpleegt u het gedeelte [Docker Engine verwijderen](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine) onderaan deze pagina.

## Installatiemethoden [\_](https://docs.docker.com/engine/install/ubuntu/#installation-methods)

U kunt Docker Engine op verschillende manieren installeren, afhankelijk van uw behoeften:

- De meeste gebruikers [stellen de repositories van Docker in](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) en installeren van daaruit, voor eenvoudige installatie en upgradetaken. Dit is de aanbevolen aanpak.

- Sommige gebruikers downloaden het DEB-pakket en [installeren het handmatig](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) en beheren upgrades volledig handmatig. Dit is handig in situaties zoals het installeren van Docker op air-gapped systemen zonder toegang tot internet.

- In test- en ontwikkelomgevingen kiezen sommige gebruikers ervoor om geautomatiseerde [gemaksscripts](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script) te gebruiken om Docker te installeren.

### Installeren met behulp van de repository [🔗](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

Voordat u Docker Engine voor de eerste keer op een nieuwe hostcomputer installeert, moet u de Docker-repository instellen. Daarna kunt u Docker installeren en bijwerken vanuit de repository.

#### De opslagplaats instellen

1. Werk de `apt`pakketindex bij en installeer pakketten om het `apt`gebruik van een repository via HTTPS toe te staan:

    ```bash
    $ sudo apt-get update

    $ sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    ```

2. Voeg de officiële GPG-sleutel van Docker toe:

    ```bash
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```

3. Gebruik de volgende opdracht om de repository in te stellen:

    ```bash
    $ echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

#### Docker Engine installeren

1. Werk de `apt`pakketindex bij en installeer de _nieuwste versie_ van Docker Engine, containerd en Docker Compose, of ga naar de volgende stap om een specifieke versie te installeren:

    ```bash
     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```

    > Ontvangt u een GPG-fout tijdens het hardlopen `apt-get update`?
    >
    > Uw standaard umask is mogelijk niet correct ingesteld, waardoor het openbare sleutelbestand voor de repo niet wordt gedetecteerd. Voer de volgende opdracht uit en probeer vervolgens uw repo opnieuw bij te werken: `sudo chmod a+r /etc/apt/keyrings/docker.gpg`.

2. Om een _specifieke versie_ van Docker Engine te installeren, vermeldt u de beschikbare versies in de repo, selecteert u en installeert u:

    a. Maak een lijst van de beschikbare versies in uw repo:

    ```bash
    $ apt-cache madison docker-ce

    docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    ```

    b. Installeer een specifieke versie met behulp van de versiereeks uit de tweede kolom, bijvoorbeeld `5:20.10.16~3-0~ubuntu-jammy`.

    ```bash
    sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
    ```

3. Controleer of Docker Engine correct is geïnstalleerd door de `hello-world` afbeelding uit te voeren.

    ```bash
    sudo service docker start
    sudo docker run hello-world
    ```

    Met deze opdracht wordt een testimage gedownload en uitgevoerd in een container. Wanneer de container wordt uitgevoerd, drukt deze een bericht af en wordt afgesloten.

Docker Engine is geïnstalleerd en draait. De `docker`groep is gemaakt, maar er worden geen gebruikers aan toegevoegd. U moet gebruiken `sudo`om Docker-opdrachten uit te voeren. Ga door naar [Linux na de installatie](https://docs.docker.com/engine/install/linux-postinstall/) om niet-bevoegde gebruikers toe te staan Docker-opdrachten uit te voeren en voor andere optionele configuratiestappen.

#### Docker-engine upgraden

Om Docker Engine te upgraden, voert u eerst uit `sudo apt-get update`, volgt u de [installatie-instructies](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) en kiest u de nieuwe versie die u wilt installeren.

### Installeren vanuit een [pakket](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package)

Als u Docker's repository niet kunt gebruiken om Docker Engine te installeren, kunt u het `.deb`bestand voor uw release downloaden en handmatig installeren. U moet elke keer dat u Docker wilt upgraden een nieuw bestand downloaden.

1. Ga naar [`https://download.docker.com/linux/ubuntu/dists/`](https://download.docker.com/linux/ubuntu/dists/), kies uw Ubuntu-versie, blader vervolgens naar `pool/stable/`, kies `amd64`, `armhf`, `arm64`of `s390x`, en download het `.deb`bestand voor de Docker Engine-versie die u wilt installeren.

2. Installeer Docker Engine door het onderstaande pad te wijzigen in het pad waar u het Docker-pakket hebt gedownload.

    ```bash
    sudo dpkg -i /path/to/package.deb
    ```

    De Docker-daemon start automatisch.

3. Controleer of Docker Engine correct is geïnstalleerd door de `hello-world` afbeelding uit te voeren.

    ```bash
    sudo docker run hello-world
    ```

    Met deze opdracht wordt een testimage gedownload en uitgevoerd in een container. Wanneer de container wordt uitgevoerd, drukt deze een bericht af en wordt afgesloten.

Docker Engine is geïnstalleerd en draait. De `docker`groep is gemaakt, maar er worden geen gebruikers aan toegevoegd. U moet gebruiken `sudo`om Docker-opdrachten uit te voeren. Ga door naar [Post-installatiestappen voor Linux](https://docs.docker.com/engine/install/linux-postinstall/) om niet-bevoegde gebruikers toe te staan Docker-opdrachten uit te voeren en voor andere optionele configuratiestappen.

#### Docker-engine upgraden

Om Docker Engine te upgraden, downloadt u het nieuwere pakketbestand en herhaalt u de [installatieprocedure](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) , waarbij u naar het nieuwe bestand wijst.

### Installeren met behulp van het [gemaksscript 🔗](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)

Docker biedt een gemaksscript op [get.docker.com](https://get.docker.com/) om Docker snel en niet-interactief in ontwikkelomgevingen te installeren. Het gemaksscript wordt niet aanbevolen voor productieomgevingen, maar kan als voorbeeld worden gebruikt om een inrichtingsscript te maken dat is afgestemd op uw behoeften. Raadpleeg ook de [installatie met behulp van de repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) -stappen voor meer informatie over installatiestappen om te installeren met behulp van de pakketrepository. De broncode voor het script is open source en is te vinden in de [`docker-install`repository op GitHub](https://github.com/docker/docker-install) .

Onderzoek altijd scripts die van internet zijn gedownload voordat u ze lokaal uitvoert. Maak uzelf vóór de installatie vertrouwd met de mogelijke risico's en beperkingen van het gemaksscript:

- Het script vereist `root`of `sudo`privileges om te worden uitgevoerd.
- Het script probeert uw Linux-distributie en -versie te detecteren en uw pakketbeheersysteem voor u te configureren, en u kunt de meeste installatieparameters niet aanpassen.
- Het script installeert afhankelijkheden en aanbevelingen zonder om bevestiging te vragen. Dit kan een groot aantal pakketten installeren, afhankelijk van de huidige configuratie van uw hostmachine.
- Het script installeert standaard de nieuwste stabiele release van Docker, containerd en runc. Als u dit script gebruikt om een machine in te richten, kan dit leiden tot onverwachte grote versie-upgrades van Docker. Test (grote) upgrades altijd in een testomgeving voordat u ze implementeert op uw productiesystemen.
- Het script is niet ontworpen om een bestaande Docker-installatie te upgraden. Wanneer u het script gebruikt om een bestaande installatie bij te werken, worden afhankelijkheden mogelijk niet bijgewerkt naar de verwachte versie, waardoor verouderde versies worden gebruikt.

> Tip: bekijk scriptstappen voordat u begint
>
> U kunt het script uitvoeren met de `DRY_RUN=1`optie om te leren welke stappen het script tijdens de installatie zal uitvoeren:
>
> ```
> $ curl -fsSL https://get.docker.com -o get-docker.sh
> $ DRY_RUN=1 sh ./get-docker.sh
> ```

In dit voorbeeld wordt het script gedownload van [get.docker.com](https://get.docker.com/) en uitgevoerd om de nieuwste stabiele release van Docker op Linux te installeren:

```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>
```

Docker is geïnstalleerd. De `docker`service start automatisch op op Debian gebaseerde distributies. Op `RPM`gebaseerde distributies, zoals CentOS, Fedora, RHEL of SLES, moet je het handmatig starten met het juiste commando `systemctl`of . `service`Zoals het bericht aangeeft, kunnen niet-rootgebruikers standaard geen Docker-opdrachten uitvoeren.

> **Docker gebruiken als een niet-bevoorrechte gebruiker, of installeren in rootless-modus?**
>
> Het installatiescript vereist `root`of `sudo`privileges om Docker te installeren en te gebruiken. Als je niet-rootgebruikers toegang wilt geven tot Docker, raadpleeg dan de [stappen na de installatie voor Linux](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) . Docker kan ook zonder `root`privileges worden geïnstalleerd of worden geconfigureerd om in de rootless-modus te draaien. Voor instructies over het uitvoeren van Docker in rootless-modus, raadpleeg [de Docker-daemon uitvoeren als een niet-rootgebruiker (rootless-modus)](https://docs.docker.com/engine/security/rootless/) .

#### Pre-releases installeren

Docker biedt ook een gemaksscript op [test.docker.com](https://test.docker.com/) om pre-releases van Docker op Linux te installeren. Dit script is gelijk aan het script op `get.docker.com`, maar configureert uw pakketbeheerder om het "test" -kanaal van onze pakketrepository in te schakelen, die zowel stabiele als pre-releases (bètaversies, release-kandidaten) van Docker bevat. Gebruik dit script om vroegtijdig toegang te krijgen tot nieuwe releases en om ze in een testomgeving te evalueren voordat ze als stabiel worden vrijgegeven.

Om de nieuwste versie van Docker op Linux te installeren vanaf het "test" -kanaal, voer je uit:

```
$ curl -fsSL https://test.docker.com -o test-docker.sh
$ sudo sh test-docker.sh
<...>
```

#### Upgrade Docker na gebruik van het gemaksscript

If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There is no advantage to re-running the convenience script, and it can cause issues if it attempts to re-add repositories which have already been added to the host machine.

## Uninstall Docker Engine[🔗](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

1. Uninstall the Docker Engine, CLI, Containerd, and Docker Compose packages:

    ```
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

    ```
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```

You must delete any edited configuration files manually.

## Next steps[🔗](https://docs.docker.com/engine/install/ubuntu/#next-steps)

- Continue to [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/).
- Review the topics in [Develop with Docker](https://docs.docker.com/develop/) to learn how to build new applications using Docker.

[vereisten](https://docs.docker.com/search/?q=requirements) , [apt](https://docs.docker.com/search/?q=apt) , [installatie](https://docs.docker.com/search/?q=installation) , [ubuntu](https://docs.docker.com/search/?q=ubuntu) , [installeren](https://docs.docker.com/search/?q=install) , [verwijderen](https://docs.docker.com/search/?q=uninstall) , [upgraden](https://docs.docker.com/search/?q=upgrade) , [bijwerken](https://docs.docker.com/search/?q=update)
