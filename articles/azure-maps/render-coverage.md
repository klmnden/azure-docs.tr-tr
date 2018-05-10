---
title: Azure Maps kapsamı oluşturma | Microsoft Docs
description: Azure Maps işleme kapsamı hakkında bilgi edinin
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 03/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: ab05277c4541ae859f79b1108c4cf8a7beb29271
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-maps-render-coverage"></a>Azure eşlemeleri kapsamı oluşturma

Azure eşlemeleri ızgara kutucukları ve vektör döşeme eşlemeleri oluşturmak için kullanır. En düşük çözünürlükte dünyaya tek bir döşeme sığar. En yüksek çözünürlükte tek bir döşeme 38 metre kare temsil eder. Bu nedenle, bir haritada yakınlaştırma gibi Kıtalar, bölgeler, şehirler ve tek tek streets hakkında giderek daha fazla ayrıntı görebilirsiniz. Daha fazla bilgi için bkz: [yakınlaştırma düzeyleri ve döşeme kılavuz](zoom-levels-and-tile-grid.md).

Ancak, Maps yok aynı düzeyde bilgi ve doğruluk tüm bölgeler için. Aşağıdaki tablolarda her bölgesinden beklediğiniz işlenmiş ayrıntı düzeyini hakkında bilgi sağlar.

## <a name="legend"></a>Gösterge

| Simgesi | Anlamı |
|--------|---------|
| ✓ | Bölge ayrıntılı veri ile temsil edilir.   |
| Ø | Bölge Basitleştirilmiş veri ile temsil edilir. |


## <a name="africa"></a>Afrika 


| Bölge | Birleşik ızgara döşeme | Birleşik vektör döşeme |
| ------ | :------------------: | :------------------: |
| Cezayir                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benin                            | ✓ | ✓ |
| Botsvana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Cabo Verde                       |   | ✓ |
| Kamerun                         | ✓ | ✓ |
| Orta Afrika Cumhuriyeti         |   | Ø |
| Çad                             |   | Ø |
| Komorolar                          |   | Ø |
| Kongo Cumhuriyeti                            | ✓ | ✓ |
| Kongo Demokratik Cumhuriyeti | ✓ | ✓ |
| Fildişi Sahili (Côte d’Ivoire)                    |   | Ø |
| Cibuti                         |   | Ø |
| Mısır                            | ✓ | ✓ |
| Ekvator Ginesi                |   | Ø |
| Eritre                          |   | Ø |
| Etiyopya                         |   | Ø |
| Gabon                            | ✓ | ✓ |
| Gambiya                           |   | Ø |
| Gana                            | ✓ | ✓ |
| Gine                           |   | Ø |
| Gine-Bissau                    |   | Ø |
| Kenya                            | ✓ | ✓ |
| Lesotho                          | ✓ | ✓ |
| Liberya                          |   | Ø |
| Libya                            |   | Ø |
| Madagaskar                       |   | Ø |
| Malavi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Moritanya                       | ✓ | ✓ |
| Mauritius                        | ✓ | ✓ |
| Mayotte                          | ✓ | ✓ |
| Fas                          | ✓ | ✓ |
| Mozambik                       | ✓ | ✓ |
| Namibya                          | ✓ | ✓ |
| Nijer                            | ✓ | ✓ |
| Nijerya                          | ✓ | ✓ |
| Reunion                          | ✓ | ✓ |
| Ruanda                           | ✓ | ✓ |
| Saint Helena, Ascension ve Tristan da Cunha |   | Ø |
| Sao Tome ve Principe            |   | Ø |
| Senegal                          | ✓ | ✓ |
| Sierra Leone                     |   | Ø |
| Somali                          |   | Ø |
| Güney Afrika                     | ✓ | ✓ |
| Güney Sudan                      |   | Ø |
| Sudan                            |   | Ø |
| Svaziland                        | ✓ | ✓ |
| Birleşik Tanzanya Cumhuriyeti      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunus                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Zambiya                           | ✓ | ✓ |
| Zimbabve                         | ✓ | ✓ |

## <a name="americas"></a>Kuzey ve Güney Amerika

