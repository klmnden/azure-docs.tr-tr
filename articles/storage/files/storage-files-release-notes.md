---
title: Azure Dosya Eşitleme aracısı sürüm notları (önizleme) | Microsoft Docs
description: Azure dosya eşitleme Aracı (Önizleme) için sürüm notları.
services: storage
author: wmgries
manager: aungoo
ms.service: storage
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: c1ca8146db8c5d67be53ba4e30d8ab0218aca104
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128767"
---
# <a name="release-notes-for-the-azure-file-sync-agent-preview"></a>Azure Dosya Eşitleme aracısı sürüm notları (önizleme)
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Kilometre Taşı | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| Haziran güncelleştirme paketi | 3.0.13.0 | 29 Haziran 2018 | (Önerilen) sürümünü destekliyor |
| 2 Yenile | 3.0.12.0 | 22 Mayıs 2018 | Desteklenen |
| Nisan güncelleştirme paketi | 2.3.0.0 | 8 Mayıs 2018 | Desteklenen |
| Mart güncelleştirme paketi | 2.2.0.0 | 12 Mart 2018 | Desteklenen |
| Şubat güncelleştirme paketi | 2.1.0.0 | 28 Şubat 2018 | Desteklenen |
| 1 Yenile | 2.0.11.0 | 8 Şubat 2018 | Desteklenen |
| Ocak güncelleştirme paketi | 1.4.0.0 | 8 Ocak 2018 | Desteklenen |
| Kasım güncelleştirme paketi | 1.3.0.0 | 30 Kasım 2017 | Desteklenen |
| Ekim güncelleştirme paketi | 1.2.0.0 | 31 Ekim 2017 | Desteklenen |
| İlk önizleme yayını | 1.1.0.0 | 26 Eylül 2017 | Desteklenen |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-30130"></a>Aracı sürümü 3.0.13.0
Aşağıdaki sürüm notları 29 Haziran 2018 yayımlanan sürümü Azure dosya eşitleme aracısı için 3.0.13.0 ' dir. 3.0.12.0 sürümü için listelenen sürüm notları yanı sıra bu notlar var.

Bu sürüm aşağıdaki düzeltmeyi içerir:
- Bir sunucu, var olan eşitleme grubuna eklenirse, eşitleme başarısız olursa yeniden ayrıştırma noktalarını sunucudaki sunucu uç nokta konumda mevcut.

## <a name="agent-version-30120"></a>Aracı sürümü 3.0.12.0
Aşağıdaki sürüm notları (22 Mayıs 2018) sürümünden itibaren Azure dosya eşitleme aracısı için 3.0.12.0 ' dir.

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server'a yüklemek ve yapılandırmak için bkz. [Azure Dosya Eşitleme (önizleme) dağıtımı için hazırlanma](storage-sync-files-planning.md) ve [Azure Dosya Eşitleme (önizleme) aracısını dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketini yükseltilmiş (Yönetici) izinleriyle yüklü olması gerekir.
- Aracı Windows Server Core veya Nano Server dağıtım seçenekleri desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.
- Depolama Eşitleme Aracı (FileSyncSvc) hizmetini sıkıştırılmış sistem birimi bilgileri (SVI) dizini olan bir birimde bulunan sunucu uç noktalarını desteklemiyor. Bu yapılandırma, beklenmeyen sonuçlara neden.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [Azure Dosya Eşitleme (önizleme) ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md).
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) veya diğer dosya filtrelerini kullanmayın. Dosya filtreleri, dosyalar dosya filtresi nedeniyle engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Sysprep Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda çalışan desteklenmez ve beklenmeyen sonuçlara yol açabilir. Aracı yükleme ve sunucu kaydı sunucu görüntüsü dağıtmak ve sysprep mini kurulum tamamlandıktan sonra olmalıdır.
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
- Bulut katmanlandırma sistem biriminde desteklenmiyor. Sistem biriminde sunucusu uç noktası oluşturmak için bulut sunucusu uç noktası oluşturulurken katmanlama devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
- Dosya sunucusu uç noktası silmeden önce çekilir değil katmanlı dosyaları kullanılamaz olur.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-2300"></a>Aracı sürümü 2.3.0.0
Aşağıdaki sürüm notları 8 Mayıs 2018 yayımlanan sürümü Azure dosya eşitleme aracısı için 2.3.0.0 ' dir. Bu notlar 2.0.11.0 sürümü için listelenen sürüm notlarına ek niteliğindedir.

Bu sürüm aşağıdaki düzeltmeleri içerir:
- Aracı güncelleştirmeleri bulut katmanlama filtre sürücüsü olmayan bellekten askıda kalabilir.
- Çok sayıda dosya eşitlerken eşitleme performansı düşürebilir.

## <a name="agent-version-2200"></a>Aracı sürümü 2.2.0.0
Aşağıdaki sürüm notları 12 Mart 2018 yayımlanan sürümü Azure dosya eşitleme aracısı için 2.2.0.0 ' dir.  Sürüm 2.1.0.0 ve 2.0.11.0 listelenen sürüm notları yanı sıra bu notlar olan

V2.1.0.0 yükleme bazı müşteriler için değil durdurma FileSyncSvc nedeniyle başarısız olur. Sorun bu güncelleştirme giderir.

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
Azure Dosya Eşitleme aracısını Windows Server'a yüklemek ve yapılandırmak için bkz. [Azure Dosya Eşitleme (önizleme) dağıtımı için hazırlanma](storage-sync-files-planning.md) ve [Azure Dosya Eşitleme (önizleme) aracısını dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin (MSI) yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve Windows Server 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [Azure Dosya Eşitleme (önizleme) ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md).
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
- Bu sürümde geliştirilmiş: 
- Hızlı DR ad eşitleme performansı önemli ölçüde artar.
- Dizinleri çok sayıda (üzerinde 10.000) silme v2 * ile yığınlardaki yapılması gerekmez.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Bu sürümde yapılan değişiklik: Katmanlama ilkesi ayarına bağlı olarak yeni dosyalar 1 saat içinde (daha önce 32 saatti) katmanlanır. Talep üzerine bir PowerShell cmdlet'i sunuyoruz. Cmdlet’i kullanarak, arka plan işlemini beklemeden katmanlamayı daha verimli bir şekilde değerlendirebilirsiniz.
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
- Bu sürümdeki değişiklik: Dosyanın yeni veya zaten katmanlı olması şartıyla, dosyalar artık diğer sunucularda katmanlı dosya olarak indirilir.

## <a name="agent-version-1100"></a>Aracı sürümü 1.1.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 1.1.0.0 sürümü için geçerlidir (ilk önizleme yayın tarihi: 9 Eylül 2017). 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server'a yüklemek ve yapılandırmak için bkz. [Azure Dosya Eşitleme (önizleme) dağıtımı için hazırlanma](storage-sync-files-planning.md) ve [Azure Dosya Eşitleme (önizleme) aracısını dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin (MSI) yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2016 ve 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [Azure Dosya Eşitleme (önizleme) ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md).
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
