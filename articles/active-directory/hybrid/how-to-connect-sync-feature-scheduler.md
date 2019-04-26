---
title: 'Azure AD Connect eşitleme: Zamanlayıcı | Microsoft Docs'
description: Bu konuda, Azure AD Connect eşitleme yerleşik Zamanlayıcı özelliği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d5f4dec48d81b032de293bb6c68ad62ac48d475
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347932"
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect eşitleme: Scheduler
Bu konuda Azure AD Connect Eşitleme'deki yerleşik Zamanlayıcı (diğer adıyla) açıklanır Eşitleme altyapısı).

Bu özellik 1.1.105.0 derleme ile (Şubat 2016'da yayımlanan) kullanıma sunulmuştur.

## <a name="overview"></a>Genel Bakış
Azure AD Connect eşitleme bir Zamanlayıcı'yı kullanarak şirket içi dizininizde değişikliğinizin eşitleyin. İki Zamanlayıcı işlemleri, bir parola eşitleme için ve başka bir nesne/öznitelik eşitleme ve bakım görevleri vardır. Bu konu, ikincisi kapsar.

Önceki sürümlerde, nesneleri ve öznitelikleri için Zamanlayıcı eşitleme altyapısında dış. Eşitleme işlemi tetiklemek için Windows Görev Zamanlayıcısı'nı veya ayrı bir Windows hizmeti kullanılır. Zamanlayıcı eşitlenecek 1.1 sürümleri yerleşik olan altyapısını ve bazı özelleştirmeye izin. Yeni varsayılan eşitleme sıklığı 30 dakikadır.

Zamanlayıcı için iki görevi sorumludur:

* **Eşitleme döngüsünü**. İçeri aktarma, eşitleme ve değişiklikleri dışarı aktarma işlemi.
* **Bakım görevleri**. Anahtar ve parola sıfırlama ve cihaz kaydı hizmeti (DRS) için sertifikaları yenileyin. İşlem günlüğünde eski girişleri temizlemek.

Zamanlayıcı her zaman çalışıyor, ancak yalnızca bir ya da hiçbiri bu görevleri çalıştırmak için yapılandırılabilir. Örneğin, kendi eşitleme döngüsü işlemi gerekiyorsa, bu Görev Zamanlayıcısı'nı devre dışı ancak bakım görevini çalıştırmaya devam.

## <a name="scheduler-configuration"></a>Zamanlayıcı'yı yapılandırma
Geçerli yapılandırma ayarlarınızı görmek için PowerShell ve çalışma gidin `Get-ADSyncScheduler`. Size bu resimdeki gibi gösterir:

![GetSyncScheduler](./media/how-to-connect-sync-feature-scheduler/getsynccyclesettings2016.png)

Görürseniz **sync komutunun veya cmdlet'i kullanılamıyor** Bu cmdlet'i çalıştırdığınızda, daha sonra PowerShell modülü yüklenmedi. Azure AD Connect ile birlikte bir etki alanı denetleyicisi veya varsayılan ayarları daha yüksek PowerShell kısıtlama düzeylerine sahip bir sunucuda çalıştırırsanız, bu sorun oluşabilir. Bu hatayı görürseniz, ardından çalıştırın `Import-Module ADSync` cmdlet kullanılabilir hale getirmek için.

* **AllowedSyncCycleInterval**. Azure AD tarafından izin verilen eşitleme döngüleri arasındaki en kısa zaman aralığı. Bu ayar daha sık eşitleyemez ve hala destekleniyor.
* **CurrentlyEffectiveSyncCycleInterval**. Zamanlamada şu anda etkin. CustomizedSyncInterval aynı değere sahip (varsa ayarlayın) AllowedSyncInterval sık değilse. Bir derleme 1.1.281 önce kullandığınız CustomizedSyncCycleInterval değiştirirseniz, bu değişiklik sonra sonraki eşitleme döngüsünde etkili olur. Derlemeden 1.1.281 değişiklik hemen geçerli olur.
* **CustomizedSyncCycleInterval**. Varsayılandan başka herhangi bir sıklıkta 30 dakika çalıştırmak için scheduler istiyorsanız bu ayarı yapılandırın. Yukarıdaki resimde, Zamanlayıcı saatte çalışacak şekilde ayarlandı. Bu ayar, bir değere AllowedSyncInterval küçük'e ayarlarsanız, ardından ikinci durum kullanılır.
* **NextSyncCyclePolicyType**. Delta veya ilk. Yalnızca işlem delta değişiklikler sonraki çalıştırması gerektiğini tanımlar veya tam bir sonraki çalışma yapması gerektiğini içeri aktarın ve eşitleyin. İkincisi de yeni veya değiştirilmiş kurallarla suretiyle.
* **NextSyncCycleStartTimeInUTC**. Sonraki Zamanlayıcı sonraki eşitleme döngüsü başlatır.
* **PurgeRunHistoryInterval**. İşlem günlükleri saklanır zaman. Bu günlükler, Eşitleme Hizmeti Yöneticisi'nde incelenebilir. 7 gün boyunca bu günlüklerini tutmak için varsayılandır.
* **SyncCycleEnabled**. Zamanlayıcı içeri aktarma, eşitleme ve dışarı aktarma işlemleri kendi işleminin bir parçası çalışıp çalışmadığını gösterir.
* **MaintenanceEnabled**. Bakım işlemi etkin olup olmadığını gösterir. Sertifikaları/anahtarlar güncelleştirilir ve operations günlüğünü temizler.
* **StagingModeEnabled**. Gösterir, [hazırlama modunda](how-to-connect-sync-staging-server.md) etkinleştirilir. Bu ayar etkinse, dışarı aktarmaları çalışmasını engeller ancak yine de içeri aktarma ve eşitleme çalıştırın.
* **SchedulerSuspended**. Connect tarafından yükseltme sırasında çalışmasını Zamanlayıcı bloğuna geçici olarak ayarlayın.

Bu ayarlarla bazılarını değiştirebilirsiniz `Set-ADSyncScheduler`. Aşağıdaki parametreleri değiştirilebilir:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

Azure AD Connect, önceki yapılarında **isStagingModeEnabled** Set-ADSyncScheduler ortaya çıkmıştır. Bu **desteklenmeyen** bu özelliği ayarlamak için. Özellik **SchedulerSuspended** yalnızca Connect tarafından değiştirilmemelidir. Bu **desteklenmeyen** bu PowerShell ile doğrudan ayarlamak için.

Zamanlayıcı yapılandırması, Azure AD'de depolanır. Hazırlık sunucusu varsa, birincil sunucuda herhangi bir değişiklik hazırlık sunucusu (dışında IsStagingModeEnabled) de etkiler.

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Sözdizimi: `Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - gün - ss saat, dd - dakika, ss - saniye

Örnek: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
3 saatte çalıştırılacak Zamanlayıcı değiştirir.

Örnek: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Her gün çalışacak şekilde Zamanlayıcı değişiklik.

### <a name="disable-the-scheduler"></a>Zamanlayıcı'yı devre dışı bırak  
Yapılandırma değişiklikleri yapmanız gerekirse, scheduler'ı devre dışı bırakmak istediğiniz. Örneğin, size [süzme](how-to-connect-sync-configure-filtering.md) veya [eşitleme kuralları için değişiklik](how-to-connect-sync-change-the-configuration.md).

Zamanlayıcı'yı devre dışı bırakmak için Çalıştır `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Zamanlayıcı'yı devre dışı bırak](./media/how-to-connect-sync-feature-scheduler/schedulerdisable.png)

Değişikliklerinizi yaptıktan sonra Zamanlayıcı'yla yeniden etkinleştirmek unutmadığınızdan `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-the-scheduler"></a>Zamanlayıcı'yı başlatma
Zamanlayıcı, 30 dakikada bir Çalıştır varsayılan olarak açıktır. Bazı durumlarda, bir eşitleme döngüsü zamanlanmış döngüleri arasında çalıştırmak isteyebilirsiniz veya farklı bir tür çalıştırmanız gerekir.

**Delta eşitleme döngüsü**  
Delta eşitleme döngüsü aşağıdaki adımları içerir:

* Delta içeri aktarma işlemi sırasında tüm bağlayıcılar
* Tüm bağlayıcılar delta eşitleme
* Tüm bağlayıcılarda dışarı aktarma

Neden bir döngü el ile çalıştırmak için ihtiyacınız olan hemen eşitlenmesi gereken bir Acil değişiklik sahip olabilir. El ile bir döngüsü çalıştırmak Powershell'den sonra çalıştırmanız gerekiyorsa `Start-ADSyncSyncCycle -PolicyType Delta`.

**Tam eşitleme döngüsü**  
Şu yapılandırma değişiklikleri birini yaptıysanız, bir tam eşitleme döngüsü (diğer adıyla) çalıştırmak ihtiyacınız İlk):

