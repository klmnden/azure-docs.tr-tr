---
title: Azure'da kapsamı işlemek konumuna bağlı Hizmetleri | Microsoft Docs
description: İşleme hakkında bilgi edinin Azure konumda kapsamı temel Hizmetleri
services: location-based-services
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 3/6/2018
ms.topic: article
ms.service: location-based-services
documentationcenter: ''
manager: cpendle
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 5a7f760a19e416f61307e94027bbdbc2bc912b58
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-location-based-services---render-coverage"></a>Hizmetler - işleme kapsamı Azure konumuna dayalı

Bu makalede Azure konum tabanlı Hizmetleri işlemek için kapsamı bilgileri sağlar. 

Kapsamı için kullanıma [ **coğrafi kodlama**](geocoding-coverage.md).  
Kapsamı için kullanıma [ **trafiği**](traffic-coverage.md).  
Kapsamı için kullanıma [ **yönlendirme**](routing-coverage.md).


**Gösterge**

| Simgesi             | Anlamı                                |
|--------------------|----------------------------------------|
| ✓                  | Ayrıntılı verilerle sağlanan ülke.   |
| Ø                  | Basitleştirilmiş verilerle sağlanan ülke. |
| Ülke eksik. | Ülke verileri belirtilmedi.          |

<br>  

## <a name="render-coverage-of-raster-tiles"></a>Izgara döşeme kapsamı oluşturma

Aşağıdaki tabloda Azure konum tabanlı Hizmetleri işlemek ızgara döşeme için kapsam bilgileri sağlar.

|Bölge|İşleme - birleşik ızgara döşeme|
|--- |:---: |
| **Africa**                         |                |
|Cezayir|✓ |
|Angola|✓ |
|Benin|✓ |
|Botsvana|✓ |
|Burkina Faso|✓ |
|Burundi|✓ |
|Kamerun|✓ |
|Kongo Cumhuriyeti|✓ |
|Kongo Demokratik Cumhuriyeti|✓ |
|Mısır|✓ |
|Gabon|✓ |
|Gana|✓ |
|Kenya|✓ |
|Lesotho|✓ |
|Malavi|✓ |
|Mali|✓ |
|Moritanya|✓ |
|Mauritius, Mayotte and Réunion|✓ |
|Fas|✓ |
|Mozambik|✓ |
|Namibya|✓ |
|Nijer|✓ |
|Nijerya|✓ |
|Ruanda|✓ |
|Senegal|✓ |
|Güney Afrika|✓ |
|Svaziland|✓ |
|Tanzanya|✓ |
|Togo|✓ |
|Tunus|✓ |
|Uganda|✓ |
|Zambiya|✓ |
|Zimbabve|✓ |
|                                |                |
| **Kuzey ve Güney Amerika**                       |                |
|Arjantin|✓ |
|Brezilya|✓ |
|Karayipler Adaları|✓ |
|Kanada|✓ |
|Şili|✓ |
|Kolombiya|✓ |
|Fransız Ginesi|✓ |
|Lesser Antilleri|✓ |
|Meksika|✓ |
|Peru|✓ |
|Amerika Birleşik Devletleri|✓ |
|Uruguay|✓ |
|Venezuela|✓ |
|                                |                |
| **Asya Pasifik**                   |                |
|Avustralya|✓ |
|Brunei|✓ |
|Guam|✓ |
|Hong Kong|✓ |
|Endonezya|✓ |
|Makau|✓ |
|Malezya|✓ |
|Yeni Zelanda|✓ |
|Filipinler|✓ |
|Singapur|✓ |
|Güney Kore|✓ |
|Tayvan|✓ |
|Tayland|✓ |
|Vietnam|✓ |
|                                |                |
| **Avrupa**                         |                |
|Arnavutluk|✓ |
|Andorra|✓ |
|Avusturya|✓ |
|Belarus|Ø  |
|Belçika|✓ |
|Bosna Hersek|✓ |
|Bulgaristan|✓ |
|Hırvatistan|✓ |
|Kıbrıs|✓ |
|Çek Cumhuriyeti|✓ |
|Danimarka|✓ |
|Estonya|✓ |
|Finlandiya|✓ |
|Fransa|✓ |
|Almanya|✓ |
|Cebelitarık|✓ |
|Yunanistan|✓ |
|Macaristan|✓ |
|İzlanda|✓ |
|İrlanda (Cumhuriyeti)|✓ |
|İtalya|✓ |
|Letonya|✓ |
|Liechtenstein|✓ |
|Lituanya|✓ |
|Lüksemburg|✓ |
|Makedonya|✓ |
|Malta|✓ |
|Moldova|✓ |
|Monako|✓ |
|Karadağ|✓ |
|Hollanda|✓ |
|Norveç|✓ |
|Polonya|✓ |
|Portekiz|✓ |
|Romanya|✓ |
|Rusya Federasyonu|✓ |
|San Marino|✓ |
|Sırbistan|✓ |
|Slovakya|✓ |
|Slovenya|✓ |
|İspanya|✓ |
|İsveç|✓ |
|İsviçre|✓ |
|Türkiye|✓ |
|Ukrayna|✓ |
|Birleşik Krallık|✓ |
|Vatikan Şehri|✓ |
|                                |                |
| **Orta Doğu**                         |                |
|Bahreyn|✓ |
|Irak|✓ |
|Ürdün|✓ |
|Kuveyt|✓ |
|Lübnan|✓ |
|Umman|✓ |
|Katar|✓ |
|Suudi Arabistan|✓ |
|Birleşik Arap Emirlikleri|✓ |
|Yemen|✓ |

  
<br>  


