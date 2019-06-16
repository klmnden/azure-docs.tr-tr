---
title: Yöneticileri Yönet kullanıcılara ve cihazlara - Azure MFA - Azure Active Directory
description: Yöneticiler, kullanıcıların kavram artırma işlemi yapmayı tekrar deneyebilir zorlama gibi kullanıcı ayarlarını nasıl değiştirebilir.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04d4848a00fd645bcf23342f27fe820ccf034a8b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66298836"
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
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.
3. Sağ tarafta seçin **multi-Factor Authentication** araç. Çok faktörlü kimlik doğrulaması sayfası açılır.
4. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir.
5. Seçin **kullanıcı ayarlarını Yönet**.
6. İçin kutuyu **seçilen kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste**.
   ![Kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste](./media/howto-mfa-userdevicesettings/reproofup.png)
7. **Kaydet**’e tıklayın.
8. Tıklayın **kapatmak**.

Kuruluşlar, aşağıdaki adımları temizlemek için bir kılavuz olarak aşağıdaki kullanarak PowerShell ile tamamlayabilir `StrongAuthenticationMethods` özniteliği:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>Kullanıcılar var olan uygulama parolalarını Sil

Bu ayar tüm bir kullanıcının oluşturduğu uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkilendirilmiş, tarayıcı olmayan uygulamalar, yeni bir parola oluşturuluncaya kadar çalışmasını durdurabilir.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcıların uygulama parolaları varolan silme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.
3. Sağ tarafta seçin **multi-Factor Authentication** araç. Çok faktörlü kimlik doğrulaması sayfası açılır.
4. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir.
5. Seçin **kullanıcı ayarlarını Yönet**.
6. İçin kutuyu **seçilen kullanıcılar tarafından oluşturulan tüm var olan uygulama parolalarını Sil**.
   ![Mevcut tüm uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. **Kaydet**’e tıklayın.
8. Tıklayın **kapatmak**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Mfa'yı bir kullanıcı için tüm hatırlanan cihazlarda geri yükle

Azure multi-Factor Authentication'ın yapılandırılabilir özelliklerden biri, kullanıcılarınızın cihazları güvenilir olarak işaretlemek için seçeneği veriyor. Daha fazla bilgi için [Azure multi-Factor Authentication'ı yapılandırma ayarlarını](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Kullanıcılar normal cihazlarından gün yapılandırılabilir bir süre için iki aşamalı doğrulamayı dışında tercih edebilirsiniz. Bir hesap tehlikede ya da güvenilir bir cihaz kaybolursa, güvenilen durumunu kaldırın ve yeniden iki aşamalı kimlik doğrulaması gerekir.

Bu onay kutusu işaretlendiğinde, **geri yükleme multi factor authentication tüm hatırlanan cihazlarda** kullanıcıları cihazlarını güvenilen olarak işaretlenmiş olsa bile, oturum açtığınızda iki aşamalı doğrulamayı gerçekleştirmek için gerekli değildir.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Mfa'yı bir kullanıcı için askıya alınmış tüm cihazlarda geri yükleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.
3. Sağ tarafta seçin **multi-Factor Authentication** araç. Çok faktörlü kimlik doğrulaması sayfası açılır.
4. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesi, sağda görüntülenir.
5. Seçin **kullanıcı ayarlarını Yönet**.
6. İçin kutuyu **geri yükleme multi factor authentication tüm hatırlanan cihazlarda**
   ![geri yükleme multi factor authentication tüm hatırlanan cihazlarda](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. **Kaydet**’e tıklayın.
8. Tıklayın **kapatmak**.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure multi-Factor Authentication'ı yapılandırma ayarları](howto-mfa-mfasettings.md)
- Kullanıcılarınızın yardıma ihtiyacınız varsa, bunları doğru noktası [iki aşamalı doğrulama için Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user.md)
