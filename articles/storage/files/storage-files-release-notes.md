---
title: Azure dosya eşitleme Aracısı sürüm notları | Microsoft Docs
description: Azure dosya eşitleme Aracısı sürüm notları.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 08/30/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: cc1b89ff94b4d4dc0b191512b110521d5fa05a7a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344375"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı sürüm notları
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Kilometre Taşı | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| Ağustos güncelleştirme paketi | 3.2.0.0 | 15 Ağustos 2018 | Desteklenen (önerilen sürüm) |
| Genel kullanılabilirlik | 3.1.0.0 | 19 Temmuz 2018 | Desteklenen |
| Haziran güncelleştirme paketi | 3.0.13.0 | 29 Haziran 2018'e | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| 2 Yenile | 3.0.12.0 | 22 Mayıs 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Nisan güncelleştirme paketi | 2.3.0.0 | 8 Mayıs 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Mart güncelleştirme paketi | 2.2.0.0 | 12 Mart 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Şubat güncelleştirme paketi | 2.1.0.0 | 28 Şubat 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| 1 Yenile | 2.0.11.0 | 8 Şubat 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Ocak güncelleştirme paketi | 1.4.0.0 | 8 Ocak 2018 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Kasım güncelleştirme paketi | 1.3.0.0 | 30 Kasım 2017 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| Ekim güncelleştirme paketi | 1.2.0.0 | 31 Ekim 2017 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |
| İlk önizleme yayını | 1.1.0.0 | 26 Eylül 2017 | Aracı sürümü 1 Ekim 2018 tarihinde sona erecek |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-3200"></a>Aracı sürüm 3.2.0.0
Aşağıdaki sürüm notları, 15 Ağustos 2018'de yayınlanan Azure dosya eşitleme aracısının sürümü için 3.2.0.0 geçerlidir. Bu Notlar 3.1.0.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürüm aşağıdaki düzeltmeyi içerir:
- Yetersiz bellek hatasını (0x8007000e) nedeniyle bellek sızıntısı ile eşitleme başarısız