## <a name="render-coverage-of-vector-tiles"></a>Vektör döşeme kapsamı oluşturma 

Aşağıdaki tabloda Azure konum tabanlı Hizmetleri işlemek vektör döşeme için kapsam bilgileri sağlar.

|Bölge|İşleme - birleşik vektör döşeme|
|--- |:---: |
| **Africa**                         |                |
|Cezayir|✓|
|Angola|✓|
|Benin|✓|
|Botsvana|✓|
|Burkina Faso|✓|
|Burundi|✓|
|Kamerun|✓|
|Orta Afrika Cumhuriyeti|Ø|
|Çad|Ø|
|Komorolar|Ø|
|Kongo Cumhuriyeti|✓|
|Kongo Demokratik Cumhuriyeti|✓|
|Fildişi Sahili (Côte d’Ivoire)|Ø|
|Cibuti|Ø|
|Mısır|✓|
|Ekvator Ginesi|Ø|
|Eritre|Ø|
|Etiyopya|Ø|
|Gabon|✓|
|Gambiya|Ø|
|Gana|✓|
|Gine|Ø|
|Gine-Bissau|Ø|
|Kenya|✓|
|Lesotho|✓|
|Liberya|Ø|
| Libya |Ø|
|Madagaskar|Ø|
|Malavi|✓|
|Mali|✓|
|Moritanya|✓|
|Mauritius|✓|
|Mayotte|✓|
|Fas|✓|
|Mozambik|✓|
|Namibya|✓|
|Nijer|✓|
|Nijerya|✓|
|Reunion|✓|
|Ruanda|✓|
|Saint Helena, Ascension ve Tristan da Cunha|Ø|
|Sao Tome ve Principe|Ø|
|Senegal|✓|
|Sierra Leone|Ø|
|Somali|Ø|
|Güney Afrika|✓|
|Güney Sudan|Ø|
|Sudan|Ø|
|Svaziland|✓|
|Birleşik Tanzanya Cumhuriyeti|✓|
|Togo|✓|
|Uganda|✓|
|Zambiya|✓|
|Zimbabve|✓|
|                                |                |
| **Kuzey ve Güney Amerika**                       |                |
|Amerikan Samoası|✓|
|Anguilla|✓|
|Antigua ve Barbuda|✓|
|Arjantin|✓|
|Aruba|✓|
|Bahamalar|✓|
|Barbados|✓|
|Belize|✓|
|Bermuda|✓|
|Plurinational durumu Bolivya|✓|
| Bonaire, Sint Eustatius ve Saba |✓|
|Brezilya|✓|
|Kanada|✓|
|Cayman Adaları|✓|
|Şili|✓|
|$Clipperton Adası|✓|
|Kolombiya|✓|
|Kosta Rika|✓|
|Küba|✓|
|Curaçao|✓|
|Dominika|✓|
|Dominik Cumhuriyeti|✓|
|Ekvador|✓|
|Falkland Adaları (Malvinas)|✓|
|Fransız Ginesi|✓|
|Grenada|✓|
|Guadalupe|✓|
|Guam|✓|
|Guatemala|✓|
|Guyana|✓|
|Haiti|✓|
|Honduras|✓|
|Jamaika|✓|
|Martinik|✓|
|Meksika|✓|
|Montserrat|✓|
|Nikaragua|✓|
|Kuzey Mariana Adaları|✓|
|Panama|✓|
|Paraguay|✓|
|Peru|✓|
|Porto Riko|✓|
|Quebec (Kanada)|✓|
|Saint Barthélemy|✓|
|Saint Kitts ve Nevis|✓|
|Saint Lucia|✓|
|Saint Martin (Fransızca bölümü) s|✓|
|Saint Pierre ve Miquelon|✓|
|Saint Vincent ve Grenadinler|✓|
|Sint Maarten (Flaman bölgesi)|✓|
|Güney Georgia ve Güney Sandwich Adaları|✓|
|Surinam|✓|
|Trinidad ve Tobago|✓|
|Turks ve Caicos Adaları|✓|
|Amerika Birleşik Devletleri|✓|
|Uruguay|✓|
|Venezuela Bolivar Cumhuriyeti|✓|
|İngiliz Virgin Adaları|✓|
|ABD Virgin Adaları|✓|
|                                |                |
| **Asya**                   |                |
|Afganistan|Ø|
|Bangladeş|Ø|
|Butan|Ø|
|Britanya Hint Okyanusu Toprakları|Ø|
|Barış Yurdu Brunei Devleti|✓|
|Kamboçya|Ø|
|Çin|Ø|
|Kore Demokratik Halk Cumhuriyeti|Ø|
|Dokdo ve Takeshima|Ø|
|Hong Kong|✓|
|Endonezya|✓|
|İran|Ø|
|Japonya|Ø|
|Kazakistan|✓|
|Kırgızistan|Ø|
|Laos Demokratik Halk Cumhuriyeti|Ø|
|Makao|✓|
|Malezya|✓|
|Maldivler|Ø|
|Moğolistan|Ø|
|Myanmar|Ø|
| Napal |Ø|
|Pakistan|Ø|
|Filipinler|✓|
|Kore Cumhuriyeti|Ø|
|Senkaku Islands|✓|
|Singapur|✓|
|Sri Lanka|Ø|
|Suriye Arap Cumhuriyeti|Ø|
|Tayvan|✓|
|Tacikistan|Ø|
|Tayland|✓|
|Timor-Leste|Ø|
|Türkmenistan|Ø|
|Özbekistan|Ø|
|Vietnam|✓|
|                                |                |
| **Avrupa**                   |                |
|Arnavutluk|✓|
|Andorra|✓|
|Ermenistan|Ø|
|Avusturya|✓|
|Azerbaycan|Ø|
|Belarus|✓|
|Belçika|✓|
|Bosna - Hersek|✓|
|Bulgaristan|✓|
|Hırvatistan|✓|
|Kıbrıs|✓|
|Czechia|✓|
|Danimarka|✓|
|Estonya|✓|
|Faroe Adaları|Ø|
|Finlandiya|✓|
|Fransa|✓|
|Gürcistan|Ø|
|Almanya|✓|
|Yunanistan|✓|
|Grönland|Ø|
|Guernsay|✓|
|Vatikan|✓|
|Macaristan|✓|
|İzlanda|✓|
|İrlanda|✓|
|Man Adası|✓|
|İtalya|✓|
|Jan Mayen|✓|
|Jersey|✓|
|Letonya|✓|
|Liechtenstein|✓|
|Lituanya|✓|
|Lüksemburg|✓|
|Makedonya|✓|
|Malta|✓|
|Moldova|✓|
|Monako|✓|
|Karadağ|✓|
|Hollanda|✓|
|Norveç|✓|
|Polonya|✓|
|Portekiz|✓|
|Romanya|✓|
|Rusya|✓|
|San Marino|✓|
|Sırbistan|✓|
|Slovakya|✓|
|Slovenya|✓|
|Güney Kurils|✓|
|İspanya|✓|
|Svalbard|✓|
|İsviçre|✓|
|Ukrayna|✓|
|Birleşik Krallık|✓|
|                                |                |
| **Avrupa**                   |                |
|Bahreyn|✓|
| Cabo Verde |✓|
|Mısır|✓|
|Irak|✓|
|İsrail|✓|
|Ürdün|✓|
|Kuveyt|✓|
|Lübnan|✓|
|Umman|✓|
|Katar|✓|
|Suudi Arabistan|✓|
|Tunus|✓|
|Türkiye|✓|
|Birleşik Arap Emirlikleri|✓|
|Yemen|✓|
|                                |                |
| **Oceania**                   |                |
|Avustralya|✓|
|Cocos (Keeling) Adaları|Ø|
|Cook Adaları|Ø|
|Fiji|Ø|
|Fransız Polinezyası|Ø|
|Kiribati|Ø|
|Marshall Adaları|Ø|
|Mikronezya|Ø|
|Nauru|Ø|
|Yeni Kaledonya|Ø|
|Yeni Zelanda|✓|
|Niue|Ø|
|Norfolk Adası|Ø|
|Palau|Ø|
|Papua Yeni Gine|Ø|
|Pitcairn|Ø|
|Samoa|Ø|
|Solomon Adaları|Ø|
|Tokelau|Ø|
|Tonga|Ø|
|Tuvalu|Ø|
|Birleşik Devletler Küçük Harici Adaları|Ø|
|Vanuatu|Ø|
|Wallis ve Futuna|Ø|





