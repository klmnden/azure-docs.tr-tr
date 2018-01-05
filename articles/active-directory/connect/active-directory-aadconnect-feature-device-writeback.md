---
title: "Azure AD Connect: cihaz geri yazma özelliğini etkinleştirme | Microsoft Docs"
description: "Bu belgede Azure AD Connect'i kullanarak cihaz geri yazma özelliğini etkinleştirme ayrıntıları"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: billmath
ms.openlocfilehash: fddbbeda50764ade149e8a8f370bf7341da01736
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
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
> <li>Şirket içi Active Directory ormanı için yalnızca bir cihaz kaydı yapılandırma nesnesi eklenebilir. Bu özellik şirket içi Active Directory için birden çok Azure AD kiracılar olduğu eşitlenir topolojisi ile uyumlu değil.</li>
>

## <a name="part-1-install-azure-ad-connect"></a>1. Kısım: Yükleme Azure AD Connect
1. Özel kullanarak Azure AD Connect'i yüklemek veya hızlı ayarlar. Microsoft, cihaz geri yazma etkinleştirmeden önce tüm kullanıcılar ve gruplar başarıyla eşitlendi başlamak önerir.

## <a name="part-2-prepare-active-directory"></a>2. Kısım: Active Directory hazırlama
Cihaz geri yazma özelliğini kullanmak için hazırlamak için aşağıdaki adımları kullanın.

1. Azure AD Connect, yüklü olduğu makinede, yükseltilmiş modda PowerShell'i başlatın.
2. Uzak sunucu yönetim AD PowerShell modülü ve komut dosyasını çalıştırmak için gereken dsacls.exe içeren araçları, Active Directory PowerShell Modülü yüklü değilse yükleyin. Şu komutu çalıştırın:

   ``` powershell
   Add-WindowsFeature RSAT-AD-Tools
   ```

3. Azure Active Directory PowerShell Modülü yüklü değilse, ardından yükleyip buradan [Azure Active Directory için Windows PowerShell Modülü (64-bit sürümü)](http://go.microsoft.com/fwlink/p/?linkid=236297). Bu bileşen, Azure AD Connect ile yüklenen oturum açma Yardımcısı üzerinde bir bağımlılığa sahiptir.  
4. Kuruluş Yöneticisi kimlik bilgileriyle aşağıdaki komutları çalıştırın ve ardından PowerShell çıkın.

   ``` powershell
   Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'
   ```

   ``` powershell
   Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}
   ```

Yapılandırma ad alanı değişikliklerin gerekli bu yana Kurumsal yönetici kimlik bilgileri gereklidir. Bir etki alanı yönetici yeterli izne sahip değil.

![Cihaz geri yazma özelliğini etkinleştirmek için Powershell](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)  

Açıklama:

* Bunlar zaten var olmadığından, oluşturur ve yeni kapsayıcılar ve nesneler CN altında yapılandırır Device Registration Configuration, CN = Services, CN = Configuration, = [orman-dn].
* Bunlar zaten var olmadığından, oluşturur ve yeni kapsayıcılar ve nesneler CN altında yapılandırır RegisteredDevices, = [etki alanı-dn]. Cihaz nesneleri, bu kapsayıcıda oluşturulur.
* Active Directory aygıtları yönetmek için Azure AD Bağlayıcısı hesabındaki gerekli izinleri ayarlar.
* Azure AD Connect üzerinde birden çok orman yüklü olsa bile bir ormanda, çalıştırmak yeterlidir.

Parametreler:

* DomainName: Active Directory cihaz nesnelerinin oluşturulacağı etki alanı. Not: Belirli bir Active Directory ormanı için tüm cihazları tek bir etki alanında oluşturulur.
* AdConnectorAccount: Azure AD Connect tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı. Bu, Azure AD Connect eşitleme tarafından AD'ye bağlanmak için kullanılan hesaptır. Hızlı ayarları kullanarak yüklediyseniz, MSOL_ ile önek hesabıdır.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>3. Kısım: Etkinleştir cihaz geri yazma özelliğini Azure AD Connect
Azure AD CONNECT'te cihaz geri yazma özelliğini etkinleştirmek için aşağıdaki yordamı kullanın.

1. Yükleme Sihirbazı'nı yeniden çalıştırın. Seçin **eşitleme seçeneklerini özelleştirme** ek görevler sayfasında ve tıklayın **sonraki**.
   ![Özel yükleme eşitleme seçeneklerini özelleştirme](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. İsteğe bağlı özellikler sayfasında, cihaz geri yazma artık gri olacaktır. Lütfen Azure AD Connect, hazırlık adımları tamamlanmış cihaz geri yazma olduğunu not out isteğe bağlı özellikler sayfasında gri. Cihaz geri yazma için kutuyu işaretleyin ve tıklatın **sonraki**. Onay kutusu hala devre dışı olup [sorun giderme bölümüne](#the-writeback-checkbox-is-still-disabled).
   ![Özel cihaz geri yazma isteğe bağlı özellikler yükleme](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Geri yazma sayfasında, varsayılan cihaz geri yazma orman olarak sağlanan etki alanı görürsünüz.
   ![Özel yükleme cihaz geri yazma hedef orman](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Hiçbir ek yapılandırma değişiklikleri Sihirbazı yüklemeyi tamamlayın. Gerekirse, başvurmak [Azure AD Connect özel yüklemesi.](active-directory-aadconnect-get-started-custom.md)
5. Etkinleştirdiyseniz [filtreleme](active-directory-aadconnectsync-configure-filtering.md) Azure AD Connect sonra yeni oluşturulan kapsayıcı CN emin olun = RegisteredDevices kapsam içinde yer almaktadır.

## <a name="part-4-verify-devices-are-synchronized-to-active-directory"></a>4. Kısım: Doğrulayın cihazlar Active Directory ile senkronize edilir
Cihaz geri yazma düzgün çalışıyor. AD için yazılmış geri alınacak aygıt nesneler için 3 saate kadar sürebilir unutmayın. Aygıtlarınızı eşitlenen düzgün bir şekilde doğrulamak için eşitleme tamamlandıktan sonra aşağıdakileri yapın:

1. Active Directory Yönetim Merkezi'ni başlatın.
2. İçinde yapılandırılmış etki alanı içinde RegisteredDevices, genişletin [Kısım 2](#part-2-prepare-active-directory).  

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

## <a name="additional-information"></a>Ek Bilgi
* [Koşullu erişim ile risk yönetme](../active-directory-conditional-access-azure-portal.md)
* [Azure Active Directory cihaz kaydı ile şirket içi koşullu erişim ayarlama](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