* Daha fazla nesne veya öznitelik, bir kaynak dizinden içeri aktarılacak eklendi
* Eşitleme kurallarında yapılan değişiklikler
* Değiştirilen [filtreleme](how-to-connect-sync-configure-filtering.md) nesneleri farklı sayıda dahil edilmesi gereken şekilde

Bu değişikliklerden birini yaptıysanız, eşitleme altyapısı bağlayıcı alanları yeniden birleştirmek için fırsatına sahip için bir tam eşitleme döngüsü çalıştırmak gerekir. Tam eşitleme döngüsü aşağıdaki adımları içerir:

* Tüm bağlayıcılar üzerinde tam içeri aktarma
* Tüm bağlayıcılar üzerinde tam eşitleme
* Tüm bağlayıcılarda dışarı aktarma

Bir tam eşitleme döngüsü başlatmak için Çalıştır `Start-ADSyncSyncCycle -PolicyType Initial` PowerShell isteminden. Bu komut, bir tam eşitleme döngüsü başlatır.

## <a name="stop-the-scheduler"></a>Zamanlayıcı'yı Durdur
Zamanlayıcı bir eşitleme döngüsü şu anda çalışıyorsa durdurun gerekebilir. Örneğin, Yükleme Sihirbazı'nı başlatın ve bu hatayı alırsınız:

