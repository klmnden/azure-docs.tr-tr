---
title: "E-posta doğrulama tüketici kaydolma - Azure Active Directory B2C sırasında devre dışı bırak"
description: "E-posta doğrulama tüketici Azure Active Directory B2C'de kaydolma sırasında devre dışı bırakma gösteren bir konu"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: f8e7e5dd8e5e5da6ff4c1e82215da019e8e101c9
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
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
