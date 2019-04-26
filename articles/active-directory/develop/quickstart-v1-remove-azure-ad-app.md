---
title: Azure Active Directory'den uygulama kaldırma
description: Azure Active Directory'den (Azure AD) uygulama kaldırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0038dcc10aafa191121b2797f68d66a3e32b3fa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298770"
---
# <a name="quickstart-remove-an-application-from-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'den uygulama kaldırma

Kurumsal geliştiricilerin ve uygulamalarını Azure Active Directory'ye (Azure AD) kaydetmiş hizmet-olarak-yazılım (SaaS) sağlayıcıların bir uygulamanın kaydını Azure AD kiracılarından kaldırmaları gerekebilir.

Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:

* [Kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma](#removing-an-application-authored-by-your-organization)
* [Başka bir kuruluş tarafından yetkilendirilmiş çok kiracılı bir uygulamayı kaldırma](#removing-a-multi-tenant-application-authorized-by-another-organization)

## <a name="prerequisites"></a>Önkoşullar

Başlamak için kendisine kayıtlı uygulamaları olan bir Azure AD kiracınız olması gerekecek.

* Henüz bir kiracınız yoksa [nasıl alabileceğinizi öğrenin](quickstart-create-new-tenant.md).
* Kiracınıza uygulama ekleme ve kaydetme hakkında bilgi edinmek için bkz. [Azure AD'ye uygulama ekleme](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="removing-an-application-authored-by-your-organization"></a>Kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma

Kuruluşunuza kayıtlı uygulamalar kiracınızın ana **Uygulama kayıtları** sayfasındaki **Uygulamalarım** filtresi altında görünür. Bu uygulamalar, Azure portalı aracılığıyla el ile veya PowerShell ya da Microsoft Graph API aracılığıyla programlama yoluyla kaydettiğiniz uygulamalardır. Daha açık bir ifadeyle bu uygulamalar kiracınızdaki bir uygulama ve hizmet sorumlusu nesnesi tarafından temsil edilir. Bu nesneler hakkında daha fazla bilgi için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Tek kiracılı bir uygulamayı dizininizden kaldırmak için

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin, **Uygulama kayıtları**'nı seçin ve ardından yapılandırmak istediğiniz uygulamayı bulun ve seçin.
    Bu eylem sizi uygulamanın **Ayarlar** sayfasını açan uygulama ana kayıt sayfasına yönlendirir.
1. Uygulamanın ana kayıt sayfasında **Sil**'i seçin.
1. Uygulamayı silmek istediğinizi onaylamak için **Evet**'i seçin.

### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>Çok kiracılı bir uygulamayı uygulamanın ana dizininden kaldırmak için

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin, **Uygulama kayıtları**'nı seçin ve ardından yapılandırmak istediğiniz uygulamayı bulun ve seçin.
    Bu eylem sizi uygulamanın **Ayarlar** sayfasını açan uygulama ana kayıt sayfasına yönlendirir.
1. **Ayarlar** sayfasında **Özellikler**'i seçin ve önce uygulamanızı tek kiracılıya çevirmek için **Çok kiracılı** anahtarını **Hayır** yapın ve ardından **Kaydet**'i seçin.
    Uygulamanın hizmet sorumlusu nesneleri, daha önce buna onay vermiş tüm kuruluşların kiracılarında kalır.
1. Uygulamanın ana kayıt sayfasında **Sil**'i seçin.
1. Uygulamayı silmek istediğinizi onaylamak için **Evet**'i seçin.

## <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Başka bir kuruluş tarafından yetkilendirilmiş çok kiracılı bir uygulamayı kaldırma

Kiracınızın ana **Uygulama kayıtları** sayfasındaki **Tüm uygulamalar** filtresinin altında görüntülenen (**Uygulamalarım** kayıtları hariç) uygulamaların bir kısmı çok kiracılı uygulamalardır.

Teknik olarak bu çok kiracılı uygulamalar başka bir kiracınındır ve sizin kiracınıza onay işlemi sırasında kaydedilmiştir. Daha açık söylersek, bunlar kiracınızda yalnızca bir hizmet sorumlusu nesnesi tarafından ve karşılık gelen herhangi bir uygulama nesnesi olmadan temsil edilir. Uygulama ve hizmet sorumlusu nesneleri arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure AD'de uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Çok kiracılı bir uygulamanın dizininize olan ve daha önce onay verdiğiniz erişimini kaldırmak için, şirket yöneticisinin uygulamanın hizmet sorumlusunu kaldırması gerekir. Yöneticinin genel yönetim erişimi olması gerekir ve sorumluyu Azure portalı aracılığıyla veya [Azure AD PowerShell Cmdlet'lerini](https://go.microsoft.com/fwlink/?LinkId=294151) kullanarak kaldırabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD v1.0 uç noktasını kullanan uygulamalar için diğer ilgili uygulama yönetimi hızlı başlangıçları hakkında bilgi edinin:

- [Azure AD'ye uygulama ekleme](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Azure AD'de bir uygulamayı güncelleştirme](quickstart-v1-update-azure-ad-app.md)
