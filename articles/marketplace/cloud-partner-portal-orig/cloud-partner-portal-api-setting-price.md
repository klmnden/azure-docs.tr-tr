---
title: Sanal makine teklifleri için fiyatlandırma | Microsoft Docs
description: Sanal makine teklifleri fiyatlandırma belirtme üç yöntem açıklanır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: a029477dfd8046863ebfe34cd839562a0b1f3d87
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61094475"
---
<a name="pricing-for-virtual-machine-offers"></a>Sanal makine tekliflerinin fiyatlandırması
==================================

Sanal makine teklifleri için fiyatlandırma belirtmek için üç yolu vardır: özelleştirilmiş çekirdek fiyatlandırma, çekirdek başına fiyatlandırma ve elektronik tablo fiyatlandırma.


<a name="customized-core-pricing"></a>Özelleştirilmiş çekirdek fiyatlandırması
-----------------------

Fiyatlandırma, her bölge ve çekirdek birleşimi için özeldir. Satış listesindeki her bir bölge belirtilmelidir **virtualMachinePricing**/**regionPrices** tanımının bölümü.  Her biri için doğru para birimi kodlarını kullanın [bölge](#regions) isteğinizdeki.  Aşağıdaki örnek, bu gereksinimleri gösterir:

``` json
    "virtualMachinePricing": 
    {
        ... 
        "coreMultiplier": 
        {
            "currency": "USD",
                "individually": 
                {
                    "sharedcore": 2,
                    "1core": 2,
                    "2core": 3,
                    "4core": 4,
                    "6core": 5,
                    "8core": 2,
                    "12core": 4,
                    "16core": 4,
                    "20core": 4,
                    "24core": 4,
                    "32core": 4,
                    "36core": 4,
                    "40core": 4,
                    "64core": 4,
                    "128core": 4
                }
        }
        ...
     }
```


<a name="per-core-pricing"></a>Çekirdek başına fiyat
----------------

Bu durumda, yayımcıları bir fiyat ABD Doları, SKU için belirtin ve diğer tüm fiyatlar otomatik olarak oluşturulur. Çekirdek başına fiyat belirtilen **tek** istek parametresi.

``` json
     "virtualMachinePricing": 
     {
         ...
         "coreMultiplier": 
         {
             "currency": "USD",
             "single": 1.0
         }
     }
```


<a name="spreadsheet-pricing"></a>Elektronik Tablo fiyatlandırması
-------------------

Yayımcı Ayrıca kendi fiyatlandırma elektronik bir geçici depolama konumuna yükleyin ve sonra dosya diğer yapıtlar gibi istek URI dahil. Elektronik ardından, belirtilen fiyat zamanlama değerlendirmek için çevrilmiş karşıya yüklendikten ve son teklif fiyatlandırma bilgileriyle güncelleştirir. Teklif için sonraki GET istekleri elektronik URI döndürür ve bölge için değerlendirilen fiyatlar.

``` json
     "virtualMachinePricing": 
     {
         ...
         "spreadSheetPricing": 
         {
             "uri": "https://blob.storage.azure.com/<your_spreadsheet_location_here>/prices.xlsx",
         }
     }
```

<a name="regions"></a>Bölgeler
-------

Aşağıdaki tabloda, farklı bölgelerdeki özelleştirilmiş çekirdek fiyatlandırma ve ilgili para birimi kodlarını belirtebilirsiniz gösterir.

| **Bölge** | **Ad**             | **Para birimi kodu** |
|------------|----------------------|-------------------|
| DZ         | Cezayir              | DZD               |
| AR         | Arjantin            | ARS               |
| AU         | Avustralya            | AUD               |
| AT         | Avusturya              | EUR               |
| BH         | Bahreyn              | BHD               |
| BY         | Belarus              | RUB               |
| BE         | Belçika              | EUR               |
| BR         | Brezilya               | USD               |
| BG         | Bulgaristan             | BGN               |
| CA         | Kanada               | CAD               |
| CL         | Şili                | CLP               |
| CO         | Kolombiya             | KOPYALA               |
| CR         | Kosta Rika           | CRC               |
| HR         | Hırvatistan              | HRK               |
| CY         | Kıbrıs               | EUR               |
| CZ         | Çek Cumhuriyeti       | CZK               |
| DK         | Danimarka              | DKK               |
| DO         | Dominik Cumhuriyeti   | USD               |
| EC         | Ekvador              | USD               |
| EG         | Mısır                | EGP               |
| SV         | El Salvador          | USD               |
| EE         | Estonya              | EUR               |
| FI         | Finlandiya              | EUR               |
| GS         | Fransa               | EUR               |
| DE         | Almanya              | EUR               |
| GR         | Yunanistan               | EUR               |
| GT         | Guatemala            | GTQ               |
| HK         | Hong Kong Çin ÖİB        | HKD               |
| HU         | Macaristan              | HUF               |
| IS         | İzlanda              | SK               |
| IN         | Hindistan                | INR               |
| Kimlik         | Endonezya            | IDR               |
| IE         | İrlanda              | EUR               |
| IL         | İsrail               | ILS               |
| BT         | İtalya                | EUR               |
| JP         | Japonya                | JPY               |
| JO         | Ürdün               | JOD               |
| KZ         | Kazakistan           | KZT               |
| KE         | Kenya                | KES               |
| KR         | Güney Kore                | KRW               |
| KW         | Kuveyt               | KWD               |
| LV         | Letonya               | EUR               |
| LI         | Lihtenştayn        | CHF               |
| LT         | Litvanya            | EUR               |
| LU         | Lüksemburg           | EUR               |
| MK         | Kuzey Makedonya      | MKD               |
| MY         | Malezya             | MYR               |
| MT         | Malta                | EUR               |
| MX         | Meksika               | MXN               |
| ME         | Karadağ           | EUR               |
| MA         | Fas              | MAD               |
| NL         | Hollanda          | EUR               |
| NZ         | Yeni Zelanda          | NZD               |
| NG         | Nijerya              | NGN               |
| NO         | Norveç               | NOK               |
| OM         | Umman                 | OMR               |
| PK         | Pakistan             | PKR               |
| PA         | Panama               | USD               |
| PY         | Paraguay             | PYG               |
| PE         | Peru                 | KALEM               |
| PH         | Filipinler          | PHP               |
| PL         | Polonya               | PLN               |
| PT         | Portekiz             | EUR               |
| PR         | Porto Riko          | USD               |
| QA         | Katar                | QAR               |
| RO         | Romanya              | RON               |
| RU         | Rusya               | RUB               |
| SA         | Suudi Arabistan         | SAR               |
| RS         | Sırbistan               | RSD               |
| SG         | Singapur            | SGD               |
| SK         | Slovakya             | EUR               |
| SI         | Slovenya             | EUR               |
| ZA         | Güney Afrika         | ZAR               |
| ES         | İspanya                | EUR               |
| LK         | Sri Lanka            | USD               |
| SE         | İsveç               | SEK               |
| CH         | İsviçre          | CHF               |
| TW         | Tayvan               | TWD               |
| TH         | Tayland             | THB               |
| TT         | Trinidad ve Tobago  | TTD               |
| TN         | Tunus              | TND               |
| TR         | Türkiye               | TRY               |
| UA         | Ukrayna              | UAH               |
| AE         | Birleşik Arap Emirlikleri | EUR               |
| GB         | Birleşik Krallık       | GBP               |
| ABD         | Amerika Birleşik Devletleri        | USD               |
| UY         | Uruguay              | UYU               |
| VE         | Venezuela            | USD               |
|  |  |  |
