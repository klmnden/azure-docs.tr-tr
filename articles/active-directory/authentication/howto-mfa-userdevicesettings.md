---
title: Yöneticileri Yönet kullanıcılara ve cihazlara - Azure MFA | Microsoft Docs
description: Bu kavram işlemimiz yeniden yapmak için kullanıcıların zorlama gibi kullanıcı ayarlarını nasıl değiştireceğinizi açıklar.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 2f73d9795ba807e5901568507ad2fae5b001c91a
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161298"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure multi Factor Authentication ile kullanıcı ayarlarını yönetme

Yönetici olarak, aşağıdaki kullanıcı ve cihaz ayarları yönetebilirsiniz:

* Kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
* Uygulama parolalarını Sil
* Tüm güvenilen cihazlara MFA gerektirme 

## <a name="require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
Bu ayar, kullanıcı yeniden kayıt işlemini tamamlamak için zorlar. Tarayıcı olmayan uygulamalar, kullanıcı bunlar için uygulama parolaları varsa çalışmaya devam eder.  Kullanıcıların uygulama parolaları da seçerek silebilirsiniz **seçilen kullanıcılar tarafından oluşturulan tüm var olan uygulama parolalarını Sil**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Nasıl kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfası açılır. 
4. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir. 
5. Seçin **kullanıcı ayarlarını Yönet**.
6. İçin kutuyu **seçilen kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste**.
   ![İletişim yöntemlerini sağlar.](./media/howto-mfa-userdevicesettings/reproofup.png)
7. **Kaydet**’e tıklayın.
8. Tıklayın **kapatmak**.

## <a name="delete-users-existing-app-passwords"></a>Kullanıcılar var olan uygulama parolalarını Sil
Bu ayar tüm bir kullanıcının oluşturduğu uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkilendirilmiş, tarayıcı olmayan uygulamalar, yeni bir parola oluşturuluncaya kadar çalışmasını durdurabilir.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcıların uygulama parolaları varolan silme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfası açılır. 
6. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir. 
7. Seçin **kullanıcı ayarlarını Yönet**.
8. İçin kutuyu **seçilen kullanıcılar tarafından oluşturulan tüm var olan uygulama parolalarını Sil**.
   ![Uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
9. **Kaydet**’e tıklayın.
10. Tıklayın **kapatmak**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Mfa'yı bir kullanıcı için tüm hatırlanan cihazlarda geri yükle
Azure multi-Factor Authentication'ın yapılandırılabilir özelliklerden biri, kullanıcılarınızın cihazları güvenilir olarak işaretlemek için seçeneği veriyor. Daha fazla bilgi için [Azure multi-Factor Authentication'ı yapılandırma ayarlarını](howto-mfa-mfasettings.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Kullanıcılar normal cihazlarından gün yapılandırılabilir bir süre için iki aşamalı doğrulamayı dışında tercih edebilirsiniz. Bir hesap tehlikede ya da güvenilir bir cihaz kaybolursa, güvenilen durumunu kaldırın ve yeniden iki aşamalı kimlik doğrulaması gerekir.

**Geri yükleme multi factor authentication tüm hatırlanan cihazlarda** kullanıcı olacak ayarlamak beden bunlar, ister kendi cihazı olarak işaretlemek seçtikleri bağımsız olarak bir sonraki oturum açışınızda iki aşamalı doğrulamayı gerçekleştirmek için güvenilir. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Mfa'yı bir kullanıcı için askıya alınmış tüm cihazlarda geri yükleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfası açılır. 
6. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir. 
7. Seçin **kullanıcı ayarlarını Yönet**.
8. İçin kutuyu **geri yükleme multi factor authentication tüm hatırlanan cihazlarda**
   ![uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/rememberdevices.png)
9. **Kaydet**’e tıklayın.
10. Tıklayın **kapatmak**.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure multi-Factor Authentication'ı yapılandırma ayarları](howto-mfa-mfasettings.md)

- Kullanıcılarınızın yardıma ihtiyacınız varsa, bunları doğru noktası [iki aşamalı doğrulama için Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user.md)
