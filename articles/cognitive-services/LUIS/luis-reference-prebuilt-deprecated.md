---
title: Kullanım dışı önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makale, Language Understanding (LUIS) kullanım dışı önceden oluşturulmuş varlık bilgileri içerir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 653fbd65e5ccfbc65781f2332b35d10237f718d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60712789"
---
# <a name="deprecated-prebuilt-entities-in-a-luis-app"></a>Bir LUIS uygulaması kullanım dışı önceden oluşturulmuş varlıklar
Aşağıdaki önceden oluşturulmuş varlıklar kullanım dışıdır ve yeni LUIS uygulamalara eklenemez.

* **DateTime**: Kullanan mevcut LUIS uygulamalar **datetime** geçirilmesi gereken **datetimeV2**, datetime varlığı kullanan önceden mevcut olan uygulamalarda çalışmaya devam etse de.
* **Coğrafya**: Kullanan mevcut LUIS uygulamalar **Coğrafya** aralık 2018'e kadar desteklenir.
* **Ansiklopedi**: Kullanan mevcut LUIS uygulamalar **Ansiklopedi** aralık 2018'e kadar desteklenir.

## <a name="geography-culture"></a>Coğrafya kültürü
**Coğrafya** yalnızca kullanılabilir `en-us` yerel ayar.

#### <a name="3-geography-subtypes"></a>3 Coğrafya alt türleri

Önceden oluşturulmuş varlık   |   Örnek konuşma   |   JSON
------|------|------|
`builtin.geography.city`   |  `seattle`    |`{ "type": "builtin.geography.city", "entity": "seattle" }`|
`builtin.geography.city`   |  `paris`    |`{ "type": "builtin.geography.city", "entity": "paris" }`|
`builtin.geography.country`|  `australia`    |`{ "type": "builtin.geography.country", "entity": "australia" }`|
`builtin.geography.country`|  `japan`    |`{ "type": "builtin.geography.country", "entity": "japan" }`|
`builtin.geography.pointOfInterest`   |   `amazon river` |`{ "type": "builtin.geography.pointOfInterest", "entity": "amazon river" }`|
`builtin.geography.pointOfInterest`   |   `sahara desert`|`{ "type": "builtin.geography.pointOfInterest", "entity": "sahara desert" }`|

## <a name="encyclopedia-culture"></a>Ansiklopedi kültürü
**Ansiklopedi** yalnızca kullanılabilir `en-US` yerel ayar.

#### <a name="encyclopedia-subtypes"></a>Ansiklopedi alt türleri
Ansiklopedi yerleşik varlık aşağıdaki tabloda 100'den fazla alt türlerini içerir: Ayrıca, ansiklopedi varlıklar, genellikle birden çok türlerine eşlenir. Örneğin, sorgu Ronald Reagan verir: 

```json
{
      "entity": "ronald reagan",
      "type": "builtin.encyclopedia.people.person"
    },
    {
      "entity": "ronald reagan",
      "type": "builtin.encyclopedia.film.actor"
    },
    {
      "entity": "ronald reagan",
      "type": "builtin.encyclopedia.government.us_president"
    },
    {
      "entity": "ronald reagan",
      "type": "builtin.encyclopedia.book.author"
    }
 ```