| Bölge | Birleşik ızgara döşeme | Birleşik vektör döşeme |
| ------ | :------------------: | :------------------: |
| Anguilla                  | ✓ | ✓ |
| Antigua ve Barbuda       | ✓ | ✓ |
| Arjantin                 | ✓ | ✓ |
| Aruba                     | ✓ | ✓ |
| Bahamalar                   | ✓ | ✓ |
| Barbados                  | ✓ | ✓ |
| Belize                    | ✓ | ✓ |
| Bermuda                   |   | ✓ |
| Plurinational durumu Bolivya |   | ✓ |
| Bonaire, Sint Eustatius ve Saba |   | ✓ |
| Brezilya                    | ✓ | ✓ |
| Kanada                    | ✓ | ✓ |
| Cayman Adaları            | ✓ | ✓ |
| Şili                     | ✓ | ✓ |
| $Clipperton Adası         |   | ✓ |
| Kolombiya                  | ✓ | ✓ |
| Kosta Rika                |   | ✓ |
| Küba                      | ✓ | ✓ |
| Curaçao                   | ✓ | ✓ |
| Dominika                  | ✓ | ✓ |
| Dominik Cumhuriyeti        | ✓ | ✓ |
| Ekvador                   |   | ✓ |
| Falkland Adaları (Malvinas) |   | ✓ |
| Fransız Ginesi             | ✓ | ✓ |
| Grönland                 |   | Ø |
| Grenada                   | ✓ | ✓ |
| Guadalupe                | ✓ | ✓ |
| Guatemala                 |   | ✓ |
| Guyana                    | ✓ | ✓ |
| Haiti                     | ✓ | ✓ |
| Honduras                  | ✓ | ✓ |
| Jamaika                   | ✓ | ✓ |
| Martinik                | ✓ | ✓ |
| Meksika                    | ✓ | ✓ |
| Montserrat                | ✓ | ✓ |
| Nikaragua                 | ✓ | ✓ |
| Kuzey Mariana Adaları  |   | ✓ |
| Panama                    | ✓ | ✓ | 
| Paraguay                  |   | ✓ |
| Peru                      | ✓ | ✓ |
| Porto Riko               | ✓ | ✓ |
| Quebec (Kanada)           |   | ✓ |
| Saint Barthélemy          | ✓ | ✓ |
| Saint Kitts ve Nevis     | ✓ | ✓ |
| Saint Lucia               | ✓ | ✓ |
| Saint Martin (Fransızca)     | ✓ | ✓ |
| Saint Pierre ve Miquelon |   | ✓ |
| Saint Vincent ve Grenadinler | ✓ | ✓ |
| Sint Maarten (Hollanda dili)      | ✓ | ✓ |
| Güney Georgia ve Güney Sandwich Adaları |   | ✓ |
| Surinam                  |   | ✓ |
| Trinidad ve Tobago       | ✓ | ✓ |
| Turks ve Caicos Adaları  | ✓ | ✓ |
| Amerika Birleşik Devletleri             | ✓ | ✓ |
| Uruguay                   | ✓ | ✓ |
| Venezuela                 | ✓ | ✓ |
| İngiliz Virgin Adaları   | ✓ | ✓ |
| ABD Virgin Adaları      | ✓ | ✓ |

## <a name="asia"></a>Asya 

| Bölge | Birleşik ızgara döşeme | Birleşik vektör döşeme |
| ------ | :------------------: | :------------------: |
| Afganistan               |   | Ø |
| Bahreyn                   | ✓ | ✓ |
| Bangladeş                |   | Ø |
| Butan                    |   | Ø |
| Britanya Hint Okyanusu Toprakları |   | Ø |
| Brunei                    | ✓ | ✓ |
| Kamboçya                  |   | Ø |
| Çin                     |   | Ø |
| Cocos (Keeling) Adaları   |   | Ø |
| Kore Demokratik Halk Cumhuriyeti |   | Ø |
| Dokdo ve Takeshima       |   | Ø |
| Hong Kong                 | ✓ | ✓ |
| Endonezya                 | ✓ | ✓ |
| İran                      |   | Ø |
| Irak                      | ✓ | ✓ |
| İsrail                    |   | ✓ |
| Japonya                     |   | Ø |
| Ürdün                    | ✓ | ✓ |
| Kazakistan                |   | ✓ |
| Kuveyt                    | ✓ | ✓ |
| Kırgızistan                |   | Ø |
| Laos Demokratik Halk Cumhuriyeti |   | Ø |
| Lübnan                   | ✓ | ✓ |
| Makao                     | ✓ | ✓ |
| Malezya                  | ✓ | ✓ |
| Maldivler                  |   | Ø |
| Moğolistan                  |   | Ø |
| Myanmar                   |   | Ø |
| Napal                     |   | Ø |
| Umman                      | ✓ | ✓ |
| Pakistan                  |   | Ø |
| Filipinler               | ✓ | ✓ |
| Katar                     | ✓ | ✓ |
| Kore Cumhuriyeti         | ✓ | Ø |
| Suudi Arabistan              | ✓ | ✓ |
| Senkaku Adaları           |   | ✓ |
| Singapur                 | ✓ | ✓|
| Sri Lanka                 |   | Ø |
| Suriye Arap Cumhuriyeti      |   | Ø |
| Tayvan                    | ✓ | ✓ |
| Tacikistan                |   | Ø |
| Tayland                  | ✓ | ✓ |
| Timor-Leste               |   | Ø |
| Türkmenistan              |   | Ø |
| Birleşik Arap Emirlikleri      | ✓ | ✓ |
| Birleşik Devletler Küçük Harici Adaları |   | Ø |
| Özbekistan                |   | Ø |
| Vietnam                   | ✓ | ✓ |
| Yemen                     | ✓ | ✓ |

