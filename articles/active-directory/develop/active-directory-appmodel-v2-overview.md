---
title: Azure Active Directory v2.0 uç noktası | Microsoft Docs
description: Hem Microsoft Account hem de Azure Active Directory oturum açma uygulamaları oluşturmaya giriş.
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
ms.openlocfilehash: b90e03ad15f3656c06f36d6114250298db91e9ad
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430336"
---
# <a name="sign-in-microsoft-account-and-azure-active-directory-users-in-a-single-application"></a>Tek bir uygulamada kullanıcılar Microsoft Account hem de Azure Active Directory'de oturum
Geçmişte, iki ayrı sistemlerle tümleştirmek hem kişisel Microsoft hesapları destekler ve iş hesaplarını Azure Active Directory'den isteyen uygulama geliştiricileri gerekiyordu. Azure Active Directory (Azure AD) v2.0 uç noktası bu süreci kolaylaştırır yeni bir kimlik doğrulama API sürümü tanıtır. Tek bir tümleştirme kullanarak oturum açma hesapları her iki tür Azure AD v2.0 uç noktası sağlar. Azure AD v2.0 uç noktası kullanan uygulamaları da REST API'lerinden tüketen [Microsoft Graph API](https://graph.microsoft.io) ya da hesap türünü kullanarak.

## <a name="getting-started"></a>Başlarken
Tercih ettiğiniz platform Microsoft kullanarak bir uygulama oluşturmak için aşağıdaki listeden açık kaynak kitaplıkları ve çerçeveleri seçin. OAuth 2.0 ve Openıd Connect protokolleri, göndermek ve protokol iletilerini doğrudan kimlik doğrulama kitaplığı kullanmadan almak için de kullanabilirsiniz.
<br />

[!INCLUDE [Azure AD v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-azure-ad-v20-endpoint"></a>Azure AD v2.0 uç noktası hakkında daha fazla bilgi edinin
Azure AD v2.0 uç noktası ile yapabilecekleriniz hakkında bilgi edinin:

* Bulma [Azure AD v2.0 uç noktası ile oluşturabileceğiniz uygulama türleri](active-directory-v2-flows.md).
* Anlamak [sınırlamaları, kısıtlamalar ve engeller](active-directory-v2-limitations.md) ile Azure AD v2.0 uç noktası.
* Azure AD v2.0 uç noktası genel bir bakış için bu videoyu izleyin:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Ek kaynaklar
Azure AD v2.0 uç noktası platformu hakkında ayrıntılı bilgi keşfedin:

* [Azure AD v2.0 protokolleri başvurusu](active-directory-v2-protocols.md)
* [Azure AD v2.0 belirteç başvurusu](active-directory-v2-tokens.md)
* [Azure AD v2.0 kimlik doğrulama kitaplıkları başvurusu](active-directory-v2-libraries.md)
* [Kapsamlar ve Azure AD v2.0 uç noktası onay](active-directory-v2-scopes.md)
* [Microsoft Graph API'si](https://graph.microsoft.io)

> [!NOTE]
> Yalnızca Azure Active Directory iş ve Okul hesaplarında oturum açmak gerekiyorsa başlayın [Azure AD Geliştirici Kılavuzu](azure-ad-developers-guide.md). Azure AD v2.0 uç noktası, kişisel Microsoft hesapları ' oturum açmak için açıkça gereken geliştiriciler tarafından kullanılması amaçlanmıştır.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
