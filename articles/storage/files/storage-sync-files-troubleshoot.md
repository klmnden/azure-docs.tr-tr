---
title: Azure dosya eşitleme sorunlarını giderme | Microsoft Docs
description: Azure dosya eşitleme ile ilgili yaygın sorunları giderin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 53297a16889190383e0455c484339adc049c1cb1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700370"
---
# <a name="troubleshoot-azure-file-sync"></a>Azure Dosya Eşitleme ile ilgili sorunları giderme
Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

Bu makalede, sorun giderme ve Azure dosya eşitleme dağıtımınıza karşılaşabileceğiniz sorunları çözmenize yardımcı olmak için tasarlanmıştır. Biz de sorunun daha kapsamlı bir araştırma gerekiyorsa sistemden önemli günlükleri toplamak nasıl açıklar. Sorunuzun yanıtını görmüyorsanız, aşağıdaki kanalları (sırayla yükselen) üzerinden bize başvurabilirsiniz:

1. [Azure depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
2. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
3. Microsoft Desteği. Azure portalında yeni bir destek isteği oluşturmak için **yardımcı** sekmesinde **Yardım + Destek** düğmesini ve ardından **yeni destek isteği**.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="im-having-an-issue-with-azure-file-sync-on-my-server-sync-cloud-tiering-etc-should-i-remove-and-recreate-my-server-endpoint"></a>My server (eşitleme, bulut katmanlama, vb..) Azure dosya eşitleme ile ilgili bir sorun yaşıyorum. Kaldırın ve paylaşabilirim my server uç noktası yeniden?
[!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## <a name="agent-installation-and-server-registration"></a>Aracı yükleme ve sunucu kaydı
<a id="agent-installation-failures"></a>**Aracı yükleme hatalarını giderme**  
Yükseltilmiş bir komut isteminde, Azure dosya eşitleme Aracısı yüklemesi başarısız olursa, aracı yükleme işlemi sırasında günlük kaydı etkinleştirmek için aşağıdaki komutu çalıştırın:

```
StorageSyncAgent.msi /l*v AFSInstaller.log
```

Yükleme hatanın nedenini belirlemek için installer.log gözden geçirin.

<a id="agent-installation-on-DC"></a>**Active Directory etki alanı denetleyicisi üzerinde aracı yüklemesi başarısız olur**  
Eşitleme Aracısı PDC rol sahibi bir Windows Server 2008 R2 veya işletim sistemi sürümden düşük olduğu bir Active Directory etki alanı denetleyicisi üzerinde yüklemeye çalışırsanız, burada eşitleme aracısı yüklemek için başarısız olur bir sorunla karşılaşabilirsiniz.

Gidermek için Windows Server 2012 R2 çalıştıran başka bir etki alanı denetleyicisine PDC rolü aktarmak ya da daha yeni eşitleme kurun.

<a id="server-registration-missing"></a>**Azure portalında kayıtlı sunucuları altında sunucu listede**  
Bir sunucu altında listede yoksa **kayıtlı sunucuları** depolama eşitleme hizmeti için:
1. Kaydetmek istediğiniz sunucuya oturum açın.
2. Dosya Gezgini'ni açın ve ardından (varsayılan konum C:\Program Files\Azure\StorageSyncAgent olan) depolama eşitleme aracısını yükleme dizinine gidin. 
3. ServerRegistration.exe çalıştırın ve depolama eşitleme hizmeti ile sunucuyu kaydetmek için sihirbazı tamamlayın.

<a id="server-already-registered"></a>**Sunucu kaydı sırasında Azure dosya eşitleme Aracısı yükleme şu iletiyi görüntüler: "Bu sunucu zaten kayıtlı"** 

![Bir sunucu kaydı iletişim kutusunun ekran görüntüsü "sunucu zaten kayıtlı" hata iletisi](media/storage-sync-files-troubleshoot/server-registration-1.png)

Sunucunun bir depolama eşitleme hizmeti ile daha önceden kaydedilen bu ileti görüntülenir. Geçerli depolama eşitleme hizmeti sunucudan kaydını silin ve ardından yeni bir depolama eşitleme hizmeti ile kaydetme hakkında bilgi için açıklanan adımları tamamlamak [Azure dosya eşitleme ile bir sunucu kaydını](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Sunucu altında listede yoksa **kayıtlı sunucuları** depolama eşitleme hizmetinde kaydını kaldırmak istediğiniz sunucuda aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Sunucu bir kümenin parçasıysa, isteğe bağlı kullanabilirsiniz *sıfırlama StorageSyncServer - CleanClusterRegistration* ayrıca küme kaydını kaldırmak için parametre.

<a id="web-site-not-trusted"></a>**Ben bir sunucuyu kaydetmek, çok sayıda "web sitesi güvenilmeyen" yanıt konusuna bakın. Neden?**  
Bu sorun oluşur, **Gelişmiş Internet Explorer güvenlik** ilke, sunucu kaydı sırasında etkinleştirilir. Doğru bir şekilde devre dışı bırakma hakkında daha fazla bilgi için **Gelişmiş Internet Explorer güvenlik** İlkesi bkz [Azure dosya eşitleme ile kullanmak için Windows Server hazırlama](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) ve [Azure dosya dağıtma Eşitleme](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Eşitleme Grup Yönetimi
<a id="cloud-endpoint-using-share"></a>**Bulut uç noktası oluşturma, şu hatayla başarısız olur: "Belirtilen Azure dosya paylaşımı farklı bir bulut uç noktası tarafından kullanılıyor"**  
Azure dosya paylaşımının zaten başka bir bulut uç noktası tarafından kullanılmakta olması durumunda bu sorun oluşur. 

Bu iletiyi görürseniz ve Azure dosya paylaşımı şu anda bir bulut uç noktası tarafından kullanılmadığından, Azure dosya paylaşımında Azure dosya eşitleme meta verileri temizlemek için aşağıdaki adımları tamamlayın:

> [!Warning]  
> Meta veriler üzerinde bir bulut uç noktası tarafından kullanılmakta olan bir Azure dosya paylaşımı siliniyor, Azure dosya eşitleme işlemlerinin başarısız olmasına neden olur. 

1. Azure portalında, Azure dosya paylaşımına gidin.  
2. Azure dosya paylaşımına sağ tıklayın ve ardından **meta verileri Düzenle**.
3. Sağ **SyncService**ve ardından **Sil**.

<a id="cloud-endpoint-authfailed"></a>**Bulut uç noktası oluşturma, şu hatayla başarısız olur: "AuthorizationFailed"**  
Kullanıcı hesabınıza bir bulut uç noktası oluşturmak için yeterli haklara sahip değilse bu sorun oluşur. 

Bulut uç noktası oluşturmak için kullanıcı hesabınızın aşağıdaki Microsoft Authorization izinleri olmalıdır:  
* Okuma: Rol tanımı al
* Yazma: Özel rol tanımı oluştur veya güncelleştir
* Okuma: Rol ataması al
* Yazma: Rol ataması oluştur

Aşağıdaki yerleşik rolleri gerekli Microsoft Authorization izinlere sahip:  
* Sahip
* Kullanıcı Erişimi Yöneticisi

Kullanıcı hesabı rolü gerekli izinlere sahip olup olmadığını belirlemek için:  
1. Azure portalında **kaynak grupları**.
2. Depolama hesabının bulunduğu kaynak grubunu seçin ve ardından **erişim denetimi (IAM)**.
3. Seçin **rol atamaları** sekmesi.
4. Seçin **rol** (örneğin, sahibi veya katkıda bulunan) kullanıcı hesabınız için.
5. İçinde **kaynak sağlayıcısı** listesinden **Microsoft Authorization**. 
    * **Rol ataması** olmalıdır **okuma** ve **yazma** izinleri.
    * **Rol tanımı** olmalıdır **okuma** ve **yazma** izinleri.

<a id="server-endpoint-createjobfailed"></a>**Sunucu uç noktası oluşturma, şu hatayla başarısız olur: "MgmtServerJobFailed" (hata kodu:-2134375898)**  
Sunucu uç noktası yolu sistem birimi ve bulut üzerinde ise bu sorun oluşur katmanlama etkinleştirilir. Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.

<a id="server-endpoint-deletejobexpired"></a>**Sunucu uç noktasını silme işlemi, şu hatayla başarısız olur: "MgmtServerJobExpired"**                
Sunucu çevrimdışı veya ağ bağlantısı yok, bu sorun oluşur. Sunucu artık kullanılabilir değilse, sunucu uç noktalarını siler portalında sunucunun kaydını silin. Sunucu uç noktalarını silmek için açıklanan adımları [Azure dosya eşitleme ile bir sunucu kaydını](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

<a id="server-endpoint-provisioningfailed"></a>**Sunucu uç noktası özellikler sayfasını açın veya Bulut katmanlama İlkesi güncelleştirilemedi**  
Sunucu uç noktasında bir yönetim işlemi başarısız olursa, bu sorun oluşabilir. Azure portalında sunucu uç noktası özellikleri sayfası açık değilse, sunucudan PowerShell komutlarıyla sunucu uç noktası güncelleniyor bu sorunu çözebilir. 

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzStorageSyncServerEndpoint `
    -SubscriptionId mysubguid `
    -ResourceGroupName myrgname `
    -StorageSyncServiceName storagesvcname `
    -SyncGroupName mysyncgroup

# Update the free space percent policy for the server endpoint
Set-AzStorageSyncServerEndpoint `
    -Id serverendpointid `
    -CloudTiering true `
    -VolumeFreeSpacePercent 60
```
<a id="server-endpoint-noactivity"></a>**Sunucu uç noktası "No etkinliği" veya "Bekliyor" durumu durumuna sahip ve "çevrimdışı görünüyor" kayıtlı sunucuları dikey penceresinde sunucu durumu**  

Depolama eşitleme İzleyicisi işlemi çalışmıyor veya sunucunun bir proxy veya Güvenlik Duvarı nedeniyle Azure dosya eşitleme hizmeti ile iletişim kuramıyor Bu sorun oluşabilir.

Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:

1. Sunucuda Görev Yöneticisi'ni açın ve Depolama Eşitleme İzleyicisi (AzureStorageSyncMonitor.exe) işleminin çalıştığından emin olun. İşlem çalışmıyorsa önce sunucuyu yeniden başlatmayı deneyin. En son Azure dosya eşitleme için sunucunun yeniden başlatılması sorunu çözmezse, yükseltme [aracı sürümü](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes).
2. Güvenlik Duvarı ve Proxy ayarlarının doğru yapılandırıldığından doğrulayın:
    - Sunucu bir güvenlik duvarının arkasındaysa 443 numaralı bağlantı noktası üzerinden giden bağlantılara izin verildiğinden emin olun. Güvenlik Duvarı trafiği belirli etki alanlarına erişimi kısıtlıyorsa, Güvenlik Duvarı'nda listelenen etki alanları onaylayın [belgeleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#firewall) erişilebilir.
    - Sunucu bir proxy'nin arkasındaysa, Proxy adımları izleyerek makineye veya uygulamaya özel proxy ayarlarını yapılandırın [belgeleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#proxy).

<a id="endpoint-noactivity-sync"></a>**Sunucu uç noktası "No etkinlik" sistem durumunu ve kayıtlı sunucuları dikey penceresinde sunucu durumu "Çevrimiçi"**  

Sunucu uç noktasını eşitleme etkinliği son iki saat içinde açtı değil "No etkinlik" sunucu uç noktası sistem durumunu gösterir.

Sunucu uç noktası eşitleme etkinliği aşağıdaki nedenlerden dolayı kaydedebilir değil:

- Aracı sürümü 4.3.0.0 veya eski yüklü olduğundan ve sunucunun bir etkin bir VSS eşitleme oturumu (SnapshotSync) vardır. Sunucu uç noktası için bir VSS eşitleme oturumu etkinken, diğer sunucu uç noktaları aynı birimde VSS eşitleme oturumu tamamlanana kadar bir Başlangıç eşitleme oturumu başlatılamıyor. Bu sorunu çözmek için aracı sürümü 5.0.2.0 yükleyin ya da daha yeni etkin olan bir birimde bir VSS oturumu eşitlenirken eşitleme birden çok sunucu uç noktalarını destekler.

    Sunucudaki geçerli eşitleme etkinliği denetlemek için bkz [nasıl geçerli eşitleme oturumunun ilerleme izlerim?](#how-do-i-monitor-the-progress-of-a-current-sync-session).

- Sunucu, en fazla eşzamanlı bir eşitleme oturum sayısını ulaştı. 
    - Aracı sürümü 4.x ve daha yeni sürümler: Kullanılabilir sistem kaynaklarına göre değişiklik gösterir.
    - Aracı sürümü 3.x: her işlemci veya en fazla 8 active eşitleme oturumu sunucu başına 2 active sync oturumları.

> [!Note]  
> Kayıtlı sunucular dikey penceresinde sunucu durumu "Çevrimdışı olarak görünür" ise, konusunda belgelenen adımları [sunucu uç noktası olan bir sistem durumu "No etkinliği" veya "Bekliyor" ve "çevrimdışı görünüyor" kayıtlı sunucuları dikey penceresinde sunucu durumu ](#server-endpoint-noactivity) bölümü.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Bir dosya my Azure dosya paylaşımı doğrudan portal üzerinden ya da SMB üzerinden oluşturduğum, ne kadar dosya sunucularına eşitleme grubundaki eşitleme zaman alır?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="serverendpoint-pending"></a>**Sunucu uç noktası durumu için birkaç saat bekleme durumunda olduğu**  
Bu sorun, veri içeren bir bulut uç noktası oluşturun ve bir Azure dosya paylaşımı kullanıyorsanız beklenmektedir. Azure dosya paylaşımının değişiklikleri tarar değişiklik numaralandırma işi, Bulut ve sunucu uç noktaları arasında dosyaları eşitleyebilmeniz için önce tamamlamanız gerekir. İşin tamamlanması için geçen süre, ad alanında Azure dosya paylaşımının boyutuna bağlıdır. Sunucu uç noktası durumu değişiklik numaralandırma işi tamamlandıktan sonra güncelleştirmeniz gerekir.

### <a id="broken-sync"></a>Eşitleme durumu nasıl izleyebilirim?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Her bir eşitleme grubu içinde tek bir sunucu uç noktalarına son tamamlanan eşitleme oturum durumunu görmek için detaya gidebilirsiniz. Yeşil sistem sütunu ve dosyaları değil eşitleniyor 0 değerini eşitleme beklendiği gibi çalışıp çalışmadığını gösterir. Aşağıdaki durum bu değilse, yaygın eşitleme hatalarıyla ve değil eşitleniyor dosyalarını işlemek nasıl bir listesi için bkz. 

![Azure portal'ın bir ekran görüntüsü](media/storage-sync-files-troubleshoot/portal-sync-health.png)

# <a name="servertabserver"></a>[Sunucu](#tab/server)
Olay Görüntüleyicisi konumunda bulunabilir sunucunun telemetri günlüklerini Git `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`. Olay 9102 karşılık gelen bir tamamlanmış eşitleme oturumu; Son eşitleme durumu için en son olay kimliği 9102 ile arayın. SyncDirection bir karşıya yükleme ve indirme, bu oturumu olup olmadığını söyler. HResult 0 ise, eşitleme oturumu başarılı oldu. Sıfır olmayan bir HResult, eşitleme sırasında bir hata oluştu anlamına gelir; Aşağıda sık karşılaşılan bir listesi için bkz. PerItemErrorCount 0'dan büyükse, bu, bazı dosyaları veya klasörleri düzgün eşitlenmeyen olduğunu anlamına gelir. HResult 0 ancak 0'dan büyük bir PerItemErrorCount olması mümkündür.

Başarılı bir şekilde karşıya bir örneği aşağıda verilmiştir. Konuyu uzatmamak amacıyla, yalnızca 9102 her koşulda yer alan değerlerden bazıları aşağıda listelenmiştir. 

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: 0, 
SyncFileCount: 2, SyncDirectoryCount: 0,
AppliedFileCount: 2, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0,
TransferredFiles: 2, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Buna karşılık, başarısız olan bir karşıya yükleme şuna benzeyebilir:

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: -2134364065,
SyncFileCount: 0, SyncDirectoryCount: 0, 
AppliedFileCount: 0, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0, 
TransferredFiles: 0, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Bazen Eşitleme oturumları genel başarısız veya sıfır olmayan PerItemErrorCount sahip ancak hala ilerleme, başarılı bir şekilde eşitleniyor bazı dosyalar ile yapın. Bu, oturumun ne kadar başarılı olup size uygulanan * alanları (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount ve AppliedSizeBytes) görülebilir. Başarısız olan ancak artan bir uygulanan * sayısına sahip birden çok eşitleme oturumu satırda görürseniz, bir destek bileti açmadan önce denemek için eşitleme zamanı vermeniz gerekir.

---

### <a name="how-do-i-monitor-the-progress-of-a-current-sync-session"></a>Geçerli bir eşitleme oturumu ilerlemesini nasıl izleyebilirim?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Eşitleme grubunuz içinde sunucu uç noktası için söz konusu gidin ve dosyaları karşıya yüklenen veya indirilen geçerli eşitleme oturumu sayısını görmek için eşitleme etkinliği bölümüne bakın. Bu durum, yaklaşık 5 dakika geciktirilecek ve eşitleme oturumunuz bu süre içinde tamamlanması küçük olursa, bu portalda bildirilmeyebilir unutmayın. 

# <a name="servertabserver"></a>[Sunucu](#tab/server)
Arama telemetri en son 9302 olayında oturum sunucuda (Olay Görüntüleyicisi'nde, uygulamaları ve Hizmetleri Logs\Microsoft\FileSync\Agent\Telemetry gidin). Bu olay, geçerli eşitleme oturumu durumunu gösterir. TotalItemCount gösterir eşitlenmesi için kaç dosyalarıdır AppliedItemCount sayısı kadar eşitlenmiş dosyaları ve PerItemErrorCount eşitleme (Bu işlemel öğrenmek için aşağıya bakın) için başarısız olan dosya sayısı.

```
Replica Sync Progress. 
ServerEndpointName: <CI>sename</CI>, SyncGroupName: <CI>sgname</CI>, ReplicaName: <CI>rname</CI>, 
SyncDirection: Upload, CorrelationId: {AB4BA07D-5B5C-461D-AAE6-4ED724762B65}. 
AppliedItemCount: 172473, TotalItemCount: 624196. AppliedBytes: 51473711577, 
TotalBytes: 293363829906. 
AreTotalCountsFinal: true. 
PerItemErrorCount: 1006.
```
---

### <a name="how-do-i-know-if-my-servers-are-in-sync-with-each-other"></a>Sunucularım birbiriyle eşitlenmiş olup olmadığını nasıl anlarım?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Verilen eşitleme grubundaki her sunucu için emin olun:
- Denenen son eşitlemenin hem karşıya yükleme ve indirme için zaman damgası, son.
- Karşıya yükleme ve indirme için yeşil durumudur.
- Eşitleme etkinliği alan çok az sayıda gösterir ya da eşitlemek için kalan dosya yok.
- Dosyaları değil eşitleniyor hem karşıya yükleme ve indirme 0 alandır.

# <a name="servertabserver"></a>[Sunucu](#tab/server)
Her sunucu için telemetri olay günlüğündeki 9102 olaylar tarafından işaretlenen tamamlanmış eşitleme oturumlara bakın (Olay Görüntüleyicisi'nde Git `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`). 

1. Son karşıya yükleme olduğundan emin olun ve indirme başarıyla tamamlandı oturumları istediğiniz belirli herhangi bir sunucu üzerinde. Bunu yapmak için PerItemErrorCount ve HResult 0 hem karşıya yükleme ve indirme olup olmadığını denetleyin (SyncDirection alan belirli bir oturum bir karşıya yükleme veya indirme oturumu olup olmadığını gösterir). Son tamamlanan eşitleme oturumu görmüyorsanız, büyük olasılıkla bir eşitleme oturumu şu anda, yalnızca eklendiğinde veya büyük miktarda veri değiştirildiğinde, beklenen sürüyor olduğundan olduğuna dikkat edin.
2. Bir sunucu bulutla tamamen güncel olduğundan ve herhangi bir yönde eşitlemeye hiçbir değişiklik olduğunda, boş Eşitleme oturumları görürsünüz. Bu yükleme tarafından gösterilir ve hangi tüm eşitleme * içinde (SyncFileCount, SyncDirCount, SyncTombstoneCount ve SyncSizeBytes) sıfır, yani eşitlemeye hiçbir şey oluştu alanları olayları indirin. Olduğundan her zaman eşitleme için yeni bir şey bu boş eşitleme oturumlar yüksek değişim sıklığı sunucularda gerçekleşmeyebilir unutmayın. Eşitleme etkinliği yok ise, bunlar her 30 dakikada gerçekleşmelidir. 
3. Tüm sunucular, son karşıya yükleme yani bulutla güncel olduğundan ve indirme oturumları boş Eşitleme oturumları makul kesin olarak sistemin bir bütün olarak eşitlenmiş olduğunu söyleyebilirsiniz. 
    
Doğrudan Azure dosya paylaşımınızı değişiklikler yaptıysanız, Azure dosya eşitleme her 24 saatte gerçekleşen değişiklik numaralandırma çalıştırmaları kadar bu değişiklik algılamaz unutmayın. Bir sunucuya hatta doğrudan Azure dosya paylaşımı yapılan son değişiklikleri eksik olduğunda bulutla güncel mi Yazar mümkündür. 

---

### <a name="how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing"></a>Belirli dosyaları veya değil eşitleniyor klasörleri olup olmadığını nasıl görebilirim?
Bazı öğeler eşitlemek başarısız olan anlamına gelir. belirtilen eşitleme oturumu için portalda, PerItemErrorCount sunucuda veya dosyaları değil eşitleniyor sayısı 0'dan büyük vardır. Dosya ve klasörleri eşitlenmesini engeller özelliklere sahip olabilir. Bu özellikleri, kalıcı ve eşitleme, örneğin dosya veya klasör adı desteklenmeyen karakterler kaldırma devam etmek için belirli bir işlem gerektirir. Bunlar ayrıca geçici dosya anlamı olabilir veya klasör eşitleme otomatik olarak devam edecek; Örneğin, dosya kapatıldığında dosyaları açık eşitleme otomatik olarak sürdürür. Azure dosya eşitleme altyapısı ilgili bir sorun algıladığında, bir hata günlüğü oluşturulur, ayrıştırılmış şu anda düzgün eşitlemiyor listeleyebilirsiniz.

Bu hataları görmek için şunu çalıştırın **FileSyncErrorsReport.ps1** PowerShell Betiği (Azure dosya eşitleme aracısının aracı yükleme dizininde bulunur) açık tanıtıcıları nedeniyle eşitlenemedi dosyaları tanımlamak için desteklenmiyor karakter veya başka sorunlar. Öğe yolunda alan dizin eşitleme ile ilgili olarak dosyasının konumunu gösterir. Düzeltme adımları için aşağıdaki genel eşitleme hata listesine bakın.

#### <a name="troubleshooting-per-filedirectory-sync-errors"></a>Dosya/dizin eşitleme hatalarını giderme
**ItemResults günlüğü - öğe başına eşitleme hatası**  

| HRESULT | HRESULT (ondalık) | Hata dizesi | Sorun | Düzeltme |
|---------|-------------------|--------------|-------|-------------|
| 0x80c80207 | -2134375929 | ECS_E_SYNC_CONSTRAINT_CONFLICT | Bir dosya veya dizin değişiklik henüz bağımlı bir klasörü henüz eşitlenmedi olduğundan eşitlenemiyor. Bu öğe, bağımlı değişiklikleri eşitlendiğinde eşitler. | Eylem gerekmiyor. |
| 0x7B | 123 | ERROR_INVALID_NAME | Dosya veya dizin adı geçersiz. | Dosya veya dizin söz konusu yeniden adlandırın. Bkz: [desteklenmeyen karakterler işleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) daha fazla bilgi için. |
| 0x8007007b | -2147024773 | STIERR_INVALID_DEVICE_NAME | Dosya veya dizin adı geçersiz. | Dosya veya dizin söz konusu yeniden adlandırın. Bkz: [desteklenmeyen karakterler işleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) daha fazla bilgi için. |
| 0x80c80018 | -2134376424 | ECS_E_SYNC_FILE_IN_USE | Bir dosya kullanımda olduğundan eşitlenemiyor. Dosya artık kullanımda olmadığında eşitlenecektir. | Eylem gerekmiyor. Azure dosya eşitleme, günde bir kez açık tanıtıcıları içeren dosyaları eşitleyin sunucudaki geçici bir VSS anlık görüntüsü oluşturur. |
| 0x80c8031d | -2134375651 | ECS_E_CONCURRENCY_CHECK_FAILED | Bir dosya değişti, ancak değişiklik henüz eşitlemeden algılanmadı. Bu değişiklik algılandıktan sonra eşitleme kurtarır. | Eylem gerekmiyor. |
| 0x80c8603e | -2134351810 | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED | Azure dosya paylaşımı sınırına ulaştığından dosya eşitlenemez. | Bu sorunu çözmek için bkz: [Azure dosya paylaşımı depolama sınırına](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#-2134351810) sorun giderme kılavuzu bölümüne. |
| 0x80070005 | -2147024891 | E_ACCESSDENIED | Bu hata aşağıdaki nedenlerle oluşabilir: dosya (NTFS EFS gibi) desteklenmeyen bir çözüm tarafından şifrelenir, bekleme durumunda dosya sahip bir silme veya dosya bir DFS-R Salt okunur çoğaltma klasöründe bulunur | Tarafından desteklenmeyen bir çözüm dosya şifrelenmişse, dosyanın şifresini çözmek ve desteklenen şifreleme çözümü kullanın. Destek çözümleri listesi için bkz. [şifreleme çözümleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#encryption-solutions) Planlama Kılavuzu'nda bölümü. Dosya durumu bekleyen bir silme ise, tüm açık dosya tanıtıcıları kapatıldıktan sonra dosya silinir. Dosya bir DFS-R Salt okunur çoğaltma klasöründe yer alıyorsa, Azure dosya eşitleme DFS-R Salt okunur çoğaltma klasörlerde sunucu uç noktalarını desteklemiyor. Bkz: [planlama kılavuzunun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#distributed-file-system-dfs) daha fazla bilgi için.
| 0x20 | 32 | ERROR_SHARING_VIOLATION | Bir dosya kullanımda olduğundan eşitlenemiyor. Dosya artık kullanımda olmadığında eşitlenecektir. | Eylem gerekmiyor. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Eşitleme sırasında bir dosya değiştirildiğinden yeniden eşitlenmesi gerekiyor. | Eylem gerekmiyor. |

#### <a name="handling-unsupported-characters"></a>İşleme desteklenmeyen karakterler
Varsa **FileSyncErrorsReport.ps1** PowerShell Betiği, desteklenmeyen karakterler nedeniyle hataları gösterir (0x7b hata kodları ve 0x8007007b), kaldırmalı veya hataya karşılık gelen dosya adlarından karakterde yeniden adlandırın. Çoğu bu karakterlerden biri standart görsel kodlaması olduğundan PowerShell büyük olasılıkla bu karakterler soru işareti ya da boş dikdörtgenler yazdırın. [Değerlendirme Aracı](storage-sync-files-planning.md#evaluation-tool) desteklenmeyen karakterler tanımlamak için kullanılabilir.

Aşağıdaki tabloda, Azure dosya eşitleme henüz desteklemediği unicode karakterlerin tümünü içerir.

| Karakter kümesi | Karakter sayısı |
|---------------|-----------------|
| <ul><li>0x0000009D (osc işletim sistemi komut)</li><li>0x00000090 (DC'leri cihaz denetimi dize)</li><li>0x0000008F (ss3 tek kaydırma üç)</li><li>0x00000081 (yüksek sekizli hazır)</li><li>0x0000007F (del Sil)</li><li>0x0000008D (RI ters satır besleme)</li></ul> | 6 |
| 0x0000FDD0 - 0x0000FDEF (Arapça sunu forms-a) | 32 |
| 0x0000FFF0 - 0x0000FFFF (özel) | 16 |
| <ul><li>0x0001FFFE - 0x0001FFFF = 2 (karakter olmayan)</li><li>0x0002FFFE - 0x0002FFFF = 2 (karakter olmayan)</li><li>0x0003FFFE - 0x0003FFFF = 2 (karakter olmayan)</li><li>0x0004FFFE - 0x0004FFFF = 2 (karakter olmayan)</li><li>0x0005FFFE - 0x0005FFFF = 2 (karakter olmayan)</li><li>0x0006FFFE - 0x0006FFFF = 2 (karakter olmayan)</li><li>0x0007FFFE - 0x0007FFFF = 2 (karakter olmayan)</li><li>0x0008FFFE - 0x0008FFFF = 2 (karakter olmayan)</li><li>0x0009FFFE - 0x0009FFFF = 2 (karakter olmayan)</li><li>0x000AFFFE - 0x000AFFFF = 2 (karakter olmayan)</li><li>0x000BFFFE - 0x000BFFFF = 2 (karakter olmayan)</li><li>0x000CFFFE - 0x000CFFFF = 2 (karakter olmayan)</li><li>0x000DFFFE - 0x000DFFFF = 2 (karakter olmayan)</li><li>0x000EFFFE - 0x000EFFFF = 2 (tanımsız)</li><li>0x000FFFFE - 0x000FFFFF = 2 (tamamlayıcı özel kullanım alanı)</li></ul> | 30 |
| 0x0010FFFE, 0x0010FFFF | 2 |

### <a name="common-sync-errors"></a>Genel eşitleme hatası
<a id="-2147023673"></a>**Eşitleme oturumu iptal edildi.**  

| | |
|-|-|
| **HRESULT** | 0x800704c7 |
| **HRESULT (ondalık)** | -2147023673 | 
| **Hata dizesi** | ERROR_CANCELLED |
| **Düzeltme gerekli** | Hayır |

Eşitleme oturumları, sunucunun yeniden veya güncelleştirilmiş, VSS anlık görüntüleri, vb. dahil olmak üzere çeşitli nedenlerden dolayı başarısız olabilir. Bu hata izleme gerektirdiği gibi görünür, ancak birkaç saat diliminde kalıcı sürece bu hatayı yok saymak güvenlidir.

<a id="-2147012889"></a>**Hizmetle bağlantı kurulamadı.**    

| | |
|-|-|
| **HRESULT** | 0x80072EE7 |
| **HRESULT (ondalık)** | -2147012889 | 
| **Hata dizesi** | WININET_E_NAME_NOT_RESOLVED |
| **Düzeltme gerekli** | Evet |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134376372"></a>**Kullanıcı isteği, hizmet tarafından kısıtladı.**  

| | |
|-|-|
| **HRESULT** | 0x80c8004c |
| **HRESULT (ondalık)** | -2134376372 |
| **Hata dizesi** | ECS_E_USER_REQUEST_THROTTLED |
| **Düzeltme gerekli** | Hayır |

Eylem gerekmiyor; Sunucu yeniden deneyecek. Bu hata iki saatten daha uzun bir süre devam ederse bir destek isteği oluşturun.

<a id="-2134364065"></a>**Eşitleme bulut uç noktası belirtilen Azure dosya paylaşımına erişemiyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c8305f |
| **HRESULT (ondalık)** | -2134364065 |
| **Hata dizesi** | ECS_E_CANNOT_ACCESS_EXTERNAL_STORAGE_ACCOUNT |
| **Düzeltme gerekli** | Evet |

Azure dosya eşitleme aracısının Azure dosya paylaşımını veya artık barındıran depolama hesabı mevcut olduğundan olabilen Azure dosya paylaşımına ulaşamadığından, bu hata oluşur. Aşağıdaki adımları aracılığıyla bu hatayı gidermek:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Depolama hesabı herhangi bir ağ kuralı içermediğinden emin olmak için kontrol edin.](#troubleshoot-network-rules)
3. [Azure dosya paylaşımının var olduğundan emin olun.](#troubleshoot-azure-file-share)
4. [Azure dosya eşitleme depolama hesabına erişimi olduğundan emin olun.](#troubleshoot-rbac)

<a id="-2134364064"></a><a id="cannot-resolve-storage"></a>**Kullanılan depolama hesabı adı çözümlenemedi.**  

| | |
|-|-|
| **HRESULT** | 0x80C83060 |
| **HRESULT (ondalık)** | -2134364064 |
| **Hata dizesi** | ECS_E_STORAGE_ACCOUNT_NAME_UNRESOLVED |
| **Düzeltme gerekli** | Evet |

1. Sunucudan depolama DNS adı çözümlemesini denetleyin.

    ```powershell
    Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 443
    ```
2. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
3. [Depolama hesabı herhangi bir ağ kuralı içermediğinden emin olmak için kontrol edin.](#troubleshoot-network-rules)

<a id="-1906441138"></a>**Eşitleme, eşitleme veritabanı ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x8e5e044e |
| **HRESULT (ondalık)** | -1906441138 |
| **Hata dizesi** | JET_errWriteConflict |
| **Düzeltme gerekli** | Evet |

Azure dosya eşitleme tarafından kullanılan iç veritabanı ile ilgili bir sorun olduğunda bu hata oluşur. Bu sorun oluştuğunda, bir destek isteği oluşturun ve bu sorunu gidermenize yardımcı olmak için sizinle iletişim kuracağız.

<a id="-2134364053"></a>**Sunucuda yüklü Azure dosya eşitleme Aracısı sürümü desteklenmiyor.**  

| | |
|-|-|
| **HRESULT** | 0x80C8306B |
| **HRESULT (ondalık)** | -2134364053 |
| **Hata dizesi** | ECS_E_AGENT_VERSION_BLOCKED |
| **Düzeltme gerekli** | Evet |

Sunucuda yüklü Azure dosya eşitleme aracısının sürümü desteklenmiyor, bu hata meydana gelir. Bu sorunu çözmek için [yükseltme]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#upgrade-paths) için bir [aracı sürümü desteklenen]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#supported-versions).

<a id="-2134351810"></a>**Azure dosya paylaşımı depolama sınırına.**  

| | |
|-|-|
| **HRESULT** | 0x80c8603e |
| **HRESULT (ondalık)** | -2134351810 |
| **Hata dizesi** | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |

Azure dosya paylaşımı depolama sınırı, hangi oluşabilir bir Azure dosya paylaşımı için kota uygulanırsa veya bir Azure dosya paylaşımı için kullanım limitlerini aşarsam ulaşıldığında bu hata oluşur. Daha fazla bilgi için [bir Azure dosya paylaşımı için geçerli limitler](storage-files-scale-targets.md).

1. Depolama eşitleme hizmeti içindeki eşitleme grubuna gidin.
2. Eşitleme grubu içinde bulut uç noktası seçin.
3. Açılan bölmede Azure dosya paylaşımı adını not edin.
4. Bağlı depolama hesabını seçin. Bu bağlantı başarısız olursa, başvurulan depolama hesabı kaldırıldı.

    ![Bir depolama hesabı bağlantısını içeren bulut uç noktası ayrıntıları bölmesinin gösteren ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

5. Seçin **dosyaları** dosya paylaşımlarının listesini görüntülemek için.
6. Bulut uç noktası tarafından başvurulan Azure dosya paylaşımı için satırın sonundaki üç noktaya tıklayın.
7. Doğrulayın **kullanım** aşağıdadır **kota**. Not alternatif bir kota belirtilmediği sürece, kota eşleşecektir [Azure dosya paylaşımının en büyük boyutunu](storage-files-scale-targets.md).

    ![Azure dosya paylaşımı özelliklerinin ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-limit-reached-1.png)

Paylaşımı dolu olduğu ve bir kota ayarlı değil, bu sorunu, olası bir yol, kendi sunucu uç noktası, kendi ayrı eşitleme gruplar halinde geçerli sunucu uç noktası, her alt olmasını sağlamaktır. Bu şekilde her bir alt klasör için ayrı ayrı Azure dosya paylaşımlarını eşitler.

<a id="-2134351824"></a>**Azure dosya paylaşımı bulunamıyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c86030 |
| **HRESULT (ondalık)** | -2134351824 |
| **Hata dizesi** | ECS_E_AZURE_FILE_SHARE_NOT_FOUND |
| **Düzeltme gerekli** | Evet |

Azure dosya paylaşımı erişilebilir değil. Bu hata oluşur. Sorun giderme için:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Azure dosya paylaşımının var olduğundan emin olun.](#troubleshoot-azure-file-share)

Azure dosya paylaşımının silinmişse, yeni bir dosya paylaşımı oluşturmak ve ardından eşitleme grubu oluşturmanız gerekir. 

<a id="-2134364042"></a>**Bu Azure aboneliğinin askıya alındığında, eşitleme duraklatıldı.**  

| | |
|-|-|
| **HRESULT** | 0x80C83076 |
| **HRESULT (ondalık)** | -2134364042 |
| **Hata dizesi** | ECS_E_SYNC_BLOCKED_ON_SUSPENDED_SUBSCRIPTION |
| **Düzeltme gerekli** | Evet |

Azure Aboneliğiniz askıya alındığında, bu hata oluşur. Azure aboneliği geri yüklendiğinde eşitleme yeniden iler hale. Bkz: [neden Azure Aboneliğim devre dışı bırakılır ve nasıl miyim yeniden?](../../billing/billing-subscription-become-disable.md) daha fazla bilgi için.

<a id="-2134364052"></a>**Depolama hesabı, bir güvenlik duvarı veya yapılandırılmış olan sanal ağları içeriyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c8306c |
| **HRESULT (ondalık)** | -2134364052 |
| **Hata dizesi** | ECS_E_MGMT_STORAGEACLSNOTSUPPORTED |
| **Düzeltme gerekli** | Evet |

Azure dosya paylaşımını bir depolama hesabı Güvenlik Duvarı nedeniyle erişilemez durumda olduğunda veya depolama hesabını bir sanal ağa ait olduğundan bu hata oluşur. Azure dosya eşitleme, bu özellik için destek henüz yok. Sorun giderme için:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Depolama hesabı herhangi bir ağ kuralı içermediğinden emin olmak için kontrol edin.](#troubleshoot-network-rules)

Bu sorunu gidermek için bu kuralları kaldırın. 

<a id="-2134375911"></a>**Eşitleme, eşitleme veritabanı ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80219 |
| **HRESULT (ondalık)** | -2134375911 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_WRITE_LOCK_TIMEOUT |
| **Düzeltme gerekli** | Hayır |

Bu hata genellikle kendisini çözümler ve varsa ortaya çıkabilir:

* Eşitleme grubundaki sunucular arasında dosya değişiklikleri, yüksek bir sayı.
* Çok sayıda hataları tek tek dosyalar ve dizinler.

Bu hata birkaç saatten daha uzun bir süre devam ederse bir destek isteği oluşturun ve bu sorunu gidermenize yardımcı olmak için sizinle iletişim kuracağız.

<a id="-2146762487"></a>**Sunucu, güvenli bir bağlantı kurulamadı. Bulut hizmeti beklenmeyen bir sertifika aldı.**  

| | |
|-|-|
| **HRESULT** | 0x800b0109 |
| **HRESULT (ondalık)** | -2146762487 |
| **Hata dizesi** | CERT_E_UNTRUSTEDROOT |
| **Düzeltme gerekli** | Evet |

Kuruluşunuz, SSL sonlandırma proxy kullanıyorsa veya kötü amaçlı bir varlık, sunucunuz ve Azure dosya eşitleme hizmeti arasındaki trafik kesintiye bu hata oluşabilir. (Kuruluşunuz bir SSL sonlandırma proxy kullandığından) bu beklenir, bir kayıt defteri geçersiz kılma ile sertifika doğrulama atlayın.

1. SkipVerifyingPinnedRootCertificate kayıt defteri değeri oluşturun.

    ```powershell
    New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Azure\StorageSync -Name SkipVerifyingPinnedRootCertificate -PropertyType DWORD -Value 1
    ```

2. Kayıtlı sunucu eşitleme hizmetini yeniden başlatın.

    ```powershell
    Restart-Service -Name FileSyncSvc -Force
    ```

Bu kayıt defteri değeri ayarlandığında Azure Dosya Eşitleme aracısı, verileri sunucu ile bulut hizmeti arasında aktarırken yerel olarak güvenilen herhangi bir SSL sertifikasını kabul eder.

<a id="-2147012894"></a>**Hizmetle bağlantı kurulamadı.**  

| | |
|-|-|
| **HRESULT** | 0x80072EE2 |
| **HRESULT (ondalık)** | -2147012894 |
| **Hata dizesi** | WININET_E_TIMEOUT |
| **Düzeltme gerekli** | Evet |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134375680"></a>**Eşitleme, kimlik doğrulaması ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80300 |
| **HRESULT (ondalık)** | -2134375680 |
| **Hata dizesi** | ECS_E_SERVER_CREDENTIAL_NEEDED |
| **Düzeltme gerekli** | Evet |

Bu hataya neden olabilir:

- Sunucu saati yanlış
- Sunucu uç noktası silinemedi
- Kimlik doğrulaması için kullanılan sertifikanın süresi doldu. 
    Sertifikanın süresi dolmuş olmadığını denetlemek için aşağıdaki adımları gerçekleştirin:  
    1. Sertifikalar MMC ek bileşenini açın, bilgisayar hesabını seçin ve sonra da için sertifikalar (yerel bilgisayar) \Personal\Certificates gidin.
    2. İstemci kimlik doğrulama sertifikasının süresi doldu, kontrol edin.

Sunucu saatinin doğru ise, bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:

1. Azure dosya eşitleme Aracısı sürüm 4.0.1.0 doğrulayın veya üstü yüklü.
2. Sunucuda aşağıdaki PowerShell komutlarını çalıştırın:

    ```powershell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
    Login-AzStorageSync -SubscriptionID <guid> -TenantID <guid>
    Reset-AzStorageSyncServerCertificate -SubscriptionId <guid> -ResourceGroupName <string> -StorageSyncServiceName <string>
    ```

<a id="-1906441711"></a><a id="-2134375654"></a><a id="doesnt-have-enough-free-space"></a>**Sunucu uç noktasını bulunduğu birim üzerinde disk alanı düşüktür.**  

| | |
|-|-|
| **HRESULT** | 0x8e5e0211 |
| **HRESULT (ondalık)** | -1906441711 |
| **Hata dizesi** | JET_errLogDiskFull |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c8031a |
| **HRESULT (ondalık)** | -2134375654 |
| **Hata dizesi** | ECS_E_NOT_ENOUGH_LOCAL_STORAGE |
| **Düzeltme gerekli** | Evet |

Bu hata toplu doldu nedeniyle oluşur. Sunucu uç noktası dışındaki dosyalara birimde alan boşaltın kullandığından bu hata genellikle oluşur. Ek sunucu uç noktaları ekleyerek birimde alan boşaltın, farklı bir birime dosyalar taşınıyor ve sunucu uç noktasını birimin boyutunu artırma açıktır.

<a id="-2134364145"></a><a id="replica-not-ready"></a>**Hizmet henüz bu sunucu uç noktası ile eşitlemek hazır değil.**  

| | |
|-|-|
| **HRESULT** | 0x80c8300f |
| **HRESULT (ondalık)** | -2134364145 |
| **Hata dizesi** | ECS_E_REPLICA_NOT_READY |
| **Düzeltme gerekli** | Hayır |

Değişiklikleri Azure dosya paylaşımında doğrudan ve değişiklik algılama sürüyor olduğundan bu hata oluşur. Değişiklik algılama tamamlandığında eşitleme ettiğiniz günde başlatılır.

[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="-2134375877"></a><a id="-2134375908"></a><a id="-2134375853"></a>**Eşitleme, birçok ayrı dosyalar ile ilgili sorunlar nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8023b |
| **HRESULT (ondalık)** | -2134364145 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c8021c |
| **HRESULT (ondalık)** | -2134375908 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_KNOWLEDGE_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c80253 |
| **HRESULT (ondalık)** | -2134375853 |
| **Hata dizesi** | ECS_E_TOO_MANY_PER_ITEM_ERRORS |
| **Düzeltme gerekli** | Evet |

Durumlarda olduğu dosya eşitleme hatalarını çok sayıda, Eşitleme oturumları başarısız olmaya başlar. <!-- To troubleshoot this state, see [Troubleshooting per file/directory sync errors]().-->

> [!NOTE]
> Azure dosya eşitleme, günde bir kez açık tanıtıcıları içeren dosyaları eşitleyin sunucudaki geçici bir VSS anlık görüntüsü oluşturur.

<a id="-2134376423"></a>**Eşitleme sunucusu uç nokta yolu ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80019 |
| **HRESULT (ondalık)** | -2134376423 |
| **Hata dizesi** | ECS_E_SYNC_INVALID_PATH |
| **Düzeltme gerekli** | Evet |

Yolun var olduğundan, yerel bir NTFS biriminde bulunduğundan ve bir yeniden ayrıştırma noktası veya mevcut bir sunucu uç noktası olmadığından emin olun.

<a id="-2134375817"></a>**Eşitleme filtresi sürücüsü sürümü aracı sürümü ile uyumlu olmadığı için başarısız oldu**  

| | |
|-|-|
| **HRESULT** | 0x80C80277 |
| **HRESULT (ondalık)** | -2134375817 |
| **Hata dizesi** | ECS_E_INCOMPATIBLE_FILTER_VERSION |
| **Düzeltme gerekli** | Evet |

Yüklenen bulut Katmanlandırma Filtresi Sürücüsü (StorageSync.sys) sürümü depolama eşitleme Aracısı (FileSyncSvc) hizmeti ile uyumlu olmadığından, bu hata oluşur. Azure dosya eşitleme aracısının yükseltilmişse, yüklemeyi tamamlamak için sunucuyu yeniden başlatın. Hata devam ederse, aracıyı kaldırmak, sunucuyu yeniden başlatın ve Azure dosya eşitleme aracısını yeniden yükleyin.

<a id="-2134376373"></a>**Hizmet şu anda kullanılamıyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c8004b |
| **HRESULT (ondalık)** | -2134376373 |
| **Hata dizesi** | ECS_E_SERVICE_UNAVAILABLE |
| **Düzeltme gerekli** | Hayır |

Azure dosya eşitleme hizmeti kullanılamadığı için bu hata oluşur. Bu hata otomatik olduğunda Azure dosya eşitleme hizmeti olduğundan çözümleme kullanılabilir yeniden.

<a id="-2134375922"></a>**Eşitleme, eşitleme veritabanı geçici bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8020e |
| **HRESULT (ondalık)** | -2134375922 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_WRITE_LEASE_LOST |
| **Düzeltme gerekli** | Hayır |

Bu hata, eşitleme veritabanı ile dahili bir sorun nedeniyle oluşur. Bu hata otomatik ne zaman çözümleme Azure dosya eşitleme yeniden deneme zaman eşitleme. Bir genişletme süre bu hata devam ederse bir destek isteği oluşturun ve bu sorunu gidermenize yardımcı olmak için sizinle iletişim kuracağız.

### <a name="common-troubleshooting-steps"></a>Genel sorun giderme adımları
<a id="troubleshoot-storage-account"></a>**Depolama hesabının var olduğunu doğrulayın.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Depolama eşitleme hizmeti içindeki eşitleme grubuna gidin.
2. Eşitleme grubu içinde bulut uç noktası seçin.
3. Açılan bölmede Azure dosya paylaşımı adını not edin.
4. Bağlı depolama hesabını seçin. Bu bağlantı başarısız olursa, başvurulan depolama hesabı kaldırıldı.
    ![Bir depolama hesabı bağlantısını içeren bulut uç noktası ayrıntıları bölmesinin gösteren ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
# Variables for you to populate based on your configuration
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"
$region = "<Az_Region>"
$resourceGroup = "<RG_Name>"
$syncService = "<storage-sync-service>"
$syncGroup = "<sync-group>"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# Log into the Azure account and put the returned account information
# in a reference variable.
$acctInfo = Connect-AzAccount

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = [System.String[]]@()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the " + `
        " selected Azure Region or the region is mistyped.")
}

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = [System.String[]]@()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    throw [System.Exception]::new("The provided resource group $resourceGroup does not exist.")
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region

# Check to make sure the provided Storage Sync Service
# exists.
$syncServices = [System.String[]]@()

Get-AzStorageSyncService -ResourceGroupName $resourceGroup | ForEach-Object {
    $syncServices += $_.DisplayName
}

if ($storageSyncServices -notcontains $syncService) {
    throw [System.Exception]::new("The provided Storage Sync Service $syncService does not exist.")
}

# Check to make sure the provided Sync Group exists
$syncGroups = [System.String[]]@()

Get-AzStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $syncService | ForEach-Object {
    $syncGroups += $_.DisplayName
}

if ($syncGroups -notcontains $syncGroup) {
    throw [System.Exception]::new("The provided sync group $syncGroup does not exist.")
}

# Get reference to cloud endpoint
$cloudEndpoint = Get-AzStorageSyncCloudEndpoint `
    -ResourceGroupName $resourceGroup `
    -StorageSyncServiceName $storageSyncService `
    -SyncGroupName $syncGroup

# Get reference to storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object { 
    $_.Id -eq $cloudEndpoint.StorageAccountResourceId
}

if ($storageAccount -eq $null) {
    Write-Host "The storage account referenced in the cloud endpoint does not exist."
}
```
---

<a id="troubleshoot-network-rules"></a>**Depolama hesabı herhangi bir ağ kuralı içermediğinden emin olmak için kontrol edin.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Depolama hesabında bir kez seçin **güvenlik duvarları ve sanal ağlar** depolama hesabının sol tarafındaki.
2. Depolama hesabı içinde **tüm ağlardan erişime izin ver** radyo düğmesini seçili olmalıdır.
    ![Bir depolama hesabı güvenlik duvarı ve ağ kuralları devre dışı gösteren ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-inaccessible-2.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
if ($storageAccount.NetworkRuleSet.DefaultAction -ne 
    [Microsoft.Azure.Commands.Management.Storage.Models.PSNetWorkRuleDefaultActionEnum]::Allow) {
    Write-Host ("The storage account referenced contains network " + `
        "rules which are not currently supported by Azure File Sync.")
}
```
---

<a id="troubleshoot-azure-file-share"></a>**Azure dosya paylaşımının var olduğundan emin olun.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Tıklayın **genel bakış** ana depolama hesabını sayfasına dönmek için sol taraftaki İçindekiler üzerinde.
2. Seçin **dosyaları** dosya paylaşımlarının listesini görüntülemek için.
3. Bulut uç noktası tarafından başvurulan dosya paylaşımı (Bu yukarıdaki 1. adımda not ettiğiniz) dosya paylaşımları listesinde göründüğünü doğrulayın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $cloudEndpoint.StorageAccountShareName -and
    $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    Write-Host "The Azure file share referenced by the cloud endpoint does not exist"
}
```
---

<a id="troubleshoot-rbac"></a>**Azure dosya eşitleme depolama hesabına erişimi olduğundan emin olun.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Tıklayın **erişim denetimi (IAM)** soldaki İçindekiler üzerinde.
1. Tıklayın **rol atamaları** kullanıcılar ve uygulamalar listesi için sekmesinde (*hizmet sorumluları*), depolama hesabınıza erişimi vardır.
1. Doğrulama **karma dosya eşitleme hizmeti** listesinde görünür **okuyucu ve veri erişimi** rol. 

    ![Depolama hesabının erişim denetimi sekmesindeki karma dosya eşitleme hizmeti hizmet sorumlusunun bir ekran görüntüsü](media/storage-sync-files-troubleshoot/file-share-inaccessible-3.png)

    Varsa **karma dosya eşitleme hizmeti** listede görüntülenmiyorsa, aşağıdaki adımları gerçekleştirin:

    - **Ekle**'ye tıklayın.
    - İçinde **rol** alanın, Seç **okuyucu ve veri erişimi**.
    - İçinde **seçin** alanına **karma dosya eşitleme hizmeti**, rolü seçin ve tıklayın **Kaydet**.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell    
$foundSyncPrincipal = $false
Get-AzRoleAssignment -Scope $storageAccount.Id | ForEach-Object { 
    if ($_.DisplayName -eq "Hybrid File Sync Service") {
        $foundSyncPrincipal = $true
        if ($_.RoleDefinitionName -ne "Reader and Data Access") {
            Write-Host ("The storage account has the Azure File Sync " + `
                "service principal authorized to do something other than access the data " + `
                "within the referenced Azure file share.")
        }

        break
    }
}

if (!$foundSyncPrincipal) {
    Write-Host ("The storage account does not have the Azure File Sync " + `
                "service principal authorized to access the data within the " + ` 
                "referenced Azure file share.")
}
```
---

### <a name="how-do-i-prevent-users-from-creating-files-containing-unsupported-characters-on-the-server"></a>Kullanıcıların sunucuda desteklenmeyen karakterler içeren dosyaları oluşturmasını nasıl önlemek?
Kullanabileceğiniz [Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya filtrelerini](https://docs.microsoft.com/windows-server/storage/fsrm/file-screening-management) sunucuda oluşturulan gelen blok dosyalarla adlarında desteklenmeyen karakterler. Desteklenmeyen karakterler çoğunu yazdırılabilir olmadığından ve kendi onaltılık gösterimleri ilk karakter olarak cast gerek PowerShell kullanarak bunu gerekebilir.

Önce kullanarak bir FSRM dosya grubu oluşturma [FsrmFileGroup yeni cmdlet](https://docs.microsoft.com/powershell/module/fileserverresourcemanager/new-fsrmfilegroup). Bu örnek yalnızca iki desteklenmeyen karakterler içerecek Grup tanımlar, ancak bu kadar çok dosyası grubunuzdaki gerekirse karakterleri içerebilir.

```powershell
New-FsrmFileGroup -Name "Unsupported characters" -IncludePattern @(("*"+[char]0x00000090+"*"),("*"+[char]0x0000008F+"*"))
```

Bir FSRM dosya grubu tanımlandıktan sonra yeni FsrmFileScreen cmdlet'ini kullanarak bir FSRM dosya filtresi oluşturabilirsiniz.

```powershell
New-FsrmFileScreen -Path "E:\AFSdataset" -Description "Filter unsupported characters" -IncludeGroup "Unsupported characters"
```

> [!Important]  
> Dosya filtreleri yalnızca Azure dosya eşitleme tarafından desteklenmeyen karakterler oluşturulmasını engellemek için kullanılması gerektiğini unutmayın. Dosya filtreleri diğer senaryolarda kullanılması durumunda eşitleme dosyaları sunucuya Azure dosya paylaşımından indirmek sürekli olarak deneyecek ve yüksek veri çıkışı içinde elde edilen dosya filtresi nedeniyle engellendi. 

## <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulutta için iki yol vardır katmanlama:

- Dosyaları katmanı, Azure dosya eşitleme Azure dosyaları için bir dosya Tier deneyip başarısız, yani başarısız olabilir.
- Dosyaları (StorageSync.sys) başarısız bir kullanıcı, katmanlı bir dosyaya erişmeye çalıştığında verileri indirmek için Azure dosya eşitleme dosya sistemi filtresi anlamına çağırma başarısız olabilir.

İki ana sınıfı ya da başarısızlığa giden oluşabilecek hatalar vardır:

- Bulut depolama hataları
    - *Geçici depolama hizmet kullanılabilirliği sorunlarını*. Daha fazla bilgi için [Azure depolama için hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).
    - *Erişilemez bir Azure dosya paylaşımının*. Bulut uç noktasına bir eşitleme grubunda hala olduğunda Azure dosya paylaşımı sildiğinizde bu hata genellikle gerçekleşir.
    - *Erişilemez bir depolama hesabı*. Bulut uç noktasına bir eşitleme grubunda olan bir Azure dosya paylaşımının hala sahipken depolama hesabını sildiğinizde bu hata genellikle gerçekleşir. 
- Sunucu hataları 
  - *Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) yüklenmedi*. İstekleri katmanlama/geri çağırma yanıtlamak için Azure dosya eşitleme dosya sistemi filtresi yüklenmesi gerekir. Yüklü filtreyi çeşitli nedenlerle oluşabilir, ancak en yaygın nedeni bir yönetici, el ile kaldırılmış olduğunu. Azure dosya eşitleme dosya sistemi filtresi, her zaman Azure dosya eşitleme için düzgün çalışması için yüklenmesi gerekir.
  - *Eksik, bozuk veya başka bozuk yeniden ayrıştırma noktası*. Bir özel veri yapısı iki bölümden oluşan bir dosya çubuğunda bir ayrıştırma noktasıdır:
    1. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasına GÇ bazı eylemler yapmanız gerekebilir işletim sistemine gösterir. bir yeniden ayrıştırma etiketi. 
    2. Yeniden ayrıştırma veriler, dosya sistemi filtresinden ilişkili bulut uç noktasına (Azure dosya paylaşımı) dosyayı URI'sini belirtir. 
        
       En yaygın yolu bir yeniden ayrıştırma noktası bozuk, yöneticinin etiket veya verisini değiştirmek deneyip denemeyeceğini ' dir. 
  - *Ağ bağlantısı sorunları*. Katmanı veya bir dosyayı geri için sunucunun internet bağlantısı olması gerekir.

Aşağıdaki bölümlerde, bulut katmanlaması sorunları gidermek ve bir sorunun bir bulut depolama sorunu veya server sorunu olup olmadığını belirlemek nasıl gösterir.

<a id="monitor-tiering-activity"></a>**Bir sunucuda katmanlama Etkinlik izleme**  
Bir sunucuda katmanlama etkinliğini izlemek için olay kimliği 9003 9016 ve 9029 Telemetri olay günlüğüne (uygulamalar ve Olay Görüntüleyicisi'nde Services\Microsoft\FileSync\Agent altında bulunur) kullanın.

- Olay Kimliği 9003 sunucu uç noktası için hata dağıtımını sağlar. Örneğin, toplam hata sayısı, hata kodu, vb. Unutmayın, hata kodu bir olay kaydedilir.
- Olay Kimliği 9016 hayalet sonuçları için bir birim sağlar. Örneğin, boş alan yüzdesi olan dosya sayısı, oturumda ghost vb. için başarısız oldu. dosya sayısı hayalet kopyası.
- Olay Kimliği 9029 hayalet oturum bilgilerini sunucu uç noktası sağlar. Örneğin, dosyaların sayısı, oturumda çalıştı dosya sayısı oturumda, dosyaların sayısı, katmanlı zaten katmanlı, vs.

<a id="monitor-recall-activity"></a>**Bir sunucuya geri çağırma Etkinlik izleme**  
Bir sunucuya geri çağırma etkinliği izlemek için olay kimliği 9005 9006 ve 9009 9059 Telemetri olay günlüğüne (uygulamalar ve Olay Görüntüleyicisi'nde Services\Microsoft\FileSync\Agent altında bulunur) kullanın.

- Olay Kimliği 9005 sunucu uç noktası için geri çağırma güvenilirlik sağlar. Örneğin, erişilen, benzersiz dosyaları toplam benzersiz başarısız erişim, vb. ile toplam sayısı.
- Sunucu uç noktası için geri çağırma hatası dağıtım öğesini belirten Olay No. 9006 sağlar. Örneğin, toplam başarısız istekler, hata kodu, vb. Unutmayın, hata kodu bir olay kaydedilir.
- Olay Kimliği 9009 sunucu uç noktası için geri çağırma oturum bilgilerini sağlar. Örneğin, DurationSeconds, CountFilesRecallSucceeded CountFilesRecallFailed, vb.
- Olay Kimliği 9059 uygulama geri çekme dağıtım için sunucu uç noktası sağlar. Örneğin, ShareId, uygulama adı ve TotalEgressNetworkBytes.

<a id="files-fail-tiering"></a>**Tier başarısız dosyaları sorunlarını giderme**  
Azure dosyaları'na Tier dosyaları başarısız olursa:

1. Olay Görüntüleyicisi'nde telemetri, işletimsel ve tanılama uygulamaları ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerinin gözden geçirin. 
   1. Azure Dosya paylaşımındaki dosyaları mevcut olmadığını doğrulayın.

      > [!NOTE]
      > Katmanlı önce bir Azure dosya paylaşımı için bir dosya yeniden eşitlenmesi gerekir.

   2. Sunucunun internet bağlantısı olduğunu doğrulayın. 
   3. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
       - Yükseltilmiş bir komut isteminde çalıştırın `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücülerini listelendiğini doğrulayın.

> [!NOTE]
> Katmanı bir dosya başarısız olursa, bir olay kimliği 9003 saatte bir Telemetri olay günlüğüne kaydedilir (hata kodu bir olay günlüğe kaydedilir). Bir sorunu tanılamak için ek bilgi gerekiyorsa işlemsel ve tanılama günlüklerinin kullanılmalıdır.

<a id="files-fail-recall"></a>**Çekilecek başarısız dosyaları sorunlarını giderme**  
Dosyaları çağrılmaya başarısız olursa:
1. Olay Görüntüleyicisi'nde telemetri, işletimsel ve tanılama uygulamaları ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerinin gözden geçirin.
    1. Azure Dosya paylaşımındaki dosyaları mevcut olmadığını doğrulayın.
    2. Sunucunun internet bağlantısı olduğunu doğrulayın. 
    3. Hizmetler MMC ek bileşenini açın ve (FileSyncSvc) depolama eşitleme aracı hizmetinin çalışıyor olduğunu doğrulayın.
    4. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
        - Yükseltilmiş bir komut isteminde çalıştırın `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücülerini listelendiğini doğrulayın.

> [!NOTE]
> Çağırmak bir dosya başarısız olursa, bir olay kimliği 9006 saatte bir Telemetri olay günlüğüne kaydedilir (hata kodu bir olay günlüğe kaydedilir). Bir sorunu tanılamak için ek bilgi gerekiyorsa işlemsel ve tanılama günlüklerinin kullanılmalıdır.

<a id="files-unexpectedly-recalled"></a>**Beklenmedik bir şekilde bir sunucuya geri dosyaları sorunlarını giderme**  
Bunlar Atla çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece virüsten koruma, yedekleme ve çok sayıda dosya okuma diğer uygulamalar istenmeyen geri çekme neden. Bu seçeneği destekleyen ürünler için çevrimdışı dosyaları atlamak, virüsten koruma taramaları veya yedekleme işleri gibi işlemler sırasında istenmeyen yeniden çağırma olaylarından kaçınılmasına yardımcı olabilir.

Çözümlerinin çevrimdışı dosyaları okumayı atlayacak şekilde yapılandırılması konusunda bilgi almak için yazılım satıcınızla iletişime geçin.

İstenmeyen geri çekme de zaman dosya Gezgini'nde dosyaları tarama gibi diğer senaryolarda oluşabilir. Dosya Gezgini'nde bulut katmanlı dosyalara sahip bir klasörün açılması, istenmeyen yeniden çağırma işlemlerine neden olabilir. Bu durumun gerçekleşme olasılığı, sunucuda virüsten koruma çözümünün etkinleştirilmiş olması halinde daha yüksektir.

> [!NOTE]
>Olay Kimliği 9059 Telemetri olay günlüğünde ınsights'taki hangi uygulamaların geri çekme neden olduğunu belirlemek için kullanın. Bu olay, uygulama geri çekme dağıtım için sunucu uç noktası sağlar ve saatte bir günlüğe kaydedilir.

## <a name="general-troubleshooting"></a>Genel sorun giderme
Bir sunucuda Azure dosya eşitleme ile sorunlarla karşılaşırsanız, aşağıdaki adımları tamamlayarak başlayın:
1. Olay Görüntüleyicisi'nde telemetri, işletimsel ve tanılama olay günlüklerini gözden geçirin.
    - Katmanlama, eşitleme ve geri çağırma sorunları günlüğe telemetri, tanılama ve operasyonel olay günlüklerini uygulamalar ve Services\Microsoft\FileSync\Agent altında.
    - Bir sunucuya (örneğin, yapılandırma ayarlarını) yönetimiyle ilgili sorunlar, uygulamalar ve Services\Microsoft\FileSync\Management işletimsel ve tanılama olay günlüklerine kaydedilir.
2. Azure dosya eşitleme hizmeti, sunucu üzerinde çalıştığını doğrulayın:
    - Hizmetler MMC ek bileşenini açın ve (FileSyncSvc) depolama eşitleme aracısı hizmetinin çalıştığını doğrulayın.
3. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
    - Yükseltilmiş bir komut isteminde çalıştırın `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücülerini listelendiğini doğrulayın.

Sorun çözülmezse AFSDiag aracı çalıştırın:
1. (Örneğin, C:\Output) AFSDiag çıkış kaydedileceği bir dizin oluşturun.
2. Yükseltilmiş bir PowerShell penceresi açın ve ardından (her komuttan sonra Enter tuşuna basın) aşağıdaki komutları çalıştırın:

    ```powershell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Azure dosya eşitleme çekirdek modu izleme düzeyi için girin **1** (Aksi takdirde, daha ayrıntılı izlemelerini oluşturmak için belirtilmediği sürece), ve ardından Enter tuşuna basın.
4. Azure dosya eşitleme kullanıcı modu izleme düzeyi için girin **1** (Aksi takdirde, daha ayrıntılı izlemelerini oluşturmak için belirtilmediği sürece), ve ardından Enter tuşuna basın.
5. Sorunu yeniden oluşturun. İşlemi tamamladığınızda, girin **D**.
6. Günlükleri içeren bir .zip dosyası ve izleme dosyaları belirttiğiniz çıkış dizinine kaydedilir.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
- [Azure dosyaları hakkında sık sorulan sorular](storage-files-faq.md)
- [Windows’ta Azure Dosyalar sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux’ta Azure Dosyalar sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
