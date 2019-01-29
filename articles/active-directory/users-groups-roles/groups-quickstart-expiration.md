---
title: Azure Active Directory'deki Office 365 grupları için süre sonu ilkesi hızlı başlangıcı | Microsoft Docs
description: Office 365 grupları için süre sonu - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 08/07/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 6d2b5201c41ba9d5c849976f0227e9abadea7658
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165243"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Hızlı Başlangıç: Office 365 grupları Azure Active Directory'de dolmak üzere

Bu hızlı başlangıçta Office 365 gruplarınız için süre sonu ilkesini ayarlayacaksınız. Kullanıcılar kendi gruplarını oluşturma iznine sahip olduğunda, kullanılmayan grupların sayısı artabilir. Kullanılmayan grupları yönetmenin bir yolu, bu grupların için sona erme ilkesi ayarlayarak grupları el ile silme zahmetini ortadan kaldırmaktır.

Süre sonu ilkesi oldukça basittir:

* Grup sahiplerine süresi dolan bir grupla ilgili yenileme bildirimi gönderilir
* Yenilenmeyen gruplar silinir
* Silinen Office 365 grupları, grup sahibi veya bir Azure AD yöneticisi tarafından 30 gün içinde geri yüklenebilir

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisite"></a>Önkoşul

Grup süre sonu ilkesini ayarlamak için kiracıda Genel Yönetici veya Kullanıcı Hesabı Yöneticisi olmanız gerekir.

## <a name="turn-on-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini açma

1. [Azure portalda](https://portal.azure.com) dizinin Genel Yönetici veya Kullanıcı Hesabı Yöneticisi hesaplarından biriyle oturum açın.

2. **Gruplar**'ı ve ardından **Genel**'i seçin.
  
  ![Self servis grup ayarları](./media/groups-quickstart-expiration/self-service-settings.png)

3. **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.

4. İşiniz tamamlandığında grup ayarlarını kaydetmek için **Kaydet**'i seçin.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. [Azure portalda](https://portal.azure.com), **Azure Active Directory** > **Gruplar** > **Süre Sonu**'nu açarak süre sonu ayarlarını açın.
  
  ![Süre sonu ayarları](./media/groups-quickstart-expiration/expiration-settings.png)

2. Süre sonu aralığını ayarlayın. Önceden belirlenmiş değerlerden birini seçin veya 31 günden yüksek bir değer girin. 

3. Sahibi olmayan gruplar için sona erme bildirimlerinin gönderileceği e-posta adresini girin.

4. Bu hızlı başlangıç için **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Tümü** olarak belirleyin.

5. İşiniz tamamlandığında süre sonu ayarlarını kaydetmek için **Kaydet**'i seçin.

İşte bu kadar! Bu hızlı başlangıçta seçilen Office 365 grupları için süre sonu ilkesini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Süre sonu ilkesi kaldırmak için**

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açtığınızdan emin olun.
2. **Azure Active Directory** > **Gruplar** > **Süre Sonu**'nu seçin.
3. **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Yok** olarak değiştirin.

**Kullanıcılar için grup oluşturma özelliğini kapatmak için**

1. **Azure Active Directory** > **Gruplar** > **Genel**'i seçin. 
2. **Kullanıcılar, Azure portallarında Office 365 grupları oluşturabilir** ayarını **Hayır** olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Teknik kısıtlamalar, özel engellenen sözcük listesi ekleme ve Office 365 uygulamalarında son kullanıcı deneyimi dahil olmak üzere süre sonu hakkında daha fazla bilgi edinmek için aşağıdaki süre sonu ilkesi makalesini inceleyin:

> [!div class="nextstepaction"]
> [Süre sonu ilkesiyle ilgili tüm ayrıntılar](groups-lifecycle.md)
