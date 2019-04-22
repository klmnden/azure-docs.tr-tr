---
title: Bir Azure dosya eşitleme dağıtımı için hazırlanma | Microsoft Docs
description: Bir Azure dosyaları dağıtımını planlarken dikkate almanız gerekenler öğrenin.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 2/7/2019
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: ad3b5a1d684c500eff3d20832d7aa290a13849b9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58918646"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Azure Dosya Eşitleme dağıtımı planlama
Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

Bu makalede Azure dosya eşitleme dağıtımı için önemli hususlar açıklanmaktadır. Ayrıca okumanızı öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md). 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-file-sync-terminology"></a>Azure dosya eşitleme terminolojisi
Bir Azure dosya eşitleme dağıtımı planlama ayrıntılarını almadan önce terminolojiyi anlamanız önemlidir.

### <a name="storage-sync-service"></a>Depolama Eşitleme Hizmeti
Depolama eşitleme hizmeti Azure dosya eşitleme için üst düzey Azure kaynağıdır. Depolama eşitleme hizmeti kaynak depolama hesabı kaynağı eşdüzeyde ve Azure kaynak grupları için benzer şekilde dağıtılabilir. Depolama hesabı kaynağı farklı bir üst düzey kaynaktan gerekir çünkü depolama eşitleme hizmeti birden çok eşitleme grupları aracılığıyla birden fazla depolama hesabı ile eşitleme ilişkisi oluşturabilirsiniz. Bir abonelikte dağıtılmış birden çok depolama eşitleme hizmeti kaynakları olabilir.

### <a name="sync-group"></a>Eşitleme grubu
Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Bir eşitleme grubu içindeki uç noktalar, birbiriyle eşitlenmiş durumda tutulur. Örneğin, iki ayrı Azure dosya eşitleme ile yönetmek istediğiniz dosyaları kümesi varsa, iki eşitleme grubu oluşturma ve farklı uç noktaları her eşitleme grubuna ekleyin. Depolama eşitleme hizmeti, ihtiyacınız kadar eşitleme grupları barındırabilirsiniz.  

### <a name="registered-server"></a>Kayıtlı sunucu
Sunucu (veya küme) arasında bir güven ilişkisi kayıtlı sunucu nesnesini temsil eder ve depolama eşitleme hizmeti. Depolama eşitleme hizmeti örneğine istediğiniz sayıda sunucusu kaydedebilirsiniz. Ancak, bir sunucu (veya küme) aynı anda yalnızca bir depolama eşitleme hizmeti ile kaydedilebilir.

### <a name="azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı
Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir. Azure dosya eşitleme aracısının üç ana bileşeni vardır: 
- **FileSyncSvc.exe**: Arka plan sunucusu uç noktalarda değişiklik izleme ve Azure oturumlarını eşitleme başlatma sorumlu Windows hizmeti.
- **StorageSync.sys**: Azure dosyaları'na katmanlama dosyaları için sorumlu olan Azure dosya eşitleme dosya sistemi filtresi (ne zaman bulut katmanlaması etkin).
- **PowerShell Yönetimi cmdlet'leri**: PowerShell cmdlet'leri Microsoft.StorageSync Azure kaynak sağlayıcısı ile etkileşim kurmak için kullanın. Bunlar aşağıdaki (varsayılan) konumda bulabilirsiniz:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Sunucu uç noktası
Sunucu uç noktası, bir sunucu birimi üzerindeki klasör gibi kayıtlı bir sunucu üzerindeki belirli bir noktayı temsil eder. Kullanıcıya ad alanlarını çakışmıyorsa birden çok sunucu uç noktaları aynı birimde mevcut olabilir (örneğin, `F:\sync1` ve `F:\sync2`). Bulut katmanlaması ilkeleri her sunucu uç noktası için ayrı ayrı yapılandırabilirsiniz. 

Sunucu uç noktası aracılığıyla bir takma noktası oluşturabilirsiniz. Sunucu uç noktasını içinde bağlama atlanır unutmayın.  

