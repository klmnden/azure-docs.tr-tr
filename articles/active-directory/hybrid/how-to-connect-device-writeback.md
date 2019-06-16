---
title: 'Azure AD Connect: Cihaz geri yazma özelliğini etkinleştirme | Microsoft Docs'
description: Bu belge, Azure AD Connect kullanarak cihaz geri yazmayı etkinleştirme işlemi açıklanmaktadır
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
ms.topic: conceptual
ms.date: 05/08/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 632f6f80184c6ba3409bd30ae070cbaefc77f036
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109493"
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Cihaz geri yazma özelliğini etkinleştirme
> [!NOTE]
> Cihaz geri yazma için Azure AD Premium aboneliği gereklidir.
> 
> 

Aşağıdaki belgeler Azure AD CONNECT'te cihaz geri yazma özelliğini etkinleştirme hakkında bilgi sağlar. Cihaz geri yazma, aşağıdaki senaryolarda kullanılır:

* Etkinleştirme [Windows iş için Hello karma sertifika güven dağıtımı kullanma](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust-prereqs#device-registration)
* ADFS cihazlara dayalı koşullu erişimi etkinleştir (2012 R2 veya üzeri) korumalı uygulamaları (bağlı olan taraf güvenleri).

Bu, ek güvenlik ve yalnızca güvenilen cihazlara verilen uygulamalara erişimi güvence sağlar. Koşullu erişim hakkında daha fazla bilgi için bkz. [koşullu erişim ile Risk yönetme](../active-directory-conditional-access-azure-portal.md) ve [şirket içi koşullu Azure Active Directory cihaz kaydı hizmetini kullanarak erişimi ayarlarken](../../active-directory/active-directory-device-registration-on-premises-setup.md).

> [!IMPORTANT]
> <li>Cihazlar, kullanıcılar aynı ormandaki bulunması gerekir. Cihazları tek bir ormana geri yazılması gerekir olduğundan, bu özellik birden çok kullanıcı ormanı olan bir dağıtım şu anda desteklemiyor.</li>
> <li>Şirket içi Active Directory ormanı için yalnızca bir cihaz kaydı yapılandırma nesnesi eklenebilir. Bu özellik şirket içi Active Directory birden çok Azure AD dizini için burada eşitlenmiş bir topoloji ile uyumlu değil.</li>

## <a name="part-1-install-azure-ad-connect"></a>1\. Bölüm: Azure AD Connect'i yükleme
Azure AD Connect'i kullanarak özel veya hızlı ayarlar. Microsoft ile başlaması cihaz geri yazmayı etkinleştirme önce tüm kullanıcılar ve gruplar başarıyla eşitlendi önerir.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>2\. Bölüm: Azure AD CONNECT'te cihaz geri yazmayı etkinleştirme
1. Yükleme Sihirbazı'nı yeniden çalıştırın. Seçin **cihaz seçeneklerini yapılandır** ek görevler sayfasında ve tıklayın **sonraki**. 

    ![Cihaz seçeneklerini yapılandır](./media/how-to-connect-device-writeback/deviceoptions.png)

    >[!NOTE]
    > Yeni yapılandırma cihaz seçenekleri, yalnızca sürüm 1.1.819.0 kullanılabilir ve daha yeni.

2. Cihaz seçenekleri sayfasında **cihaz geri yazmayı yapılandırma**. Seçeneğini **cihaz geri yazmayı devre dışı bırak** cihaz geri yazmayı etkinleştirilene kadar kullanılamaz. Tıklayarak **sonraki** sihirbazdaki sonraki sayfaya taşınır.
    ![Seçtiğiniz cihaz işlemi](./media/how-to-connect-device-writeback/configuredevicewriteback1.png)

3. Geri yazma sayfasında, varsayılan cihaz geri yazma ormanı sağlanan etki alanı görürsünüz.
   ![Özel yükleme cihaz geri yazma hedef orman](./media/how-to-connect-device-writeback/writebackforest.png)

4. **Cihaz kapsayıcı** sayfasında iki kullanılabilir seçeneklerden birini kullanarak active directory hazırlama bir seçenek sağlar:

    a. **Kuruluş Yöneticisi kimlik bilgileri sağlama**: Azure AD Connect orman cihazlar geri yazılması gereken yere orman için kuruluş yöneticisi kimlik bilgilerini sağladıysanız cihaz geri yazma yapılandırması sırasında otomatik olarak hazırlar.

    b. **PowerShell betiğini indir**: Azure AD Connect otomatik-active directory cihaz geri yazma için hazırlamak bir PowerShell Betiği oluşturur. Kuruluş Yöneticisi kimlik bilgilerini Azure AD Connect'e bağlanan sağlanamaz durumda PowerShell betiğini indirmek için önerilir. İndirilen PowerShell betiğini sağlamak **CreateDeviceContainer.psq** nerede cihazları yazılacak geri için orman Kurumsal yöneticisine.
    ![Active directory ormanı hazırlama](./media/how-to-connect-device-writeback/devicecontainercreds.png)
    
    Active directory ormanı hazırlamak için aşağıdaki işlemler gerçekleştirilir:
    * Bunlar zaten mevcut, oluşturur ve yeni kapsayıcılar ve nesneler CN altında ayarlarsa Device Registration Configuration, CN = Services, CN = Configuration, = [dn orman].
    * Bunlar zaten mevcut, oluşturur ve yeni kapsayıcılar ve nesneler CN altında ayarlarsa RegisteredDevices, = [dn etki alanı]. Cihaz nesneleri, bu kapsayıcı içinde oluşturulur.
    * Active Directory cihazları yönetmek için Azure AD Bağlayıcısı hesabındaki gerekli izinleri ayarlar.
    * Azure AD Connect birden çok ormanı üzerinde yüklü olsa bile, bir ormanda üzerinde çalıştırmak için yalnızca gerekir.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Cihazlar Active Directory'ye eşitlenen doğrulayın
Cihaz geri yazma düzgün şekilde çalışıyor. Cihaz nesneleri, yazılan sonradan AD'ye olması için 3 saate kadar sürebilir dikkat edin.  Eşitleme kuralları tamamladıktan sonra cihazlarınızı eşitlenen düzgün bir şekilde doğrulamak için aşağıdakileri yapın:

1. Active Directory Yönetim Merkezi'ni başlatın.
2. RegisteredDevices, Federasyon etki alanı içinde genişletin.

   ![Active Directory Yönetim Merkezi kayıtlı cihazlar](./media/how-to-connect-device-writeback/devicewriteback5.png)

3. Geçerli kayıtlı cihazlar burada listelenir.

   ![Active Directory Yönetim Merkezi kayıtlı cihazlar listesi](./media/how-to-connect-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Koşullu erişimi etkinleştirme
İçinde bu senaryoyu etkinleştirmek için ayrıntılı yönergeler mevcuttur [şirket içi koşullu Azure Active Directory cihaz kaydı hizmetini kullanarak erişimi ayarlarken](../../active-directory/active-directory-device-registration-on-premises-setup.md).

## <a name="troubleshooting"></a>Sorun giderme
### <a name="the-writeback-checkbox-is-still-disabled"></a>Geri yazma onay yine de devre dışı bırakıldı
Yukarıdaki adımları izlediyseniz olsa bile cihaz geri yazma için onay kutusunu etkin değilse, aşağıdaki adımları, Yükleme Sihirbazı'nı doğrulama aracılığıyla yönlendirecektir kutusu etkinleştirmeden önce.

İlk şey ilk:

* Cihazları mevcut olduğu orman, böylece cihaz nesnesini ve ilişkili öznitelikleri mevcut olduğundan Windows 2012 R2 düzeyine yükseltildiğinde Orman şeması olmalıdır.
* Yükleme Sihirbazı'nı zaten çalışıyorsa, değişikliklerin algılanmaz. Bu durumda, yükleme sihirbazını tamamlayın ve yeniden çalıştırın.
* Aslında Active Directory Bağlayıcısı tarafından kullanılan doğru kullanıcı başlatma komut dosyasında belirttiğiniz hesabının olduğundan emin olun. Bunu doğrulamak için aşağıdaki adımları izleyin:
  * Başlat menüsünden açmak **eşitleme hizmeti**.
  * Açık **Bağlayıcılar** sekmesi.
  * Active Directory Domain Services türüyle bağlayıcıyı bulun ve seçin.
  * Altında **eylemleri**seçin **özellikleri**.
  * Git **Active Directory ormanına Bağlan**. Etki alanı ve kullanıcı adı için betik sağlanan hesabı bu ekran eşleşmeye belirttiğinizi doğrulayın.
    ![Eşitleme Hizmeti Yöneticisi'nde Bağlayıcısı hesabı](./media/how-to-connect-device-writeback/connectoraccount.png)

Active Directory yapılandırmasını doğrulayın:

* Cihaz Kayıt Hizmeti'ni konumda bulunduğundan emin olun (CN DeviceRegistrationService, CN = cihaz kayıt Services, CN = Device Registration Configuration, CN = Services, CN = Configuration =) yapılandırma adlandırma bağlamında.

![Sorun giderme, yapılandırma ad alanında DeviceRegistrationService](./media/how-to-connect-device-writeback/troubleshoot1.png)

* Yapılandırma ad alanı arama yaparak yalnızca bir yapılandırma nesnesi olduğunu doğrulayın. Varsa birden fazla yinelenen silin.

![Sorun giderme, yinelenen nesneleri arayın](./media/how-to-connect-device-writeback/troubleshoot2.png)

* Cihaz kaydı hizmeti nesnesinde msDS-DeviceLocation öznitelik varsa ve bir değer emin olun. Arama bu konumu ve emin ile objectType msDS-DeviceContainer mevcut.

![Sorun giderme, msDS-DeviceLocation](./media/how-to-connect-device-writeback/troubleshoot3.png)

![Sorun giderme, RegisteredDevices nesne sınıfı](./media/how-to-connect-device-writeback/troubleshoot4.png)

* Önceki adım bulunan kayıtlı cihazlar kapsayıcısı üzerinde Active Directory Bağlayıcısı tarafından kullanılan hesap gerekli izinlere sahip olduğunu doğrulayın. Bu beklenen bu kapsayıcıdaki izinleri.

![Sorun giderme, kapsayıcı üzerindeki izinleri doğrulayın](./media/how-to-connect-device-writeback/troubleshoot5.png)

* Active Directory hesabına sahip izinleri üzerinde CN doğrulayın Device Registration Configuration, CN = Services, CN = Yapılandırma nesnesi =.

![Sorun giderme, cihaz kaydı yapılandırma üzerindeki izinleri doğrulayın](./media/how-to-connect-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Ek Bilgi
* [Koşullu erişim ile risk yönetme](../active-directory-conditional-access-azure-portal.md)
* [Azure Active Directory cihaz kaydı hizmetini kullanarak şirket içi koşullu erişim ayarlama](../../active-directory/active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

