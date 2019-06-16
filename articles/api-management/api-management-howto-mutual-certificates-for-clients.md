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
ms.date: 05/30/2019
ms.author: apimpm
ms.openlocfilehash: 5427c4050b6b70c18da7a1899d16e448c41e81c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427308"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Sertifika kimlik doğrulaması API Management istemcisini kullanarak API güvenliğini sağlama

API Management API'leri (yani, API Management istemciden) erişimin güvenliğini sağlama olanağı sunuyor istemci sertifikaları kullanarak. Gelen sertifikasını doğrulamak ve sertifika özellikleri kullanım ilkesi ifadeleri istenen değerleri karşı denetleyin.

İstemci sertifikalarını (yani, arka uç API Management'a) kullanarak bir API'nin arka uç hizmetine erişim güvenliğini sağlama hakkında daha fazla bilgi için bkz: [istemcisini kullanarak arka uç Hizmetleri, sertifika kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

> [!IMPORTANT]
> Alma ve tüketim katmanındaki istemci sertifikalarını doğrulamak için önce "istek istemci sertifikası"Özel etki alanları"dikey penceresinde, aşağıda gösterildiği gibi ayarlama" etkinleştirmeniz gerekir.

![İstemci sertifikası isteme](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Sertifikayı veren ve konu denetleniyor

Bir istemci sertifikasının konu ve veren denetlemek için ilkeleri yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Denetimi sertifika iptal listesi kullanımı devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` yerine `context.Request.Certificate.Verify()`.
> İstemci sertifika kendinden imzalı ise, kök (veya Ara) CA sertifikaları olmalıdır [karşıya](api-management-howto-ca-certificates.md) için API Management'tan `context.Request.Certificate.Verify()` ve `context.Request.Certificate.VerifyNoRevocation()` çalışmak için.

## <a name="checking-the-thumbprint"></a>Parmak denetimi

Bir istemci sertifikası parmak izi denetlemek için ilkeleri yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Denetimi sertifika iptal listesi kullanımı devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` yerine `context.Request.Certificate.Verify()`.
> İstemci sertifika kendinden imzalı ise, kök (veya Ara) CA sertifikaları olmalıdır [karşıya](api-management-howto-ca-certificates.md) için API Management'tan `context.Request.Certificate.Verify()` ve `context.Request.Certificate.VerifyNoRevocation()` çalışmak için.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API yönetimi için karşıya bir parmak izi sertifikalara karşı denetleniyor

Aşağıdaki örnekte, nasıl bir istemci sertifikası karşı API Management'a yüklenen sertifikaların parmak izi kontrol edileceğini göstermektedir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Denetimi sertifika iptal listesi kullanımı devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` yerine `context.Request.Certificate.Verify()`.
> İstemci sertifika kendinden imzalı ise, kök (veya Ara) CA sertifikaları olmalıdır [karşıya](api-management-howto-ca-certificates.md) için API Management'tan `context.Request.Certificate.Verify()` ve `context.Request.Certificate.VerifyNoRevocation()` çalışmak için.

> [!TIP]
> Bu konuda açıklanan istemci sertifikası kilitlenme sorunu [makale](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) kendisini çeşitli yollarla bildirim, örneğin istekleri dondurma, istekleri sonucu `403 Forbidden` aşımından sonra durum kodu `context.Request.Certificate` olduğu `null`. Bu sorun genellikle etkiler `POST` ve `PUT` istekleri içerik uzunluğu yaklaşık 60 KB veya daha büyük.
> Bu sorunu gerçekleşmesini önlemek için aşağıda gösterildiği gibi "Özel etki alanları" dikey penceresinde istenen ana bilgisayar adları için "Anlaş istemci sertifikası" ayarı açın. Bu özellik tüketim katmanında kullanılamıyor.

![İstemci sertifikası için anlaş](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

-   [Arka uç Hizmetleri istemcisini kullanarak sertifika kimlik doğrulaması güvenliğini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [Sertifikaları karşıya yükleme](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
