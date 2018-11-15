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
ms.openlocfilehash: ecb95f0440751a6cdbf81dbf02c62bed6b5e780b
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51286699"
---
# <a name="about-v20"></a>v2.0 Hakkında

Platform ve v2.0 uç noktası önizleme aşamasında bulunmuş ve sürekli olarak geliştirilmiştir. Günümüzde JavaScript tek sayfalı uygulama (SPA) senaryolarının tüm özellikleri tamamlanmıştır ve durumu önizlemeden genel kullanılabilirliğe değiştirebilmemiz için bize geri bildirim vermeniz amacıyla sizi MSAL.js’yi kullanarak tarayıcı tabanlı uygulamalar oluşturmaya davet ediyoruz.

> [!NOTE]
> MSAL Android, iOS ve .NET için hala geliştirilmekte olan özellikler bulunmaktadır. Bunları kullanarak uygulamalar oluşturabilir ve bize geri bildirim gönderebilirsiniz.

ADAL ve MSAL ile oluşturulmuş tüm uygulamalarınızı içermek ve kullanılabilirliğini artırmak için Azure portal geliştirici deneyimi önemli ölçüde güncelleştirilmiştir.

Geçmişte Azure Active Directory’deki (Azure AD) hem kişisel Microsoft hesaplarını hem de iş hesaplarını desteklemek isteyen uygulama geliştiricilerinin iki farklı sistemi kullanarak tümleştirmesi gerekirdi. V2.0 uç noktası ve platformu, bu süreci basitleştiren bir kimlik doğrulaması API sürümü sağlar. Bu, tek bir tümleştirme aracılığıyla iki hesap türünden de oturum açılabilmesini sağlar. V2.0 uç noktasını kullanan uygulamalar da iki hesap türünden birini kullanarak [Microsoft Graph API](https://developer.microsoft.com/graph)’deki REST API’lerini tüketebilir.

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
* [Kimlik belirteçleri başvurusu](id-tokens.md)
* [v2.0 kimlik doğrulama kitaplıkları başvurusu](reference-v2-libraries.md)
* [İzinler ve onay v2.0](v2-permissions-and-consent.md)
* [Microsoft Graph API'si](https://developer.microsoft.com/graph)

> [!NOTE]
> Yalnızca Azure Active Directory’deki iş ve okul hesaplarında oturum açılması gerekiyorsa [Azure AD Geliştirici Kılavuzu](v1-overview.md) ile başlayın. V2.0 uç noktasını özel olarak kişisel Microsoft hesapları ile oturum açması gereken geliştiricilerin kullanması amaçlanmıştır.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
