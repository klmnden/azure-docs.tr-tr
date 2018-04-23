---
title: Coğrafi kodlama kapsamı Azure konumda temel Hizmetleri | Microsoft Docs
description: Coğrafi kodlama hakkında bilgi edinin Azure konumda kapsamı temel Hizmetleri
services: location-based-services
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: article
ms.service: location-based-services
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: b2ae0f2cb8eba6ca42b3c82458d1fadf4ea93cdf
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-location-based-services---geocoding-coverage"></a>Azure konumu coğrafi kodlama kapsamı Hizmetleri - temel
Azure konum tabanlı hizmetler (lb) adresleri, yerler ve dünyanın coğrafi diğer varlıklar bakarak ayrıntılı coğrafi kodlama bilgi sağlar. Bu, burada bunlar ve doğruluk çeşitli düzeylerde ararken arama hizmeti adresleri bulmak beklediğiniz olamaz bilmek isteyen müşteriler için önemlidir. Bölgeler düşük güvenilirlik kapsamı ile arama yaparken, düşük güvenilirlik arama sonuçları bekleyebilirsiniz. Aşağıdaki tabloda Azure lb arama coğrafi kodlama için kapsam bilgileri sağlar.

| Bölge             | Adres noktaları<sup>1</sup>|Barındırmak numaraları<sup>2</sup>| Sokak düzeyi | Şehir düzeyi | İlgilenilen noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| **Kuzey ve Güney Amerika**                                            |                 |                |              |            |                    |
| Anguilla                                            |                 |                |              |      ✓     |          ✓         |
| Antarktika                                          |                 |                |              |      ✓     |          ✓         |
| Antigua ve Barbuda                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Arjantin                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Aruba                                               |                 |                |              |      ✓     |          ✓         |
| Bahamalar                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Barbados                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Belize                                              |                 |                |              |      ✓     |          ✓         |
| Bermuda                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bolivya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bonaire, Sint Eustatius ve Saba|                 |                |              |      ✓     |          ✓         |
| Brezilya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kanada                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Cayman Adaları                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Şili                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| $Clipperton Adası                                   |                 |                |              |      ✓     |                    |
| Kolombiya                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosta Rika                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Küba                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Dominika                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Dominicana                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Ekvador                                             |                 |                |       ✓      |      ✓     |          ✓         |
| El Salvador                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Falkland Adaları                                    |                 |                |              |      ✓     |          ✓         |
| Fransız Ginesi                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Grenada                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Guadalupe|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Guam                                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Guatemala                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Haiti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Honduras                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Jamaika                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Martinik                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Meksika                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Montserrat                                          |                 |                |              |      ✓     |          ✓         |
| Nikaragua                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Panama                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Paraguay                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Peru                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Porto Riko                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Saint Barthélemy                                    |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Kitts ve Nevis                               |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Lucia                                         |                 |                |              |      ✓     |          ✓         |
| Saint Martin                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Pierre ve Miquelon|                 |                |       ✓      |      ✓     |          ✓         |
| Saint Vincent ve Grenadinler                    |                 |                |              |      ✓     |          ✓         |
| Sint Maarten                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Güney Georgia ve Güney Sandwich Adaları        |                 |                |              |      ✓     |          ✓         |
| Surinam                                            |                 |                |              |      ✓     |          ✓         |
| Trinidad ve Tobago                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Devletler Küçük Harici Adaları                |                 |                |              |      ✓     |          ✓         |
| Amerika Birleşik Devletleri                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Uruguay                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Venezuela                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Þehirlerini - İngiliz                            |                 |                |              |      ✓     |          ✓         |
| Þehirlerini - ABD                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| |                 |                |              |            |                    |
| **Asya Pasifik**                                        |                 |                |              |            |                    |
| Amerikan Samoası                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Avustralya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bangladeş                                          |                 |                |              |      ✓     |          ✓         |
| Butan                                              |                 |                |              |      ✓     |          ✓         |
| Britanya Hint Okyanusu Toprakları                      |                 |                |              |      ✓     |          ✓         |
| Brunei                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kamboçya                                            |                 |                |              |      ✓     |          ✓         |
| Çin |                 |                |              |      ✓     |          ✓         |
| Christmas Adası                                    |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Cocos (Keeling) Adaları|                 |                |              |      ✓     |          ✓         |
| Komorolar|                 |                |              |      ✓     |          ✓         |
| Cook Adaları                                        |                 |                |              |      ✓     |          ✓         |
| Fiji |                 |                |              |      ✓     |          ✓         |
| Fransız Polinezyası                                    |                 |                |              |      ✓     |          ✓         |
| Heard ve McDonald Adaları                   |                 |                |              |      ✓     |          ✓         |
| Hong Kong                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Endonezya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Japonya                                               |                 |                |              |      ✓     |          ✓         |
| Kiribati                                            |                 |                |              |      ✓     |          ✓         |
| Laos                                                |                 |                |              |      ✓     |          ✓         |
| Makao                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malezya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mikronezya |                 |                |              |      ✓     |          ✓         |
| Moğolistan                                            |                 |                |              |      ✓     |          ✓         |
| Nauru                                               |                 |                |              |      ✓     |          ✓         |
| Napal|                 |                |              |      ✓     |          ✓         |
| Yeni Kaledonya                                       |                 |                |              |      ✓     |          ✓         |
| Yeni Zelanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Niue                                                |                 |                |              |      ✓     |          ✓         |
| Norfolk Adası                                      |                 |                |              |      ✓     |          ✓         |
| Kuzey Kore                                         |                 |                |              |      ✓     |          ✓         |
| Kuzey Mariana Adaları                            |                 |                |       ✓      |      ✓     |          ✓         |
| Pakistan                                            |                 |                |              |      ✓     |          ✓         |
| Palau |                 |                |              |      ✓     |          ✓         |
| Papua Yeni Gine                                    |                 |                |              |      ✓     |          ✓         |
| Paracel Adaları                                     |                 |                |              |      ✓     |                    |
| Filipinler                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Pitcairn                                            |                 |                |              |      ✓     |          ✓         |
| Samoa                                               |                 |                |              |      ✓     |          ✓         |
| Senkaku Islands                                     |        ✓        |                |              |      ✓     |          ✓         |
| Singapur                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Solomon Adaları                                     |                 |                |              |      ✓     |          ✓         |
| Güney Kore                                         |                 |                |              |      ✓     |          ✓         |
| Güney Kurils                                     |        ✓        |                |              |      ✓     |          ✓         |
| Spratly Adaları                                     |                 |                |              |      ✓     |                    |
| Sri Lanka                                           |                 |                |              |      ✓     |          ✓         |
| Tayvan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tayland                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Tokelau                                             |                 |                |              |      ✓     |          ✓         |
| Tonga                                               |                 |                |              |      ✓     |          ✓         |
| Turks ve Caicos Adaları                            |                 |                |              |      ✓     |          ✓         |
| Tuvalu                                              |                 |                |              |      ✓     |          ✓         |
| Vanuatu                                             |                 |                |              |      ✓     |          ✓         |
| Vietnam                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Wallis ve Futuna|                 |                |              |      ✓     |          ✓         |
| |                 |                |              |            |                    |
| **Avrupa**                                              |                 |                |              |            |                    |
| Arnavutluk                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Andorra                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ermenistan                                             |                 |                |              |      ✓     |          ✓         |
| Avusturya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Azerbaycan                                          |                 |                |              |      ✓     |          ✓         |
| Belçika                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bosna-Hersek                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bulgaristan                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Belerus|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hırvatistan                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kıbrıs                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Çek Cumhuriyeti                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Danimarka                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Estonya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Faroe Adaları                                       |                 |                |              |      ✓     |          ✓         |
| Finlandiya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Fransa                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gürcistan                                             |                 |                |              |      ✓     |          ✓         |
| Almanya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gibralter                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Yunanistan                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Grönland                                           |                 |                |              |      ✓     |          ✓         |
| Guernsey                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Macaristan                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İzlanda                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İrlanda (Cumhuriyeti)                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Man Adası                                         |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İtalya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jan Mayen                                           |        ✓        |                |              |      ✓     |          ✓         |
| Jersey                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kazakistan                                          |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kosova                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Kırgızistan                                          |                 |                |              |      ✓     |          ✓         |
| Letonya                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Liechtenstein                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lituanya                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Lüksemburg                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Makedonya                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Malta                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Moldova                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Monako                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Karadağ                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hollanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Norveç                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Polonya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Portekiz                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Romanya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rusya Federasyonu                                  |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| San Marino                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Sırbistan                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovakya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovenya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İspanya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Svalbard                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| İsveç                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İsviçre                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tacikistan                                          |                 |                |              |      ✓     |          ✓         |
| Türkiye                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Türkmenistan                                        |                 |                |              |      ✓     |          ✓         |
| Ukrayna                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Birleşik Krallık                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Özbekistan                                          |                 |                |              |      ✓     |          ✓         |
| Vatikan Şehri                                        |                 |                |       ✓      |      ✓     |          ✓         |
| |                 |                |              |            |                    |
| **Orta Doğu Afrika**                                  |                 |                |              |            |                    |
| Afganistan                                         |                 |                |              |      ✓     |          ✓         |
| Cezayir                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Angola                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bahreyn                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Benin                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Botsvana                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Bouvet Adası                                       |                 |                |              |      ✓     |          ✓         |
| Burkina Faso                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Burundi                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Kamerun                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Cabo Verde|                 |                |       ✓      |      ✓     |          ✓         |
| Orta Afrika Cumhuriyeti                            |                 |                |       ✓      |      ✓     |          ✓         |
| Çad                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Cumhuriyeti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Fildişi Sahili (Côte d’Ivoire)                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Demokratik Cumhuriyeti                        |                 |                |       ✓      |      ✓     |          ✓         |
| Cibuti                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Mısır                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ekvator Gine Cumhuriyeti                      |                 |                |       ✓      |      ✓     |          ✓         |
| Eritre                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Etiyopya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Fransız Güney Toprakları|                 |                |              |      ✓     |          ✓         |
| Gabon                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gambiya                                              |                 |                |              |      ✓     |          ✓         |
| Gana                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gine                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Gine-Bissau                                       |                 |                |       ✓      |      ✓     |          ✓         |
| İran                                                |                 |                |              |      ✓     |          ✓         |
| Irak                                                |                 |                |       ✓      |      ✓     |          ✓         |
| İsrail                                              |                 |                |              |      ✓     |          ✓         |
| Ürdün                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kenya                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Kuveyt                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lübnan                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Lesotho                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Liberya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Libya|                 |                |       ✓      |      ✓     |          ✓         |
| Madagaskar                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Malavi                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Maldivler |                 |                |              |      ✓     |          ✓         |
| Mali                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Marshall Adaları |                 |                |              |      ✓     |          ✓         |
| Moritanya                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Mauritius                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Mayotte                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Fas                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Mozambik                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Myanmar                                             |                 |                |              |      ✓     |          ✓         |
| Namibya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Nijer                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Nijerya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Umman                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Katar                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Reunion|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Ruanda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Helena                                        |                 |                |              |      ✓     |          ✓         |
| Suudi Arabistan                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Senegal                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Seyşeller                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Sierra Leone                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Somali                                             |                 |                |              |      ✓     |          ✓         |
| Güney Afrika                                        |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Güney Sudan                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Sudan                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Svaziland                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Suriye                                               |                 |                |              |      ✓     |          ✓         |
| Sao Tome ve Principe, Demokratik Cumhuriyeti       |                 |                |       ✓      |      ✓     |          ✓         |
| Tanzanya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Togo                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Tunus                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Uganda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Arap Emirlikleri                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Batı banka                                           |                 |                |              |      ✓     |          ✓         |
| Yemen                                               |                 |                |              |      ✓     |          ✓         |
| Zambiya                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Zimbabve                                            |                 |                |       ✓      |      ✓     |          ✓         |


<br>
<br>




|Koşullar|Tanımlar|
|--- |--- |
|**<sup>1</sup> -adresi noktası**|Çevrimiçi arama noktası adresi dizinde uyarlanmıştır. Bu alan yalnızca desteklenen zaman arayın ve coğrafi kodlama desteklenir.|
|**<sup>2</sup> -barındırmak numarası**|Çevrimiçi arama adresi ilişkilendirme dizinde uyarlanmıştır. Bu alan yalnızca desteklenen zaman arayın ve coğrafi kodlama desteklenir.|
|**<sup>3</sup> -kullanıcı tarafından atanan kodu**|Çevrimiçi arama belirli kod, bir resmi ISO 3166-1 kod değil.|
