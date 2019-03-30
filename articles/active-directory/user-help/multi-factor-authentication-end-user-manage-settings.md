---
title: İki adımlı doğrulama ayarlarınızı - Azure Active Directory yönetme | Microsoft Docs
description: İletişim bilgilerinizi değiştirmek veya cihazlarınızı yapılandırmak da dahil olmak üzere Azure multi-Factor Authentication kullanımını yönetin.
services: active-directory
keywords: çok faktörlü kimlik doğrulama istemcisi, kimlik doğrulama sorunu, bağıntı kimliği
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 433c2d712ca4867a5ec59f86c333511070b6d507
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665058"
---
# <a name="manage-your-settings-for-two-step-verification"></a>İki adımlı doğrulama ayarlarınızı yönetme
Bu makalede, iki aşamalı doğrulama veya çok faktörlü kimlik doğrulaması ayarlarını güncelleştirme hakkında sorular yanıtlanmaktadır. Hesabınızda oturum açma sorunları yaşıyorsanız başvurmak [iki aşamalı doğrulama konusunda sorun mu yaşıyorsunuz](multi-factor-authentication-end-user-troubleshoot.md) sorun giderme Yardımı.

## <a name="where-to-find-the-settings-page"></a>Ayarlar sayfasını nerede bulacağını
Şirketinizin Azure multi-Factor Authentication kurulumu nasıl ayarladığına bağlı olarak, ayarlarınızı telefon numaranız gibi değiştirebileceğiniz birkaç yerde mevcuttur.

Şirketinizin destek, iki aşamalı doğrulama yönetmek için bu yönergeleri izleyerek belirli bir URL veya adımları gönderdi. Aksi takdirde, aşağıdaki yönergeleri herkes için çalışması gerekir. Aşağıdaki adımları izleyin, ancak aynı seçenekleri göremiyorsanız, bu iş veya Okul hesabınız, kendi portalı özelleştirilmiş anlamına gelir. Azure multi-Factor Authentication portalı bağlantısını için yöneticinize başvurun.

**Ek güvenlik doğrulama sayfasına gidin**

- https://aka.ms/MFASetup kısmına gidin.

    ![Düzenleyebileceğinizi](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

Bu bağlantıya tıkladığınızda sizin için işe yaramazsa, ayrıca ulaşmak için **ek güvenlik doğrulaması** sayfasında aşağıdaki adımları izleyerek:

1. Oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. Sağ üst hesabınızın adını seçin ve ardından **profili**.

3. Seçin **ek güvenlik doğrulaması**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. Ek güvenlik doğrulama sayfasına ayarlarınızla yükler.

    ![Düzenleyebileceğinizi](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Telefon numaramı değiştirin ya da ikincil numara eklemek istiyorum
İkincil kimlik doğrulama telefon numarasını yapılandırmak önemlidir.  Birincil telefon numaranızı ve mobil uygulamanıza büyük olasılıkla aynı telefonda olduğu için ikincil bir telefon numarası, telefonunuz kaybolur veya çalınırsa hesabınıza geri almak mümkün olmayacak en hızlı yoludur.

> [!NOTE]
> Birincil telefon numaranızı erişimi ve hesabınıza alınırken yardıma ihtiyacınız yoksa bkz [iki aşamalı doğrulama konusunda sorun mu yaşıyorsunuz](multi-factor-authentication-end-user-troubleshoot.md) daha fazla yardım makalesi.  

**Birincil telefon numaranızı değiştirmek için:**  

1. Üzerinde **ek güvenlik doğrulaması** sayfasında, geçerli bir telefon numarası ile metin kutusunu seçin ve yeni telefon numaranızı ile düzenleyin.  
2. **Kaydet**’i seçin.  
3. Bu telefon numarası için tercih edilen doğrulama seçeneğini kullanan bir sayı ise, kaydedebilmek için önce yeni numarası doğrulamanız gerekir.  

**İkincil bir telefon numarası eklemek için:**  

1. Ek güvenlik doğrulama sayfasında yanındaki kutuyu işaretleyin **alternatif kimlik doğrulama telefonu.**  
2. İkincil bir telefon numaranız, metin kutusuna girin.  
3. Seçin **Kaydet** ve yaptığınız değişiklikleri tamamlandı.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>İki aşamalı doğrulamayı yeniden güvenilir olarak işaretlediğiniz bir cihaz gerektirir

Kuruluş ayarlarınıza bağlı olarak bildiren bir onay kutusu olabilir "için tekrar sorma **X** gün" gerçekleştirdiğinizde iki aşamalı doğrulama tarayıcınızda. Bu onay kutusunu işaretleyin ve Cihazınızı kaybeder veya hesabınızın güvenliği ihlal olduğunu düşündüğünüz tüm cihazlarınıza iki aşamalı doğrulama geri yüklemeniz gerekir.

1. Ek güvenlik doğrulama sayfasında **önceden güvenilen cihazlara geri yükleme çok faktörlü kimlik doğrulaması**.
2. Herhangi bir cihazda oturum açtığınızda iki aşamalı doğrulamayı gerçekleştirmek üzere istenir.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Nasıl Microsoft Authenticator eski aygıttan temizlemek ve yeni bir hareket?
Cihazınızın uygulamayı kaldırın veya Cihazınızı sıfırladığınızda, arka uçta etkinleştirme kaldırmaz. Daha fazla bilgi için [Microsoft Authenticator](user-help-auth-app-download-install.md).

## <a name="next-steps"></a>Sonraki adımlar
* Sorun giderme ipuçları alın ve Yardım [iki aşamalı doğrulama konusunda sorun mu yaşıyorsunuz](multi-factor-authentication-end-user-troubleshoot.md)
* Ayarlanan [uygulama parolaları](multi-factor-authentication-end-user-app-passwords.md) iki aşamalı doğrulamayı desteklemeyen tüm uygulamalar için.
