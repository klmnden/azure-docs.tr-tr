---
title: Azure haritalar, coğrafi kodlama kapsamı | Microsoft Docs
description: Azure haritalar, coğrafi kodlama kapsamı hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a5e5f4ab286289e223a2fe10ff8cf45f43309f04
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65785951"
---
# <a name="azure-maps-geocoding-coverage"></a>Azure haritalar coğrafi kodlama kapsamı

Azure Haritalar ile konum için arama yaparken, arama hizmeti arama terimlerinizi alır ve coğrafi kodlama adlı bir işlem enlem ve boylam koordinatlarını döndürür. Ancak, Maps yok aynı düzeyde bilgi ve tüm bölge ve ülke doğruluk. Konumlar ne tür güvenilir bir şekilde her bölgede arayabilirsiniz belirlemek için bu makaleyi kullanın. 

Bir ülke/bölge içinde geocode yeteneği, yol veri kapsamı ve coğrafi kodlama hizmeti coğrafi kodlama duyarlığını temel bağlıdır. Aşağıdaki kategorilere ayırma kullanılan her ülkesinde/bölgesinde coğrafi kodlama destek düzeyini belirtin.
* **Adres noktaları** -adresleri veri bir enlem/boylam koordinatını (özellik sınırı) adresi paket içinde Çözüldü olabilir. 'Çatı ' doğru bazen adlandırılır. Doğruluk adresleri için kullanılabilir en üst düzey budur. 
* **Sayı barındırmak** -adresleri Sokak üzerinde bir enlem/boylam koordinatını için ilişkilendirilmiş.
* **Sokak düzeyi** -adresleri olan sokak adresi içeren enlem/boylam koordinatını çözümlenen. Bina numarasını işlenmeyebilir.
* **Şehir düzeyinde** -Şehir yer adlarına desteklenir.

## <a name="americas"></a>Kuzey ve Güney Amerika

| Ülke/Bölge                                       | Adres noktaları | Ev numaraları | Sokak düzeyi | Şehir düzeyinde | İlgi noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Anguilla                                            |                 |                |              |      ✓     |          ✓         |
| Antarktika                                          |                 |                |              |      ✓     |          ✓         |
| Antigua ve Barbuda                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Arjantin                                           |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Aruba                                               |                 |                |              |      ✓     |          ✓         |
| Bahamalar                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Barbados                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Belize                                              |                 |                |              |      ✓     |          ✓         |
| Bermuda                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bolivya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bonaire, Sint Eustatius ve Saba                   |                 |                |              |      ✓     |          ✓         |
| Brezilya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kanada                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kayman Adaları                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Şili                                               |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Kolombiya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosta Rika                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Küba                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Dominica                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Dominicana                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Ekvador                                             |                 |                |       ✓      |      ✓     |          ✓         |
| El Salvador                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Falkland Adaları                                    |                 |                |              |      ✓     |          ✓         |
| Fransız Ginesi                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Grenada                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Guadalupe                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Guam                                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Guatemala                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Guyana                                              |                |             |           |      ✓     |                 |
| Haiti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Honduras                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Jamaika                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Martinik                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Meksika                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Montserrat                                          |                 |                |              |      ✓     |          ✓         |
| Nikaragua                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Panama                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Paraguay                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Peru                                                |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Porto Riko                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Saint Barthélemy                                    |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Kitts ve Nevis                               |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Lucia                                         |                 |                |              |      ✓     |          ✓         |
| Saint Martin                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Pierre ve Miquelon                           |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Vincent ve Grenadinler                    |                 |                |              |      ✓     |          ✓         |
| Sint Maarten                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Güney Georgia ve Güney Sandwich Adaları        |                 |                |              |      ✓     |          ✓         |
| Surinam                                            |                 |                |              |      ✓     |          ✓         |
| Trinidad ve Tobago                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Devletler Küçük Harici Adaları                |                 |                |              |      ✓     |          ✓         |
| Amerika Birleşik Devletleri                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Uruguay                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Venezuela                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Britanya Virjin Adaları                              |                 |                |              |      ✓     |          ✓         |
| ABD Virgin Adaları                                 |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |

