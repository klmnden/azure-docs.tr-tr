---
title: Azure dosya eşitleme Aracısı sürüm notları | Microsoft Docs
description: Azure dosya eşitleme Aracısı sürüm notları.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 1/14/2019
ms.author: wgries
ms.component: files
ms.openlocfilehash: 006a8172faae529ce9943773552da325bfae3f4b
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321543"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı sürüm notları
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Kilometre Taşı | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| Güncelleştirme paketi - Ocak 2019 [KB4481059](https://support.microsoft.com/help/4481059)| 4.3.0.0 | 14 Ocak 2019 | Desteklenen (önerilen sürüm) |
| Aralık 2018'e güncelleştirme paketi - [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10 Aralık 2018'e | Desteklenen |
| Aralık 2018'e güncelleştirme paketi | 4.1.0.0 | 4 Aralık 2018'e | Desteklenen |
| V4 sürüm | 4.0.1.0 | 13 Kasım 2018 | Desteklenen |
| Eylül 2018'e güncelleştirme paketi | 3.3.0.0 | 24 Eylül 2018 | Desteklenen |
| Ağustos 2018 güncelleştirme paketi | 3.2.0.0 | 15 Ağustos 2018 | Desteklenen |
| Genel kullanılabilirlik | 3.1.0.0 | 19 Temmuz 2018 | Desteklenen |
| Süresi dolmuş olan aracıları | 1.1.0.0 - 3.0.13.0 | Yok | Desteklenmiyor - Aracı sürümleri 1 Ekim 2018 tarihinde süresi doldu |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

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
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracıyı en az 2 GiB bellek gerektirir. Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.
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
- 2.048 karakterden uzun yollar.
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
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez. Portalı'nda sunucu adını güncelleştirmek için kaydını silin ve sunucuyu yeniden kaydedin.

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
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) Planlama Kılavuzu'nda bölümü. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (Yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) veya diğer dosya filtrelerini kullanmayın. Dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep, Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalıştırmak desteklenmez ve beklenmeyen sonuçlara neden olabilir. Sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra aracı yükleme ve sunucu kaydı gerçekleşmelidir.
- Yinelenen verileri kaldırma ve bulut katmanlaması aynı birimde desteklenmez.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- 2.048 karakterden uzun yollar.
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
- Sunucunun yeniden adlandırılması durumunda Portalı'nda sunucu adını güncelleştirilmez. Portalı'nda sunucu adını güncelleştirmek için kaydını silin ve sunucuyu yeniden kaydedin.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama iş her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
