---
title: API Management - Azure API Management sertifika kimlik doğrulaması kullanan istemci API'ları güvenli | Microsoft Docs
description: İstemci sertifikalarını kullanan API'lerine erişimi güvenli öğrenin
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
ms.openlocfilehash: 841825923819bdb257e5b5983071d999cca805e9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
ms.locfileid: "26406752"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Sertifika kimlik doğrulaması API Management istemcisini kullanarak API güvenliğini sağlama

API Management güvenli API'leri (yani, API Management istemcisini) erişim olanağı sunar istemci sertifikalarını kullanan. Şu anda, istenen değerle istemci sertifikanın parmak izini kontrol edebilirsiniz. Ayrıca karşı API Management yüklenen mevcut sertifika parmak izini kontrol edebilirsiniz.  

İstemci sertifikalarını (yani, arka uç için API Management) kullanan bir API'nin arka uç hizmetine erişim güvenliğini sağlama hakkında daha fazla bilgi için bkz: [arka uç hizmetlerini kullanan istemci sertifikası kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

## <a name="checking-the-expiration-date"></a>Sona erme tarihini denetleniyor

İlkeleri, sertifikanın geçerlilik süresinin dolup olmadığını denetlemek için yapılandırılabilir:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a>Sertifikayı veren ve konu denetleniyor

İlkeleri veren ve bir istemci sertifikasının konu denetlemek için yapılandırılabilir:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a>Parmak izi denetleniyor

İlkeleri, bir sertifikanın parmak izini istemci denetlemek için yapılandırılabilir:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API Management karşıya bir parmak izi sertifikalara karşı denetleniyor

Aşağıdaki örnek, bir sertifikanın parmak izini istemci API Management karşıya sertifikalara karşı denetleyin gösterilmektedir: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Sonraki adım

*  [Arka uç hizmetlerini kullanan istemci sertifikası kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
*  [Nasıl sertifikalarını karşıya yükleme](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