Önceden oluşturulmuş varlık   |   Önceden oluşturulmuş varlık (alt türleri)   |   Örnek konuşma
------|------|------|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.people.person`| `bryan adams` |
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.film.producer`| `walt disney` |
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.film.cinematographer`| `adam greenberg`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.royalty.monarch`| `elizabeth ii`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.film.director`| `steven spielberg`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.film.writer`| `alfred hitchcock`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.film.actor`| `robert de niro`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.martial_arts.martial_artist`| `bruce lee`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.architecture.architect`| `james gallier`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.geography.mountaineer`| `jean couzy`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.celebrities.celebrity`| `angelina jolie`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.music.musician`| `bob dylan`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.soccer.player`| `diego maradona`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.baseball.player`| `babe ruth`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.basketball.player`| `heiko schaffartzik`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.olympics.athlete`| `andre agassi`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.basketball.coach`| `bob huggins`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.american_football.coach`| `james franklin`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.cricket.coach`| `andy flower`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.ice_hockey.coach`| `david quinn`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.ice_hockey.player`| `vincent lecavalier`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.government.politician`| `harold nicolson`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.government.us_president`| `barack obama`|
`builtin.encyclopedia.people.person`| `builtin.encyclopedia.government.us_vice_president`| `dick cheney`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.organization.organization`| `united nations`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.sports.league`| `american league`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.ice_hockey.conference`| `western hockey league`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.baseball.division`| `american league east`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.baseball.league`| `major league baseball`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.basketball.conference`| `national basketball league`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.basketball.division`| `pacific division`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.soccer.league`| `premier league`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.american_football.division`| `afc north`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.broadcast.broadcast`| `nebraska educational telecommunications`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.broadcast.tv_station`| `abc`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.broadcast.tv_channel`| `cnbc world`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.broadcast.radio_station`| `bbc radio 1`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.business.operation`| `bank of china`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.music.record_label`| `pixar`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.aviation.airline`| `air france`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.automotive.company`| `general motors`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.music.musical_instrument_company`| `gibson guitar corporation` |
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.tv.network`| `cartoon network`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.education.educational_institution`| `cornwall hill college` |
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.education.school`| `boston arts academy`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.education.university`| `johns hopkins university`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.sports.team`| `united states national handball team`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.basketball.team`| `chicago bulls`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.sports.professional_sports_team`| `boston celtics`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.cricket.team`| `mumbai indians`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.baseball.team`| `houston astros`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.american_football.team`| `green bay packers`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.ice_hockey.team`| `hamilton bulldogs`|
`builtin.encyclopedia.organization.organization`| `builtin.encyclopedia.soccer.team`| `fc bayern munich`|
`builtin.encyclopedia.organization.organization |builtin.encyclopedia.government.political_party|pertubuhan kebangsaan melayu singapura`|
`builtin.encyclopedia.time.event`| `builtin.encyclopedia.time.event`| `1740 batavia massacre`|
`builtin.encyclopedia.time.event`| `builtin.encyclopedia.sports.championship_event`| `super bowl xxxix`|
`builtin.encyclopedia.time.event`| `builtin.encyclopedia.award.competition`| `eurovision song contest 2003`|
`builtin.encyclopedia.tv.series_episode`| `builtin.encyclopedia.tv.series_episode`| `the magnificent seven`|
`builtin.encyclopedia.tv.series_episode`| `builtin.encyclopedia.tv.multipart_tv_episode`| `the deadly assassin`|
`builtin.encyclopedia.commerce.consumer_product`| `builtin.encyclopedia.commerce.consumer_product`| `nokia lumia 620`|
`builtin.encyclopedia.commerce.consumer_product`| `builtin.encyclopedia.music.album`| `dance pool`|
`builtin.encyclopedia.commerce.consumer_product`| `builtin.encyclopedia.automotive.model`| `pontiac fiero`|
`builtin.encyclopedia.commerce.consumer_product`| `builtin.encyclopedia.computer.computer`| `toshiba satellite`|
`builtin.encyclopedia.commerce.consumer_product`| `builtin.encyclopedia.computer.web_browser`| `internet explorer`|
`builtin.encyclopedia.commerce.brand`| `builtin.encyclopedia.commerce.brand`| `diet coke`|
`builtin.encyclopedia.commerce.brand`| `builtin.encyclopedia.automotive.make`| `chrysler`|
`builtin.encyclopedia.music.artist`| `builtin.encyclopedia.music.artist`| `michael jackson`|
`builtin.encyclopedia.music.artist`| `builtin.encyclopedia.music.group`| `the yardbirds`|
`builtin.encyclopedia.music.music_video`| `builtin.encyclopedia.music.music_video`| `the beatles anthology`|
`builtin.encyclopedia.theater.play`| `builtin.encyclopedia.theater.play`| `camelot`|
`builtin.encyclopedia.sports.fight_song`| `builtin.encyclopedia.sports.fight_song`| `the cougar song`|
`builtin.encyclopedia.film.series`| `builtin.encyclopedia.film.series`| `the twilight saga`|
`builtin.encyclopedia.tv.program`| `builtin.encyclopedia.tv.program`| `late night with david letterman`|
`builtin.encyclopedia.radio.radio_program`| `builtin.encyclopedia.radio.radio_program`| `grand ole opry`|
`builtin.encyclopedia.film.film`| `builtin.encyclopedia.film.film`| `alice in wonderland`|
`builtin.encyclopedia.cricket.tournament`| `builtin.encyclopedia.cricket.tournament`| `cricket world cup`|
`builtin.encyclopedia.government.government`| `builtin.encyclopedia.government.government`| `european commission`|
`builtin.encyclopedia.sports.team_owner`| `builtin.encyclopedia.sports.team_owner`| `bob castellini`|
`builtin.encyclopedia.music.genre`| `builtin.encyclopedia.music.genre`| `eastern europe`|
`builtin.encyclopedia.ice_hockey.division`| `builtin.encyclopedia.ice_hockey.division`| `hockeyallsvenskan`|
`builtin.encyclopedia.architecture.style`| `builtin.encyclopedia.architecture.style`| `spanish colonial revival architecture`|
`builtin.encyclopedia.broadcast.producer`| `builtin.encyclopedia.broadcast.producer`| `columbia tristar television`|
`builtin.encyclopedia.book.author`| `builtin.encyclopedia.book.author`| `adam maxwell`|
`builtin.encyclopedia.religion.founding_figur`| `builtin.encyclopedia.religion.founding_figur`| `gautama buddha`|
`builtin.encyclopedia.martial_arts.martial_art`| `builtin.encyclopedia.martial_arts.martial_art`| `american kenpo`|
`builtin.encyclopedia.sports.school`| `builtin.encyclopedia.sports.school`| `yale university`|
`builtin.encyclopedia.business.product_line`| `builtin.encyclopedia.business.product_line`| `canon powershot`|
`builtin.encyclopedia.internet.website`| `builtin.encyclopedia.internet.website`| `bing`|
`builtin.encyclopedia.time.holiday`| `builtin.encyclopedia.time.holiday`| `easter`|
`builtin.encyclopedia.food.candy_bar`| `builtin.encyclopedia.food.candy_bar`| `cadbury dairy milk`|
`builtin.encyclopedia.finance.stock_exchange`| `builtin.encyclopedia.finance.stock_exchange`| `tokyo stock exchange`|
`builtin.encyclopedia.film.festival`| `builtin.encyclopedia.film.festival`| `berlin international film festival`|

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [boyut](luis-reference-prebuilt-dimension.md), [e-posta](luis-reference-prebuilt-email.md) varlıkları ve [numarası](luis-reference-prebuilt-number.md). 

