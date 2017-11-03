---
title: "Azure çoklu oturum açma SAML Protokolü | Microsoft Docs"
description: "Bu makalede Azure Active Directory'de üzerinde tek oturum SAML Protokolü"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: f41402fc2cb282975b93071d998365fdb0a21941
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# Çoklu oturum açma SAML Protokolü
Bu makalede Azure Active Directory (Azure AD) için çoklu oturum açmayı destekleyen yanıtlar ve SAML 2.0 kimlik doğrulama isteklerini kapsar.

Aşağıdaki Protokolü diyagram tek oturum açma sırası açıklar. Geçirmek için bir HTTP yeniden yönlendirme bağlama (hizmet sağlayıcısı) bulut hizmeti kullanan bir `AuthnRequest` Azure ad (kimlik doğrulama isteği) öğesi (Kimlik sağlayıcısı). Ardından Azure AD sonrası bağlama HTTP post kullanan bir `Response` bulut hizmetine öğesi.

![Çoklu oturum açma iş akışı](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## AuthnRequest
Bulut Hizmetleri gönderme kullanıcı kimlik doğrulaması istemek için bir `AuthnRequest` Azure ad öğesi. Örnek SAML 2.0 `AuthnRequest` şöyle:

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
| Kimlik |Gerekli |Azure AD doldurmak için bu öznitelik kullanan `InResponseTo` döndürülen yanıtın özniteliği. Kimliği bir GUID dize gösterimini için "id" gibi bir dizeyi önüne eklediğinizden ortak bir strateji olacak şekilde bir rakamla başlayamaz. Örneğin, `id6c1c178c166d486687be4aaf5e482730` geçerli kimliğidir. |
| Sürüm |Gerekli |Bu olmalıdır **2.0**. |
| IssueInstant |Gerekli |Bu, UTC değerine sahip DateTime dizedir ve [gidiş dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD bu türde bir DateTime değer bekler ancak değerlendirmek veya değerini kullanın. |
| AssertionConsumerServiceUrl |İsteğe bağlı |Sağlanırsa, bu eşleşmelidir `RedirectUri` Azure AD bulut hizmetinin. |
| ForceAuthn |İsteğe bağlı | Bu mantıksal bir değerdir. TRUE ise, bu geçerli bir oturum Azure AD ile olsa bile yeniden kimlik doğrulaması için kullanıcı zorlanır anlamına gelir. |
| IsPassive |İsteğe bağlı |Azure AD kullanıcı sessizce, kullanıcı etkileşimi olmadan oturum tanımlama bilgisi varsa kullanarak kimlik doğrulamalıdır olup olmadığını belirten bir Boole değeri budur. True ise, Azure AD oturum tanımlama bilgisi kullanarak kullanıcı kimlik doğrulamasını dener. |

Diğer tüm `AuthnRequest` öznitelikleri gibi onay, hedef, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex ve ProviderName **göz ardı**.

Azure AD de yoksayar `Conditions` öğesinde `AuthnRequest`.

### Veren
`Issuer` Öğesinde bir `AuthnRequest` tam olarak eşleşmelidir **ServicePrincipalNames** Azure AD bulut hizmetinde. Genellikle, bu ayarlanır **uygulama kimliği URI'si** uygulama kaydı sırasında belirtilir.

Bir örnek SAML alıntı içeren `Issuer` öğesi şu şekilde görünür:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### NameIDPolicy
Bu öğe bir belirli ad kimliği biçimi yanıt istediğinde ve isteğe bağlı olarak `AuthnRequest` Azure AD ile gönderilmiş öğeler.

Bir örnek `NameIdPolicy` öğesi şu şekilde görünür:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Varsa `NameIDPolicy` , isteğe bağlı içerebilir sağlanmış `Format` özniteliği. `Format` Özniteliği yalnızca aşağıdakilerden biri olabilir değerleri; herhangi bir değer sonuç hata.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory ikili bir tanımlayıcı olarak NameID talebi verir.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory e-posta adresi biçiminde NameID talebi verir.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Bu değer talep biçimini seçmek için Azure Active Directory izin verir. Azure Active Directory NameID ikili bir tanımlayıcı olarak verir.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Active Directory geçerli SSO işlemi için benzersiz olan rastgele oluşturulan bir değer olarak NameID talebi verir. Bunun anlamı değeri geçicidir ve kimlik doğrulaması yapan kullanıcıyı tanımlamak için kullanılamaz.

Azure AD yoksayar `AllowCreate` özniteliği.

### RequestAuthnContext
`RequestedAuthnContext` Öğesi istenen kimlik doğrulama yöntemlerini belirtir. İsteğe bağlı olarak `AuthnRequest` Azure AD ile gönderilmiş öğeler. Azure AD destekleyen tek `AuthnContextClassRef` değeri: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### Kapsamı
`Scoping` Kimlik sağlayıcıları listesini içeren öğesi, isteğe bağlı olarak `AuthnRequest` Azure AD ile gönderilmiş öğeler.

Sağlanırsa, içermeyen `ProxyCount` özniteliği `IDPListOption` veya `RequesterID` öğesi, desteklenmediği gibi.

### İmza
İçermeyen bir `Signature` öğesinde `AuthnRequest` öğeleri, Azure AD desteklemediği oturumu olarak kimlik doğrulama istekleri.

### Konu
Azure AD yoksayar `Subject` öğesinin `AuthnRequest` öğeleri.

## Yanıt
Bir istenen oturum açma işlemi başarıyla tamamlandığında, Azure AD bulut hizmeti için bir yanıt gönderir. Başarılı bir oturum açma girişimi örnek yanıt şöyle görünür:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
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

### Yanıt
`Response` Öğesi yetkilendirme isteğinin sonucunu içerir. Azure AD kümeleri `ID`, `Version` ve `IssueInstant` değerler `Response` öğesi. Ayrıca, aşağıdaki öznitelikler ayarlar:

* `Destination`: Oturum açma başarıyla tamamlandığında, bu ayarlanır `RedirectUri` hizmet sağlayıcısının (bulut hizmeti).
* `InResponseTo`: Bu ayar `ID` özniteliği `AuthnRequest` yanıt başlatılan öğesi.

### Veren
Azure AD kümeleri `Issuer` öğesine `https://login.microsoftonline.com/<TenantIDGUID>/` burada <TenantIDGUID> Azure AD kiracısı Kiracı kimliğidir.

Örneğin, bir örnek yanıt veren öğesiyle şöyle:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### Durum
`Status` Öğenin ilettiği başarılı veya başarısız oturum açma. İçerdiği `StatusCode` bir kod veya isteğin durumunu temsil eden iç içe geçmiş kodları kümesi içeren öğe. Ayrıca içerir `StatusMessage` oturum açma işlemi sırasında oluşturulan özel hata iletileri içeren öğe.

<!-- TODO: Add a authentication protocol error reference -->

Başarısız oturum açma girişiminde SAML yanıt verilmiştir.

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

### onaylama işlemi
Ek olarak `ID`, `IssueInstant` ve `Version`, Azure AD aşağıdaki öğeleri ayarlar `Assertion` yanıt öğesidir.

#### Veren
Bu ayar `https://sts.windows.net/<TenantIDGUID>/`burada <TenantIDGUID> Azure AD kiracısı Kiracı kimliğidir.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### İmza
Azure AD yanıt olarak bir başarılı oturum açma onaylama imzalar. `Signature` Öğe bulut hizmeti onaylama bütünlüğünü doğrulamak için kaynak kimliğini doğrulamak için kullanabileceğiniz dijital bir imza içeriyor.

Bu dijital imzayı üretmek için imzalama anahtarı Azure AD kullanır `IDPSSODescriptor` meta veri belgesi öğesidir.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### Konu
Onaylama işlemi deyimlerinde konu asıl belirtir. İçerdiği bir `NameID` kimliği doğrulanmış kullanıcının temsil eden öğe. `NameID` Yalnızca belirteç kitlesi hizmet sağlayıcısına yönelik hedeflenen bir tanımlayıcı bir değerdir. Kalıcı - iptal edilebilir, ancak hiçbir zaman atanır. Da opak, olması kullanıcı hakkında hiçbir şey açığa çıkarmadığınızdan ve öznitelik sorguları için tanımlayıcı olarak kullanılamaz.

`Method` Özniteliği `SubjectConfirmation` öğesi ayarlanmış her zaman `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### Koşullar
Bu öğe SAML onayların kabul edilebilir kullanım tanımlayan koşulları belirtir.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` Ve `NotOnOrAfter` öznitelikleri sırasında onaylama olduğu geçerli aralığı belirtin.

* Değeri `NotBefore` özniteliği, çok veya biraz eşittir (saniyeden az) değerinden daha sonra `IssueInstant` özniteliği `Assertion` öğesi. Azure AD kendisi ve bulut hizmeti (hizmet sağlayıcısı) arasındaki zaman farklılığı hesaba katmaz ve tüm arabellek bu sefer eklemez.
* Değeri `NotOnOrAfter` 70 dakika değerini daha sonra bir özniteliktir `NotBefore` özniteliği.

#### Hedef kitle
Bu, bir hedef kitle tanımlayan bir URI içeriyor. Azure AD değerine bu öğenin değerini ayarlar `Issuer` öğesinin `AuthnRequest` , başlatılan oturum açma. Değerlendirilecek `Audience` değeri, değerini kullanmak `App ID URI` uygulama kaydı sırasında belirtildi.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Gibi `Issuer` değeri `Audience` değeri tam olarak eşleşmelidir Azure AD bulut hizmeti temsil eden hizmet asıl adlarından biri. Ancak, varsa değerini `Issuer` öğesi bir URI değeri değil `Audience` yanıt değeri `Issuer` değeri önekiyle `spn:`.

#### AttributeStatement
Bu konu veya kullanıcı hakkında talepleri içerir. Aşağıdaki alıntı bir örnek içeren `AttributeStatement` öğesi. Üç nokta öğesi birden çok öznitelikleri ve öznitelik değerleri dahil olduğunu gösterir.

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

* **Ad talep** : değerini `Name` özniteliği (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) kimliği doğrulanmış kullanıcının kullanıcı asıl adı olduğu gibi `testuser@managedtenant.com`.
* **Objectıdentifier talep** : değerini `ObjectIdentifier` özniteliği (`http://schemas.microsoft.com/identity/claims/objectidentifier`) olan `ObjectId` dizin nesnesinin kimliği doğrulanmış kullanıcının Azure AD'de temsil eder. `ObjectId`bir, genel benzersiz sabittir ve kimliği doğrulanmış kullanıcının güvenli tanımlayıcı yeniden kullanabilirsiniz.

#### AuthnStatement
Bu öğe, onaylama işlemi konu belirli bir zamandaki belirli bir şekilde doğrulandı onaylar.

* `AuthnInstant` Özniteliği, Azure AD ile hangi kullanıcı kimlik doğrulaması süreyi belirtir.
* `AuthnContext` Öğesi, kullanıcının kimliğini doğrulamak için kullanılan kimlik doğrulaması bağlamı belirtir.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```