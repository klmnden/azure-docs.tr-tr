---
title: "Azure Active Directory erişim sorunlarını giderme | Microsoft Belgeleri"
description: "Kuruluşunuzun çevrimiçi kaynaklara erişirken karşılaştığı sorunları çözmek için uygulayabileceğiniz adımları öğrenin."
services: active-directory
keywords: "cihaz temelli koşullu erişim, cihaz kaydı, cihaz kaydını etkinleştirme, cihaz kaydı ve MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fd2076f22c6048fda83d6da3b069e2805afb453f


---
# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Azure Active Directory erişim sorunlarını giderme
Kuruluşunuzun SharePoint Online intranetine erişmeye çalışıyorsunuz ve bir "erişim reddedildi" hata iletisi alıyorsunuz. Ne yaparsınız?


Bu makalede, kuruluşunuzun çevrimiçi kaynaklara erişirken karşılaştığı sorunları çözmenize yardımcı olabilecek düzeltme adımları anlatılmaktadır.

Azure Active Directory (Azure AD) erişim sorunlarını çözmenize yardımcı olması için cihaz platformunuzun anlatıldığı bölüme gidin:

* Windows cihazı
* iOS cihazı (iPhone'lar ve iPad'ler ile ilgili yardım için yakında tekrar kontrol edin.)
* Android cihazı (Android telefonlar ve tabletler ile ilgili yardım için yakında tekrar kontrol edin.)

## <a name="access-from-a-windows-device"></a>Windows cihazından erişim
Cihazınız aşağıdaki platformlardan birini çalıştırıyorsa bir uygulamaya veya hizmete erişmeyi denediğinizde görüntülenen hata iletisi için bir sonraki bölüme bakın:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Cihaz kayıtlı değil
Cihazınız Azure AD'ye kayıtlı değilse ve uygulama, cihaz temelli bir ilkeyle korunuyorsa şu hata iletilerinden birinin görüntülendiği bir sayfa görebilirsiniz:

![Kaydedilmemiş cihazlar için "Buradan oraya ulaşamazsınız" iletileri](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

Cihazınız, kuruluşunuzda Active Directory etki alanına katılmışsa şunu deneyin:

1. Windows'da iş hesabınızı (Active Directory hesabınız) kullanarak oturum açtığınızdan emin olun.
2. Kurumsal ağınıza bir sanal özel ağ (VPN) veya DirectAccess aracılığıyla bağlanın.
3. Bağlandıktan sonra, Windows oturumunuzu kilitlemek için Windows logosu tuşu ile birlikte L tuşuna basın.
4. Windows oturumunuzun kilidini açmak için iş hesabınızın kimlik bilgilerini girin.
5. Bir dakika bekleyin ve ardından uygulamaya veya hizmete tekrar erişmeyi deneyin.
6. Aynı sayfayı görürseniz **More details** (Diğer ayrıntılar) bağlantısına tıklayın ve ardından, bu ayrıntılarla birlikte yöneticinizle iletişime geçin.

Cihazınız etki alanına katılmamışsa ve Windows 10 çalıştırıyorsa iki seçeneğiniz vardır:

* Azure AD'ye Katılım’ı çalıştırmak
* Windows'a iş veya okul hesabınızı eklemek

Bu iki seçenek arasındaki farklar hakkında bilgi edinmek için bkz. [Çalışma alanınızda Windows 10 cihazlarını kullanma](active-directory-azureadjoin-windows10-devices.md).

Azure AD Join'i çalıştırmak için cihazınızın çalıştırdığı platforma göre aşağıdaki adımları uygulayın. (Azure AD Join özelliği, Windows telefonlarda kullanılamıyor.)

**Windows 10 Yıldönümü Güncelleştirmesi**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **İş veya okul erişimi** öğesine tıklayın.
3. **Bağlan**'a tıklayın.
4. **Bu cihazı Azure AD'ye ekle**'ye tıklayın.
5. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
6. Oturumu kapatın ve iş hesabınızla oturum açın.
7. Uygulamaya erişmeyi tekrar deneyin.

**Windows 10 Kasım 2015 Güncelleştirmesi**

1. **Ayarlar** uygulamasını başlatın.
2. **Sistem** > **Hakkında** öğesine tıklayın.
3. **Azure AD'ye Katıl**’a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Oturumu kapatın ve ardından iş hesabınızla (Azure AD hesabınız) oturum açın.
6. Uygulamaya erişmeyi tekrar deneyin.

İş veya okul hesabınızı eklemek için şu adımları uygulayın:

**Windows 10 Yıldönümü Güncelleştirmesi**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **İş veya okul erişimi** öğesine tıklayın.
3. **Bağlan**'a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Uygulamaya erişmeyi tekrar deneyin.

**Windows 10 Kasım 2015 Güncelleştirmesi**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **Hesaplarınız** öğesine tıklayın.
3. **İş veya okul hesabı ekle**’ye tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Uygulamaya erişmeyi tekrar deneyin.

Cihazınız etki alanına katılmamışsa ve Windows 8.1 çalıştırıyorsa Workplace Join özelliğini kullanmak ve Microsoft Intune'a kaydolmak için şu adımları uygulayın:

1. **PC Ayarları**'nı açın.
2. **Ağ** > **Çalışma Alanı**’na tıklayın.
3. **Katıl**’a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. **Aç**’a tıklayın.
6. Uygulamaya erişmeyi tekrar deneyin.

### <a name="browser-is-not-supported"></a>Tarayıcı desteklenmiyor
Şu tarayıcılardan birini kullanarak bir uygulamaya veya hizmete erişmeyi deniyorsanız erişiminiz engellenebilir:

* Windows 10 veya Windows Server 2016'da Chrome ve Firefox'a ek olarak, Microsoft Edge ya da Microsoft Internet Explorer dışındaki diğer tüm tarayıcılar
* Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2'de Firefox

Şuna benzer bir hata sayfası görürsünüz:

![Desteklenmeyen tarayıcılar için "Buradan oraya ulaşamazsınız" iletisi](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

Tek düzeltme seçeneği, cihaz platformunuz için uygulamanın desteklediği bir tarayıcı kullanılmasıdır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory koşullu erişimi](active-directory-conditional-access.md)




<!--HONumber=Nov16_HO2-->