Sunucu uç noktası sistem biriminde oluşturabilirsiniz ancak bunu yaparsanız iki sınırlamalar uygulanır:
* Bulut katmanlaması etkinleştirilemez.
* (Burada sistem hızlı bir şekilde tüm ad boşluğunu getirir ve içerik geri çağırma başlar) hızlı ad alanı geri yükleme yapılamaz.


> [!Note]  
> Yalnızca taşınabilir olmayan birimlerin desteklenir.  Sunucu uç noktası yolu için uzak bir paylaşımdan eşlenen sürücüler desteklenmez.  Ayrıca, bir sunucu uç noktası bulunabilir ancak sistem birimi bulut Windows üzerinde katmanlama sistem birimi üzerinde desteklenmiyor.

Bir eşitleme grubuna sunucu uç noktası olarak varolan bir dosya kümesini içeren bir sunucu konumu eklerseniz, bu dosyaları diğer uç noktalardan eşitleme grubu zaten bulunan diğer dosyaları ile birleştirilir.

### <a name="cloud-endpoint"></a>Bulut uç noktası
Bulut uç noktasına bir eşitleme grubunun bir parçası olan bir Azure dosya paylaşımıdır. Tüm Azure dosya paylaşımı eşitler ve Azure dosya paylaşımının yalnızca bir bulut uç noktası üyesi olabilir. Bu nedenle, bir Azure dosya paylaşımı yalnızca bir eşitleme grubunun bir üyesi olabilir. Bulut uç noktasına bir eşitleme grubuna olarak var olan bir dosya kümesini içeren bir Azure dosya paylaşımı eklerseniz, diğer uç noktalardan eşitleme grubu zaten bulunan diğer dosyaları var olan dosyaları birleştirilir.

> [!Important]  
> Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımı üzerindeki değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunmaları gerekir. Bir değişiklik algılama işi bulut uç noktası için 24 saatte bir kere başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımı için yapılan değişiklikler son değiştirilme zamanı SMB güncelleştirmez ve bir değişiklik eşitleme tarafından görülmez. Daha fazla bilgi için [Azure sık sorulan sorular dosyaları](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulut katmanlaması olduğundan, sık erişilen dosyaları önbelleğe alınır yerel sunucuda tüm dosyaları Azure İlkesi ayarlarına göre dosyaları katmanlı sırasında Azure dosya eşitleme'nin isteğe bağlı bir özelliktir. Daha fazla bilgi için [anlama bulut Katmanlandırma](storage-sync-cloud-tiering.md).

## <a name="azure-file-sync-system-requirements-and-interoperability"></a>Azure dosya eşitleme sistem gereksinimleri ve birlikte çalışabilirlik 
Bu bölümde, Azure dosya eşitleme Aracısı sistem gereksinimleri ve Windows Server özelliklerini ve rollerini ve üçüncü taraf çözümlerle birlikte çalışabilirlik kapsar.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure dosya eşitleme dağıtmadan önce sisteminizin Azure dosya eşitleme Değerlendirme Aracı'nı kullanma ile uyumlu olduğunu değerlendirmelidir. Bu araç için dosya sistemini ve veri kümesi gibi desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü ile ilgili olası sorunları kontrol eden bir Azure PowerShell cmdlet'i kullanılır. Kendi denetimleri en kapsayan Not ancak tüm Aşağıda sözü edilen Özellikler; dağıtımınızın düzgün gider dikkatli bir şekilde sağlamak için bu bölümün geri kalanında okuma öneririz. 

#### <a name="download-instructions"></a>Yükleme yönergeleri
1. PackageManagement en son sürümüne sahip ve PowerShellGet yüklü olduğundan emin olun (Bu, Önizleme modülleri yüklemenize olanak tanır)
    
    ```powershell
        Install-Module -Name PackageManagement -Repository PSGallery -Force
        Install-Module -Name PowerShellGet -Repository PSGallery -Force
    ```
 
2. PowerShell yeniden Başlat
3. Modülleri yükleme
    
    ```powershell
        Install-Module -Name Az.StorageSync -AllowPrerelease -AllowClobber -Force
    ```

#### <a name="usage"></a>Kullanım  
Değerlendirme araç birkaç farklı yollarla çağırabilirsiniz: sistem denetimleri, veri kümesi denetimleri veya her ikisi de gerçekleştirebilirsiniz. Sistem ve veri kümesi denetimleri gerçekleştirmek için: 

```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Yalnızca Veri kümenizi test etmek için:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Yalnızca sistem gereksinimleri test etmek için:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
CSV sonuçları görüntülemek için:
```powershell
    $errors = Invoke-AzStorageSyncCompatibilityCheck […]
    $errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="system-requirements"></a>Sistem Gereksinimleri
- Windows Server 2012 R2, Windows Server 2016 veya Windows Server 2019 çalıştıran bir sunucu:

    | Sürüm | Desteklenen SKU'ları | Desteklenen dağıtım seçenekleri |
    |---------|----------------|------------------------------|
    | Windows Server 2019 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi ile sunucu) |
    | Windows Server 2016 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi ile sunucu) |
    | Windows Server 2012 R2 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi ile sunucu) |

    Windows Server'ın gelecek sürümlerinden yayınlandıkça eklenir. Önceki Windows sürümlerinde, kullanıcı geri bildirimleri temelinde eklenebilir.

    > [!Important]  
    > Windows Update'ten en son güncelleştirmeleri ile güncel Azure dosya eşitleme ile kullandığınız tüm sunucuları tutulması önerilir. 

