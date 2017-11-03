---
title: "Azure AD SAML Protokolü başvurusu | Microsoft Docs"
description: "Bu makalede, Azure Active Directory'de çoklu oturum açma ve tek Sign-Out SAML profilleri genel bir bakış sağlar."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.openlocfilehash: 7361d05850cf3ae997c0c186bf9a674c139f1f9e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# Azure Active Directory SAML Protokolü kullanma
Uygulamaların kullanıcılarına çoklu oturum açma deneyimini sağlamak üzere etkinleştirmek için azure Active Directory (Azure AD) kullanan SAML 2.0 protokolü. [Çoklu oturum açma](active-directory-single-sign-on-protocol-reference.md) ve [tek oturum kapatma](active-directory-single-sign-out-protocol-reference.md) SAML profilleri Azure ad açıklayan SAML onaylar, protokolleri ve bağlamaları kimlik sağlayıcı hizmeti nasıl kullanılır.

SAML protokolü, kimlik sağlayıcısı (Azure AD) ve hizmet sağlayıcısı (kendileri hakkında bilgi alışverişi için uygulama) gerektirir.

Bir uygulamayı Azure AD ile kaydedildikten sonra uygulama geliştiricisi Azure AD ile Federasyon ile ilgili bilgileri kaydeder. Bu içerir **yeniden yönlendirme URI'si** uygulamanın.

Azure Active Directory Kiracı özel ve ortak (Kiracı bağımsız) tek oturum açma ve çoklu oturum kapatma uç noktalarını kullanıma sunar. Bu URL'leri adreslenebilir konumlarını temsil eden--meta verileri okuyabilmesi için bitiş noktasına gitmek için bunlar yalnızca bir tanımlayıcıları--değildir.

* Kiracı özgü endpoint adresindedir `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  <TenantDomainName> Yer tutucu Azure AD kiracısı Tenantıd GUID'si veya kaydedilmiş bir etki alanı adını temsil eder. Örneğin, contoso.com Kiracı Federasyon meta verilerinin konumunda bulunuyor: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracı bağımsız endpoint adresindedir `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Bu uç nokta adresi **ortak** , bir kiracı etki alanı adı veya kimliği yerine görüntülenir

Azure AD yayımladığı Federasyon meta veri belgelerini hakkında daha fazla bilgi için bkz: [Federasyon meta verileri](active-directory-federation-metadata.md).
