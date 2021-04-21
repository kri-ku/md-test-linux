# Viikko 3: Versionhallinta

Tämä on kolmas tehtäväpalautukseni kurssille:

Palvelinten hallinta: ICT4TN022-3011,

Opettaja: Tero Karvinen,

<https://terokarvinen.com/2021/configuration-management-systems-palvelinten-hallinta-ict4tn022-spring-2021/>

--------------

Ympäristö:
- Lenovo Thinkpad x270: Intel(R)Core i5-6200U AMD64_x86, 8 Gt, 128 Gt SSD
- Ubuntu 20.04: Tehtävät tehty Virtual Boxilla, jolla debian-live-10.7.0-amd64-xfce+nonfree.iso

---------
Klo 08:45 pe 16.4.2021

Aloitin tehtävät luomalla uuden repositorion GitHubiin, ja kloonaamalla sen koneelleni `$ git clone <repositorion osoite>`. Tämä raportti on kirjoitettu nanolla tuohon kansioon.
Markdown-formaatin muotoilussa olen käyttänyt apuna sivua [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

![Osoitteen löytäminen GitHubista](/pictures/1.png)

### d) Git log, git diff, git blame

#### Git log

Syötin tämän repositorion alla komennon `$ git log`. 

```
commit af22f25aff26ebc7b7c7ce3e1bc94b850fb8c755 (HEAD -> main, origin/main, origin/HEAD)
Author: kristiina kumila <kristiinakumila@gmail.com>
Date:   Fri Apr 16 09:08:04 2021 +0300

    Links added

commit 368fe6b0e4efe49669664fee8e1144ceda9c8246
Author: kristiina kumila <kristiinakumila@gmail.com>
Date:   Fri Apr 16 09:05:23 2021 +0300

    First commit

commit 69b0075256d42faa0cc1ed8c73444462954fcf95
Author: kri-ku <58989427+kri-ku@users.noreply.github.com>
Date:   Fri Apr 16 08:35:02 2021 +0300

    Initial commit
```
Jokainen lisätty commit (muutos, joka on lisätty versionhallintaan) tulostuu terminaaliin.
Muutoksessa on yksilöivä koodi, muutoksen tekijä ja muutospäivämäärä. Myös commit-viesti (muutosta lisättäessä kirjoitettava "mitä on tehty" -rivi) tulostuu. Commit-koodi on tärkeä, sillä sitä hyödynnetään
esimerkiksi kun halutaan palata johonkin aikaisempaan tilaan.

#### Git diff
Komento `$ git diff` vertaa tiedostoni nykyistä tilaa edelliseen committiin. Poistetut tai muokatut rivit tulostuivat punaisella, ja lisäykset vihreällä. Komennolle voidaan antaa
myös erilaisia parametrejä, jotka mahdollistavat esimerkiksi tiettyjen tiedostojen tai committien vertailun.  

![git diff](/pictures/2.png)

#### Git blame

Tämä komento oli itselleni uusi tuttavuus, joten googlasin sen, ([git blame](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame#:~:text=The%20git%20blame%20command%20is%20used%20to%20examine%20the%20contents,author%20of%20the%20modifications%20was.&text=git%20blame%20and%20git%20log,history%20of%20a%20file's%20contents.)).
Komento voidaan kohdistaa johonkin tiettyyn tiedostoon. Lopputuloksena tulostuvat muutokset riveittäin, niiden tekijät ja ajat. Ensimmäisenä tässäkin tulostuu commitin yksilöivä
koodi, ja tieto siitä, onko muutos paikallinen vai tehty yhteiseen repositorioon.

![git blame](/pictures/3.png)

### e) Tyhmä muutos gittiin

Muokkasin tiedostoa README.md, `$ nano README.md`.

![README muokkaus](/pictures/4.png)

Tajusin, että ennen komennon `$ git reset -- hard` kokeilua kannattanee tarkistaa onko kaikki tärkeä tieto jo laitettu versionhallintaan.

`$ git status`

![git status](/pictures/5.png)

Kaikkia kuvia ei oltu vielä lisätty Githubiin. Lisäsin ne komennoilla:

```
$ git add pictures/
$ git commit -m "add pictures"
$ git push

```

Syötin terminaaliin komennon `$ git reset --hard`.

```
Unstaged changes after reset:
M	README.md
 
```

Tiedosto pitää ensin olla versionhallinnassa, että sen voi resetoida aikaisempaan tilaan.

```
$ git add .
$ git reset --hard

	HEAD is now at f21f113 push before trying to reset

```
README.md tiedosto on palautunut muutoksia edeltävään tilaan.

Pidin tauon noin klo 11. Jatkoin taas klo 16.

### f) Uusi Salt-moduli 

Päätin ladata palomuurin, ufw:n. Poistin ensin aikaisemman ufw-latauksen 
koneeltani, ja latasin sen uudeleen.

```
$ sudo apt-get purge ufw
$ sudo apt-get -y install ufw

```
Seuraavaksi tarkoitukseni oli selvittää muuttuneet tiedostot. Ufw:n
asetukset löytyivät kansion /etc/ufw alta.

```
$ cd etc/ufw
$ find -printf "%T+ %p\n"|sort
```
![muuttuneet](/pictures/6.png)

Kuvassa viimeiset kuusi riviä on leimattu tälle päivälle. Avasin palomuurille
 portin 22 ja otin sen käyttöön.

```
$ sudo ufw allow 22/tcp
$ sudo ufw enable

```
Tutkin jälleen muuttuneita tiedostoja.

![muuttuneet2](/pictures/7.png)

Näyttäisi siltä, että viimeksi muuttuneet tiedostot ovat ufw.conf, 
user6.rules, user.rules.

Pidin jälleen tauon noin 17:35.
Aloitin tekemään Salt-modulia. Klo 19.30.

Tein kansion ufw:asetuksille.

```
$ cd /srv/salt
$ mkdir ufw
$ cd ufw
$ sudoedit init.sls

```
```
ufw:
 pkg.installed

```
Lisäsin tilan minioneille komennolla `$ sudo salt '*' state.apply ufw`.

```
kristiina@fishcake:/srv/salt/ufw$ sudo salt '*' state.apply ufw
kristiina:
----------
          ID: ufw
    Function: pkg.installed
      Result: True
     Comment: The following packages were installed/updated: ufw
     Started: 19:40:41.274301
    Duration: 8348.341 ms
     Changes:   
              ----------
              ufw:
                  ----------
                  new:
                      0.36-1
                  old:

Summary for kristiina
------------
Succeeded: 1 (changed=1)
Failed:    0
------------
Total states run:     1
Total run time:   8.348 s

```

Tilan lisääminen näyttäisi toimivan. Komento `$ sudo ufw status` näyttää kuitenkin toistaiseksi palomuurin olevan
pois päältä. 
Tilaan pitää lisätä vielä määritykset ufw:n käynnistämiseksi ja (ainakin) portin 22 avaus liikenteelle.

Helpoin tapa rules-tiedostojen kopioimiseksi minioneille lienee ensin avata portti 22 (`$ sudo ufw allow 22/tcp`)
ja kopioida muuttuneet .rules-päätteiset tiedostot kansioon /srv/salt/ufw.
Eli 

```
$ sudo ufw allow 22/tcp
	Rules updated
	Rules updated (v6)

$ sudo cp /etc/ufw/user.rules /srv/salt/ufw/
$ sudo cp /etc/ufw/user6.rules /srv/salt/ufw/
$ sudo cp /etc/ufw/ufw.conf /srv/salt/ufw/
 
```
Kopion myös ufw.conf-tiedoston viimeisellä rivillä. Jatkoin muokkaamalla tilaa `$ sudoedit /srv/salt/ufw/init.sls`.

```
ufw:
 pkg.installed

/etc/ufw/user.rules:
  file.managed:
    - source: salt://ufw/user.rules        

/etc/ufw/user6.rules:
  file.managed:
    - source: salt://ufw/user6.rules

/etc/ufw/ufw.conf:
  file.managed:
    - source: salt://ufw/ufw.conf   
```

Tilan pitäisi nyt kopioida konfigurointitiedostot minioneille.
Poistin jälleen ufwn `$ sudo apt-get purge ufw`, ja ajoin tilan `$ sudo salt '*' state.apply ufw`.
Tulos oli erikoinen: tiedostoja ei löydy /srv/salt/ufw-kansiosta!

![ufwtila](/pictures/8.png)

Päädyin googlettelemaan ja löysin kurssin aikasemmin tehneen [Hannu Kankkusen blogipostauksen](https://hannukankkunen.wordpress.com/2018/12/10/palvelinten-hallinta-h7/)
 samasta aiheesta. Ilokseni huomasin Kankkusen tehtävän olevan melko samankaltainen omaani verrattuna. En kuitenkaan 
onnistunut löytämään tietoa miksei, tiedostojani löydy. Poistin jällee ufw:n asennuksen, ja muokkasin tilan seuraavanlaiseksi:

```
ufw:
   pkg.installed

'ufw allow 22/tcp':
  cmd.run

/etc/ufw/ufw.conf:
  file.managed:
    - source: salt://ufw/ufw.conf


```

Ajoin tilan onnistuneesti.
![ufw toimii](/pictures/9.png)

Koitin vielä muokata tilan seuraavasti:

```
ufw:
   pkg.installed

'ufw allow 22/tcp':
  cmd.run:
    - creates: /etc/ufw/user.rules, /etc/ufw/user6.rules

/etc/ufw/ufw.conf:
  file.managed:
    - source: salt://ufw/ufw.conf

ufw.service:
  service.running:
    - watch:
      - file: /etc/ufw/user.rules
      - file: /etc/ufw/user6.rules
      - file: /etc/ufw/ufw.conf

```
Poistin jälleen ufw:n asennuksen ja ajoin tilan uudelleen. Viimeinen muokkaus ei kuitenkaan mennyt läpi.

Kello oli tässä vaiheessa noin 21:15. Päätin jatkaa tehtäviä myöhemmin.

Sunnuntaina 18.4.2021 klo 16:45

Tein tämän kohdan aikaisemmin, ja raportoin sen tähän muistinvaraisesti.

Tajusin (ystäväni avustuksella) tutkia kansioiden oikeuksia. Kansiolla user.rules ja user6.rules, ei ollut
tarvittavia oikeuksia ohjelman suorittamiseen. Korjasin tämän komennolla `$ sudo chmod 777 *.rules
`, joka antoi laajat oikeudet kaikille. Oikeudet ovat ehkä liian laajat, mutta toimivat tähän tarkoitukseen. 

Ajettuani tilan komennolla `$ sudo salt '*' state.apply ufw`, vastauksena tuli onnistuminen kaikille tilan kohdille.
Tutkin vielä ufw:n tilaa komennolla `sudo ufw status verbose`, jolloin huomasin ufw:n olevan vielä pois päältä.
Tiedoston /srv/salt/ufw/init.sls lopputulos näyttää seuraavalta: 

```
ufw:
   pkg.installed

/etc/ufw/ufw.conf:
  file.managed:
    - source: salt://ufw/ufw.conf

/etc/ufw/user.rules:
  file.managed:
    - source: salt://ufw/user.rules
    - target: ../user.rules

/etc/ufw/user6.rules:
  file.managed:
    - source: salt://ufw/user6.rules
    - target: ../user6.rules

'ufw enable':
  cmd.run

ufw.service:
  service.running:
    - watch:
      - file: /etc/ufw/user.rules
      - file: /etc/ufw/user6.rules
      - file: /etc/ufw/ufw.conf

```
Pitäisköhän kohtaan "ufw enable" lisätä rivi, jotta ajettaminen tehtäisiin vain, jos ohjelma ei ole jo päällä?

Poistin jälleen ufw:n asennuksen ja ajoin tilan uudelleen.
![ufw lopputulos](/pictures/10.png)
![ufw lopputulos](/pictures/11.png)
![ufw lopputulos](/pictures/12.png)
-----------

EDIT 21.4 ke tuntien jälkeen
Kohtaan cmd.run pitää lisätä, komento, jotta tilasta tulisi idempotentti.
Muokkasin tiedostoa init.sls vielä kerran seuraavaksi:

```
ufw:
   pkg.installed

/etc/ufw/ufw.conf:
  file.managed:
    - source: salt://ufw/ufw.conf

/etc/ufw/user.rules:
  file.managed:
    - source: salt://ufw/user.rules

/etc/ufw/user6.rules:
  file.managed:
    - source: salt://ufw/user6.rules

'ufw enable':
  cmd.run:
    - unless: 'ufw status | grep active'

ufw.service:
  service.running:
    - watch:
      - file: /etc/ufw/user.rules
      - file: /etc/ufw/user6.rules
      - file: /etc/ufw/ufw.conf

```
Tilan ajaminen onnistuu. En ole aivan varma cmd.run-kohdan lisäyksestä. Tarkoituksena olisi siis, että
ufw enable -komento ajetaan, jos palvelu ei ole vielä päällä.
----

Lähteet:
<http://terokarvinen.com/2016/publish-your-project-with-github/>,

<https://terokarvinen.com/2018/apache-user-homepages-automatically-salt-package-file-service-example/>

Lauri Koskinen
