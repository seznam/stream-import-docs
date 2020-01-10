XML bude rozdeleno do dvou volani.

V jednom si nacteme vsechny tagy a jejich strukturu.

Ve druhem volani si nacteme veskere epizody. Ty v sobe budou mit vazbu na dane tagy.


Tagy:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<tags>
 
 <tag id='10010412' type='show' name='A DOST!'> 
  <!-- tag - Štítek obsahu, 
   name: název štítku, doporučená délka je 15 znaků v případě type='show', v případě type='playlist' je to 27 znaků
   type: type = 'channel' / 'show' / 'season' / 'playlist' (nemůže být prázdný) 
       štítek obsahu označený jako:
          show je pořad (může obsahovat pouze série a epizody)
          season je série pořadu (může být pouze v pořadu a obsahuje pouze epizody)
          playlist je skupina videí, které nejsou zařazeny do pořadu (pořad bez sérií v podstatě)
          channel je kanál neboli soubor pořadů a playlistů (Například kanál Zábava obsahující vtipné pořady a playlisty)  
   id: ID štítku (int/string), určujete si sami, v případě použití import-api pracujete s obsahem na základě VAŠEHO id, nikoliv našeho
  --->
  <parents>
   <parent id='1000124'/>
   <parent id='12142124'/>
  <!---
  Parents: jsou hierarchicky nadřazené štítky, použitelné v kombinaci pořad a série, tzn. mám dva tagy, jeden je pořad, druhý je série. U série je veden jako parent pořad, ke kterému série (a bonusy) patři.


  Pokud nemá parenta, nechávám prázdné <parents> </parents>, tagy bez parenta budou zařazeny do tagu služby (ten neposílate, je automaticky vytvořen u nás), Série nesmí být bez parenta

  Upozornění: Feed tagů musí jít od těch nejvyšší vrstvy po nejnižší (Kanály - pořady - série - playlisty) jinak by se mohlo stát, že se bude nejprve importovat tag s parent ID tagu, který u nás ještě nemáme.
  --->
  </parents>
  <perex>Jan Tuna vás provede záplavou zboží a ukáže vám, kteří obchodníci z vás jen tahají peníze. Testy potravin, produktů a služeb vám prozradí, co je opravdu kvalitní a spolehlivé. Řekněte šuntům A DOST!</perex>
  <!---
  Perex - perex, který se zobrazí na Streamu u pořadu. Doporučená délka je 120 znaků. !!! Je vyžadováno v případě tag.type = "show". !!!
  --->
  <origin>https://www.stream.cz/porady/a-dost</origin>
  <!---
   Origin - odkaz na původní destinaci obsahu na vaší straně, například importuji rubriku krimi, zde dávám odkaz: https://www.novinky.cz/krimi/. Slouží k našemu debugu, v budoucnu může být i produktově využit k prokliku na obsah.
  --->
  <imageSquare url="//d11-a.sdn.szn.cz/d_11/c_img_G_J/NpACRC.jpeg"/>
  <!--- ImageSquare - Ikonka pořadu --->
  <imageHeader url="//d11-a.sdn.szn.cz/d_11/c_img_G_J/RDKCRB.jpeg"/>
  <!--- ImageHeader - Hlavička pořadu --->

 </tag>

 <!---
    Zde jen ukázka, jak založit sérii k pořadu.
 --->
 <tag id='2190' type='season' name='Epizody'>
  <parents>
   <parent id='10010412'/> 
  </parents>
 </tag>
</tags>



