---
title: Azure AD Federasyon meta verileri | Microsoft Docs
description: Bu makalede, Azure Active Directory belirteçleri kabul Hizmetleri için Azure Active Directory yayımladığı Federasyon meta veri belgesi açıklanır.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcabf51b3a368841f7f135a32c4824eb3db571ee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299830"
---
# <a name="federation-metadata"></a>Federasyon meta verileri
Azure Active Directory (Azure AD), hizmetler için Azure AD sorunlarını güvenlik belirteçleri kabul edecek şekilde yapılandırılmış bir Federasyon meta veri belgesi yayımlar. Federasyon meta veri belge biçimi açıklanan [Web Hizmetleri Federasyon dili (WS-Federation) sürüm 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), genişleten [OASISgüvenlikonaylamaişlemibiçimlendirmedili(SAML)v2.0içinmetaverileri](https://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Kiracıya özgü ve Kiracı bağımsız meta veri uç noktaları
Azure AD Kiracı özgü ve Kiracı bağımsız uç noktaları yayımlar.

Kiracıya özel uç noktaları, belirli bir kiracı için tasarlanmıştır. Kiracıya özgü Federasyon meta verileri kiracıya özgü veren ve uç nokta bilgileri de dahil olmak üzere, Kiracı hakkındaki bilgileri içerir. Erişimi kısıtlamak için tek bir kiracının uygulamaları kiracıya özel uç noktaları kullanın.

Kiracı bağımsız uç noktaları tüm Azure AD kiracıları için ortak olan bilgiler sağlar. Bu bilgiler, barındırılan kiracılara geçerlidir *login.microsoftonline.com* ve kiracılar arasında paylaşılır. Belirli bir kiracı ile ilişkili olmadığından çok kiracılı uygulamalar için Kiracı bağımsız uç noktaları önerilir.

## <a name="federation-metadata-endpoints"></a>Federasyon meta veri uç noktaları
Azure AD Federasyon meta verileri en yayımlar `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

İçin **kiracıya özel uç noktaları**, `TenantDomainName` şu türlerden biri olabilir:

* Gibi bir kaydedilmiş bir etki alanı adını bir Azure AD Kiracı: `contoso.onmicrosoft.com`.
* Sabit gibi tenant ID etki alanının `72f988bf-86f1-41af-91ab-2d7cd011db45`.

İçin **Kiracı bağımsız uç noktaları**, `TenantDomainName` olduğu `common`. Bu belge, login.microsoftonline.com bulunan tüm Azure AD kiracıları için ortak olan Federasyon meta veri öğeleri listeler.

Örneğin, bir kiracıya özgü uç nokta olabilir `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Kiracı bağımsız uç nokta [ https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml ](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Bir tarayıcıda bu URL'yi yazarak, Federasyon meta veri belgesi görüntüleyebilirsiniz.

## <a name="contents-of-federation-metadata"></a>Federasyon meta verileri içeriği
Aşağıdaki bölümde, Azure AD tarafından yayınlanan belirteçleri tüketen hizmetler tarafından ihtiyaç duyulan bilgileri sağlar.

### <a name="entity-id"></a>Varlık Kimliği
`EntityDescriptor` Öğesi içeren bir `EntityID` özniteliği. Değerini `EntityID` öznitelik veren, belirteci veren diğer bir deyişle, güvenlik belirteci hizmeti (STS) temsil eder. Bir belirteci aldığınızda sağlayıcısını doğrulamak önemlidir.

Aşağıdaki meta verileri kiracıya özgü bir örneği gösterilmektedir. `EntityDescriptor` öğesi ile bir `EntityID` öğesi.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Kiracı kimliği Kiracı bağımsız uç noktasını bir kiracıya özgü oluşturmak için Kiracı Kimliğinizle değiştirin `EntityID` değeri. Sonuç değerini belirteci veren aynı olacaktır. Belirli bir kiracının sağlayıcısını doğrulamak çok kiracılı bir uygulama stratejisi sağlar.

Aşağıdaki meta verileri, Kiracı bağımsız bir örneği gösterilmektedir. `EntityID` öğesi. Lütfen aşağıdakilere dikkat edin, `{tenant}` yer tutucu bir değişmez değerdir.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Belirteç imzalama sertifikaları
Bir hizmeti bir Azure AD kiracısı tarafından verilen bir belirteç aldığında Federasyon meta veri belgesi yayımlanan bir imzalama anahtarı ile belirtecinin imzası doğrulanması gerekir. Ortak kısmını kiracılar belirteç imzalama için kullanan sertifikaları Federasyon meta verileri içerir. Sertifika ham bayt görünür `KeyDescriptor` öğesi. Belirteç imzalama sertifikasının yalnızca geçerli değerini `use` özniteliği `signing`.

Azure AD tarafından yayımlanmış Federasyon meta veri belgesi imzalama sertifikasını güncelleştirmek Azure AD, hazırlama gibi birden çok imza anahtarlarının olabilir. Federasyon meta veri belgesi birden fazla sertifika içerdiğinde, belirteçleri doğrulama hizmet belgedeki tüm sertifikaları desteklemelidir.

Aşağıdaki meta verileri, bir örneği gösterilmektedir. `KeyDescriptor` bir imzalama anahtarı ile öğesi.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

`KeyDescriptor` İki yerde Federasyon meta veri belgesi; WS-Federasyon özgü ve SAML özgü bölümleri öğe görüntülenir. Her iki bölümde yayımlanan sertifikalar aynı olacaktır.

WS-Federasyon özgü bölümünde, WS-Federasyon meta veri okuyucusu sertifikalardan okuduğunuz bir `RoleDescriptor` öğeyle `SecurityTokenServiceType` türü.

Aşağıdaki meta verileri, bir örneği gösterilmektedir. `RoleDescriptor` öğesi.

```
<RoleDescriptor xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:fed="https://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="https://docs.oasis-open.org/wsfed/federation/200706">
```

SAML özel bölümde, WS-Federasyon meta veri okuyucusu sertifikalardan okuduğunuz bir `IDPSSODescriptor` öğesi.

Aşağıdaki meta verileri, bir örneği gösterilmektedir. `IDPSSODescriptor` öğesi.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Kiracıya özgü ve Kiracı bağımsız sertifikaları biçimi hiçbir fark yoktur.

### <a name="ws-federation-endpoint-url"></a>WS-Federasyon uç nokta URL'si
Federasyon meta verileri Azure AD kullanımları çoklu oturum açma ve WS-Federation Protokolü oturum kapatma tek URL içerir. Bu uç nokta görünür `PassiveRequestorEndpoint` öğesi.

Aşağıdaki meta verileri, bir örneği gösterilmektedir. `PassiveRequestorEndpoint` bir kiracıya özgü uç nokta için öğesi.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Kiracı bağımsız uç noktası için WS-Federasyon URL'sini WS-Federasyon uç noktası aşağıdaki örnekte gösterildiği gibi görünür.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokol uç nokta URL'si
Federasyon meta verileri, Azure AD çoklu oturum açma ve kapatma SAML 2.0 protokolü tek kullanan bir URL içerir. Bu uç noktaları görünür `IDPSSODescriptor` öğesi.

Oturum açma ve oturum kapatma URL'leri görünür `SingleSignOnService` ve `SingleLogoutService` öğeleri.

Aşağıdaki meta verileri, bir örneği gösterilmektedir. `PassiveResistorEndpoint` bir kiracıya özgü uç nokta için.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Benzer şekilde ortak SAML 2.0 protokolü bitiş noktası uç noktaları aşağıdaki örnekte gösterildiği gibi Kiracı bağımsız Federasyon meta verilerinde yayımlanır.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
