---
title: Azure Dosya Eşitleme aracısı sürüm notları (önizleme) | Microsoft Docs
description: Azure Dosya Eşitleme aracısı sürüm notları (önizleme)
services: storage
documentationcenter: ''
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: fabfb02a199744c63c3a2191589fd450cfdd0304
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="release-notes-for-the-azure-file-sync-agent-preview"></a>Azure Dosya Eşitleme aracısı sürüm notları (önizleme)
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Sürüm | Sürüm tarihi | Destek bitiş tarihi |
|---------|--------------|------------------|
| 2.1.0.0 | 28 Şubat 2018 | Geçerli sürüm |
| 2.0.11.0 | 8 Şubat 2018 | Geçerli sürüm |
| 1.1.0.0 | 26 Eylül 2017 | 30 Temmuz 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

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
- Bir sunucudan tek seferde çok sayıda (10.000’den fazla) dizinin silinmesi eşitleme hatalarına neden olabilir. Dizinleri 10. 000'den küçük toplu işler halinde silin. Sonraki toplu işi silmeden önce silme işlemlerinin başarıyla eşitlendiğinden emin olun.
- Bu sürüm, bir birimin kökündeki eşitleme köküne yönelik destek ekler.
- Bir işletim sistemini veya uygulama disk belleğini sunucu uç noktasının içinde depolamayın.
- Bu yayındaki değişiklik: Bulut katmanlaması (EventID 9016), eşitleme karşıya yükleme durumu (EventID 9302) ve eşitlenmeyen dosyaların (EventID 9900) toplam çalışma zamanlarını izlemek için yeni olaylar eklendi.
- Bu yayındaki değişiklik: Hızlı DR ad alanı eşitleme performansı önemli ölçüde arttı.
 
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
