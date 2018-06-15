---
title: Azure AD SAML Protokolü başvurusu | Microsoft Docs
description: Bu makalede, Azure Active Directory'de çoklu oturum açma ve tek Sign-Out SAML profilleri genel bir bakış sağlar.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 3a38d5e7a33a681c2e6d4964863d25f5cfbd6725
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34157648"
---
# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Azure Active Directory SAML Protokolü kullanma
Uygulamaların kullanıcılarına çoklu oturum açma deneyimini sağlamak üzere etkinleştirmek için azure Active Directory (Azure AD) kullanan SAML 2.0 protokolü. [Çoklu oturum açma](active-directory-single-sign-on-protocol-reference.md) ve [tek oturum kapatma](active-directory-single-sign-out-protocol-reference.md) SAML profilleri Azure ad açıklayan SAML onaylar, protokolleri ve bağlamaları kimlik sağlayıcı hizmeti nasıl kullanılır.

SAML protokolü, kimlik sağlayıcısı (Azure AD) ve hizmet sağlayıcısı (kendileri hakkında bilgi alışverişi için uygulama) gerektirir.

Bir uygulamayı Azure AD ile kaydedildikten sonra uygulama geliştiricisi Azure AD ile Federasyon ile ilgili bilgileri kaydeder. Bu içerir **yeniden yönlendirme URI'si** ve **meta veri URI'sini** uygulamanın.

Azure AD kullanır **meta veri URI'sini** imzalama anahtarı ve oturum kapatma bulut hizmetinin URI almak için bulut hizmeti. Uygulama meta verileri URI desteklemiyorsa, geliştirici URI oturum kapatma sağlamak için Microsoft desteğine başvurun ve imzalama anahtarı.

Azure Active Directory Kiracı özel ve ortak (Kiracı bağımsız) tek oturum açma ve çoklu oturum kapatma uç noktalarını kullanıma sunar. Bu URL'leri adreslenebilir konumlarını temsil eden--meta verileri okuyabilmesi için bitiş noktasına gitmek için bunlar yalnızca bir tanımlayıcıları--değildir.

* Kiracı özgü endpoint adresindedir `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`. <TenantDomainName> Yer tutucu Azure AD kiracısı Tenantıd GUID'si veya kaydedilmiş bir etki alanı adını temsil eder. Örneğin, contoso.com Kiracı Federasyon meta verilerinin altındadır: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracı bağımsız endpoint adresindedir `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Bu uç nokta adresi **ortak** , bir kiracı etki alanı adı veya kimliği yerine görüntülenir

Azure AD yayımladığı Federasyon meta veri belgelerini hakkında daha fazla bilgi için bkz: [Federasyon meta verileri](active-directory-federation-metadata.md).
