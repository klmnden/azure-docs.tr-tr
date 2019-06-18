---
title: Sayfa sözleşmesi - Azure Active Directory B2C'yi seçin. | Microsoft Docs
description: Azure Active Directory B2C'de bir sayfa sözleşme seçme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7aab43695f0b11590d8bd2aa011073ba04d95250
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66513006"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Özel ilkeleri kullanarak, Azure Active Directory B2C, bir sayfa sözleşme seçin

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Kullanıcı akışları veya özel ilkeler kullanıyor olursanız olun, Azure Active Directory (Azure AD) B2C ilkelerinizi JavaScript istemci tarafı kod etkinleştirebilirsiniz. Uygulamalarınız için JavaScript'i etkinleştirmek için bir öğeye ekleyin, [özel ilke](active-directory-b2c-overview-custom.md), sayfa sözleşme seçin ve [b2clogin.com](b2clogin.md) isteklerinizdeki. Bir sayfa sözleşme, Azure AD B2C sağlayan öğeleri ve sağladığınız içeriği işbirliğidir. Bu makalede, özel bir ilke yapılandırarak Azure AD B2C'de bir sayfa sözleşme seçin anlatılmaktadır.

> [!NOTE]
> Kullanıcı akışları için JavaScript etkinleştirmek istiyorsanız, bkz. [sözleşme JavaScript ve sayfa, Azure Active Directory B2C sürümlerinde](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>DataUri değerleri Değiştir

Özel ilkelerinizi etkinleştirmiş olabilirsiniz [ContentDefinitions](contentdefinitions.md) kullanıcı yolculuğunda kullanılan HTML şablonları tanımlayın. **ContentDefinition** içeren bir **DataUri** Azure AD B2C tarafından sağlanan sayfa öğelerine başvurur. **LoadUri** sağladığınız HTML ve CSS içeriği göreli yolu.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Bir sayfa sözleşme seçmek için değiştirme **DataUri** değerler, [ContentDefinitions](contentdefinitions.md) ilkelerinizi içinde. Eski geçiş tarafından **DataUri** değerleri için yeni değerleri, sabit bir paket seçeneğini belirliyoruz. Bu paket kullanmanın avantajı, değiştirme yüklemeyecekseniz ve beklenmeyen davranışlara neden sayfanızda anlarsınız ' dir. 

Bir sayfa anlaşmasını oluşturan ayarlanacak bulmak için aşağıdaki tabloyu kullanın **DataUri** değerleri. 

| Eski DataUri değer | Yeni DataUri değeri |
| ----------------- | ----------------- |
| urn: com:microsoft:aad:b2c:elements:idpselection:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:unifiedssd:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0 | 
| urn: com:microsoft:aad:b2c:elements:claimsconsent:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:multifactor:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:multifactor:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0 |
| urn: com:microsoft:aad:b2c:elements:selfasserted:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:selfasserted:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0 | 
| urn: com:microsoft:aad:b2c:elements:unifiedssp:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:unifiedssp:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0 |
| urn: com:microsoft:aad:b2c:elements:globalexception:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:globalexception:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0 |

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı arabirimi, uygulamalarınızın nasıl özelleştirebileceğiniz hakkında daha fazla bilgi [özel bir ilke kullanarak Azure Active Directory B2C'de, uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md).