## <a name="asia-pacific"></a>Asya Pasifik

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Sokak düzeyi | Şehir düzeyinde | İlgi noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Amerikan Samoası                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Avustralya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bangladeş                                          |                 |                |              |      ✓     |          ✓         |
| Butan                                              |                 |                |              |      ✓     |          ✓         |
| Britanya Hint Okyanusu Toprakları                      |                 |                |              |      ✓     |          ✓         |
| Brunei                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kamboçya                                            |                 |                |              |      ✓     |          ✓         |
| Çin                                               |                 |                |              |      ✓     |          ✓         |
| Christmas Adası                                    |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Cocos (Keeling) Adaları                             |                 |                |              |      ✓     |          ✓         |
| Komorlar                                             |                 |                |              |      ✓     |          ✓         |
| Cook Adaları                                        |                 |                |              |      ✓     |          ✓         |
| Fiji                                                |                  |                |              |      ✓     |          ✓        |
| FRANSIZ POLİNEZYASI                                    |                 |                |              |      ✓     |          ✓         |
| Heard Adası ve McDonald Adaları                   |                 |                |              |      ✓     |          ✓         |
| Hong Kong SAR                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Endonezya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Hindistan                                               |        ✓        |        ✓       |       ✓      |      ✓     |                   |
| Japonya                                               |                 |                |              |      ✓     |          ✓         |
| Kiribati                                            |                 |                |              |      ✓     |          ✓         |
| Güney Kore                                         |                 |                |              |      ✓     |          ✓         |
| Laos                                                |                 |                |              |      ✓     |          ✓         |
| Makao ÖİB                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malezya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mikronezya                                          |                 |                |              |      ✓     |          ✓         |
| Moğolistan                                            |                 |                |              |      ✓     |          ✓         |
| Nauru                                               |                 |                |              |      ✓     |          ✓         |
| Nepal                                               |                 |                |              |      ✓     |          ✓         |
| Yeni Kaledonya                                       |                 |                |              |      ✓     |          ✓         |
| Yeni Zelanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Niue                                                |                 |                |              |      ✓     |          ✓         |
| Norfolk Adası                                      |                 |                |              |      ✓     |          ✓         |
| Kuzey Kore                                         |                 |                |              |      ✓     |          ✓         |
| Kuzey Mariana Adaları                            |                 |                |       ✓      |      ✓     |          ✓         |
| Pakistan                                            |                 |                |              |      ✓     |          ✓         |
| Palau                                               |                 |                |              |      ✓     |          ✓         |
| Papua Yeni Gine                                    |                 |                |              |      ✓     |          ✓         |
| Paracel Adaları                                     |                 |                |              |      ✓     |                    |
| Filipinler                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Pitcairn                                            |                 |                |              |      ✓     |          ✓         |
| Samoa                                               |                 |                |              |      ✓     |          ✓         |
| Senkaku Adaları                                     |        ✓        |                |              |      ✓     |          ✓         |
| Singapur                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Solomon Adaları                                     |                 |                |              |      ✓     |          ✓         |
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
| Wallis ve Futuna                                   |                 |                |              |      ✓     |          ✓         |

