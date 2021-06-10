![](assets/stream-logo.svg)

# Stream.cz Import API Documentation

Welcome to the Stream.cz Import API documentation where you will find all necessary
information to submit data to stream.cz videoportal.

## REST Import API

The documentation is available in the `openapi.json` as an OpenAPI v3 JSON definition.
The documentation can be viewed in any OpenAPI-capable viewer such as ReDoc or Swagger UI.

## XML Feed

The XML file will be split into two parts and subsequent calls. In the first one, we need
to submit a tag to the videoportal in order to file an episode under it. The second one is
to create an episode under the previously created tag.

### Tag's XML Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tags>

    <tag id='10010412' type='show' name='A DOST!'>
    <!-- tag - content's tag,
    name: name of the tag, recommended length 15 characters for `show` and 27 chars for `playlist`
    type: type = 'channel' / 'show' / 'season' / 'playlist' (not nullable, cannot be empty)
          channel (kanál) - set of shows and playlists (e.g. channel "Zábava")
          show (pořad) - contains only series/seasons or episodes
          season - a serie of a show (filed only under a show and can contain only episodes)
          playlist - a set of episodes which are not filed under a show (set of episodes)
    id: tag's ID (int/string), determined on your side and in case of using the Import API
    you are working with this ID
    -->
        <parents>
            <parent id='1000124' />
            <parent id='12142124' />
            <!--
            Parents: hierarchically ordered tags, usable in a combination of a show and season.
            E.g. I have two tags, one is a show and the other one is a season. The show is
            a parent to the season which contains episodes and bonuses.

            In case of no parent, just empty `<parents></parents>` is allowed. Tags without
            a parent (orphaned tags) will be filed under the service tag which is associated
            with you automatically. A season doesn't have to have a parent.

            Warning: feed of tags must go chronologically from the highest level
            (channel - show - season - playlist) otherwise, there can be a discrepancy between
            the tag's parents which don't exist yet.
            -->
        </parents>
        <perex>
          Jan Tuna vás provede záplavou zboží a ukáže vám, kteří obchodníci z vás jen
          tahají peníze. Testy potravin, produktů a služeb vám prozradí, co je opravdu
          kvalitní a spolehlivé. Řekněte šuntům A DOST!
        </perex>
        <!--
        Perex - description (perex), which is displayed on the show's page. Recommended
        length is 120 characters. Required for type = "show".
        -->
        <origin>https://www.stream.cz/porady/a-dost</origin>
        <!---
        Origin - a link to the original destianation of the content on your side.
        E.g. I am importing the Krimi section from novinky.cz, I will put the link
        https://www.novinky.cz/krimi/. Used for debugging purposes mainly.
        -->
        <imageSquare url="//d11-a.sdn.szn.cz/d_11/c_img_G_J/NpACRC.jpeg" />
        <!-- ImageSquare - Icon of the show -->
        <imageHeader url="//d11-a.sdn.szn.cz/d_11/c_img_G_J/RDKCRB.jpeg" />
        <!-- ImageHeader - Header of the show -->

    </tag>

    <!--
    Here is an example how to create a season to a show.
    -->
    <tag id='2190' type='season' name='Epizody'>
        <parents>
            <parent id='10010412' />
        </parents>
    </tag>
</tags>
```

### Episodes example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<episodes>
    <episode
      id='10018725'
      name='TEASER: Tanec s párky Honzy Tuny'
      publish='1531916949'
      expiration='1532916949'
      expirationHomepage='1533916949'
      productPlacement='1'
      originViews='20700'
    >
    <!--
    id: your own ID of the episode
    name: name of the episode, recommended length is 75 chars
    publish: date of publication as a UNIX timestamp
    expiration: date of expiration (when to remove the video from the site) as a UNIX timestamp
    productPlacement: whether episode contains product placement (0/1)
    expirationHomepage: date of expiration on Seznam.cz Homepage recommending system
    (as UNIX timestamp). Cannot be larger than 2147483647 (year 2038). It is not compulsory,
    if empty the recommending expiration will be done by our internal systems.
    originViews: Number of views that will be added to our own views during import
    (will be shown on the web).
    -->
        <perex>Sledujte pondělní A DOST! o tom, jestli párky v rohlíku vůbec obsahují maso.</perex>
        <!--
        Perex - description (perex), which is displayed on the episode's page.
        The recommended length is 125 characters.
        -->
        <origin>https://www.stream.cz/adost/10018725-teaser-tanec-s-parky-honzy-tuny</origin>
        <!--
        Origin - a link to the original content on your site. Used for debugging.
        -->
        <images>
            <image url='//d11-a.sdn.szn.cz/d_11/c_img_H_J/XV9Fv8.jpeg'>
                <sources>
                    <source label="Routers" url="http://www.reuters.com/"></source>
                </sources>
            </image>
            <!--
            Image
            url: link to the image, when publishing we will resize it and use a cutout in
            16:9 ratio. The link must be without protocol (http, https) and must start with
            just two slashes (//) as seen in the example.

            In case the image is not on Seznam CDN we can't process it.

            source: name of the image's source (e.g. Reuters)
            -->
        </images>
        <video
          duration='37'
          url='https://v11-a.sdn.szn.cz/v_11/vmd/10027773?fl=mdk,f117c301|'
          isLive='0'
        >
            <source label='Reuters' url='http://www.reuters.com' />
        </video>
        <!--
        Video
        duration: duration of the video in seconds
        url: link to the video on Seznam CDN

        source: name of the video's source (e.g. Reuters)
        -->
        <advert>
            <preroll />
            <midroll time='13' />
            <postroll />
            <!--
            Advert types:
            - preroll: ad at the beginning of the video
            - midroll: ad in the video, time is for a timestamp in seconds when the ad can play
            - postroll: ad at the end of the video

            These items are used for controlling the number of ads on the video. In some
            cases, we don't want any ads on the
            video which is achieved by leaving the `<advert>` empty.
            -->
        </advert>
        <inTags>
            <inTag id='2190' />
            <!-- Categorization of an episode to tags. -->
        </inTags>
        <originTag>10010412</originTag>
        <!--
        Direct parent of the episode. If the episode belongs to multiple tags we need to
        show only one on the episode page.
        originTag is not required and if not set the originTag will be automatically assumed.

        Warning: originTag cannot be a "season" tag
        -->
        <geoblocks>
            <geoblock code='cz' permission='allow' />
            <!--
            Block or allow video to be played based on the geographical location of the user.

            code: 2-letter country code ("cz")
            permission: <allow, deny> if we want to block or allow the country
            -->
        </geoblocks>
        <editorTags>
            <editorTag>Tuna</editorTag>
            <editorTag>Testy</editorTag>
            <!--
            Editorial tags from the back office.
            -->
        </editorTags>
        <authors>
            <author>Karel Sestak</author>
        </authors>
    </episode>
    <next url="" />
    <!--
    Path to next XML file to import. Used for pagination. Bear in mind the import can take
    a long time and because of it the paging should be static and the offset constant.
    -->
</episodes>
```
