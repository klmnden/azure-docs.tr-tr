---
title: Azure AD Federasyon meta verileri | Microsoft Docs
description: "Bu makalede, Azure Active Directory belirteçleri kabul Hizmetleri için Azure Active Directory yayımladığı Federasyon meta veri belgesi açıklanmaktadır."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 58e5f62009e4e8b688108c6098ea8eabe8020e51
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="federation-metadata"></a>Federasyon meta verileri
Azure Active Directory (Azure AD) Azure AD verir güvenlik belirteçleri kabul etmesi için yapılandırılmış bir Federasyon meta veri belgesi Hizmetleri için yayımlar. Federasyon meta veri belgesi biçimi açıklanan [Web Hizmetleri Federasyon dili (WS-Federation) sürüm 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), genişleten [OASISgüvenlikonaylamaişlemibiçimlendirmedili(SAML)v2.0içinmetaveriler](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Kiracı özgü ve Kiracı bağımsız meta veri uç noktaları
Azure AD Kiracı özgü ve Kiracı bağımsız uç noktaları yayımlar.

Kiracı özel uç noktaları, belirli bir kiracı için tasarlanmıştır. Kiracı özgü Federasyon meta verilerinin Kiracı özgü düzenleyici ve uç nokta bilgileri de dahil olmak üzere, Kiracı hakkında bilgi içerir. Tek bir kiracı için erişimi kısıtlamak uygulamaları Kiracı özel uç noktaları kullanın.

Kiracı bağımsız uç noktalar tüm Azure AD kiracıları için genel bilgileri sağlayın. Konumunda barındırılan kiracılar bu bilgileri uygulandığı *login.microsoftonline.com* ve kiracılar arasında paylaşılır. Belirli bir kiracı ile ilişkili olmadığından Kiracı bağımsız uç noktaları çok kiracılı uygulamalar için önerilir.

## <a name="federation-metadata-endpoints"></a>Federasyon meta veri uç noktaları
Azure AD konumundaki Federasyon meta verilerini yayımlayan `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

İçin **Kiracı özel uç noktaları**, `TenantDomainName` aşağıdaki türlerden biri olabilir:

* Gibi bir kayıtlı etki alanı adı bir Azure ad Kiracı: `contoso.onmicrosoft.com`.
* Değişmez etki alanı kimliği gibi Kiracı `72f988bf-86f1-41af-91ab-2d7cd011db45`.

İçin **Kiracı bağımsız uç noktaları**, `TenantDomainName` olan `common`. Bu belge, login.microsoftonline.com barındırılan tüm Azure AD kiracıları için ortak olan Federasyon meta veri öğeleri listeler.

Örneğin, bir kiracı özel uç noktası olabilir `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Kiracı bağımsız uç noktası [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Bu URL'yi bir tarayıcıda yazarak, Federasyon meta veri belgesi görüntüleyebilirsiniz.

## <a name="contents-of-federation-metadata"></a>Federasyon meta veri içeriğini
Aşağıdaki bölümde, Azure AD tarafından yayınlanan belirteçleri tüketen Hizmetleri tarafından gereken bilgileri sağlar.

### <a name="entity-id"></a>Varlık Kimliği
`EntityDescriptor` Öğesi içeren bir `EntityID` özniteliği. Değeri `EntityID` öznitelik veren, belirteç veren diğer bir deyişle, güvenlik belirteci hizmeti (STS) temsil eder. Bir belirteç aldığınızda veren doğrulamak önemlidir.

Aşağıdaki meta verileri kiracıya özgü bir örnek gösterilir `EntityDescriptor` öğesi ile bir `EntityID` öğesi.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Kiracı bağımsız uç noktada Kiracı kimliği Kiracı özgü oluşturmak için Kiracı Kimliğinizle değiştirin `EntityID` değeri. Sonuç değeri Belirteç Verenin aynı olacaktır. Belirli bir kiracı için veren doğrulamak çok kiracılı uygulama stratejisi sağlar.

Aşağıdaki meta verileri, Kiracı bağımsız bir örnek göstermektedir `EntityID` öğesi. Lütfen aşağıdakilere dikkat edin, `{tenant}` sabit, yer tutucu değerdir.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Belirteç imzalama sertifikaları
Bir hizmeti bir Azure AD kiracısı tarafından verilen bir belirteç aldığında, Federasyon meta veri belgesi yayımlanan bir imzalama anahtarı ile belirtecinin imzası doğrulanması gerekir. Federasyon meta verilerinin kiracılar belirteç imzalama için kullanan sertifikaları ortak kısmını içerir. Sertifika ham bayt görünür `KeyDescriptor` öğesi. Belirteç imzalama sertifikasının yalnızca imzalamak için geçerli değeri `use` özniteliği `signing`.

Azure AD tarafından yayınlanan Federasyon meta veri belgesi imzalama sertifikasını güncelleştirmek Azure AD zaman hazırlama gibi birden çok imzalama anahtarı olabilir. Federasyon meta veri belgesi birden fazla sertifika bulunuyorsa, belirteçleri doğrularken bir hizmet belgesinde tüm sertifikaları desteklemelidir.

Aşağıdaki meta verileri bir örnek göstermektedir `KeyDescriptor` imzalama anahtarı ile öğesi.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

`KeyDescriptor` Öğe Federasyon meta veri belgesi; WS-Federasyon özgü bölüm ve SAML özgü bölümünde iki yerde görüntülenir. Her iki bölümlerde yayımlanan sertifikalar aynı olacaktır.

WS-Federasyon özgü bölümünde bir WS-Federasyon meta veri okuyucusu sertifikalardan okuduğunuz bir `RoleDescriptor` öğeyle `SecurityTokenServiceType` türü.

Aşağıdaki meta verileri bir örnek göstermektedir `RoleDescriptor` öğesi.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

SAML özgü bölümünde bir WS-Federasyon meta veri okuyucusu sertifikalardan okuduğunuz bir `IDPSSODescriptor` öğesi.

Aşağıdaki meta verileri bir örnek göstermektedir `IDPSSODescriptor` öğesi.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Kiracı özgü ve Kiracı bağımsız sertifikaların biçiminde farklılıkları yoktur.

### <a name="ws-federation-endpoint-url"></a>WS-Federasyon uç nokta URL'si
Federasyon meta verilerini Azure AD kullanımları tek oturum açma ve WS-Federasyon protokolünde oturum kapatma tek URL içerir. Bu uç noktaya görünür `PassiveRequestorEndpoint` öğesi.

Aşağıdaki meta verileri bir örnek göstermektedir `PassiveRequestorEndpoint` öğesi için bir kiracı özel uç noktası.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Kiracı bağımsız uç noktası için WS-Federasyon URL WS-Federasyon uç aşağıdaki örnekte gösterildiği gibi görünüyor.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML Protokolü uç nokta URL'si
Federasyon meta verilerinin tek oturum açma ve tek SAML 2.0 protokolü oturum kapatma için Azure AD kullanır URL'sini içerir. Bu uç noktalar görünür `IDPSSODescriptor` öğesi.

Oturum açma ve oturum kapatma URL'leri görünür `SingleSignOnService` ve `SingleLogoutService` öğeleri.

Aşağıdaki meta verileri bir örnek göstermektedir `PassiveResistorEndpoint` Kiracı özgü uç noktası için.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Benzer şekilde uç noktaları ortak SAML 2.0 protokolü uç noktaları için aşağıdaki örnekte gösterildiği gibi Kiracı bağımsız Federasyon meta verilerinde yayımlanır.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
