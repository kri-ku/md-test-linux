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

Tämä komento oli itselleni uusi tuttavuus, joten googlasin sen. ([git blame](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame#:~:text=The%20git%20blame%20command%20is%20used%20to%20examine%20the%20contents,author%20of%20the%20modifications%20was.&text=git%20blame%20and%20git%20log,history%20of%20a%20file's%20contents.))
Komento voidaan kohdistaa johonkin tiettyyn tiedostoon. Lopputuloksena tulostuvat muutokset riveittäin, niiden tekijät ja ajat. Ensimmäisenä tässäkin tulostuu commitin yksilöivä
koodi, ja tieto siitä, onko muutos paikallinen vai tehty yhteiseen repositorioon.

![git blame](/pictures/3.png)

```
```

### e) Tyhmä muutos gittiin
### f) Uusi Salt-moduli 

----

Lähteet:
<http://terokarvinen.com/2016/publish-your-project-with-github/>,

