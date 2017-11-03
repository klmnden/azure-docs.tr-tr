---
title: "Azure AD Connect eşitleme: Zamanlayıcı | Microsoft Docs"
description: "Bu konuda, Azure AD Connect eşitleme yerleşik Zamanlayıcı özelliğinde açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 63f69756b3933fecdec75cc677e1098447e5b94e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect eşitleme: Zamanlayıcı
Bu konu, Azure AD Connect eşitleme yerleşik zamanlayıcıda (paketini açıklar Eşitleme altyapısı).

Bu özellik, (Şubat 2016 yayımlandı) yapı ile 1.1.105.0 sunulmuştur.

## <a name="overview"></a>Genel Bakış
Azure AD Connect eşitleme bir Zamanlayıcı'yı kullanarak şirket içi dizininizde değişikliğinizin eşitleyin. İki Zamanlayıcı işlemleri, parola eşitleme için bir ve nesne/özniteliği eşitleme ve bakım görevleri için başka bir vardır. Bu konu, ikincisi kapsar.

Önceki sürümlerde, nesneleri ve öznitelikleri için Zamanlayıcı'yı eşitleme altyapısında dış. Eşitleme işlemi tetiklemek için Windows Görev Zamanlayıcısı'nı veya ayrı bir Windows hizmet kullanılır. Zamanlayıcı eşitleme 1.1 sürümleri yerleşik olan altyapısı ve bazı özelleştirme izin vermeyin. Yeni varsayılan eşitleme sıklığı 30 dakikadır.

Zamanlayıcı iki görevlerden sorumlu şöyledir:

* **Eşitleme döngüsü**. İçeri aktarma, eşitleme ve değişiklikleri dışarı aktarma işlemi.
* **Bakım görevleri**. Anahtarları ve parola sıfırlama ve cihaz kaydı hizmeti (DRS) için sertifikaları yenileyin. İşlem günlüğünde eski girişleri temizlemek.

Zamanlayıcı her zaman çalışıyor, ancak yalnızca bir veya hiçbiri bu görevleri çalıştırmak için yapılandırılabilir. Örneğin, kendi eşitleme döngüsü işlemi değiştirilmesi gerekiyorsa, bu Görev Zamanlayıcısı'ndaki devre dışı ancak hala bakım görevini çalıştırın.

## <a name="scheduler-configuration"></a>Zamanlayıcı'yı yapılandırma
Geçerli yapılandırma ayarlarınızı görmek için PowerShell ve Çalıştır gidin `Get-ADSyncScheduler`. Size bu resimdeki şöyle gösterir:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Görürseniz **eşitleme komutu veya cmdlet kullanılamıyor** bu cmdlet'ini çalıştırdığınızda, ardından PowerShell modülünün yüklenmedi. Azure AD Connect ile birlikte bir etki alanı denetleyicisinde veya varsayılan ayarlar daha yüksek PowerShell kısıtlama düzeylerine sahip bir sunucuda çalıştırırsanız bu sorun oluşabilir. Bu hatayı görürseniz, ardından çalıştırın `Import-Module ADSync` cmdlet kullanılabilir yapmak için.

* **AllowedSyncCycleInterval**. Azure AD tarafından izin verilen eşitleme döngüleri arasında kısa zaman aralığı. Bu ayar daha sık eşitleyemez ve hala desteklenmiyor.
* **CurrentlyEffectiveSyncCycleInterval**. Zamanlamada şu anda etkin. CustomizedSyncInterval aynı değere sahiptir (varsa ayarlayın) AllowedSyncInterval daha sık değilse. 1.1.281 önce bir derleme kullanma CustomizedSyncCycleInterval değiştirirseniz, bu değişiklik bir sonraki eşitleme döngüsünden sonra etkinleşir. Derleme 1.1.281 değişikliği hemen etkinleşir.
* **CustomizedSyncCycleInterval**. Varsayılandan başka bir sıklıkla 30 dakika çalıştırmak için Zamanlayıcı istiyorsanız bu ayarı yapılandırın. Yukarıdaki resimde, Zamanlayıcı saatte çalışacak şekilde ayarlandı. Bu ayar, bir değere AllowedSyncInterval değerinden daha düşük'e ayarlarsanız, ardından ikinci kullanılır.
* **NextSyncCyclePolicyType**. Delta veya ilk. Yalnızca işlem delta değişiklikler, sonraki çalıştırma gerektiğini tanımlar veya sonraki çalıştırmada tam bunu yaparsanız içeri aktarma ve eşitleme. İkincisi de yeni veya değiştirilmiş kuralları yeniden işleyin.
* **NextSyncCycleStartTimeInUTC**. Sonraki Zamanlayıcı sonraki eşitleme döngüsü başlatır.
* **PurgeRunHistoryInterval**. İşlem günlükleri saklanması zaman. Bu günlükler Eşitleme Hizmeti Yöneticisi'nde incelenebilir. Bu günlükler 7 gündür tutmak için varsayılandır.
* **SyncCycleEnabled**. Zamanlayıcı alma, eşitleme ve dışarı aktarma işlemlerini kendi işleminin bir parçası çalışıp çalışmadığını gösterir.
* **MaintenanceEnabled**. Bakım işleminin etkin olup olmadığını gösterir. Sertifikaları/anahtarları güncelleştirir ve işlem günlük temizler.
* **StagingModeEnabled**. Gösterir, [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode) etkinleştirilir. Bu ayar etkinleştirilirse, çalışan dışarı bastırır ancak hala içeri aktarma ve eşitleme çalıştırın.
* **SchedulerSuspended**. Connect tarafından yükseltme sırasında geçici olarak bloğuna çalışmasını Zamanlayıcısı'nı ayarlayın.

