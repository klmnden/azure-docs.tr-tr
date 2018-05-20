---
title: 'Azure AD Connect: cihaz geri yazma özelliğini etkinleştirme | Microsoft Docs'
description: Bu belgede Azure AD Connect'i kullanarak cihaz geri yazma özelliğini etkinleştirme ayrıntıları
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: billmath
ms.openlocfilehash: c813be558df9dc3bdfd9850402b9458f1fdf971a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: cihaz geri yazma özelliğini etkinleştirme
> [!NOTE]
> Azure AD Premium aboneliği cihaz geri yazma için gereklidir.
> 
> 

Aşağıdaki belgeler Azure AD CONNECT'te cihaz geri yazma özelliğini etkinleştirme hakkında bilgi sağlar. Cihaz geri yazma aşağıdaki senaryolarda kullanılır:

* ADFS cihaza bağlı olarak koşullu erişim etkinleştir (2012 R2 veya üstü) korumalı uygulamaları (bağlı olan taraf güvenleri).

Bu, ek güvenlik ve uygulamalara erişimi yalnızca güvenilen cihazlara verilen güvence sağlar. Koşullu erişim hakkında daha fazla bilgi için bkz: [koşullu erişim ile Risk yönetme](../active-directory-conditional-access-azure-portal.md) ve [şirket içi koşullu Azure Active Directory cihaz kaydı kullanarak erişimi ayarlama](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Cihazları kullanıcıların aynı ormanda yer almalıdır. Cihazları tek bir ormana geri yazılmalıdır olduğundan, bu özellik birden çok kullanıcı ormanı olan bir dağıtım şu anda desteklemiyor.</li>
> <li>Şirket içi Active Directory ormanı için yalnızca bir cihaz kaydı yapılandırma nesnesi eklenebilir. Bu özellik şirket içi Active Directory için birden çok Azure AD dizinlerinden burada eşitlenir topolojisi ile uyumlu değil.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>1. Kısım: Yükleme Azure AD Connect
Özel kullanarak Azure AD Connect'i yüklemek veya hızlı ayarlar. Microsoft, cihaz geri yazma etkinleştirmeden önce tüm kullanıcılar ve gruplar başarıyla eşitlendi başlamak önerir.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>2. Kısım: Etkinleştir cihaz geri yazma özelliğini Azure AD Connect
1. Yükleme Sihirbazı'nı yeniden çalıştırın. Seçin **aygıt seçenekleri yapılandırmak** ek görevler sayfasında ve tıklayın **sonraki**. 

    ![Cihaz seçeneklerini yapılandır](./media/active-directory-aadconnect-feature-device-writeback/deviceoptions.png)

    >[!NOTE]
    > Yeni yapılandırma aygıt seçenekleri, yalnızca sürüm 1.1.819.0 kullanılabilir ve daha yeni.

2. Aygıt seçenekleri sayfasında seçin **cihaz geri yazma yapılandırma**. İçin seçenek **cihaz geri yazma özelliğini devre dışı bırakma** cihaz geri yazma etkinleştirilene kadar kullanılamaz. Tıklayın **sonraki** Sihirbazı sonraki sayfaya gitmek için.
    ![Seçtiğiniz cihaz işlemi](./media/active-directory-aadconnect-feature-device-writeback/configuredevicewriteback1.png)

3. Geri yazma sayfasında, varsayılan cihaz geri yazma orman olarak sağlanan etki alanı görürsünüz.
   ![Özel yükleme cihaz geri yazma hedef orman](./media/active-directory-aadconnect-feature-device-writeback/writebackforest.png)

4. **Cihaz kapsayıcısı** sayfa iki kullanılabilir seçeneklerden birini kullanarak active directory hazırlama bir seçenek sağlar:

    a. **Kuruluş Yöneticisi kimlik bilgileri sağlama**: kuruluş yöneticisi kimlik bilgilerini cihazlar gereken yere geri yazılması için ormanın sağlanıyorsa, Azure AD Connect orman otomatik olarak sırasında yapılandırmasını hazırlar cihaz geri yazma.

    b. **PowerShell komut dosyasını karşıdan yükleyin**: Azure AD Connect oluşturur otomatik active directory cihaz geri yazma için hazırlayabilirsiniz bir PowerShell Betiği. Kuruluş Yöneticisi kimlik bilgilerini Azure AD Connect sağlanamaz durumunda, PowerShell Betiği indirmek için önerilir. İndirilen PowerShell Betiği sağlamak **CreateDeviceContainer.psq** aygıtları yazılacağı geri için ormanın kuruluş yöneticisi için.
    ![Etkin Dizin orman hazırlama](./media/active-directory-aadconnect-feature-device-writeback/devicecontainercreds.png)
    
    Active directory ormanı hazırlamak için aşağıdaki işlemler gerçekleştirilir:
    * Bunlar zaten var olmadığından, oluşturur ve yeni kapsayıcılar ve nesneler CN altında yapılandırır Device Registration Configuration, CN = Services, CN = Configuration, = [orman-dn].
    * Bunlar zaten var olmadığından, oluşturur ve yeni kapsayıcılar ve nesneler CN altında yapılandırır RegisteredDevices, = [etki alanı-dn]. Cihaz nesneleri, bu kapsayıcıda oluşturulur.
    * Active Directory aygıtları yönetmek için Azure AD Bağlayıcısı hesabındaki gerekli izinleri ayarlar.
    * Azure AD Connect üzerinde birden çok orman yüklü olsa bile bir ormanda, çalıştırmak yeterlidir.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Cihazlar Active Directory ile eşitlenmiş doğrulayın
Cihaz geri yazma düzgün çalışıyor. AD için yazılmış geri alınacak aygıt nesneler için 3 saate kadar sürebilir unutmayın.  Eşitleme kuralları tamamladıktan sonra aygıtlarınızı eşitlenen düzgün bir şekilde doğrulamak için aşağıdakileri yapın:

1. Active Directory Yönetim Merkezi'ni başlatın.
2. Federe etki alanı içinde RegisteredDevices genişletin.

   ![Active Directory Yönetim Merkezi'ni cihazların kayıtlı](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)

3. Geçerli kayıtlı cihazlar var. listelenir.

   ![Active Directory Yönetim Merkezi'ni kayıtlı cihazlar listesi](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Koşullu erişimi etkinleştirme
Bu senaryoyu etkinleştirmek için ayrıntılı yönergeler içinde kullanılabilir [şirket içi koşullu Azure Active Directory cihaz kaydı kullanarak erişimi ayarlama](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="troubleshooting"></a>Sorun giderme
### <a name="the-writeback-checkbox-is-still-disabled"></a>Geri yazma onay kutusu hala devre dışı
Yukarıdaki adımları izlemenize rağmen cihaz geri yazma için onay kutusunu etkin değilse, aşağıdaki adımlar, Yükleme Sihirbazı'nı doğrulama aracılığıyla yönlendirecek kutusunu etkinleştirilmeden önce.

İlk şey ilk:

* Windows Server 2012R2 en az bir ormanda olduğundan emin olun. Cihaz nesne türü mevcut olması gerekir.
* Yükleme Sihirbazı'nı zaten çalışıyorsa, herhangi bir değişiklik algılanmaz. Bu durumda, Yükleme Sihirbazı'nı tamamlayın ve yeniden çalıştırın.
* Aslında Active Directory Bağlayıcısı tarafından kullanılan doğru kullanıcı başlatma betiği sağladığınız hesap olduğundan emin olun. Bunu doğrulamak için aşağıdaki adımları izleyin:
  * Başlat menüsünden açmak **eşitleme hizmeti**.
  * Açık **Bağlayıcılar** sekmesi.
  * Active Directory etki alanı Hizmetleri türüyle Bağlayıcısı'nı bulun ve seçin.
  * Altında **Eylemler**seçin **özellikleri**.
  * Git **Active Directory ormanına Bağlan**. Etki alanı ve kullanıcı adı için betik sağlanan hesap bu ekran eşleşmesi belirttiğinizi doğrulayın.
    ![Eşitleme Hizmeti Yöneticisi'nde Bağlayıcısı hesabı](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Active Directory yapılandırmasını doğrulayın:

* Cihaz kaydı hizmeti konumda bulunduğundan emin olun (CN DeviceRegistrationService, CN = cihaz kayıt Services, CN = Device Registration Configuration, CN = Services, CN = Configuration =) yapılandırma adlandırma bağlamında altında.

![Sorun giderme, yapılandırma ad alanındaki DeviceRegistrationService](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Yapılandırma ad alanı arayarak yalnızca bir yapılandırma nesnesi olduğunu doğrulayın. Varsa birden fazla, yinelenen silin.

![Sorun giderme, yinelenen nesneleri arayın](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Cihaz kayıt hizmeti nesnesinde msDS-DeviceLocation özniteliği var ve bir değere sahip emin olun. Arama bu konumu ve emin olun ile objectType msDS-DeviceContainer mevcut.

![Sorun giderme, msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Sorun giderme, RegisteredDevices nesne sınıfı](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Önceki adım bulunan kayıtlı cihazlar kapsayıcısı üzerinde Active Directory Bağlayıcısı tarafından kullanılan hesap gerekli izinlere sahip doğrulayın. Bu kapsayıcı beklenen izinlerini şudur:

![Sorun giderme, kapsayıcısı üzerindeki izinleri doğrulayın](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Active Directory hesabına sahip izinleri üzerinde CN doğrulayın Device Registration Configuration, CN = Services, CN = = Yapılandırma nesnesi.

![Sorun giderme, cihaz kaydı yapılandırma üzerindeki izinleri doğrulayın](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Ek Bilgiler
* [Koşullu erişim ile risk yönetme](../active-directory-conditional-access-azure-portal.md)
* [Azure Active Directory cihaz kaydı ile şirket içi koşullu erişim ayarlama](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