- 2 GiB bellek en az bir sunucu.

    > [!Important]  
    > Sunucu, dinamik belleği etkin bir sanal makinede çalışıyorsa, sanal makine bellek ile bir en az 2048 MiB yapılandırılması gerekir.
    
- Yerel olarak bağlı bir birim NTFS dosya sistemiyle biçimlendirilmiş.

### <a name="file-system-features"></a>Dosya sistemi özellikleri

| Özellik | Destek durumu | Notlar |
|---------|----------------|-------|
| Erişim denetim listeleri (ACL'ler) | Tam olarak desteklenir | Windows ACL Azure dosya eşitleme tarafından korunur ve sunucu uç noktaları üzerinde Windows Server tarafından zorunlu tutulmaz. Windows ACL değil (henüz) dosyaları doğrudan bulutta eriştiyseniz Azure dosyaları tarafından desteklenmiyor. |
| Sabit bağlantılar | Atlandı | |
| Simgesel bağlantılar | Atlandı | |
| Bağlama noktaları | Kısmen destekleniyor | Bağlama noktaları kökünde sunucu uç noktası olabilir, ancak bir sunucu uç noktasının ad alanında içeriyorsa bunlar atlanır. |
| Merkezleriyle | Atlandı | Örneğin, dağıtılmış dosya sistemi DfrsrPrivate ve DFSRoots klasörler. |
| Yeniden ayrıştırma noktaları | Atlandı | |
| NTFS sıkıştırması | Tam olarak desteklenir | |
| Seyrek dosyalar | Tam olarak desteklenir | Seyrek dosya eşitleme (engellenmediğinden), ancak bunlar tam bir dosya olarak buluta eşitleyin. Dosya içeriği, bulutta (veya başka bir sunucuya) değiştirirseniz, değişikliğin indirildiğinde artık seyrek dosyasıdır. |
| Alternatif veri akışları (REKLAM) | Korunur, ancak eşitlenmedi | Örneğin, dosya sınıflandırma altyapısı tarafından oluşturulan sınıflandırma etiketleri eşitlenmedi. Her bir sunucu uç noktaları dosyalarda mevcut sınıflandırma etiketleri dokunulmadan kalır. |

> [!Note]  
> Yalnızca NTFS birimleri desteklenir. ReFS, FAT, FAT32 ve diğer dosya sistemleri desteklenmez.

### <a name="files-skipped"></a>Atlanan Dosyaları

| Dosya/klasör | Not |
|-|-|
| Desktop.ini | Dosya sistemine özgü |
| ethumbs.db$ | Küçük resimleri için geçici dosya |
| ~$\*.\* | Office geçici dosya |
| \*.tmp | Geçici dosya |
| \*.laccdb | Access DB dosyayı kilitleme|
| 635D02A9D91C401B97884B82B3BCDAEA.* | İç eşitleme dosya|
| \\Sistem birimi bilgileri | Birime özel klasörü |
| $RECYCLE. DEPO| Klasör |
| \\SyncShareState | Sync klasörü |

### <a name="failover-clustering"></a>Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi için "Genel kullanım için dosya sunucusu" dağıtım seçeneği Azure dosya eşitleme tarafından desteklenir. Yük Devretme Kümelemesi desteklenmiyor "Uygulama verileri için genişleme dosya sunucusunda" (SOFS) veya kümelenmiş paylaşılan birimleri (CSV).

> [!Note]  
> Her düğümde düzgün çalışması için bir yük devretme kümesinde eşitleme için Azure dosya eşitleme aracısının yüklenmesi gerekir.

### <a name="data-deduplication"></a>Yinelenen verileri kaldırma
**Aracı sürümü 5.0.2.0**   
Yinelenen verileri kaldırma, Windows Server 2016 ve Windows Server 2019 üzerinde etkin katmanlama bulutla birimlerde desteklenir. Bulut katmanlaması etkin bir birimde yinelenen verileri kaldırma etkinleştirme, daha fazla depolama alanı sağlama olmadan daha fazla dosyaları şirket içi önbellek olanak tanır.

**Windows Server 2012 R2 veya daha eski Aracı sürümleri**  
Bulut katmanlaması etkin olmayan birimler için Windows Server yinelenen verileri birimde etkinleştiriliyor kaldırma Azure dosya eşitleme destekler.

### <a name="distributed-file-system-dfs"></a>Dağıtılmış dosya sistemi (DFS)
Azure dosya eşitleme DFS ad alanları (DFS-N) ve DFS Çoğaltma (DFS-R) ile birlikte çalışabilirliği destekler.

**DFS ad alanları (DFS-N)**: Azure dosya eşitleme DFS-N sunucularına tam olarak desteklenir. Bir veya daha fazla DFS-N üye sunucu uç noktaları ve bulut uç noktası arasında verileri eşitleme için Azure dosya eşitleme aracısını yükleyebilirsiniz. Daha fazla bilgi için [DFS ad alanlarına genel bakış](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS Çoğaltma (DFS-R)**: Çoğu durumda, hem de çoğaltma çözümleri DFS-R ve Azure dosya eşitleme olduğundan DFS-R Azure dosya eşitleme ile değiştirerek öneririz. DFS-R ve Azure dosya eşitleme birkaç senaryo istediğiniz birlikte kullanabilirsiniz ancak vardır:

- Azure dosya eşitleme dağıtımı için DFS-R dağıtımdan geçiriliyor. Daha fazla bilgi için [DFS Çoğaltma (DFS-R) dağıtımı için Azure dosya eşitleme geçirme](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Dosya verilerinizin bir kopyasının gereken her şirket içi sunucu doğrudan internet'e bağlanabilir.
- Dal sunucuları, Azure dosya eşitleme kullanmak istediğiniz bir hub'ın tek sunucuya verileri birleştirin.

Azure dosya eşitleme ve DFS-R için yan yana çalışmak:

1. Azure dosya eşitleme bulut katmanlama gereken devre dışı bırakılacak DFS-R çoğaltılmış klasörleri içeren birimlerde.
2. Sunucu uç noktaları DFS-R Salt okunur çoğaltma klasörlerde yapılandırılmamalıdır.

Daha fazla bilgi için [DFS Çoğaltmaya genel bakış](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
Sysprep kullanarak Azure dosya eşitleme aracısının yüklü olduğu bir sunucuda desteklenmez ve beklenmeyen sonuçlara neden olabilir. Sunucu görüntüsünü dağıtma ve sysprep mini kurulumu tamamladıktan sonra aracı yükleme ve sunucu kaydı gerçekleşmelidir.

### <a name="windows-search"></a>Windows Search
Bulut katmanlaması bir sunucu uç noktasında etkinleştirildiğinde, katmanlı halde bulunan dosyaları atlandı ve tarafından Windows Search dizin değil. Düzgün olmayan katmanlı dosyaları dizine eklenir.

### <a name="antivirus-solutions"></a>Virüsten koruma çözümleri
Bilinen kötü amaçlı kod için dosyaları tarama tarafından virüsten koruma çalıştığı için bir virüsten koruma ürününüzün katmanlı dosyalar geri çağırma bildirimi yayımlayabiliriz neden olabilir. Sürümlerinde 4.0 ve Azure dosya eşitleme aracısının, yukarıda güvenli Windows öznitelik FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS kümesi katmanlı dosyalar sahip. Bu öznitelik kümesi (çoğu otomatik olarak bunu) okuma dosyalarıyla atlamak için çözümün nasıl yapılandırılacağını öğrenmek için yazılım satıcınıza danışmanlık öneririz.

Microsoft'un şirket içi virüsten koruma çözümleri, Windows Defender ve System Center Endpoint Protection (SCEP) hem de otomatik olarak ayarlanmış bu özniteliğe sahip okuma dosyaları atla. Biz bunları test ve küçük bir sorun belirledik: var olan bir eşitleme grubuna bir sunucu eklediğinizde, 800 bayt (yeni sunucuda indirilen) çekilir küçük dosyaları. Bu dosyalar yeni sunucuda kalır ve katmanlama boyut gereksinimini karşılamayan beri katmanlanmış olmaz değil (> 64kb).

### <a name="backup-solutions"></a>Yedekleme çözümleri
Virüsten koruma çözümleri gibi yedekleme çözümleri, katmanlı dosyalar geri çağırma bildirimi yayımlayabiliriz neden olabilir. Bulut yedekleme çözümü yerine şirket içi yedekleme ürün Azure dosya paylaşımını yedekleme kullanmanızı öneririz.

Şirket içi yedekleme çözümü kullanıyorsanız, yedeklemeler eşitleme grubundaki bulut katmanlamasını devre dışı olan bir sunucuda gerçekleştirilmelidir. Bir geri yükleme gerçekleştirirken, birim düzeyinde veya dosya düzeyinde geri yükleme seçenekleri kullanın. Eşitleme grubundaki tüm uç noktalar için dosyaları dosya düzeyinde geri yükleme seçeneğini kullanarak geri eşitlenir ve mevcut dosyaları yedeklemeden geri sürümle değiştirilir.  Azure dosya paylaşımını veya diğer sunucu uç noktaları yeni dosya sürümlerinde birim düzeyinde geri yüklemeler yerini almaz.

> [!Note]  
> Tam kurtarma (BMR) geri yükleme beklenmeyen sonuçlara neden olabilir ve şu anda desteklenmiyor.

> [!Note]  
> Bulut katmanlaması etkin olan birimlerde VSS anlık görüntülerini (önceki sürümler sekmesini dahil) şu anda desteklenmemektedir. Bulut katmanlaması etkin olduğunda, bir dosyayı yedekten geri yüklemek için Azure dosya paylaşımı anlık görüntüleri kullanın.

### <a name="encryption-solutions"></a>Şifreleme çözümleri
Şifreleme çözümleri için destek nasıl uygulanana üzerinde bağlıdır. Azure dosya eşitleme ile çalışma bilinmektedir:

- BitLocker şifrelemesi
- Azure Information Protection, Azure Rights Management Services'i (Azure RMS) ve Active Directory RMS

Azure dosya eşitleme ile çalışmıyor bilinmektedir:

- NTFS dosya sistemi (EFS) şifreli

Genel olarak, Azure dosya eşitleme BitLocker gibi dosya sistemi aşağıda sit şifreleme çözümleri ve Azure Information Protection gibi dosya biçiminde uygulanan çözümleri ile birlikte çalışabilirlik desteklemelidir. Dosya sistemi (NTFS EFS gibi) yukarıda sit çözümleri için özel hiçbir birlikte çalışabilirlik yapıldı.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Diğer hiyerarşik Depolama Yönetimi (HSM) çözümleri
Diğer bir HSM çözümleri, Azure dosya eşitleme ile kullanılmalıdır.

## <a name="region-availability"></a>Bölge kullanılabilirliği
Azure dosya eşitleme yalnızca şu bölgelerde kullanılabilir:

| Bölge | Veri merkezi konumu |
|--------|---------------------|
| Avustralya Doğu | New South Wales |
| Avustralya Güneydoğu | Victoria |
| Güney Brezilya | Sao Paolo durumu |
| Orta Kanada | Toronto |
| Doğu Kanada | Quebec City |
| Orta Hindistan | Pune |
| Orta ABD | Iowa |
| Doğu Asya | Hong Kong |
| Doğu ABD | Virginia |
| Doğu ABD 2 | Virginia |
| Japonya Doğu | Tokyo, Saitama |
| Japonya Batı | Osaka |
| Orta Kuzey ABD | Illinois |
| Kuzey Avrupa | İrlanda |
| Orta Güney ABD | Texas |
| Güney Hindistan | Chennai |
| Güneydoğu Asya | Singapur |
| Birleşik Krallık Güney | Londra |
| Birleşik Krallık Batı | Cardiff |
| Batı Avrupa | Hollanda |
| Batı ABD | Kaliforniya |

Azure dosya eşitleme depolama eşitleme hizmeti ile aynı bölgede olan bir Azure dosya paylaşımı ile eşitlemeyi destekler.

### <a name="azure-disaster-recovery"></a>Azure olağanüstü durum kurtarma
Bir Azure bölgesi kaybına karşı korumak için Azure dosya eşitleme ile entegre olur [coğrafi olarak yedekli depolama yedekliliği](../common/storage-redundancy-grs.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (GRS) seçeneği. GRS depolama çoğaltmasını birincil bölgedeki normal şekilde etkileşime, depolama ve depolama arasında zaman uyumsuz engelle eşleştirilmiş ikincil bölge'de faydalanır. Geçici veya kalıcı olarak çevrimdışı yapılacak Azure bölgesini neden olağanüstü bir durumda, Microsoft tarafından eşleştirilen bölgeye yük devretme depolama. 

> [!Warning]  
> Azure dosya paylaşımınızı GRS depolama hesabındaki bir bulut uç noktası olarak kullanıyorsanız, depolama hesabı yük devretme başlatma olmamalıdır. Bunun yapılması ayrıca çalışma ve Mayıs durdurmak için neden Eşitleme beklenmeyen veri kaybı durumunda yeni katmanlı dosyalar neden olur. Bir Azure bölgesi kaybı söz konusu olduğunda, Microsoft Azure dosya eşitleme ile uyumlu bir şekilde depolama hesabı yük devretmeyi tetikler.

Coğrafi olarak yedekli depolama ve Azure dosya eşitleme arasında yük devretme tümleştirmeyi desteklemek için tüm Azure dosya eşitleme bölgeler depolaması tarafından kullanılan ikincil bölgeye eşleşen bir ikincil bölge ile'eşlenirler. Bu çiftler aşağıdaki gibidir:

| Birincil bölge      | Eşleştirilmiş bölge      |
|---------------------|--------------------|
| Avustralya Doğu      | Avustralya Güneydoğu |
| Avustralya Güneydoğu | Avustralya Doğu     |
| Orta Kanada      | Doğu Kanada        |
| Doğu Kanada         | Orta Kanada     |
| Orta Hindistan       | Güney Hindistan        |
| Orta ABD          | Doğu ABD 2          |
| Doğu Asya           | Güneydoğu Asya     |
| Doğu ABD             | Batı ABD            |
| Doğu ABD 2           | Orta ABD         |
| Kore Orta       | Kore Güney        |
| Kore Güney         | Kore Orta      |
| Kuzey Avrupa        | Batı Avrupa        |
| Orta Kuzey ABD    | Orta Güney ABD   |
| Güney Hindistan         | Orta Hindistan      |
| Güneydoğu Asya      | Doğu Asya          |
| Birleşik Krallık Güney            | Birleşik Krallık Batı            |
| Birleşik Krallık Batı             | Birleşik Krallık Güney           |
| Batı Avrupa         | Kuzey Avrupa       |
| Batı ABD             | Doğu ABD            |

## <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik Duvarı ve Ara sunucu ayarlarını göz önünde bulundurun.](storage-sync-files-firewall-and-proxy.md)
* [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
* [Azure dosyaları'nı dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
* [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
