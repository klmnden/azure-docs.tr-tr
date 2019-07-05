---
title: Sınırlamalar ve bilinen sorunların Azure API Management API içeri aktarma | Microsoft Docs
description: Bilinen sorunlar ve kısıtlamalar açık API, WSDL veya WADL biçimleri kullanarak Azure API Management içe ayrıntıları.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2019
ms.author: apipm
ms.openlocfilehash: 7f7c37843ccaf78c7b7e6ec7a959106df45053d6
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461619"
---
# <a name="api-import-restrictions-and-known-issues"></a>API içeri aktarma kısıtlamaları ve bilinen sorunlar

## <a name="about-this-list"></a>Bu liste hakkında

API içeri aktarılırken arasında bazı kısıtlamalar gelen veya başarıyla içeri aktarmadan önce düzeltilmesi gereken sorunları belirlemek. Bu makalede belgeleri bunlar düzenlenmiş tarafından API içeri aktarma biçimi.

## <a name="open-api"> </a>Openapı/Swagger

Openapı belgenizi alma hataları almaya önceden doğruladınız emin olun. Her iki Tasarımcı (Tasarım - ön uç - Openapı belirtimi Düzenleyicisi) Azure portalında veya üçüncü taraf aracı gibi kullanarak bunu yapabilirsiniz <a href="https://editor.swagger.io">Swagger Editor</a>.

### <a name="open-api-general"> </a>Genel

-   Gerekli parametreleri yolu hem de sorgu genelinde benzersiz adlara sahip olmalıdır. (Openapı bir parametre adı yalnızca içinde örneğin yol, sorgu, üst bilgisi bir konumu benzersiz olması gerekir. Ancak, API Yönetimi'nde (Bu, Openapı desteklemeyen) yolu hem de sorgu parametreleri tarafından ayrılmış işlemleri izin veriyoruz. That's neden parametre adları tüm URL şablonu içinde benzersiz olması zorunlu kılarız.)
-   **\$Ref** işaretçileri, harici dosyalara başvuruda bulunamaz.
-   **x-ms-yolları** ve **x sunucuları** uzantıları yalnızca desteklenir.
-   Özel uzantılar içeri aktarma işlemi sırasında yok sayılır ve olmayan kaydedilmiş veya de dışarı aktarma için korunur.
-   **Özyineleme** -API Management, tanımlanan tanımları yinelemeli olarak (örneğin, kendilerini kaynağa başvuran şemaları) desteklemez.
-   Kaynak dosya URL'si (varsa) için göreli sunucu URL'leri uygulanır.

### <a name="open-api-v2"> </a>Openapı sürüm 2

-   Yalnızca JSON biçimi desteklenmiyor.

### <a name="open-api-v3"> </a>Openapı sürüm 3

-   Çok sayıda varsa **sunucuları** belirtilirse, API Management, ilk HTTPs URL'sini seçmek çalışır. -İlk HTTP URL'si HTTPs URL'leri değilseniz. HTTP URL'leri - değilse sunucu URL'si boş olur.
-   **Örnekler** desteklenmez, ancak **örnek** olduğu.
-   **Multipart/form-data** desteklenmiyor.

> [!IMPORTANT]
> OpenAPI içeri aktarma ile ilgili önemli bilgiler ve ipuçları için bu [belgeye](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) bakın.

## <a name="wsdl"> </a>WSDL

WSDL dosyaları SOAP geçişi ve SOAP ve REST API'leri oluşturmak için kullanılır.

-   **SOAP bağlamaları** -style "belgesi" ve "değişmez" kodlama yalnızca SOAP bağlamaları desteklenir. "Rpc" stil veya SOAP kodlamasına için desteği yoktur.
-   **WSDL: import** -bu özniteliği desteklenmiyor. Müşteriler, bir belgeye Imports birleştirmeniz gerekir.
-   **Birden çok bölümü olan iletiler** -bu tür iletileri desteklenmez.
-   **WCF wsHttpBinding** -Windows Communication Foundation ile oluşturulan SOAP Hizmetleri basicHttpBinding kullanması gereken - wsHttpBinding desteklenmez.
-   **MTOM** - MTOM kullanan hizmetler <em>olabilir</em> çalışır. Resmi destek şu anda sunulan değil.
-   **Özyineleme** -türlere yinelemeli olarak tanımlanan (örneğin, bir dizi kendileri için bakın) APIM tarafından desteklenmez.
-   **Birden çok ad** - birden çok ad şemada kullanılabilir, ancak yalnızca hedef ad alanı, ileti bölümlerini tanımlamak için kullanılabilir. Giriş veya çıkış diğer öğeleri tanımlamak için kullanılan ad alanları hedefinden korunmaz. Böyle bir WSDL belgesi aktarılabilen olsa da, dışarı aktarma üzerinde tüm ileti bölümleri WSDL hedef ad alanı olacaktır.
-   **Diziler** - SOAP ve REST örnekte gösterilen dizileri yalnızca sarmalanmış destekler dönüştürme:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"> </a>WADL

Şu anda bilinen WADL alma herhangi bir sorun vardır.
