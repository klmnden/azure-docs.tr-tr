---
title: E-posta doğrulama tüketici Azure Active Directory B2C'de kaydolma sırasında devre dışı bırakma | Microsoft Docs
description: E-posta doğrulama tüketici Azure Active Directory B2C'de kaydolma sırasında devre dışı bırakma gösteren bir konu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 24242054ea4af3797fbeca1ed48bb698e4f4296b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712020"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Tüketicinin kaydolma devre dışı bırak e-posta doğrulama
Etkinleştirildiğinde, Azure Active Directory (Azure AD) B2C tüketici uygulamaları için bir e-posta adresi sağlayarak ve yerel bir hesap oluşturma kaydolun olanağı sağlar. Azure AD B2C, kayıt işlemi sırasında doğrulamak tüketicilere gerektirerek geçerli e-posta adresleri sağlar. Ayrıca, kötü amaçlı bir otomatik işlem'in uygulamaları için sahte hesapları oluşturma engeller.

Bazı uygulama geliştiricileri e-posta doğrulama kaydolma işlemi sırasında atlayın ve bunun yerine e-posta adresini doğrulayın. daha sonra bir tüketiciye sahip tercih edilir. Bunu desteklemek için Azure AD B2C e-posta doğrulama devre dışı bırakmak için yapılandırılabilir. Bunun yapılması daha sorunsuz bir kaydolma işlemi oluşturur ve geliştiricilerin sahip değilse bu Tüketiciler, e-posta adresinden doğruladıktan tüketicileri ayırt esnekliği sağlar.

Varsayılan olarak, e-posta doğrulama açık kayıt ilkeleri vardır. Devre dışı bırakmak için aşağıdaki adımları kullanın:

1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklatın **kayıt ilkeleri** veya **oturum açma veya kaydolma ilkeleri** için yapılandırdığınıza bağlı olarak kaydolma.
3. Açmak için ilke (örneğin, "B2C_1_SiUp") tıklayın. Tıklatın **Düzenle** dikey pencerenin üstündeki.
4. Tıklatın **sayfa UI özelleştirme**.
5. Tıklatın **yerel hesap kayıt sayfasına**.
6. Tıklatın **e-posta adresi** içinde **adı** sütunu altında **kaydolma özniteliklerini** bölümü.
7. İki durumlu **bölgedeki tüm sitelerden** için seçenek **Hayır**.
8. Tıklatın **Tamam** ulaşana kadar altındaki **Düzenle İlkesi** dikey.
9. Tıklatın **kaydetmek** dikey pencerenin üstündeki. İşiniz bittiğinde!

> [!NOTE]
> E-posta doğrulama kayıt işlemini devre dışı bırakma istenmeyen posta göndermek için neden olabilir. Varsayılan devre dışı bırakırsanız, kendi doğrulama sistemi ekleme öneririz.
> 
> 

Biz her zaman görüş ve öneriler için açık! Bu konu ile bir güçlükle sahip veya bu içeriğin geliştirilmesi için öneriler varsa, sayfanın sonundaki görüşlerinize değer veriyoruz. Özellik istekleri için bunları Ekle [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