## <a name="agent-version-3100"></a>Aracı sürümü 3.1.0.0
Aşağıdaki sürüm notları (19 Temmuz 2018'den yayımlanan) Azure dosya eşitleme aracısının sürümü için 3.1.0.0 ' dir.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç, dosya sistemi ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları denetleyen bir AzureRM PowerShell cmdlet'i kullanılır. Yükleme ve kullanım yönergeleri için bkz. [Değerlendirme Aracı](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-planning#evaluation-tool) Planlama Kılavuzu'nda bölümü. 

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
- Depolama eşitleme hizmeti ve/veya depolama hesabını bir farklı kaynak grubuna veya aboneliğe taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-30130"></a>Aracı sürümü 3.0.13.0
Aşağıdaki sürüm notları, 29 Haziran 2018'de yayınlanan Azure dosya eşitleme aracısının sürümü için 3.0.13.0 geçerlidir. Bu Notlar 3.0.12.0 sürümü için listelenen sürüm notlarına ek olarak var.

Bu sürüm aşağıdaki düzeltmeyi içerir:
- Eşitleme başarısız olursa bir sunucu var olan bir eşitleme grubuna eklenirse, yeniden ayrıştırma noktalarını sunucudaki sunucu uç noktası konumda mevcut.

## <a name="agent-version-30120"></a>Aracı sürümü 3.0.12.0
Aşağıdaki sürüm notları (yayımlanma tarihi 22 Mayıs 2018) Azure dosya eşitleme aracısının sürümü için 3.0.12.0 ' dir.

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
 
### <a name="server-endpoints"></a>Sunucu uç noktaları
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Bulut katmanlaması sistem birimi üzerinde desteklenmiyor. Sunucu uç noktası sistem biriminde oluşturmak için sunucu uç noktası oluşturulurken katmanlama Bulut devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
- Dosyalar sunucu uç noktasını silmeden önce çekilir değil, katmanlı dosyalar kullanılamaz hale gelir.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-2300"></a>Aracı sürüm 2.3.0.0
Aşağıdaki sürüm notları, yayımlanma tarihi 8 Mayıs 2018 Azure dosya eşitleme aracısının sürümü için 2.3.0.0 geçerlidir. Bu notlar 2.0.11.0 sürümü için listelenen sürüm notlarına ek niteliğindedir.

Bu sürüm aşağıdaki düzeltmeleri içerir:
- Bulut katmanlaması filtre sürücüsü olmayan bellekten aracı güncelleştirmeleri askıda kalabilir.
- Çok sayıda dosya eşitlerken eşitleme performansı düşürebilir.

## <a name="agent-version-2200"></a>Aracı sürümü 2.2.0.0
Aşağıdaki sürüm notları, 12 Mart 2018'de yayınlanan Azure dosya eşitleme aracısının sürümü için 2.2.0.0 geçerlidir.  Bu notlardır 2.1.0.0 ve 2.0.11.0 sürümü için listelenen sürüm notlarına ek olarak

Bazı müşteriler için v2.1.0.0 yüklemesi değil durdurma FileSyncSvc nedeniyle başarısız olur. Sorun bu güncelleştirme düzeltmeleri.

## <a name="agent-version-2100"></a>Aracı sürümü 2.1.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 28 Şubat 2018’de yayınlanan 2.1.0.0 sürümü için geçerlidir. Bu notlar 2.0.11.0 sürümü için listelenen sürüm notlarına ek niteliğindedir.

Bu yayın aşağıdaki değişiklikleri içerir:
- Küme yük devretme işlemesinde geliştirmeler.
- Katmanlı dosyaların güvenilir işlemesi iyileştirmeler.
- Bir Windows Server 2008 R2 etki alanı ortamına eklenen etki alanı denetleyicisi makinelerinde aracıyı yükleme desteği.
- Bu sürümdeki düzeltmeler: çok fazla dosya içeren sunucularda aşırı tanılama oluşturulması.
- Oturum hatalarında hata işleme için iyileştirmeler.
- Dosya aktarımı sorunlarında hata işleme için iyileştirmeler.
- Bu sürümdeki değişiklikler: Bir sunucu uç noktasında etkinleştirildiğinde bulut katmanı çalıştırmak için varsayılan aralık artık 1 saattir. 
- Geçici engelleme sorunu: Azure Dosya Eşitleme (Depolama Eşitleme Hizmeti) kaynaklarını yeni bir Azure aboneliğine taşıma.

## <a name="agent-version-20110"></a>Aracı sürümü 2.0.11.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 9 Şubat 2018’de yayınlanan 2.0.11.0 sürümü için geçerlidir. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin (MSI) yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- Bu yayın DFS-R desteği ekler. Daha fazla bilgi için bkz. [Planlama kılavuzu](storage-sync-files-planning.md#distributed-file-system-dfs).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) veya diğer dosya filtrelerini kullanmayın. Dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Kayıtlı Sunucuların çoğaltılması (VM kopyalama dahil) beklenmeyen sonuçlara yol açabilir. Özellikle, eşitleme hiçbir zaman yakınsamayabilir.
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
 
### <a name="server-endpoints"></a>Sunucu uç noktaları
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sunucu uç noktası, sistem birimi üzerinde olamaz. Örneğin, C:\MyFolder bir bağlama noktasıysa C:\MyFolder kabul edilebilir bir yol değildir.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bu sürüm, bir birimin kökündeki eşitleme köküne yönelik destek ekler.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
- Bu yayındaki değişiklik: Bulut katmanlaması (EventID 9016), eşitleme karşıya yükleme durumu (EventID 9302) ve eşitlenmeyen dosyaların (EventID 9900) toplam çalışma zamanlarını izlemek için yeni olaylar eklendi.
- Bu sürümde geliştirildi: 
- Hızlı DR ad alanı eşitleme performansı önemli ölçüde arttı.
- V2 * ile gruplar halinde yapılacak dizinleri çok sayıda (üzerinde 10.000) silinmesi gerekmez.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Bu sürümde yapılan değişiklik: Katmanlama ilkesi ayarına bağlı olarak yeni dosyalar 1 saat içinde (daha önce 32 saatti) katmanlanır. Talep üzerine bir PowerShell cmdlet'i sunuyoruz. Cmdlet’i kullanarak, arka plan işlemini beklemeden katmanlamayı daha verimli bir şekilde değerlendirebilirsiniz.
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
- Bu sürümdeki değişiklik: Dosyanın yeni veya zaten katmanlı olması şartıyla, dosyalar artık diğer sunucularda katmanlı dosya olarak indirilir.

## <a name="agent-version-1100"></a>Aracı sürümü 1.1.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 1.1.0.0 sürümü için geçerlidir (ilk önizleme yayın tarihi: 9 Eylül 2017). 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Yükleme ve Azure dosya eşitleme aracısını Windows Server ile yapılandırma hakkında daha fazla bilgi için bkz. [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin (MSI) yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md).
- FSRM veya diğer dosya filtrelerini kullanmayın. Dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Kayıtlı Sunucuların çoğaltılması (VM kopyalama dahil) beklenmeyen sonuçlara yol açabilir. Özellikle, eşitleme hiçbir zaman yakınsamayabilir.
- Yinelenen verileri kaldırma ve bulut katmanlaması aynı birimde desteklenmez.
 
### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- 2.048 karakterden uzun yollar.
- 2 KB'den büyükse bir güvenlik tanımlayıcısının DACL kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla ACE olduğunda geçerlidir.)
- Güvenlik tanımlayıcısının denetim için kullanılan SACL bölümü.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar. 
    
    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoints"></a>Sunucu uç noktaları
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sunucu uç noktası, sistem birimi üzerinde olamaz. Örneğin, C:\MyFolder bir bağlama noktasıysa C:\MyFolder kabul edilebilir bir yol değildir.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir ve CSV’lerle desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir sunucudan tek seferde çok sayıda (10.000’den fazla) dizinin silinmesi eşitleme hatalarına neden olabilir. Dizinleri 10. 000'den küçük toplu işler halinde silin. Sonraki toplu işi silmeden önce silme işlemlerinin başarıyla eşitlendiğinden emin olun.
- Bir birimin kökünde sunucu uç noktası şu anda desteklenmemektedir.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Dosyaların doğru şekilde geri çağrılabildiğinden emin olmak için sistem yeni veya değiştirilmiş dosyaları 32 saate kadar katmanlamayabilir. Bu işlem yeni bir sunucu uç noktası yapılandırıldıktan sonra ilk kez katmanlamayı içerir. Talep üzerine bir PowerShell cmdlet'i sunuyoruz. Cmdlet’i kullanarak, arka plan işlemini beklemeden katmanlamayı daha verimli bir şekilde değerlendirebilirsiniz.
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
