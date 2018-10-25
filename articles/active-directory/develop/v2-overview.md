---
title: v2.0 Hakkında | Azure
description: V2.0 uç noktası ve platform hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 265d34c91a8c803256e718899f5b6ce2738a88e5
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956441"
---
# <a name="about-v20"></a>v2.0 Hakkında

Platform ve v2.0 uç noktası önizleme aşamasında bulunmuş ve sürekli olarak geliştirilmiştir. Günümüzde JavaScript tek sayfalı uygulama (SPA) senaryolarının tüm özellikleri tamamlanmıştır ve durumu önizlemeden genel kullanılabilirliğe değiştirebilmemiz için bize geri bildirim vermeniz amacıyla sizi MSAL.js’yi kullanarak tarayıcı tabanlı uygulamalar oluşturmaya davet ediyoruz.

> [!NOTE]
> MSAL Android, iOS ve .NET için hala geliştirilmekte olan özellikler bulunmaktadır. Bunları kullanarak uygulamalar oluşturabilir ve bize geri bildirim gönderebilirsiniz.

ADAL ve MSAL ile oluşturulmuş tüm uygulamalarınızı içermek ve kullanılabilirliğini artırmak için Azure portal geliştirici deneyimi önemli ölçüde güncelleştirilmiştir.

Geçmişte Azure Active Directory’deki (Azure AD) hem kişisel Microsoft hesaplarını hem de iş hesaplarını desteklemek isteyen uygulama geliştiricilerinin iki farklı sistemi kullanarak tümleştirmesi gerekirdi. V2.0 uç noktası ve platformu, bu süreci basitleştiren bir kimlik doğrulaması API sürümü sağlar. Bu, tek bir tümleştirme aracılığıyla iki hesap türünden de oturum açılabilmesini sağlar. V2.0 uç noktasını kullanan uygulamalar da iki hesap türünden birini kullanarak [Microsoft Graph API](https://graph.microsoft.io)’deki REST API’lerini tüketebilir.

## <a name="getting-started"></a>Başlarken

Microsoft açık kaynaklı kütüphane ve çerçevelerini kullanarak bir uygulama oluşturmak için aşağıdaki listeden favori platformunuzu seçin. Bir kimlik doğrulaması kütüphanesi kullanmadan doğrudan protokol iletileri almak ve göndermek için aynı zamanda OAuth 2.0 ve OpenID Connect protokollerini kullanabilirsiniz.

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>V2.0 uç noktası ve platform hakkında bilgi edinin

Azure AD v2.0 uç noktası ile yapabilecekleriniz hakkında bilgi edinin:

* [Azure AD v2.0 uç noktası ile oluşturabileceğiniz uygulama türlerini](v2-app-types.md) keşfedin.
* Azure AD v2.0 uç noktasının [sınırlamalarını, kısıtlamalarını ve engellerini](active-directory-v2-limitations.md) anlayın.
* Azure AD v2.0 uç noktasına genel bir bakış için bu videoyu izleyin:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Ek kaynaklar

V2.0 hakkında ayrıntılı bilgileri keşfedin:

* [v2.0 protokolleri başvurusu](active-directory-v2-protocols.md)
* [Erişim belirteçleri başvurusu](access-tokens.md)
* [`id_tokens` başvurusu](id-tokens.md)
* [v2.0 kimlik doğrulama kitaplıkları başvurusu](reference-v2-libraries.md)
* [v2.0’da kapsamlar ve onay](v2-permissions-and-consent.md)
* [Microsoft Graph API’si](https://graph.microsoft.io)

> [!NOTE]
> Yalnızca Azure Active Directory’deki iş ve okul hesaplarında oturum açılması gerekiyorsa [Azure AD Geliştirici Kılavuzu](v1-overview.md) ile başlayın. V2.0 uç noktasını özel olarak kişisel Microsoft hesapları ile oturum açması gereken geliştiricilerin kullanması amaçlanmıştır.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
