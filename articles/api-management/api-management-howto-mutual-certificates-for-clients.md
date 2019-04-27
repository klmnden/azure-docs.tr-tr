---
title: Sertifika kimlik doğrulaması API Management - Azure API Management istemcisini kullanarak API'leri güvenli hale getirme | Microsoft Docs
description: İstemci sertifikaları kullanarak API'leri güvenli öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 450ebc621758363c5ea9ab6d631cd6c7df38794b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657722"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Sertifika kimlik doğrulaması API Management istemcisini kullanarak API güvenliğini sağlama

API Management API'leri (yani, API Management istemciden) erişimin güvenliğini sağlama olanağı sunuyor istemci sertifikaları kullanarak. Şu anda, istenen değerle bir istemci sertifikasının parmak izini kontrol edebilirsiniz. Ayrıca, karşı API Management'a yüklenen mevcut sertifika parmak izini kontrol edebilirsiniz.  

İstemci sertifikalarını (yani, arka uç API Management'a) kullanarak bir API'nin arka uç hizmetine erişim güvenliğini sağlama hakkında daha fazla bilgi için bkz: [istemcisini kullanarak arka uç Hizmetleri, sertifika kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="checking-the-expiration-date"></a>Sona erme tarihini denetleniyor

Sertifikanın süresi dolmuş olmadığını denetlemek için ilkeleri yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a>Sertifikayı veren ve konu denetleniyor

Bir istemci sertifikasının konu ve veren denetlemek için ilkeleri yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a>Parmak denetimi

Bir istemci sertifikası parmak izi denetlemek için ilkeleri yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API yönetimi için karşıya bir parmak izi sertifikalara karşı denetleniyor

Aşağıdaki örnekte, nasıl bir istemci sertifikası karşı API Management'a yüklenen sertifikaların parmak izi kontrol edileceğini göstermektedir: 

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Sonraki adım

*  [Arka uç Hizmetleri istemcisini kullanarak sertifika kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
*  [Sertifikaları karşıya yükleme](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

