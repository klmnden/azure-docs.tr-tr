---
title: Süre sonu ilkesi Hızlı Başlangıç için Office 365 grupları - Azure Active Directory | Microsoft Docs
description: Office 365 grupları için süre sonu - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0573448c753c763e818d641216033dbeacb9e9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60471497"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Hızlı Başlangıç: Office 365 grupları Azure Active Directory'de dolmak üzere

Bu hızlı başlangıçta Office 365 gruplarınız için süre sonu ilkesini ayarlayacaksınız. Kullanıcılar kendi gruplarını oluşturma iznine sahip olduğunda, kullanılmayan grupların sayısı artabilir. Kullanılmayan grupları yönetmenin bir yolu, bu grupların için sona erme ilkesi ayarlayarak grupları el ile silme zahmetini ortadan kaldırmaktır.

Süre sonu ilkesi oldukça basittir:

* Grup sahiplerine süresi dolan bir grupla ilgili yenileme bildirimi gönderilir
* Yenilenmeyen gruplar silinir
* Silinen Office 365 grupları, grup sahibi veya bir Azure AD yöneticisi tarafından 30 gün içinde geri yüklenebilir

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisite"></a>Önkoşul

Bir genel yönetici veya Kullanıcı Yöneticisi grubu süre sonlarını ayarlama kuruluştaki olması gerekir.

## <a name="turn-on-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini açma

1. Oturum [Azure portalında](https://portal.azure.com) bir genel yönetici veya Kullanıcı Yöneticisi kuruluş için olan bir hesapla.

2. **Gruplar**'ı ve ardından **Genel**'i seçin.
  
   ![Self Servis grup ayarları sayfası](./media/groups-quickstart-expiration/self-service-settings.png)

3. **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.

4. İşiniz tamamlandığında grup ayarlarını kaydetmek için **Kaydet**'i seçin.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. Oturum [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory** > **grupları** > **sona erme** için sona erme Ayarları'nı açın.
  
   ![Grubun sona erme Ayarları sayfası](./media/groups-quickstart-expiration/expiration-settings.png)

2. Süre sonu aralığını ayarlayın. Önceden belirlenmiş değerlerden birini seçin veya 31 günden yüksek bir değer girin. 

3. Sahibi olmayan gruplar için sona erme bildirimlerinin gönderileceği e-posta adresini girin.

4. Bu hızlı başlangıç için **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Tümü** olarak belirleyin.

5. İşiniz tamamlandığında süre sonu ayarlarını kaydetmek için **Kaydet**'i seçin.

İşte bu kadar! Bu hızlı başlangıçta seçilen Office 365 grupları için süre sonu ilkesini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="to-remove-the-expiration-policy"></a>Süre sonu ilkesi kaldırmak için

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açtığınızdan emin olun.
2. **Azure Active Directory** > **Gruplar** > **Süre Sonu**'nu seçin.
3. **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Yok** olarak değiştirin.

### <a name="to-turn-off-user-creation-for-groups"></a>Grupları için kullanıcı oluşturma devre dışı bırakmak için

1. **Azure Active Directory** > **Gruplar** > **Genel**'i seçin. 
2. **Kullanıcılar, Azure portallarında Office 365 grupları oluşturabilir** ayarını **Hayır** olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Teknik kısıtlamalar, özel engellenen sözcük listesi ekleme ve Office 365 uygulamalarında son kullanıcı deneyimi dahil olmak üzere süre sonu hakkında daha fazla bilgi edinmek için aşağıdaki süre sonu ilkesi makalesini inceleyin:

> [!div class="nextstepaction"]
> [Süre sonu ilkesiyle ilgili tüm ayrıntılar](groups-lifecycle.md)