![SyncCycleRunningError](./media/how-to-connect-sync-feature-scheduler/synccyclerunningerror.png)

Bir eşitleme döngüsü çalışırken yapılandırma değişiklik yapamazsınız. Zamanlayıcı, işlem tamamlandığında, ancak değişikliklerinizi hemen haline da durdurabilirsiniz kadar bekleyebilir. Geçerli döngüsünü zararlı değildir ve bekleyen değişiklikleri olan sonraki çalıştırma işlenir.

1. PowerShell cmdlet'i ile kendi geçerli döngüyü durdurmak için Zamanlayıcı bildirerek başlayın `Stop-ADSyncSyncCycle`.
2. Ardından Zamanlayıcısı durduruluyor 1.1.281 önce bir yapı kullanıyorsanız, geçerli görevi geçerli bağlayıcısından durdurmaz. Durdurmak için bağlayıcıyı zorlamak için aşağıdaki eylemleri gerçekleştirin: ![StopAConnector](./media/how-to-connect-sync-feature-scheduler/stopaconnector.png)
   * Başlangıç **eşitleme hizmeti** Başlat menüsünden. Git **Bağlayıcılar**, durumu ile birlikte Bağlayıcısı vurgulayın **çalıştıran**seçip **Durdur** eylemlerine.

Zamanlayıcı, hala etkin olduğu ve bir sonraki fırsatta yeniden başlatır.

## <a name="custom-scheduler"></a>Özel bir zamanlayıcı
Bu bölümde belgelenen cmdlet'leri yalnızca derleme içinde kullanılabilir [1.1.130.0](reference-connect-version-history.md#111300) ve daha sonra.

Yerleşik Zamanlayıcı gereksinimlerinize uygun değil ise, PowerShell kullanarak bağlayıcılar zamanlayabilirsiniz.

### <a name="invoke-adsyncrunprofile"></a>Çağırma ADSyncRunProfile
Bu şekilde bir bağlayıcı için bir profil başlayabilirsiniz:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Kullanılacak adlarını [bağlayıcı adlarıyla](how-to-connect-sync-service-manager-ui-connectors.md) ve [çalıştırma profili adları](how-to-connect-sync-service-manager-ui-connectors.md#configure-run-profiles) bulunabilir [Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi](how-to-connect-sync-service-manager-ui.md).

![Çalıştırma profili çağırma](./media/how-to-connect-sync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Cmdlet'tir zaman uyumlu, bağlayıcı işlemi, başarıyla veya hatayla tamamlanana kadar başka bir deyişle, Denetim dönmez.

Bağlayıcılarınızı zamanladığınızda bunları şu sırayla zamanlamak için önerilir:

1. (Tam/değişiklik) Active Directory gibi şirket içi dizinlerden içeri aktarma
2. (Tam/değişiklik) Azure AD'den içeri aktarma
3. (Tam/değişiklik) Active Directory gibi şirket içi dizinlerden eşitleme
4. (Tam/değişiklik) Azure ad eşitleme
5. Azure AD'ye dışarı aktarma
6. Active Directory gibi şirket içi dizinlere dışarı aktarma

Yerleşik Zamanlayıcı bağlayıcıları nasıl çalışır bu sırasıdır.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Meşgul veya boşta olup olmadığını görmek için eşitleme altyapısı da izleyebilirsiniz. Eşitleme altyapısı boşta ve bir bağlayıcı çalışmıyor Bu cmdlet, boş bir sonuç döndürür. Bir bağlayıcı çalışıyorsa, bu bağlayıcı adını döndürür.

```
Get-ADSyncConnectorRunStatus
```

![Bağlayıcı durumu çalıştırın](./media/how-to-connect-sync-feature-scheduler/getconnectorrunstatus.png)  
Yukarıdaki resimde, ilk eşitleme altyapısı boşta olduğu bir durumdan çizgidir. Azure AD Bağlayıcısı çalışırken ikinci satırdan.

## <a name="scheduler-and-installation-wizard"></a>Scheduler ve Yükleme Sihirbazı
Ardından yükleme sihirbazını başlatırsanız, Zamanlayıcı geçici olarak askıya alınır. Bu davranış, yapılandırma değişiklikleri yapmanızı ve eşitleme altyapısının etkin olarak çalışıyorsa bu ayarları uygulanamaz varsayılır olmasıdır. Bu nedenle, Yükleme Sihirbazı, eşitleme altyapısının eşitleme eylemleri gerçekleştirmeyi durdurur beri açık bırakmayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