Bu ayarlarla bazılarını değiştirebilirsiniz `Set-ADSyncScheduler`. Aşağıdaki parametreleri değiştirilebilir:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

Azure AD Connect, önceki derlemelerde **isStagingModeEnabled** Set-ADSyncScheduler ortaya çıkmıştır. Bu **desteklenmeyen** bu özelliği ayarlamak için. Özellik **SchedulerSuspended** yalnızca Connect tarafından değiştirilmemelidir. Bu **desteklenmeyen** bu PowerShell ile doğrudan ayarlamak için.

Zamanlayıcı yapılandırması Azure AD içinde depolanır. Hazırlama sunucunuz varsa, birincil sunucu üzerindeki herhangi bir değişiklik hazırlama sunucunun (dışında IsStagingModeEnabled) de etkiler.

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Sözdizimi:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - gün ss - saat, dd dakika, ss - saniye

Örnek:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
3 saatte bir çalıştırılacak Zamanlayıcı değiştirir.

Örnek:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Her gün çalışacak şekilde Zamanlayıcı değişiklik.

### <a name="disable-the-scheduler"></a>Zamanlayıcı'yı devre dışı bırak  
Yapılandırma değişiklikleri yapmanız gerekirse, Zamanlayıcı'yı devre dışı bırakmak istediğiniz. Örneğin, ne zaman, [filtrelemeyi yapılandırma](active-directory-aadconnectsync-configure-filtering.md) veya [eşitleme kuralları için değişiklik](active-directory-aadconnectsync-change-the-configuration.md).

