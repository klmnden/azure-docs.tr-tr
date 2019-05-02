---
title: Azure çoklu oturum açma SAML Protokolü | Microsoft Docs
description: Bu makalede, Azure Active Directory'de bulunan tek oturum SAML Protokolü
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 033740d1ae75bb6f6fe8509d9ad123d55d9c6770
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704996"
---
# <a name="single-sign-on-saml-protocol"></a>Çoklu oturum açma SAML Protokolü

Bu makale, Azure Active Directory (Azure AD) için çoklu oturum açmayı destekleyen yanıtlar ve SAML 2.0 kimlik doğrulama isteklerini kapsar.

Aşağıdaki Protokolü diyagramda tek oturum açma sıralamasını açıklar. Bulut hizmeti (hizmet sağlayıcı) geçirmek için bir HTTP yeniden yönlendirme bağlamasında kullanan bir `AuthnRequest` Azure AD'ye (kimlik doğrulama isteği) öğesi (Kimlik sağlayıcısı yerine). Ardından Azure AD bağlama göndermek için HTTP post kullanan bir `Response` bulut hizmeti için öğesi.

![Çoklu oturum açma iş akışı](./media/single-sign-on-saml-protocol/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Bulut Hizmetleri gönderme için kullanıcı kimlik doğrulaması istemek üzere bir `AuthnRequest` Azure AD'ye öğesi. Bir örnek SAML 2.0 `AuthnRequest` aşağıdaki örnekteki gibi görünebilir:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kimlik | Gerekli | Azure AD doldurmak için bu özniteliği kullanan `InResponseTo` döndürülen yanıtın özniteliği. Bir GUID dize gösterimi için "id" gibi bir dize önüne eklediğinizden ortak bir strateji, bu nedenle kimliği bir sayı ile başlayamaz. Örneğin, `id6c1c178c166d486687be4aaf5e482730` geçerli kimliğidir. |
| Version | Gerekli | Bu parametre ayarlanmalıdır **2.0**. |
| IssueInstant | Gerekli | DateTime UTC değeri dize budur ve [gidiş dönüş biçim ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD, bu türde bir DateTime değeri Bekliyor ancak değil değerlendirmek veya değeri kullanın. |
| AssertionConsumerServiceUrl | İsteğe bağlı | Bu parametre belirtilmezse, eşleşmelidir `RedirectUri` Azure ad bulut hizmeti. |
| ForceAuthn | İsteğe bağlı | Bir boolean değer budur. TRUE ise, Azure AD ile geçerli bir oturum olsa bile yeniden kimlik doğrulaması için kullanıcı zorlanır anlamına gelir. |
| IsPassive | İsteğe bağlı | Azure AD kullanıcı sessizce, kullanıcı etkileşimi olmadan oturum tanımlama bilgisi varsa kullanarak kimlik doğrulamalıdır olup olmadığını belirten bir Boole değeri budur. True ise, Azure AD oturum tanımlama bilgisini kullanarak kullanıcı kimlik doğrulaması yapmayı deneyeceksiniz. |

Diğer tüm `AuthnRequest` onay, hedef, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex ve ProviderName gibi öznitelikleri **göz ardı**.

Azure AD ayrıca yoksayar `Conditions` öğesinde `AuthnRequest`.

### <a name="issuer"></a>Veren

`Issuer` Öğesinde bir `AuthnRequest` biriyle tam olarak eşleşmelidir **ServicePrincipalNames** Azure ad'deki bulut hizmetinde. Genellikle, bu ayar **uygulama kimliği URI'si** uygulama kaydı sırasında belirtilir.

İçeren bir SAML alıntı `Issuer` öğesi, aşağıdaki örneğe benzer görünür:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Bu öğe bir belirli ad kimliği biçimi yanıt isterse ve isteğe bağlı olarak `AuthnRequest` Azure AD'ye gönderilen öğeleri.

A `NameIdPolicy` öğesi, aşağıdaki örneğe benzer görünür:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Varsa `NameIDPolicy` , isteğe bağlı içerebilir sağlanır `Format` özniteliği. `Format` Özniteliği yalnızca aşağıdakilerden biri olabilir değerleri; herhangi bir değer sonuç hata.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory, ikili bir tanımlayıcı olarak Nameıd talebi verir.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory, e-posta adresi biçiminde Nameıd talebi verir.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Bu değer, talep biçimini seçmek için Azure Active Directory izin verir. Azure Active Directory Nameıd ikili bir tanımlayıcı olarak yayınlar.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Active Directory, geçerli SSO işlemi için benzersiz olan rastgele oluşturulmuş bir değer olarak Nameıd talebi verir. Başka bir deyişle, değer geçicidir ve kimlik doğrulaması yapan kullanıcıyı tanımlamak için kullanılamaz.

Azure AD yoksayar `AllowCreate` özniteliği.

### <a name="requestauthncontext"></a>RequestAuthnContext
`RequestedAuthnContext` Öğesi istenen kimlik doğrulama yöntemlerini belirtir. İsteğe bağlı olarak `AuthnRequest` Azure AD'ye gönderilen öğeleri. Azure AD destekler `AuthnContextClassRef` gibi değerler `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Kapsam belirleme
`Scoping` Kimlik sağlayıcılarının listesi içeren bir öğesi isteğe bağlı olarak `AuthnRequest` Azure AD'ye gönderilen öğeleri.

Sağlanırsa, içermez `ProxyCount` özniteliği `IDPListOption` veya `RequesterID` öğesi, desteklenen değildir.

### <a name="signature"></a>İmza
İçermeyen bir `Signature` öğesinde `AuthnRequest` öğeleri, Azure AD desteklemediğinden kimlik doğrulama isteklerini imzalanmış.

### <a name="subject"></a>Subject
Azure AD yoksayar `Subject` öğesinin `AuthnRequest` öğeleri.

## <a name="response"></a>Yanıt
Bir istenen oturum açma başarıyla tamamlandığında, Azure AD bulut hizmeti için bir yanıt gönderir. Başarılı bir oturum açma girişimi yanıt aşağıdaki örneğe benzer şekilde görünür:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Yanıt

`Response` Öğesi yetkilendirme isteğinin sonucunu içerir. Azure AD kümeleri `ID`, `Version` ve `IssueInstant` değerler `Response` öğesi. Ayrıca, aşağıdaki öznitelikleri ayarlar:

* `Destination`: Oturum açma başarıyla tamamlandığında, bu ayar `RedirectUri` hizmet sağlayıcısının (bulut hizmeti).
* `InResponseTo`: Bu ayar `ID` özniteliği `AuthnRequest` yanıt başlatılan öğesi.

### <a name="issuer"></a>Veren

Azure AD kümeleri `Issuer` öğesine `https://login.microsoftonline.com/<TenantIDGUID>/` burada \<TenantIDGUID > Azure AD kiracısını Kiracı Kimliğini gösterir.

Örneğin, veren öğesi ile bir yanıt aşağıdaki örneğe benzer görünebilir:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Durum

`Status` Öğenin ilettiği başarılı veya başarısız oturum açma. İçerdiği `StatusCode` bir kod veya isteğinin durumunu temsil eden iç içe geçmiş kodları kümesi içeren öğe. Ayrıca `StatusMessage` oturum açma işlemi sırasında oluşturulan özel hata iletileri içeren öğe.

<!-- TODO: Add an authentication protocol error reference -->

Aşağıdaki örnek, bir SAML yanıtını başarısız oturum açma denemesi için ' dir.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Onaylama işlemi

Ek olarak `ID`, `IssueInstant` ve `Version`, Azure AD şu öğeleri ayarlar `Assertion` yanıtın öğesi.

#### <a name="issuer"></a>Veren

Bu ayar `https://sts.windows.net/<TenantIDGUID>/`burada \<TenantIDGUID > Azure AD kiracısını Kiracı Kimliğini gösterir.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>İmza

Azure AD, onaylama işlemi yanıt olarak bir başarılı oturum açma imzalar. `Signature` Öğesi bulut hizmeti onaylama bütünlüğünü doğrulamak için kaynak kimliğini doğrulamak için kullanabileceğiniz bir dijital imza içeriyor.

Bu dijital imzayı üretmek için Azure AD imzalama anahtarı kullanan `IDPSSODescriptor` , meta veri belgesi öğesidir.

```
<ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Subject

Bu, sahibi olan onaylama deyimlerinde sorumlu belirtir. İçerdiği bir `NameID` öğesi, kimliği doğrulanmış bir kullanıcıyı temsil eder. `NameID` Belirteç hedef kitlesi hizmet sağlayıcısı yönlendirildiği bir hedeflenen tanımlayıcı bir değerdir. Kalıcı - iptal edilebilir, ancak hiçbir zaman atanır. Kullanıcı ile ilgili herhangi bir şey açığa çıkarmadığınızdan ve öznitelik sorgular için bir tanımlayıcı olarak kullanılamaz, ayrıca, donuktur.

`Method` Özniteliği `SubjectConfirmation` her zaman ayarlanır `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Koşullar

Bu öğe, SAML onaylamalarını kullanımını kabul edilebilir tanımlayan koşulları belirtir.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` Ve `NotOnOrAfter` öznitelikler sırasında onaylama geçerli aralığı belirtin.

* Değerini `NotBefore` gelen veya biraz özniteliği eşittir (bir saniyeden az) değerini daha sonra `IssueInstant` özniteliği `Assertion` öğesi. Azure AD, kendisini (hizmet sağlayıcı) bulut hizmeti arasında zaman fark hesaplamaz ve şu anda herhangi bir arabellek eklemez.
* Değerini `NotOnOrAfter` özniteliktir 70 dakika değerini daha sonra `NotBefore` özniteliği.

#### <a name="audience"></a>Hedef kitle

Bu, bir hedef kitle tanımlayan bir URI içeriyor. Azure AD, bu öğenin değeri değerine ayarlar `Issuer` öğesinin `AuthnRequest` başlatılan oturum açma. Değerlendirilecek `Audience` değeri, değerini kullanın `App ID URI` uygulama kaydı sırasında belirtildi.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Gibi `Issuer` değeri `Audience` değeri tam olarak eşleşmelidir bulut hizmeti Azure AD'de temsil eden hizmet asıl adlarına biri. Ancak, varsa değerini `Issuer` öğesi bir URI değeri değil `Audience` yanıt değer `Issuer` değeri önekiyle `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Bu konu veya kullanıcı hakkında talepleri içerir. Aşağıdaki alıntıda bir örnek içeren `AttributeStatement` öğesi. Öğe birden çok öznitelik ve öznitelik değerleri içerebilir, nokta gösterir.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```        

* **Talep adı** -değerini `Name` özniteliği (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) olduğu gibi kimliği doğrulanmış kullanıcının kullanıcı asıl adını `testuser@managedtenant.com`.
* **Objectıdentifier talep** -değerini `ObjectIdentifier` özniteliği (`http://schemas.microsoft.com/identity/claims/objectidentifier`) olan `ObjectId` dizin nesnesinin kimliği doğrulanmış kullanıcının Azure AD'de temsil eder. `ObjectId` bir sabit, genel olarak benzersiz olan ve kimliği doğrulanmış kullanıcının güvenli tanımlayıcısı yeniden kullanabilirsiniz.

#### <a name="authnstatement"></a>AuthnStatement

Bu öğe, onaylama işlemi konu belirli bir zamandaki belirli bir şekilde doğrulandı onaylar.

* `AuthnInstant` Özniteliği ile Azure AD, kullanıcının kimliğinin süreyi belirtir.
* `AuthnContext` Öğesi, kullanıcının kimliğini doğrulamak için kullanılan kimlik doğrulaması bağlamı belirtir.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