## <a name="oceania"></a>Oceania

| Bölge | Birleşik ızgara döşeme | Birleşik vektör döşeme |
| ------ | :------------------: | :------------------: |
| Amerikan Samoası            |   | ✓ |
| Avustralya                 | ✓ | ✓ |
| Cook Adaları              |   | Ø |
| Fiji                      |   | Ø |
| Fransız Polinezyası          |   | Ø |
| Guam                      | ✓ | ✓ |
| Kiribati                  |   | Ø |
| Marshall Adaları          |   | Ø |
| Mikronezya                |   | Ø |
| Nauru                     |   | Ø |
| Yeni Kaledonya             |   | Ø |
| Yeni Zelanda               | ✓ | ✓ |
| Niue                      |   | Ø |
| Norfolk Adası            |   | Ø |
| Palau                     |   | Ø |
| Papua Yeni Gine          |   | Ø |
| Pitcairn                  |   | Ø |
| Samoa                     |   | Ø |
| Solomon Adaları           |   | Ø|
| Tokelau                   |   | Ø |
| Tonga                     |   | Ø |
| Tuvalu                    |   | Ø |
| Vanuatu                   |   | Ø |
| Wallis ve Futuna         |   | Ø |


## <a name="europe"></a>Avrupa

| Bölge | Birleşik ızgara döşeme | Birleşik vektör döşeme |
| ------ | :------------------: | :------------------: |
| Arnavutluk                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Ermenistan                   |   | Ø |
| Avusturya                   | ✓ | ✓ |
| Azerbaycan                |   | Ø |
| Belarus                   | Ø | ✓ |
| Belçika                   | ✓ | ✓ |
| Bosna Hersek        | ✓ | ✓ |
| Bulgaristan                  | ✓ | ✓ |
| Hırvatistan                   | ✓ | ✓ |
| Kıbrıs                    | ✓ | ✓ |
| Çek Cumhuriyeti            | ✓ | ✓ |
| Danimarka                   | ✓ | ✓ |
| Estonya                   | ✓ | ✓ |
| Faroe Adaları             |   | Ø |
| Finlandiya                   | ✓ | ✓ |
| Fransa                    | ✓ | ✓ |
| Gürcistan                   |   | Ø |
| Almanya                   | ✓ |✓ |
| Cebelitarık                 | ✓ |   |
| Yunanistan                    | ✓ | ✓ |
| Guernsey                  |   | ✓ |
| Macaristan                   | ✓ | ✓ |
| İzlanda                   | ✓ | ✓ |
| İrlanda (Cumhuriyeti)     | ✓ | ✓ |
| Man Adası               |   | ✓ |
| İtalya                     | ✓ | ✓ |
| Jan Mayen                 |   | ✓ |
| Jersey                    |   | ✓ |
| Letonya                    | ✓ | ✓ |
| Liechtenstein             | ✓ | ✓ |
| Lituanya                 | ✓ | ✓ |
| Lüksemburg                | ✓ | ✓ |
| Makedonya                 | ✓ | ✓ |
| Malta                     | ✓ | ✓ |
| Moldova                   | ✓ | ✓ |
| Monako                    | ✓ | ✓ |
| Karadağ                | ✓ | ✓ |
| Hollanda               | ✓ | ✓ |
| Norveç                    | ✓ | ✓ |
| Polonya                    | ✓ | ✓ |
| Portekiz                  | ✓ | ✓ |
| Romanya                   | ✓ | ✓ |
| Rusya Federasyonu        | ✓ | ✓ |
| San Marino                | ✓ | ✓ |
| Sırbistan                    | ✓ | ✓ |
| Slovakya                  | ✓ | ✓ |
| Slovenya                  | ✓ | ✓ |
| Güney Kurils           |   | ✓ |
| İspanya                     | ✓ | ✓ |
| Svalbard                  |   | ✓ |
| İsveç                    | ✓ |   |
| İsviçre               | ✓ | ✓ |
| Türkiye                    | ✓ | ✓ |
| Ukrayna                   | ✓ | ✓ |
| Birleşik Krallık            | ✓ | ✓ |
| Vatikan Şehri              | ✓ | ✓ |

## <a name="next-steps"></a>Sonraki adımlar

Azure eşlemeleri oluşturma hakkında daha fazla bilgi için bkz: [yakınlaştırma düzeyleri ve döşeme kılavuz](zoom-levels-and-tile-grid.md).

Hakkında bilgi edinin [hizmeti Yönlendirme eşlemeleri için kapsamı alanlar](routing-coverage.md). 