Zamanlayıcı'yı devre dışı bırakmak için Çalıştır `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Zamanlayıcı'yı devre dışı bırak](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

Yaptığınız değişiklikler yaptığınızda, Zamanlayıcı'yla yeniden etkinleştirmek unutmadığınızdan `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-the-scheduler"></a>Zamanlayıcı'yı başlatma
Zamanlayıcı 30 dakikada çalıştırın varsayılan olarak açıktır. Bazı durumlarda, zamanlanmış döngüleri arasında bir eşitleme döngüsü çalıştırmak isteyebilirsiniz veya farklı bir tür çalıştırmanız gerekir.

**Delta eşitleme döngüsü**  
Bir delta eşitleme döngüsü aşağıdaki adımları içerir:

* Tüm bağlayıcılar üzerinde delta içeri aktarma
* Tüm bağlayıcılar delta eşitleme
* Tüm bağlayıcıların dışarı aktarma

El ile bir döngüsü çalıştırmak gereken neden olan hemen eşitlenmesi gereken bir Acil değişiklik sahip olabilir. El ile bir döngü çalıştırmak Powershell'den sonra çalıştırmanız gerekiyorsa `Start-ADSyncSyncCycle -PolicyType Delta`.

**Tam eşitleme döngüsü**  
Aşağıdaki yapılandırma değişikliklerini birini yaptıysanız, bir tam eşitleme döngüsü (paketini çalıştırmanız gerekir İlk):

* Daha fazla nesne veya bir kaynak dizinden içeri aktarılacak öznitelik eklendi
* Eşitleme kuralları için yapılan değişiklikler
* Değiştirilen [filtreleme](active-directory-aadconnectsync-configure-filtering.md) nesneleri farklı sayıda dahil edilecek şekilde

Ardından bu değişikliklerden birini yaptıysanız, eşitleme altyapısı bağlayıcı alanları yeniden birleştirmek için Fırsat nedenle bir tam eşitleme döngüsü çalıştırmanız gerekir. Bir tam eşitleme döngüsü aşağıdaki adımları içerir:

* Tüm bağlayıcılar üzerinde tam içeri aktarma
* Tüm bağlayıcılar üzerinde tam eşitleme
* Tüm bağlayıcıların dışarı aktarma

Tam eşitleme döngüsü başlatmak için Çalıştır `Start-ADSyncSyncCycle -PolicyType Initial` PowerShell isteminde. Bu komut, bir tam eşitleme döngüsü başlatır.

## <a name="stop-the-scheduler"></a>Zamanlayıcı'yı Durdur
Zamanlayıcı bir eşitleme döngüsü şu anda çalışıyorsa durdurun gerekebilir. Örneğin, Yükleme Sihirbazı'nı başlatın ve bu hatayı alırsınız:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Bir eşitleme döngüsü çalıştırıldığında, yapılandırma değişiklikleri yapamazsınız. Zamanlayıcı işlemi tamamlandığında, ancak hemen değişikliklerinizi olabilmeniz de durdurabilirsiniz kadar bekleyin. Geçerli döngüsü durdurma zararlı değil ve bekleyen değişiklikleri sonraki çalıştırma ile işlenir.

1. Başlatma Zamanlayıcısı'nı kendi geçerli döngüyü PowerShell cmdlet'iyle durdurmak için eklenene `Stop-ADSyncSyncCycle`.
2. Yapı 1.1.281 önce kullanırsanız, Zamanlayıcısı durduruluyor geçerli görevini geçerli bağlayıcısından durdurmaz. Durdurmak için bağlayıcı zorlamak için aşağıdaki eylemleri gerçekleştirin: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
   * Başlat **eşitleme hizmeti** Başlat menüsünden. Git **Bağlayıcılar**, durumu bağlayıcısıyla vurgulayın **çalıştıran**seçip **durdurmak** eylemlerine.

Zamanlayıcı hala etkin ve bir sonraki fırsatta yeniden başlatır.

## <a name="custom-scheduler"></a>Özel Zamanlayıcı
Bu bölümde belgelenen cmdlet'leri yalnızca yapı içinde kullanılabilir [1.1.130.0](active-directory-aadconnect-version-history.md#111300) ve daha sonra.

Ardından yerleşik Zamanlayıcı gereksinimlerinizi karşılamadığı, PowerShell kullanarak bağlayıcılar zamanlayabilirsiniz.

### <a name="invoke-adsyncrunprofile"></a>Çağırma ADSyncRunProfile
Bu şekilde bir bağlayıcı için bir profil başlatabilirsiniz:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Kullanılmak üzere adları [bağlayıcı adlarıyla](active-directory-aadconnectsync-service-manager-ui-connectors.md) ve [çalıştırma profili adları](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) bulunabilir [Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi](active-directory-aadconnectsync-service-manager-ui.md).

![Çalıştırma profili çağırma](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Cmdlet zaman uyumlu, bağlayıcı başarıyla veya bir hata ile işlemi tamamlanana kadar başka bir deyişle, bu denetim döndürmüyor.

Bağlayıcılar zamanladığınızda, bunları şu sırayla zamanlamak için önerilir:

1. (Tam/Delta) Active Directory gibi şirket içi dizinlere alın
2. (Tam/Delta) Azure AD'den alma
3. (Tam/Delta) Active Directory gibi şirket içi dizin eşitlemesi
4. (Tam/Delta) Azure AD'den eşitleme
5. Azure AD dışarı aktarma
6. Active Directory gibi şirket içi dizinlere dışarı aktarma

Bu sırada, yerleşik Zamanlayıcı bağlayıcıları nasıl çalıştığı yerdir.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Eşitleme altyapısı meşgul ya da boş olup olmadığını görmek için de izleyebilirsiniz. Eşitleme altyapısı boşta ise ve bir bağlayıcı çalışmıyor Bu cmdlet boş bir sonuç döndürür. Bir bağlayıcı çalışıyorsa, bağlayıcı adını döndürür.

```
Get-ADSyncConnectorRunStatus
```

![Bağlayıcı durumu çalıştırın](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Yukarıdaki resimde, ilk eşitleme altyapısı boşta olduğu bir durumdan satırdır. Azure AD Bağlayıcısı ne zaman çalışmasını ikinci satır.

## <a name="scheduler-and-installation-wizard"></a>Zamanlayıcı ve Yükleme Sihirbazı
Yükleme Sihirbazı'nı başlatın, ardından Zamanlayıcı geçici olarak askıya alındı. Yapılandırma değişiklikleri yapın ve eşitleme altyapısı etkin olarak çalışıyorsa, bu ayarları uygulanamaz varsayıldığından bu davranıştır. Bu nedenle, Yükleme Sihirbazı'nı, tüm eşitleme Eylemler gerçekleştirmeyi eşitleme altyapısı durdurur beri açık bırakmayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
