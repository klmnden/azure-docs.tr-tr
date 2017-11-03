---
title: "SAML Protokolü Azure çoklu oturum açma | Microsoft Docs"
description: "Bu makalede Azure Active Directory'de tek Sign-Out SAML Protokolü"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 45e4705f53d80b5fe852c484b5e64d18a8e24f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# Çoklu oturum kapatma SAML Protokolü
Azure Active Directory (Azure AD) destekleyen SAML 2.0 tarayıcı tek oturum kapatma profil web. Tek doğru çalışması için oturum kapatma **LogoutURL** uygulama açıkça uygulama kaydı sırasında Azure AD ile kaydedilmesi gerekir. Azure AD LogoutURL bunlar oturumu kapattınız sonra kullanıcıları yeniden yönlendirmek için kullanır.

Bu diyagramda, Azure AD çoklu oturum kapatma işlemini iş akışı gösterilmektedir.

![Çoklu oturum açma iş akışı çıkışı](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
Bulut hizmeti gönderir bir `LogoutRequest` bir oturum sonlandırıldı göstermek için Azure ad ileti. Aşağıdaki alıntı bir örnek göstermektedir `LogoutRequest` öğesi.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
`LogoutRequest` Azure AD ile gönderilen öğesi, aşağıdaki öznitelikler gerektirir:

* `ID`: Bu, oturum kapatma isteği tanımlar. Değeri `ID` bir sayı ile başlamamalıdır. Eklenecek tipik uygulamadır **kimliği** için bir GUID dize gösterimi.
* `Version`: Bu öğenin değerini ayarlamak **2.0**. Bu değer gereklidir.
* `IssueInstant`: Bu bir `DateTime` dize koordine Evrensel Saat (UTC) değerine sahip ve [gidiş dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD bu türde bir değer Bekliyor, ancak bu zorunlu değildir.

### Veren
`Issuer` Öğesinde bir `LogoutRequest` tam olarak eşleşmelidir **ServicePrincipalNames** Azure AD bulut hizmetinde. Genellikle, bu ayarlanır **uygulama kimliği URI'si** uygulama kaydı sırasında belirtilir.

### NameID
Değeri `NameID` öğesi tam olarak eşleşmelidir `NameID` imzalanmakta çıkışı kullanıcının.

## LogoutResponse
Azure AD gönderir bir `LogoutResponse` yanıt olarak bir `LogoutRequest` öğesi. Aşağıdaki alıntı bir örnek göstermektedir `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Azure AD kümeleri `ID`, `Version` ve `IssueInstant` değerler `LogoutResponse` öğesi. Ayrıca ayarlar `InResponseTo` değerini öğesine `ID` özniteliği `LogoutRequest` yanıt elicited.

### Veren
Azure AD bu değeri ayarlar `https://login.microsoftonline.com/<TenantIdGUID>/` burada <TenantIdGUID> Azure AD kiracısı Kiracı kimliğidir.

Değeri değerlendirmek için `Issuer` öğenin değerini kullanmak **uygulama kimliği URI'si** uygulama kaydı sırasında sağlanan.

### Durum
Azure AD kullanır `StatusCode` öğesinde `Status` başarısını veya başarısızlığını oturum kapatma belirtmek için öğesi. Oturum kapatma girişimi başarısız olduğunda `StatusCode` öğesi özel hata iletileri de içerebilir.