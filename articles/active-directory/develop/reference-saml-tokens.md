---
title: Farklı bir belirteç hakkında bilgi edinin ve talep türleri Azure AD tarafından desteklenen | Microsoft Docs
description: Anlama ve Azure Active Directory (AAD) verilen SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçlere talep değerlendirmek için bir kılavuz
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58a8d3b62fab7614375436846888b78113740ce2
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406610"
---
# <a name="azure-ad-saml-token-reference"></a>Azure AD SAML belirteç başvurusu

Azure Active Directory (Azure AD) güvenlik belirteçleri her kimlik doğrulama akışı işlenmesinde çeşitli yayar. Bu belge, biçimi, güvenlik özellikleri ve her belirteç türünün içeriğini açıklar.

## <a name="claims-in-saml-tokens"></a>SAML belirteç içindeki talep

> [!div class="mx-codeBreakAll"]
> | Ad | Eşdeğer JWT talep | Açıklama | Örnek |
> | --- | --- | --- | ------------|
> |Hedef kitle | `aud` |Amaçladığınız alıcının belirtecin. Belirteç alan uygulamasını İzleyici değerin doğru olduğundan ve farklı bir kitle için hedeflenen tarafından istenen belirteçleri Reddet doğrulamanız gerekir. | `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>`  |
> | Kimlik Doğrulaması Anı | |Kimlik doğrulaması oluştuğu saat ve tarihi kaydeder. | `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | 
> |Kimlik Doğrulama Yöntemi | `amr` |Belirtecin konu kimliğinin nasıl tanımlar. | `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` |
> |Ad | `given_name` |İlk sağlar veya "Azure AD kullanıcı nesnesindeki belirlenen kullanıcı adı verilen". | `<Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">`<br>`<AttributeValue>Frank<AttributeValue>`  |
> |Gruplar | `groups` |Öznenin grup üyeliklerini temsil eden nesne kimlikleri sağlar. Bu değerler benzersiz (nesne kimliği bakın) ve güvenli bir şekilde bir kaynağa erişim yetkisi zorlama gibi erişimi yönetmek için kullanılabilir. Grupları talepte bulunan gruplarını, uygulama başına temelinde, uygulama bildiriminin "groupMembershipClaims" özelliği aracılığıyla yapılandırılır. Bir null değeri tüm grupları hariç, yalnızca güvenlik grupları ve Office 365 dağıtım listeleri Active Directory güvenlik grubu üyelikleri ve bir değeri "Tüm" dahil "IDAP" değerini içerecektir. <br><br> **Notları**: <br> Fazla kullanım talebe kullanıcı gruplarının listesini içeren Graph uç noktası işaret talep kaynağı eklenecektir sonra kullanıcının grup sayısı sınırı (SAML, JWT için 200 150) üzerinden aşması durumunda. (içinde. | `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` |
> | Fazla kullanım göstergesi grupları | `groups:src1` | Uzunluğu sınırlı olmayan belirteç istekleri için (bkz `hasgroups` yukarıda), ancak yine de çok büyük belirteç için bir bağlantı kullanıcı için tam grupları listesine eklenir. SAML için bunun yerine yeni bir talep olarak eklenir `groups` talep. | `<Attribute Name=" http://schemas.microsoft.com/claims/groups.link">`<br>`<AttributeValue>https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects<AttributeValue>` |
> |Kimlik Sağlayıcı | `idp` |Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. Farklı bir kiracıya veren kullanıcı hesabının bulunduğu sürece bu değeri veren talep değeri için aynıdır. | `<Attribute Name=" http://schemas.microsoft.com/identity/claims/identityprovider">`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` |
> |IssuedAt | `iat` |Belirteç verildiği zaman depolar. Genellikle, belirteç güncellik ölçmek için kullanılır. | `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` |
> |Sertifikayı Veren | `iss` |Oluşturur ve belirteci döndürür güvenlik belirteci hizmeti (STS) tanımlar. Azure AD döndüren belirteçlerinde veren sts.windows.net ' dir. Azure AD dizini Kiracı kimliği verenin talep değeri GUID'dir. Kiracı kimliği dizinin değişmez ve güvenilir bir tanımlayıcıdır. | `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` |
> |Soyadı | `family_name` |Son adını, soyadını veya kullanıcının aile adı Azure AD kullanıcı nesnesinde tanımlanan sağlar. | `<Attribute Name=" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">`<br>`<AttributeValue>Miller<AttributeValue>` |
> |Ad | `unique_name` |Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değer, bir kiracıda benzersiz olması garanti edilmez ve yalnızca görüntüleme amaçları için kullanılmak üzere tasarlanmıştır. | `<Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>`|
> |Nesne kimliği | `oid` |Azure AD'de bir nesnenin benzersiz bir tanımlayıcı içerir. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Azure AD'ye sorgulardaki bir nesne tanımlamak için nesne Kimliğini kullanın. | `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` |
> |Roller | `roles` |Konu grubu üyeliği üzerinden doğrudan ve dolaylı olarak verilmiş ve rol tabanlı erişim denetimi uygulamak için kullanılabilir tüm uygulama rolleri temsil eder. Uygulama rolleri aracılığıyla uygulama başına temelinde, tanımlanan `appRoles` uygulama bildiriminin özelliğidir. `value` Rol talebi görüntülenen değeri her uygulama rolü özelliğidir. | `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`|
> |Subject | `sub` |Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar tanımlar. Bu değer sabittir ve sonraki atanamaz veya yeniden, bu nedenle bu yetkilendirme denetimleri güvenli bir şekilde gerçekleştirmek için kullanılabilir. Konu her zaman Azure AD sorunlarını belirteçlerinde bulunduğundan, bu değer bir genel amaçlı yetkilendirme sistemde kullanılması önerilir. <br> `SubjectConfirmation` bir talep değil. Bu konu belirtecin nasıl doğrulanır açıklar. `Bearer` Konu belirtecin kendi mülkü tarafından doğrulandığını gösterir. | `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>`|
> |Kiracı Kimliği | `tid` |Belirteci veren dizin Kiracı tanımlayan bir sabit, yeniden kullanılabilir olmayan tanımlayıcısı. Bu değer, çok kiracılı bir uygulamadaki kiracıya özgü directory kaynaklarına erişmek için kullanabilirsiniz. Örneğin, bu değer, Graph API'sine çağrıda kiracıda tanımlamak için kullanabilirsiniz. | `<Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>`|
> |Belirteç Ömrü | `nbf`, `exp` |Belirtecin geçerli olduğu zaman aralığını tanımlar. Belirteci doğrular hizmet geçerli bir tarih belirteci reddetme belirteç ömrü içinde başka olduğunu doğrulamanız gerekir. Hizmet belirteci ömrü aralığı dışındaki beş dakikaya kadar herhangi bir farklılığın saatindeki ("saat eğriltme") hesabı için Azure AD arasında sağlayabilir ve hizmeti. | `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br>|

## <a name="sample-saml-token"></a>Örnek SAML belirteci

Bu, tipik bir SAML belirtecinin bir örnektir.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="https://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="https://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="https://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="https://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="https://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="https://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>https://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

## <a name="related-content"></a>İlgili içerik
* Azure AD Graph bkz [ilke işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ve [ilke varlığı](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)belirteç ömrü ilkesi Azure AD Graph API'si aracılığıyla yönetme hakkında daha fazla bilgi için.
* Daha fazla bilgi ve örnekler de dahil olmak üzere, PowerShell cmdlet'leri aracılığıyla ilkeleri yönetme örnekleri için bkz. [Azure AD'de yapılandırılabilir belirteç ömrünü](active-directory-configurable-token-lifetimes.md). 
* Ekleme [özel ve isteğe bağlı taleplerin](active-directory-optional-claims.md) uygulamanızın belirteçleri.
* Kullanım [çoklu oturum açma (SSO) ile SAML](single-sign-on-saml-protocol.md).
* Kullanım [Azure çoklu oturum açma SAML Protokolü](single-sign-out-saml-protocol.md)
