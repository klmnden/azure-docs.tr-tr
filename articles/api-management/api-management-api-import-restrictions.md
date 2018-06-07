---
title: Azure API Management API alma bilinen sorunlar ve kısıtlama | Microsoft Docs
description: Bilinen sorunlar ve açık API, WSDL veya WADL biçimlerini kullanarak Azure API Management içe kısıtlamalar ayrıntıları.
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
ms.openlocfilehash: 03d785898398cb0bcd7b43e8d7feab705bce4b34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598479"
---
# <a name="api-import-restrictions-and-known-issues"></a>API içeri aktarma kısıtlamaları ve bilinen sorunlar
## <a name="about-this-list"></a>Bu liste hakkında
Bir API içeri aktarırken, bazı kısıtlamalar gelen veya başarıyla içeri aktarmadan önce düzeltilinceye gereken sorunları tanımlamak. Bu makale belgeleri bunlar, organize API tarafından alma biçimi.

## <a name="open-api"> </a>OpenAPI/Swagger
OpenAPI belgenizi alma hata alıyorsanız, doğrulandı, - ya da Tasarımcısı'nı kullanarak Azure portalında (Tasarım - ön uç - OpenAPI belirtimi Düzenleyicisi), emin olun veya bir üçüncü taraf aracı gibi <a href="http://www.swagger.io">Swagger Editor</a>.

* Yalnızca JSON biçimine OpenAPI için desteklenir.
* Gerekli Parametreler yol ve sorgu arasında benzersiz adlara sahip olmalıdır. (OpenAPI bir parametre adı yalnızca bir konum içinde örneğin yolu, sorgu, üstbilgi benzersiz olması gerekir.  Ancak, API Management'te biz işlemlerinin (hangi OpenAPI desteklemez) yol ve sorgu parametreleri tarafından ayrılmış izin verin. Bu nedenle biz parametre adları tüm URL şablonu içinde benzersiz olması gerekir.)
* Kullanarak başvurulan şemaları **$ref** özellikleri diğer içeremez **$ref** özellikleri.
* **$ref** işaretçileri dış dosyalar başvuruda bulunamaz.
* **x-ms-yolları** ve **x sunucuları** yalnızca desteklenen uzantıları.
* Özel uzantılar içeri aktarma işlemi sırasında yok sayılır ve kaydedilmez veya dışarı aktarma için korunur.

> [!IMPORTANT]
> OpenAPI içeri aktarma ile ilgili önemli bilgiler ve ipuçları için bu [belgeye](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/) bakın.

## <a name="wsdl"> </a>WSDL
WSDL dosyaları veya bir SOAP ve REST API'si arka ucu olarak hizmet SOAP doğrudan API'ları oluşturmak için kullanılır.
* **SOAP bağlamaları** -stil "belgesi" ve "hazır" kodlaması yalnızca SOAP bağlamaları desteklenir. "Rpc" stili veya SOAP kodlama için desteği yoktur.
* **WSDL: import** -bu öznitelik desteklenmiyor. Müşteriler, bir belgeye içeri aktarmalar birleştirmeniz gerekir.
* **Birden çok bölümü olan iletiler** -bu tür iletilerin desteklenmez.
* **WCF wsHttpBinding** -Windows Communication Foundation ile oluşturulan SOAP Hizmetleri basicHttpBinding kullanması gereken - wsHttpBinding desteklenmiyor.
* **MTOM** - MTOM kullanarak Hizmetleri <em>olabilir</em> çalışır. Resmi desteği şu anda önerilmez.
* **Özyineleme** -türlerini tanımlanan yinelemeli olarak (örneğin, bir dizi kendileri için bakın) APIM tarafından desteklenmez.

## <a name="wadl"> </a>WADL
Şu anda hiçbir bilinen WADL alma sorunları vardır.