## <a name="europe"></a>Avrupa

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Sokak düzeyi | Şehir düzeyinde | İlgi noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Arnavutluk                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Andorra                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ermenistan                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Avusturya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Azerbaycan                                          |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Belçika                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bosna Hersek                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bulgaristan                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Belarus                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hırvatistan                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kıbrıs                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Çek Cumhuriyeti                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Danimarka                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Estonya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Faroe Adaları                                       |                 |                |              |      ✓     |          ✓         |
| Finlandiya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Fransa                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gürcistan                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Almanya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Cebelitarık                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Yunanistan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Grönland                                           |                 |                |              |      ✓     |          ✓         |
| Guernsey                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Macaristan                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İzlanda                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İrlanda                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Man Adası                                         |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İtalya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jan Mayen                                           |        ✓        |                |              |      ✓     |          ✓         |
| Jersey                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kazakistan                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosova                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Kırgızistan                                          |                 |                |              |      ✓     |          ✓         |
| Letonya                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Lihtenştayn                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Litvanya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lüksemburg                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kuzey Makedonya                                     |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malta                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Moldova                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Monako                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Karadağ                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hollanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Norveç                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Polonya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Portekiz                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| \+ Azor Adaları ve Madeira                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Romanya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rusya Federasyonu                                  |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| San Marino                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Sırbistan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovakya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovenya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İspanya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Svalbard                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| İsveç                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İsviçre                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tacikistan                                          |                 |                |              |      ✓     |          ✓         |
| Türkiye                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Türkmenistan                                        |                 |                |              |      ✓     |          ✓         |
| Ukrayna                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Birleşik Krallık                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Özbekistan                                          |                 |                |              |      ✓     |          ✓         |
| Vatikan                                        |                 |                |       ✓      |      ✓     |          ✓         |


## <a name="middle-east-and-africa"></a>Orta Doğu ve Afrika

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Sokak düzeyi | Şehir düzeyinde | İlgi noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Afganistan                                         |                 |                |              |      ✓     |          ✓         |
| Cezayir                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Angola                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bahreyn                                             |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Benin                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Botsvana                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Bouvet Adası                                       |                 |                |              |      ✓     |          ✓         |
| Burkina Faso                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Burundi                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Kamerun                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Cabo Verde                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Orta Afrika Cumhuriyeti                            |                 |                |       ✓      |      ✓     |          ✓         |
| Çad                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Cumhuriyeti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Fildişi Sahili (Côte d’Ivoire)                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Demokratik Cumhuriyeti                    |                 |                |       ✓      |      ✓     |          ✓         |
| Cibuti                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Mısır                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ekvator Ginesi Cumhuriyeti                      |                 |                |       ✓      |      ✓     |          ✓         |
| Eritre                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Etiyopya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Fransız Güney Toprakları|                        |                |              |      ✓     |          ✓         |
| Gabon                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gambia                                              |                 |                |              |      ✓     |          ✓         |
| Gana                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gine                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Gine-Bissau                                       |                 |                |       ✓      |      ✓     |          ✓         |
| İran                                                |                 |                |              |      ✓     |          ✓         |
| Irak                                                |                 |                |       ✓      |      ✓     |          ✓         |
| İsrail                                              |        ✓        |       ✓        |              |      ✓     |          ✓         |
| Ürdün                                              |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Kenya                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Kuveyt                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lübnan                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Lesoto                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Liberya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Libya                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Madagaskar                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Malavi                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Maldivler                                            |                 |                |              |      ✓     |          ✓         |
| Mali                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Marshall Adaları                                    |                 |                |              |      ✓     |          ✓         |
| Moritanya                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Mauritius                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Mayotte                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Fas                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mozambik                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Myanmar                                             |                 |                |              |      ✓     |          ✓         |
| Namibya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Nijer                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Nijerya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Umman                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Katar                                               |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Réunion                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rwanda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Helena                                        |                 |                |              |      ✓     |          ✓         |
| Suudi Arabistan                                        |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Senegal                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Seyşeller                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Sierra Leone                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Somali                                             |                 |                |              |      ✓     |          ✓         |
| Güney Afrika                                        |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Güney Sudan                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Sudan                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Svaziland                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Suriye                                               |                 |                |              |      ✓     |          ✓         |
| Sao Tome ve Principe                               |                 |                |       ✓      |      ✓     |          ✓         |
| Tanzanya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Togo                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Tunus                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Uganda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Arap Emirlikleri                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Yemen                                               |                 |                |              |      ✓     |          ✓         |
| Zambiya                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Zimbabve                                            |                 |                |       ✓      |      ✓     |          ✓         |



## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar ile coğrafi kodlama hakkında daha fazla bilgi için bkz: [arama](https://docs.microsoft.com/rest/api/maps/search) başvuru sayfalarına.

Hakkında bilgi edinin [kapsamı alanları haritalar için hizmet trafiği](traffic-coverage.md). 

