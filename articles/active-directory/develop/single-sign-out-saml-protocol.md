---
title: Azure çoklu oturum açma SAML Protokolü | Microsoft Docs
description: Bu makalede Azure Active Directory çoklu oturum kapatma SAML Protokolü
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06fd36935c1f43cc14697748666eccd9e6d31168
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545955"
---
# <a name="single-sign-out-saml-protocol"></a>Çoklu oturum kapatma SAML Protokolü

Web tarayıcı çoklu oturum kapatma profili Azure Active Directory (Azure AD) destekler SAML 2.0. Tek doğru şekilde çalışması için oturum kapatma **LogoutURL** için uygulamayı açıkça uygulama kaydı sırasında Azure AD ile kayıtlı olması gerekir. Azure AD, bunlar oturumunuz sonra kullanıcıların yönlendirileceği LogoutURL kullanır.

Azure AD çoklu oturum kapatma işleminin bir iş akışı aşağıdaki diyagramda gösterilmiştir.

![İş akışını Azure AD çoklu oturum](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
Bulut hizmeti gönderen bir `LogoutRequest` bir oturum sonlandırıldı göstermek için Azure AD'ye ileti. Aşağıdaki alıntıda bir örneği gösterilmektedir. `LogoutRequest` öğesi.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
`LogoutRequest` Aşağıdaki öznitelikler Azure AD'ye gönderilen bir öğe gerektiriyor:

* `ID` -Bu, oturum kapatma isteği tanımlar. Değerini `ID` bir sayıyla başlamamalıdır. Eklenecek tipik uygulamadır **kimliği** GUID dize gösterimi.
* `Version` -Ayarlamak için bu öğenin değeri **2.0**. Bu değer gereklidir.
* `IssueInstant` -Bu, bir `DateTime` dize koordine Evrensel Saat (UTC) değeriyle ve [gidiş dönüş biçim ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD, bu türde bir değer Bekliyor, ancak zorunlu değildir.

### <a name="issuer"></a>Veren
`Issuer` Öğesinde bir `LogoutRequest` biriyle tam olarak eşleşmelidir **ServicePrincipalNames** Azure ad'deki bulut hizmetinde. Genellikle, bu ayar **uygulama kimliği URI'si** uygulama kaydı sırasında belirtilir.

### <a name="nameid"></a>Nameıd
Değerini `NameID` öğesi tam olarak eşleşmelidir `NameID` imzalanmış çıkış kullanıcının.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD gönderen bir `LogoutResponse` yanıt olarak bir `LogoutRequest` öğesi. Aşağıdaki alıntıda bir örneği gösterilmektedir. `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure AD kümeleri `ID`, `Version` ve `IssueInstant` değerler `LogoutResponse` öğesi. Ayrıca ayarlar `InResponseTo` değerini öğesine `ID` özniteliği `LogoutRequest` yanıt elicited.

### <a name="issuer"></a>Veren
Azure AD bu değeri ayarlar `https://login.microsoftonline.com/<TenantIdGUID>/` burada \<TenantIdGUID > Azure AD kiracısını Kiracı Kimliğini gösterir.

Değerini değerlendirmek için `Issuer` öğesi, değerini kullanın **uygulama kimliği URI'si** uygulama kayıt sırasında sağlanan.

### <a name="status"></a>Durum
Azure AD kullanımları `StatusCode` öğesinde `Status` başarısı veya başarısızlığı oturum kapatma belirtmek için öğesi. Oturum kapatma girişimi başarısız olduğunda, `StatusCode` öğesi özel hata iletileri de içerebilir.
