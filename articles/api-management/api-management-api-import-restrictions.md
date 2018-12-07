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
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: c55a80749506b0a03af2f8c5f0179b67c8a78d15
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016751"
---
# <a name="api-import-restrictions-and-known-issues"></a>API içeri aktarma kısıtlamaları ve bilinen sorunlar
## <a name="about-this-list"></a>Bu liste hakkında
API içeri aktarılırken arasında bazı kısıtlamalar gelen veya başarıyla içeri aktarmadan önce düzeltilmesi gereken sorunları belirlemek. Bu makalede belgeleri bunlar düzenlenmiş tarafından API içeri aktarma biçimi.

## <a name="open-api"> </a>Openapı/Swagger
Openapı belgenizi alma hataları alıyorsanız, doğrulandı, - ya da Azure portalında (Tasarım - ön uç - Openapı belirtimi Düzenleyicisi), tasarımcıyı kullanarak sağlamak veya bir üçüncü taraf aracı gibi <a href="https://editor.swagger.io">Swagger Editor</a>.

* Yalnızca JSON biçimi için Openapı desteklenir.
* Gerekli parametreleri yolu hem de sorgu genelinde benzersiz adlara sahip olmalıdır. (Openapı bir parametre adı yalnızca içinde örneğin yol, sorgu, üst bilgisi bir konumu benzersiz olması gerekir.  Ancak, API Yönetimi'nde (Bu Openapı desteklemez) yolu hem de sorgu parametreleri tarafından ayrılmış işlemleri izin veriyoruz. Bu nedenle parametre adları tüm URL şablonu içinde benzersiz olması zorunlu kılarız.)
* Kullanarak başvurulan şemalar **$ref** özellikleri diğer içeremez **$ref** özellikleri.
* **$ref** işaretçileri, harici dosyalara başvuruda bulunamaz.
* **x-ms-yolları** ve **x sunucuları** uzantıları yalnızca desteklenir.
* Özel uzantılar içeri aktarma işlemi sırasında yok sayılır ve kaydedilmez veya dışarı aktarma için korunur.
* **Özyineleme** -olan tanımlara yinelemeli olarak tanımlanan (kendilerini gibi bakın) APIM tarafından desteklenmez.

> [!IMPORTANT]
> OpenAPI içeri aktarma ile ilgili önemli bilgiler ve ipuçları için bu [belgeye](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) bakın.

## <a name="wsdl"> </a>WSDL
WSDL dosyaları, SOAP doğrudan API'ları oluşturmak veya bir SOAP ve REST API arka uç olarak sunmak için kullanılır.
* **SOAP bağlamaları** -style "belgesi" ve "değişmez" kodlama yalnızca SOAP bağlamaları desteklenir. "Rpc" stil veya SOAP kodlamasına için desteği yoktur.
* **WSDL: import** -bu özniteliği desteklenmiyor. Müşteriler, bir belgeye Imports birleştirmeniz gerekir.
* **Birden çok bölümü olan iletiler** -iletileri bu tür desteklenmiyor.
* **WCF wsHttpBinding** -Windows Communication Foundation ile oluşturulan SOAP Hizmetleri basicHttpBinding kullanması gereken - wsHttpBinding desteklenmiyor.
* **MTOM** - MTOM kullanan hizmetler <em>olabilir</em> çalışır. Resmi destek şu anda sunulmaz.
* **Özyineleme** -türlere yinelemeli olarak tanımlanan (örneğin, bir dizi kendileri için bakın) APIM tarafından desteklenmez.

## <a name="wadl"> </a>WADL
Şu anda bilinen WADL alma herhangi bir sorun vardır.
