---
title: Azure Active Directory v2.0 uç | Microsoft Docs
description: Microsoft Account ve Azure Active Directory oturum açma ile uygulamaları oluşturmak için bir giriş.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: hirsin, jmprieur, elisol, dastrock
ms.custom: aaddev
ms.openlocfilehash: 1c91c1ed8358f58ab7a4d9a697ec2d7933c4f137
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316747"
---
# <a name="sign-in-microsoft-account-and-azure-active-directory-users-in-a-single-application"></a>Microsoft Account hem de Azure Active Directory'de kullanıcılar tek bir uygulamada oturum
Geçmişte, hem kişisel Microsoft hesaplarını destekler ve iş hesaplarını Azure Active Directory'den isteyen uygulama geliştiricileri iki ayrı sistemlerle tümleştirmek gerekiyordu. Azure Active Directory (Azure AD) v2.0 uç noktası bu işlemi basitleştirir yeni bir kimlik doğrulama API sürümü tanıtır. Tek bir tümleştirme kullanarak oturum açma hesapları her iki tür Azure AD v2.0 uç sağlar. Azure AD v2.0 uç kullanan uygulamalar, REST API'lerinin ayrıca tüketebileceği [Microsoft Graph API](https://graph.microsoft.io) ya da hesap türünü kullanarak.

## <a name="getting-started"></a>Başlarken
En sevdiğiniz platformu Microsoft kullanarak bir uygulama oluşturmak için aşağıdaki listeden açık kaynak kitaplıkları ve çerçeveleri seçin. OAuth 2.0 ve Openıd Connect protokolleri, gönderip protokol iletilerini doğrudan bir kimlik doğrulama kitaplığı kullanmadan için de kullanabilirsiniz.
<br />

[!INCLUDE [Azure AD v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-azure-ad-v20-endpoint"></a>Azure AD v2.0 uç hakkında daha fazla bilgi edinin
Azure AD v2.0 uç noktası ile yapabilecekleriniz hakkında bilgi edinin:

* Bul [Azure AD v2.0 uç noktası ile oluşturabileceğiniz uygulama türleri](active-directory-v2-flows.md).
* Anlamak [sınırlamalar, sınırlamaları ve kısıtlamaları](active-directory-v2-limitations.md) Azure AD v2.0 uç noktası ile.
* Azure AD v2.0 uç genel bir bakış için bu videoyu izleyin:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Ek kaynaklar
Azure AD v2.0 uç noktası platformu hakkında ayrıntılı bilgi keşfedin:

* [Azure AD v2.0 protokolleri başvurusu](active-directory-v2-protocols.md)
* [Azure AD v2.0 başvuru belirteçleri](active-directory-v2-tokens.md)
* [Azure AD v2.0 kimlik doğrulama kitaplıkları başvurusu](active-directory-v2-libraries.md)
* [Kapsamlar ve Azure AD v2.0 uç onayı](active-directory-v2-scopes.md)
* [Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Yalnızca oturum açma iş ve Okul hesapları Azure Active Directory'den gerekiyorsa, başlayın [Azure AD Geliştirici Kılavuzu](active-directory-developers-guide.md). Azure AD v2.0 uç açıkça Microsoft Kişisel hesaplarında oturum açmanız gerekir geliştiriciler tarafından kullanılmaya yöneliktir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
