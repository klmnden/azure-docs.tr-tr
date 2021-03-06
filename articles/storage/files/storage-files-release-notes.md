---
title: Azure dosya eşitleme Aracısı sürüm notları | Microsoft Docs
description: Azure dosya eşitleme Aracısı sürüm notları.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 6/27/2019
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: ba7b8ae5c7e68c22b83583de6cd0b84f621d4b25
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786659"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı sürüm notları
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Kilometre Taşı | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| V7 Yayın - [KB4490495](https://support.microsoft.com/help/4490495)| 7.0.0.0 | 19 Haziran 2019 | [Yayını](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#automatic-agent-lifecycle-management) |
| Güncelleştirme paketi - Haziran 2019 [KB4489739](https://support.microsoft.com/help/4489739)| 6.3.0.0 | 27 Haziran 2019 | Desteklenen (önerilen sürüm) |
| Güncelleştirme paketi - Haziran 2019 [KB4489738](https://support.microsoft.com/help/4489738)| 6.2.0.0 | 13 Haziran 2019 | Desteklenen |
| Güncelleştirme paketi - Mayıs 2019 [KB4489737](https://support.microsoft.com/help/4489737)| 6.1.0.0 | 7 Mayıs 2019 | Desteklenen |
| V6 Yayın - [KB4489736](https://support.microsoft.com/help/4489736)| 6.0.0.0 | 21 Nisan 2019 | Desteklenen |
| Güncelleştirme paketi - Nisan 2019 [KB4481061](https://support.microsoft.com/help/4481061)| 5.2.0.0 | 4 Nisan 2019 | Desteklenen |
| Güncelleştirme paketi - Mart 2019 [KB4481060](https://support.microsoft.com/help/4481060)| 5.1.0.0 | 7 Mart 2019 | Desteklenen |
| V5 Yayın - [KB4459989](https://support.microsoft.com/help/4459989)| 5.0.2.0 | 12 Şubat 2019 | Desteklenen |
| Güncelleştirme paketi - Ocak 2019 [KB4481059](https://support.microsoft.com/help/4481059)| 4.3.0.0 | 14 Ocak 2019 | Desteklenen |
| Aralık 2018'e güncelleştirme paketi - [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10 Aralık 2018'e | Desteklenen |
| Aralık 2018'e güncelleştirme paketi | 4.1.0.0 | 4 Aralık 2018'e | Desteklenen |
| V4 sürüm | 4.0.1.0 | 13 Kasım 2018 | Desteklenen |
| Eylül 2018'e güncelleştirme paketi | 3.3.0.0 | 24 Eylül 2018 | Destekleniyor - 19 Temmuz 2019 ' aracı sürümü dolacak |
| Ağustos 2018 güncelleştirme paketi | 3.2.0.0 | 15 Ağustos 2018 | Destekleniyor - 19 Temmuz 2019 ' aracı sürümü dolacak |
| Genel kullanılabilirlik | 3.1.0.0 | 19 Temmuz 2018 | Destekleniyor - 19 Temmuz 2019 ' aracı sürümü dolacak |
| Süresi dolmuş olan aracıları | 1.1.0.0 - 3.0.13.0 | Yok | Desteklenmiyor - Aracı sürümleri 1 Ekim 2018 tarihinde süresi doldu |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-7000"></a>Aracı sürümü 7.0.0.0'dan
Aşağıdaki sürüm notları (19 Haziran 2019'da yayımlanan) Azure dosya eşitleme aracısının sürümü için 7.0.0.0'dan ' dir.

### <a name="improvements-and-issues-that-are-fixed"></a>Geliştirmeler ve düzeltilen sorunlar

- Büyük dosya paylaşımı boyutları için destek
    - Büyük Azure dosya paylaşımlarını Önizleme ile biz dosya eşitleme için destek sınırlarımızı artmaktadır. Bu ilk adım, Azure dosya eşitleme artık yukarı 25 TB ve 50million dosyalarına tek bir ad alanı eşitlemeyi destekler. Büyük dosya paylaşımı önizleme uygulamak için bu formu doldurmanız https://aka.ms/azurefilesatscalesurvey. 
- Geliştirilmiş Azure yedekleme dosya düzeyinde geri yükleme
    - Azure Backup kullanılarak geri tek tek dosyalar artık algılandı ve sunucu uç noktası için daha hızlı bir şekilde eşitlenen.
- Gelişmiş bulut geri çağırma cmdlet'i güvenilirlik katmanı 
    - Geri çağırma cmdlet (Invoke-StorageSyncFileRecall) katmanlama bulut artık desteklediği dosya başına yeniden deneme sayısı ve yeniden deneme gecikmesi için robocopy benzer.
- Yalnızca TLS 1.2 desteği (TLS 1.0 ve 1.1 devre dışı)
    - TLS 1.0 ve 1.1 devre dışı olan sunucularda TLS 1.2 kullanarak azure dosya eşitleme destekler. Sunucuda TLS 1.0 ve 1.1 devre dışı bırakıldı, önce bu geliştirme, sunucu kaydı başarısız olur.
- Çeşitli performans ve güvenilirlik geliştirmeleri eşitleyin ve bulut katmanlaması
    - Bu sürümdeki birkaç güvenilirlik ve performans geliştirmeleri vardır. Bunlardan bazıları, bant genişliği azaltma zamanlama kümesi olduğunda bulut katmanlamayı daha verimli ve Azure dosya eşitleme bütün bir çözüm, bu durumlarda daha iyi hale için hedeflenir.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracıyı Nano Server dağıtım seçeneği desteklenmiyor.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracıyı en az 2 GiB bellek gerektirir. Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Azure dosya eşitleme aracısının sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterler olan dosyalar. Bkz: [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) desteklenmeyen karakterler listesi.
- Dosyaları ya da bir nokta ile biten dizinleri.
- 2\.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar erişilemez hale gelecektir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktasını silindikten sonra 30 gün geçtiğinde veya Bulut uç noktası silinirse değil geri katmanlı dosyalar kullanılamaz.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya bir sunucu uç noktası konumu uygulama disk belleği dosyası depolamayın.
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken/MIR seçeneği dosya zaman damgası korumak için kullanın. Bu, eski dosyalar son erişilen dosyaların daha çabuk katmanlı garanti eder.

## <a name="agent-version-6300"></a>Aracı sürümü 6.3.0.0
Aşağıdaki sürüm notları, 27 Haziran 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 6.3.0.0 geçerlidir. Bu Notlar 6.0.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Uygulama ayarlarına erişme veya bir sunucu uç noktası konumu SMB üzerinden gözatma Windows Server 2012 R2'de yavaş 
- Azure dosya eşitleme v6 aracıyı yükledikten sonra daha yüksek CPU kullanımı
- Bulut katmanlaması telemetri geliştirmeleri

## <a name="agent-version-6200"></a>Aracı sürümü 6.2.0.0
Aşağıdaki sürüm notları 13 Haziran 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 6.2.0.0 ' dir. Bu Notlar 6.0.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Arka plan geri çağırma dosya sunucusuna yüklerken, sunucu uç noktası oluşturduktan sonra yüksek CPU kullanımı ortaya çıkabilir
- Eşitleme ve bulut katmanlandırma işlemlerini belirteci süre sonu nedeniyle ECS_E_SERVER_CREDENTIAL_NEEDED hatasıyla başarısız olabilir
- Dosyayı indirmek için URL ayrılmış karakterleri içeriyorsa, bir dosya geri çağırma başarısız olabilir 

## <a name="agent-version-6100"></a>Aracı sürümü 6.1.0.0
Aşağıdaki sürüm notları 6 Mayıs 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 6.1.0.0 ' dir. Bu Notlar 6.0.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Azure dosya eşitleme Aracısı sürüm 6.0 yüklü olan sunucularda Aracısı sürümü ve sunucu uç noktası yapılandırmasını görüntülemek Windows Admin Center başarısız olur.

## <a name="agent-version-6000"></a>Aracı sürümü 6.0.0.0
Aşağıdaki sürüm notları (22 Nisan 2019'da yayımlanan) Azure dosya eşitleme aracısının sürümü için 6.0.0.0 ' dir.

### <a name="improvements-and-issues-that-are-fixed"></a>Geliştirmeler ve düzeltilen sorunlar

- Aracı otomatik güncelleştirmesi desteği
  - Biz Geri bildiriminizi heard ve Azure dosya eşitleme sunucusu aracının otomatik güncelleştirme özelliği eklendi. Daha fazla bilgi için [Azure dosya eşitleme Aracısı güncelleştirme ilkesi](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#azure-file-sync-agent-update-policy).
- Azure dosya desteği ACL'leri paylaşın
  - Sunucu uç noktaları arasında ACL'leri eşitleniyor Azure dosya eşitleme her zaman desteklemiştir ancak ACL'leri bulut uç noktasına (Azure dosya paylaşımı) eşitlenmedi. Bu sürüm, bulut uç noktaları ile sunucu arasındaki eşitleme ACL'ler için destek ekler.
- Paralel karşıya yükleme ve sunucu uç noktası için Eşitleme oturumları indirin 
  - Sunucu uç noktaları, dosya yükleme ve indirme aynı anda artık desteklenmektedir. Artık bunu dosyaları tamamlamak bir yükleme bekleniyor, Azure dosya paylaşımına yüklenebilir. 
- Birim ve katmanlama durumunu almak için yeni bulut Katmanlandırma cmdlet'leri
  - İki yeni, sunucu yerel PowerShell cmdlet'leri artık bulut katmanlama ve dosya geri çağırma bilgi almak için kullanılabilir. Bunlar sunucusundaki günlük kaydı bilgilerini iki olay kanallarından kullanımına:
    - Get-StorageSyncFileTieringResult tüm dosyaları ve katmanlı henüz yollarına ve nedeni raporlarda neden listelenir.
    - Get-StorageSyncFileRecallResult tüm dosya geri çağırma olayları bildirir. Geri her dosya ve yolu listeler yanı sıra başarı veya hata için bu geri çağırma.
  - Varsayılan olarak, hem 1 MB'a kadar her olay kanalları depolayabilir: olay kanal boyutunu artırarak rapor dosyaları miktarını artırabilir.
- FIPS modundayken desteği
  - Azure dosya eşitleme, artık Azure dosya eşitleme aracısının yüklü olduğu sunucular etkinleştirme FIPS modundayken destekler.
    - FIPS modundayken sunucunuzdaki etkinleştirmeden önce Azure dosya eşitleme aracısını yükleyin ve [PackageManagement Modülü](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) sunucunuzdaki. FIPS sunucuda zaten etkin değilse [el ile yükleme](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download) [PackageManagement Modülü](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) sunucunuza.
- Çeşitli güvenilirlik geliştirmeleri için bulut katmanlama ve eşitleme

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracıyı Nano Server dağıtım seçeneği desteklenmiyor.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracıyı en az 2 GiB bellek gerektirir. Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Azure dosya eşitleme aracısının sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterler olan dosyalar. Bkz: [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) desteklenmeyen karakterler listesi.
- Dosyaları ya da bir nokta ile biten dizinleri.
- 2\.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar erişilemez hale gelecektir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktasını silindikten sonra 30 gün geçtiğinde veya Bulut uç noktası silinirse değil geri katmanlı dosyalar kullanılamaz.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya bir sunucu uç noktası konumu uygulama disk belleği dosyası depolamayın.
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken/MIR seçeneği dosya zaman damgası korumak için kullanın. Bu, eski dosyalar son erişilen dosyaların daha çabuk katmanlı garanti eder.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-5200"></a>Aracı sürümü 5.2.0.0
Aşağıdaki sürüm notları, 4 Nisan 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 5.2.0.0 geçerlidir. Bu Notlar 5.0.2.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Çevrimdışı veri aktarımı ve veri aktarımı için güvenilirlik geliştirmeleri özellikleri Sürdür
- Eşitleme telemetri geliştirmeleri

## <a name="agent-version-5100"></a>Aracı sürümü 5.1.0.0
Aşağıdaki sürüm notları 7 Mart 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 5.1.0.0 ' dir. Bu Notlar 5.0.2.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Dosyaları sunucu üzerinde değişiklik numaralandırma başarısız olduysa 0x80c8031d (ECS_E_CONCURRENCY_CHECK_FAILED) hata ile eşitlemek başarısız olabilir
- Eşitleme oturumu veya dosya 0x80072f78 (WININET_E_INVALID_SERVER_RESPONSE) hata alırsa, eşitleme işlemi şimdi yeniden dener
- Dosyaları eşitleme 0x80c80203 (ECS_E_SYNC_INVALID_STAGED_FILE) hatasıyla başarısız olabilir
- Yüksek bellek kullanımı, dosyaları koruyamadı oluşabilir.
- Bulut katmanlaması telemetri geliştirmeleri 

## <a name="agent-version-5020"></a>Aracı sürümü 5.0.2.0
Aşağıdaki sürüm notları (12 Şubat 2019'da yayımlanan) Azure dosya eşitleme aracısının sürümü için 5.0.2.0 ' dir.

### <a name="improvements-and-issues-that-are-fixed"></a>Geliştirmeler ve düzeltilen sorunlar

- Azure kamu bulutu için destek
  - Azure kamu bulutu için Önizleme desteği ekledik. Bu bir teknik listelenen abonelik ve Microsoft'tan bir özel aracı karşıdan gerektirir. Önizleme erişim elde etmek için lütfen doğrudan adresinden bize e-posta [ AzureFiles@microsoft.com ](mailto:AzureFiles@microsoft.com).
- Yinelenen verileri kaldırma desteği
    - Yinelenen verileri kaldırma işlemi artık Windows Server 2016 ve Windows Server 2019 üzerinde etkin katmanlama bulut ile tam olarak desteklenir. Bulut katmanlaması etkin bir birimde yinelenen verileri kaldırma etkinleştirme, daha fazla depolama alanı sağlama olmadan daha fazla dosyaları şirket içi önbellek olanak tanır.
- Çevrimdışı veri aktarımı (örneğin aracılığıyla Data Box) için destek
    - Azure dosya eşitleme ile büyük miktarda veriyi seçtiğiniz herhangi bir araçla kolayca geçirin. Azure Data Box, AzCopy ve hatta üçüncü taraf geçiş hizmetleri seçebilirsiniz. Hayır büyük kullanılacak miktarda bant genişliği, Azure Data Box – söz konusu olduğunda, verilerinizi almak istediğiniz yeterlidir, var. posta! Daha fazla bilgi için bkz. [çevrimdışı veri aktarımı Docs](https://aka.ms/AFS/OfflineDataTransfer).
- Geliştirilmiş eşitleme performansı
    - Müşteriler aynı birimde birden çok sunucu uç noktaları ile yavaş eşitleme performansı Bu sürümden önce yaşamış olabilirsiniz. Azure dosya eşitleme, günde bir kez açık tanıtıcıları içeren dosyaları eşitleyin sunucudaki geçici bir VSS anlık görüntüsü oluşturur. Eşitleme, VSS eşitleme oturumu etkin olduğunda, bir birimde eşitleniyor birden çok sunucu uç noktaları artık desteklemektedir. Daha fazla bekleyen bir VSS eşitleme hakkında diğer sunucu uç noktaları birim üzerindeki devam edebilmek için tamamlamak için oturum eşitleyin.
- Portalda izleme geliştirildi
    - Grafikleri görüntülemek için depolama eşitleme hizmeti portalda eklenmiştir:
        - Eşitlenen dosya sayısı
        - Aktarılan verilerin boyutu
        - Eşitleme değil dosya sayısı
        - Geri veri boyutu
        - Sunucu bağlantısı durumu
    - Daha fazla bilgi için bkz. [İzleyici Azure dosya eşitleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring).
- Geliştirilmiş ölçeklenebilirlik ve güvenilirlik
    - Dosya sistemi nesneleri (dizinleri ve dosyaları) bir dizinde en fazla sayısı 1.000.000 arttı. Önceki sınırı 200.000 oluştu.
    - Eşitleme büyük dosyalar için bir aktarım kesildiğinde yeniden göndermeden yerine veri aktarımı sürdürme dener 

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracıyı en az 2 GiB bellek gerektirir. Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.
- FIPS modunda desteklenmez ve devre dışı bırakılmalıdır. 

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Azure dosya eşitleme aracısının sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterler olan dosyalar. Bkz: [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) desteklenmeyen karakterler listesi.
- Dosyaları ya da bir nokta ile biten dizinleri.
- 2\.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar erişilemez hale gelecektir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktasını silindikten sonra 30 gün geçtiğinde veya Bulut uç noktası silinirse değil geri katmanlı dosyalar kullanılamaz.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya bir sunucu uç noktası konumu uygulama disk belleği dosyası depolamayın.
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken/MIR seçeneği dosya zaman damgası korumak için kullanın. Bu, eski dosyalar son erişilen dosyaların daha çabuk katmanlı garanti eder.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-4300"></a>Aracı sürümü 4.3.0.0
Aşağıdaki sürüm notları 14 Ocak 2019 yayımlanan Azure dosya eşitleme aracısının sürümü için 4.3.0.0 ' dir. Bu Notlar 4.0.1.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Dosyaları, Azure dosya eşitleme aracısının sürümüne yükselttikten sonra değil katmanlanır 4.x.
- AfsUpdater.exe üzerinde Windows Server 2019 artık desteklenmektedir.
- Eşitleme için çeşitli güvenilirlik geliştirmeleri. 

## <a name="agent-version-4200"></a>Aracı sürümü 4.2.0.0
Aşağıdaki sürüm notları, 10 Aralık 2018'e yayımlanan Azure dosya eşitleme aracısının sürümü için 4.2.0.0 geçerlidir. Bu Notlar 4.0.1.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Bir VSS anlık görüntü oluşturulduğunda, Durma hatası 0x3B veya durdurma Hatası 0x1E ortaya çıkabilir.  
- Bir bellek sızıntısı oluşabilir, bulut katmanlaması etkin  

## <a name="agent-version-4100"></a>Aracı sürümü 4.1.0.0
Aşağıdaki sürüm notları, yayın tarihi: 4 Aralık 2018 Azure dosya eşitleme aracısının sürümü için 4.1.0.0 geçerlidir. Bu Notlar 4.0.1.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:  
- Sunucu nedeniyle bulut katmanlandırma bellek sızıntısı yanıt vermeyi durdurabilir.  
- Aracı yüklemesi şu hata ile başarısız olur: 1921 hata oluştu. 'Depolama eşitleme Aracısı' (FileSyncSvc) hizmeti durdurulamadı.  Sistem hizmetlerini durdurmak için yeterli ayrıcalığınız olduğundan emin olun.  
- Depolama Eşitleme Aracı (FileSyncSvc) hizmeti, bellek kullanımı yüksek olduğunda kilitlenebilir.  
- Çeşitli güvenilirlik geliştirmeleri için eşitleme katmanlama ve bulut.

## <a name="agent-version-4010"></a>Aracı sürümü 4.0.1.0
Aşağıdaki sürüm notları (13 Kasım 2018 yayımlanan) Azure dosya eşitleme aracısının sürümü için 4.0.1.0 ' dir.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracıyı en az 2 GiB bellek gerektirir. Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.
- FIPS modunda desteklenmez ve devre dışı bırakılmalıdır. 
- Bir VSS anlık görüntü oluşturulduğunda, Durma hatası 0x3B veya durdurma Hatası 0x1E ortaya çıkabilir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Azure dosya eşitleme aracısının sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.
- Yinelenen verileri kaldırma ve bulut katmanlaması aynı birimde desteklenmez.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterler olan dosyalar. Bkz: [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) desteklenmeyen karakterler listesi.
- Dosyaları ya da bir nokta ile biten dizinleri.
- 2\.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar erişilemez hale gelecektir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktasını silindikten sonra 30 gün geçtiğinde veya Bulut uç noktası silinirse değil geri katmanlı dosyalar kullanılamaz.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya bir sunucu uç noktası konumu uygulama disk belleği dosyası depolamayın.
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Tarih temelli bulut katmanlaması ilke ayarı, belirtilen gün sayısı içinde eriştiyseniz önbelleğe alınması dosyaları belirtmek için kullanılır. Daha fazla bilgi için bkz. [bulut katmanlama genel bakış](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken/MIR seçeneği dosya zaman damgası korumak için kullanın. Bu, eski dosyalar son erişilen dosyaların daha çabuk katmanlı garanti eder.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-3300"></a>Aracı sürümü 3.3.0.0
Aşağıdaki sürüm notları, 24 Eylül 2018'de yayınlanan Azure dosya eşitleme aracısının sürümü için 3.3.0.0 geçerlidir. Bu Notlar 3.1.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürümde giderilen sorunlar listesi:
- Kayıtlı sunucu durum, "Azure dosya eşitleme Aracısı sürüm 3.1 veya 3.2 yükseltildikten sonra çevrimdışı görünüyor" şeklindedir.
- Depolama eşitleme Aracısı (FileSyncSvc) hizmeti nedeniyle uzun yollar dosyaları kilitleniyor.
- Sunucu kaydı hatasıyla başarısız oluyor: Dosya veya derleme Kailani.Afs.StorageSyncProtocol.V3 yüklenemedi.

## <a name="agent-version-3200"></a>Aracı sürüm 3.2.0.0
Aşağıdaki sürüm notları, 15 Ağustos 2018'de yayınlanan Azure dosya eşitleme aracısının sürümü için 3.2.0.0 geçerlidir. Bu Notlar 3.1.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürüm aşağıdaki düzeltmeyi içerir:
- Yetersiz bellek hatasını (0x8007000e) nedeniyle bellek sızıntısı ile eşitleme başarısız

## <a name="agent-version-3100"></a>Aracı sürümü 3.1.0.0
Aşağıdaki sürüm notları (19 Temmuz 2018'den yayımlanan) Azure dosya eşitleme aracısının sürümü için 3.1.0.0 ' dir.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.
- FIPS modunda desteklenmez ve devre dışı bırakılmalıdır. 

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) veya diğer dosya filtrelerini kullanmayın. Dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra aracı yükleme ve sunucu kaydı gerçekleşmelidir.
- Yinelenen verileri kaldırma ve bulut katmanlaması aynı birimde desteklenmez.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- 2\.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar kullanılamaz hale gelir.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
