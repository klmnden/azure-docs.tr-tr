---
title: "Azure Active Directory v2.0 uç | Microsoft Docs"
description: "Microsoft Account ve Azure Active Directory oturum açma ile uygulamaları oluşturmaya giriş bilgileri."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: a6a7c6bdf3deaee3a3949fe409a7fab6b7664695
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Oturum açma Microsoft Account & Azure AD kullanıcıların tek bir uygulamada
Geçmişte, hem kişisel Microsoft hesaplarını destekler ve iş hesaplarını Azure Active Directory'den isteyen uygulama geliştiricisi iki ayrı sistemlerle tümleştirmek için gereklidir.  **Azure AD v2.0 uç** her iki türdeki bir basit tümleştirmesi kullanılarak hesaba oturum olanak tanıyan yeni bir kimlik doğrulama API sürümü tanıtır.  V2.0 uç noktası kullanan uygulamalar, REST API'lerinin ayrıca tüketebileceği [Microsoft Graph](https://graph.microsoft.io) ya da türde bir hesabı kullanarak.

## <a name="getting-started"></a>Başlarken
En sevdiğiniz platformu bizim açık kaynak kitaplıkları ve çerçevelerini kullanarak bir uygulama oluşturmak için aşağıdaki listeden seçin.  Alternatif olarak, bir kimlik doğrulama kitaplığı kullanmadan doğrudan protokol iletilerini & Gönder için OAuth 2.0 & Openıd Connect Protokolü Belgelerimizi kullanabilirsiniz.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Yenilikler
Burada yer alan bilgiler nedir & v2.0 uç noktası ile kurulamadığı anlamak yararlı olacaktır.

* Hakkında bilgi edinin [yapı v2.0 uç noktası ile uygulama türleri](active-directory-v2-flows.md).
* Anlamak [sınırlamalar, sınırlamaları ve kısıtlamaları](active-directory-v2-limitations.md) v2.0 uç noktası ile.
* Bu genel bakış videosu v2.0 uç noktası için gözden geçirin:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Başvuru
Bu bağlantılar platform derinlemesine keşfetmek için kullanışlıdır:

* [v2.0 protokol başvurusu](active-directory-v2-protocols.md)
* [v2.0 belirteç başvurusu](active-directory-v2-tokens.md)
* [v2.0 Kitaplığı Başvurusu](active-directory-v2-libraries.md)
* [Kapsamlar ve v2.0 uç onayı](active-directory-v2-scopes.md)
* [Microsoft Graph](https://graph.microsoft.io)

> [!NOTE]
> Yalnızca oturum açma iş ve Okul hesapları Azure Active Directory'den gerekiyorsa, ile başlamalı bizim [Azure AD Geliştirici Kılavuzu](active-directory-developers-guide.md).  V2.0 uç açıkça Microsoft Kişisel hesaplarında oturum açmanız gerekir geliştiriciler tarafından kullanılmaya yöneliktir.


[!INCLUDE  [Help and Support Options](../../../includes/active-directory-develop-help-support-include.md)]
