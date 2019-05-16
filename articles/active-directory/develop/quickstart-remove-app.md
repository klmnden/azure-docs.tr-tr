---
title: Microsoft kimlik platformuna kaydedilmiş bir uygulamayı kaldırma | Azure
description: Microsoft kimlik platformuna kaydedilmiş bir uygulamayı kaldırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: eddf16ea4179d3c254538a367ceb80ed02598345
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545738"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform"></a>Hızlı Başlangıç: Microsoft kimlik platformu ile kayıtlı bir uygulamaya Kaldır

Microsoft kimlik platformuna kayıtlı uygulamaları olan kurumsal geliştiricilerin ve hizmet olarak yazılım (SaaS) sağlayıcılarının bir uygulamanın kaydını kaldırması gerekebilir.

Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:

* Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma
* Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

## <a name="prerequisites"></a>Önkoşullar

Kayıtlı uygulamalar ona içeren bir kiracısına sahip olmanız gerekir. Uygulama eklemeyi ve kaydetmeyi öğrenmek için bkz. [Microsoft kimlik platformuna uygulama kaydetme](quickstart-register-app.md).

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma

Kendiniz veya kuruluşunuz tarafından kaydedilmiş uygulamalar gerek bir uygulama nesnesi, gerekse kiracınızdaki bir hizmet sorumlusu nesnesi tarafından temsil edilir. Daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](active-directory-application-objects.md).

### <a name="to-remove-an-application"></a>Uygulama kaldırma

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
3. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet ve ardından **uygulama kayıtları**. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtikten sonra uygulamanın **Genel Bakış** sayfasını görebilirsiniz.
4. **Genel Bakış** sayfasından **Sil**'i seçin.
5. Uygulamayı silmek istediğinizi onaylamak için **Evet**'i seçin.

   > [!NOTE]
   > Uygulama silmek için uygulamanın sahiplerinden biri olarak listelenmeniz veya yönetici ayrıcalıklarına sahip olmanız gerekir.

## <a name="remove-an-application-authored-by-another-organization"></a>Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

Bir kiracı bağlamında **Uygulama kayıtlarını** görüntülüyorsanız, **Tüm uygulamalar** sekmesinin altında görünen uygulamaların bir alt kümesi başka bir kiracıdandır ve kiracınıza onay işlemi sırasında kaydedilmiştir. Daha açık söylersek, bunlar kiracınızda yalnızca bir hizmet sorumlusu nesnesi tarafından ve karşılık gelen herhangi bir uygulama nesnesi olmadan temsil edilir. Uygulama ve hizmet sorumlusu nesneleri arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure AD'de uygulama ve hizmet sorumlusu nesneleri](active-directory-application-objects.md).

Bir uygulamanın dizininize olan daha önce onay verdiğiniz erişimini kaldırmak için şirket yöneticisinin uygulamanın hizmet sorumlusunu kaldırması gerekir. Yöneticinin genel yönetim erişimi olması gerekir ve uygulamayı Azure portalı aracılığıyla kaldırabilir veya erişimi kaldırmak için [Azure AD PowerShell Cmdlet'lerini](https://go.microsoft.com/fwlink/?LinkId=294151) kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ilgili diğer uygulama yönetimi hızlı başlangıçları hakkında bilgi edinin:

* [Microsoft kimlik platformuna uygulama kaydetme](quickstart-register-app.md)
* [Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma](quickstart-configure-app-access-web-apis.md)
* [Bir uygulamayı web API'lerini kullanıma sunacak şekilde yapılandırma](quickstart-configure-app-expose-web-apis.md)
* [Bir uygulama tarafından desteklenen hesapları değiştirme](quickstart-modify-supported-accounts.md)