```
Epizody:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<episodes>
  <episode id='10018725' name='TEASER: Tanec s párky Honzy Tuny' publish='1531916949' expiration='1532916949' expirationHomepage='1533916949' productPlacement='1' originViews='20700' >
  <!---
    id: vaše id epizody 
    name: název epizody, doporučená délka je 75 znaků.
    publish: datum publikace jako unix timestamp
    expiration: datum expirace jako unix timestamp, znamená SKUTEČNÉ smazání ze systému (ne deleted = 1, ale DELETE).
    productPlacement: zda epizoda obsahuje productPlacement (0/1)
    expirationHomepage: datum expirace doporučování na HP jako unix timestamp, do té doby se bude epizoda posílat i do homepage boxíku. Nesmí být větší než 2147483647 (rok 2038). Atribut je nepovinný, pokud nebude vyplněn nebo bude vyplněn špatně, bude expiraci určovat doporučování.
    originViews: Počet shlédnutí. Bude přičten k shlédnutí na Videoportále
  --->
  <perex>Sledujte pondělní A DOST! o tom, jestli párky v rohlíku vůbec obsahují maso.</perex>
  <!---
      Perex - perex, který se zobrazí na Streamu u epizody, doporučená délka je 125 znaků.
  --->
  <origin>https://www.stream.cz/adost/10018725-teaser-tanec-s-parky-honzy-tuny</origin>
  <!---
      Origin - odkaz na původní destinaci obsahu na vaší straně, například adresa konkrétní epizody na původní službě. Slouží k debugu.
  --->
  <images>
    <image url='//d11-a.sdn.szn.cz/d_11/c_img_H_J/XV9Fv8.jpeg'>
       <sources>
          <source label="Routers" url="http://www.reuters.com/">
       </sources>
    <image/>
    <!---
     Image
         url: link na obrázek, při výdeji bude použito zmenšení a výřez v poměru 16:9. Na Streamu bude obrázek použit ve dvou velikostech se stejným poměrem.
              link musí být bez http/https a musí mít na začátku dvě lomítka (viz vzor výše)


            V případě, že obrázek nebude v CDN Seznam.cz (automaticky zdetekujeme), bude nahrán do ní a použit výdej z ní.

         source: název zdroje (např. Reuters)
                 (Momentálně předpokládáme, že zdroj je maximálně jeden)
    --->
  </images>
  <video duration='37' url='https://v11-a.sdn.szn.cz/v_11/vmd/10027773?fl=mdk,f117c301|' isLive='0'>
    <source label='Reuters' url='http://www.reuters.com'/>
  </video>
  <!---
      Video
          duration: délka v sekundách
          url: link videa, v případě, že video nebude v CDN, bude založeno nahrání videa do CDN později.

         source: název zdroje (např. Reuters)
                 (Momentálně předpokládáme, že zdroj je maximálně jeden)
  --->
  <advert >
   <preroll />
   <midroll time='13' />
   <postroll />
   <!---
       Advert 
           preroll značí, zda se může přehrát reklama na začátku
           midroll značí, zda a kde (označeno jako time v sekundách) se může hrát midroll
           postroll značí, zda se může přehrát reklama na konci

       Časy midrollů slouží k zobrazení reklamy ve vhodném okamžiku dle definice služby.

       Všechny tyto položky jsou zde primárně z důvodu možnosti označit video, u kterého je nevhodné
       přehrávat reklamu z etických důvodu atd. vždy podléha kontrole. V případě, že nechceme u videa
       vůbec přehrávat reklamu, necháváme advert prázdný. 

       V případě, že nechceme v epizodě reklamu, necháváme advert prázdný.
   --->
  </advert>
  <inTags>
   <inTag id='2190' />
   <!---
   Do jakých štítků tato epizoda patří.
   --->
  </inTags>
  <originTag>10010412</originTag>
   <!---
       Přímí rodič epizody. Pokud epizoda patří pod více tagů, tak v náhledu epizody na webovce budeme mít vždy odkaz jen na jednoho z rodičů.
       Příklad: Máme epizodu z 'A DOST!' v tagu 'Epizody', v tagu 'To nej z A DOST!' a v tagu 'Honza Tuna'. Při zobrazení na webovce chceme, aby tam byl vždy odkaz na pořad 'A DOST!'. Dáme tedy jako originTag ID pořadu 'A DOST!'
       originTag není povinný, pokud jej nezadáte, tak si sami určíme přímého rodiče
       !!! Pozor !!! originTag nesmí být tag s kategorií 'season'
   --->
  <geoblocks>
   <geoblock code='cz' permission='allow' />
   <!---
     Geoblokací buď povolíme (allow) nebo zakážeme (deny) v lokalitách. Například: code=cz a permission=allow znamená, že hrajeme pouze v ČR a nikde jinde.
   --->
  </geoblocks>
  <editorTags>
     <editorTag>Tuna<editorTag/>
     <editorTag>Testy<editorTag/>
     <!---
        Doprovodné editorské štítky vašich redakcí.
     --->
  </editorTags>
  <authors>
       <author>Karel Sestak</author>
  </authors>
 </episode>
 <next url="" /> 
 <!---
     Zde patří link, kde máme pokračovat v importu. Tedy například na /api bude v next url /api/20, čímž načteme další položky. Pozor, import může trvat dlouho, proto je nutné vymyslet limit a offset tak, aby se indexy v průběhu neposunuly.
 --->
</episodes>
```
