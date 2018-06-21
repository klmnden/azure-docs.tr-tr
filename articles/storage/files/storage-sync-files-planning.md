---
title: Bir Azure dosya eşitleme (Önizleme) dağıtımı için planlama | Microsoft Docs
description: Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 81b760e3a911bacb9c01106d59577d794788abe8
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296179"
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Bir Azure dosya eşitleme (Önizleme) dağıtımı için planlama
Esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun tanırken kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmek için Azure dosya eşitleme (Önizleme) kullanın. Azure dosya eşitleme, Windows Server Hızlı Azure dosya paylaşımınıza önbelleğine dönüştürür. SMB ve NFS FTPS çeşitli verilerinize yerel olarak erişmek için Windows Server üzerinde kullanılabilir herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gerektiği kadar önbellekleri olabilir.

Bu makalede, Azure dosya eşitleme dağıtımı için önemli noktalar açıklanır. Ayrıca okumanızı öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md). 

## <a name="azure-file-sync-terminology"></a>Azure dosya eşitleme terminolojisi
Bir Azure dosya eşitleme dağıtımını planlama ayrıntılarını almadan önce terminoloji anlamak önemlidir.

### <a name="storage-sync-service"></a>Depolama Eşitleme Hizmeti
Depolama eşitleme Azure dosya eşitleme için üst düzey Azure kaynağı hizmetidir. Depolama eşitleme hizmeti kaynak eş depolama hesabı kaynağının ve Azure kaynak gruplarını benzer şekilde dağıtılabilir. Depolama hesabı kaynağı farklı bir üst düzey kaynaktan gerekir çünkü depolama eşitleme hizmeti birden çok eşitleme grubu aracılığıyla birden çok depolama hesapları ile eşitleme ilişkisi oluşturabilirsiniz. Bir abonelik dağıtılan birden çok depolama eşitleme hizmeti kaynak olabilir.

### <a name="sync-group"></a>Eşitleme grubu
Eşitleme grubu bir Grup dosyası için eşitleme topolojisi tanımlar. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş tutulur. Örneğin, Azure dosya eşitleme ile yönetmek istediğiniz dosyaları iki ayrı kümesi varsa, iki eşitleme grubu oluşturun ve farklı uç noktaları her eşitleme grubuna eklemeniz. Bir depolama eşitleme hizmeti gerektiği kadar eşitleme grubu barındırabilir.  

### <a name="registered-server"></a>Kayıtlı sunucu
Kayıtlı sunucu nesnesi sunucunuz (veya küme) arasında bir güven ilişkisi temsil eder ve depolama eşitleme hizmeti. Bir depolama eşitleme hizmeti örneğine istediğiniz kadar sunucusu kaydedebilirsiniz. Ancak, bir sunucu (veya küme) aynı anda yalnızca bir depolama eşitleme hizmeti ile kaydedilebilir.

### <a name="azure-file-sync-agent"></a>Azure dosya eşitleme aracı
Azure dosya eşitleme Aracısı'nı Windows Server'ın bir Azure dosya paylaşımı ile senkronize sağlayan indirilebilir bir pakettir. Azure dosya eşitleme Aracısı üç ana bileşeni vardır: 
- **FileSyncSvc.exe**: arka plan sunucu uç noktalarda değişiklikleri izleme ve Eşitleme oturumları Azure başlatmaktan sorumludur Windows hizmeti.
- **StorageSync.sys**: Azure dosyaları katmanlama dosyaları için sorumlu Azure dosya eşitleme dosya sistemi filtresi (zaman bulut katmanlandırma etkindir).
- **PowerShell Yönetimi cmdlet'leri**: Microsoft.StorageSync Azure kaynak sağlayıcısı ile etkileşim kurmak için kullanın PowerShell cmdlet'leri. Bunlar aşağıdaki (varsayılan) konumlarda bulabilirsiniz:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Sunucusu uç noktası
Sunucusu uç noktası kayıtlı bir sunucuda, bir sunucu birimdeki bir klasörü gibi belirli bir konuma temsil eder. Kendi ad alanları çakışmıyorsa birden çok sunucu bitiş noktaları aynı birimde bulunabilir (örneğin, `F:\sync1` ve `F:\sync2`). Bulut katmanlama ilkeleri her sunucusu uç noktası için ayrı ayrı yapılandırabilirsiniz. 

Sunucusu uç noktası aracılığıyla bir başlatma noktası oluşturabilirsiniz. Not: bağlama sunucusu uç noktası içinde atlandı.  

Sistem biriminde sunucusu uç noktası oluşturabilirsiniz ancak bunu yaparsanız iki kısıtlamalar şunlardır:
* Bulut katmanlandırma etkinleştirilemez.
* (Burada sistem hızlı bir şekilde tüm ad alanı getirir ve içeriği geri çağırma başlar) hızlı ad alanı geri yükleme gerçekleştirilmez.


> [!Note]  
> Yalnızca çıkarılamaz birimleri desteklenir.  Uzak bir paylaşımdan eşlenen sürücüler için bir sunucu bitiş noktası yolu desteklenmez.  Ayrıca, bir sunucu uç noktası bulunabilir sistem birimi rağmen bulut Windows'ta katmanlama sistem biriminde desteklenmiyor.

Var olan bir dosya sunucusu uç noktası bir eşitleme grubuna sahip bir sunucu konumu eklerseniz, bu dosyaları eşitleme grubundaki diğer uç nokta zaten bulunan diğer dosyaları ile birleştirilir.

### <a name="cloud-endpoint"></a>Bulut uç noktası
Bulut uç noktasına bir eşitleme grubunun parçası olan bir Azure dosya paylaşımıdır. Tüm Azure dosya paylaşımı eşitlemeler ve Azure dosya paylaşımının yalnızca bir bulut uç noktası üyesi olabilir. Bu nedenle, Azure dosya paylaşımının yalnızca bir eşitleme grubunun bir üyesi olabilir. Dosyaları var olan bir bulut uç noktasına bir eşitleme grubuna sahip bir Azure dosya paylaşımı eklerseniz, var olan dosyaları eşitleme grubundaki diğer uç nokta zaten bulunan diğer dosyaları ile birleştirilir.

> [!Important]  
> Azure dosya eşitleme değişiklikleri Azure dosya paylaşımına doğrudan destekler. Ancak, Azure dosya paylaşımında yapılan değişiklikler ilk Azure dosya eşitleme değişiklik algılama işi tarafından bulunması gerekir. Bir değişiklik algılama işi yalnızca bir kez her 24 saatte bir bulut uç noktası için başlatılır. Ayrıca, bir Azure dosya paylaşımına REST protokolü üzerinden yapılan değişiklikler son değişiklik zamanını SMB güncelleştirilmez ve değişiklik olarak eşitlemeden görünmez. Daha fazla bilgi için bkz: [Azure sık sorulan sorular dosyaları](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulut katmanlandırma isteğe bağlı özellik Azure dosya eşitleme, sık kullanılan ya da dosya boyutu 64 KiB Azure dosyaları katmanlı büyük erişilebilir. Bir dosya katmanlı, Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasını yerel olarak bir işaretçi veya yeniden ayrıştırma noktası ile değiştirir. Yeniden ayrıştırma noktası Azure dosyaları dosyaya bir URL temsil eder. Bir katmanlı dosyası üçüncü taraf uygulamalar katmanlı dosyaları tanımlayabilmeniz NTFS "Çevrimdışı" özniteliği vardır. Bir kullanıcı bir katmanlı dosya açıldığında, Azure dosya eşitleme sorunsuz bir şekilde Azure dosyaları dosya verilerinden kullanıcının dosya sisteminde yerel olarak depolanmaz bilmenize gerek olmadan geri çeker. Bu işlev hiyerarşik Depolama Yönetimi (HSM) de denir.

> [!Important]  
> Bulut katmanlandırma desteklenmiyor Windows sistemi birimlerinde sunucu uç noktalar için.

## <a name="azure-file-sync-interoperability"></a>Azure dosya eşitleme birlikte çalışabilirlik 
Bu bölüm, Windows Server özellikleri ve rolleri ve üçüncü taraf çözümleri ile birlikte çalışabilirlik Azure dosya eşitleme kapsar.

### <a name="supported-versions-of-windows-server"></a>Windows Server'ın desteklenen sürümleri
Şu anda, Azure dosya eşitleme tarafından Windows Server'ın desteklenen sürümleri şunlardır:

| Sürüm | Desteklenen SKU'ları | Desteklenen dağıtım seçenekleri |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi olan sunucu) |
| Windows Server 2012 R2 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi olan sunucu) |

Windows Server'ın gelecek sürümleri yayımlanır yayımlanmaz eklenecektir. Windows'un önceki sürümlerinde, kullanıcı geri bildirim göre eklenebilir.

> [!Important]  
> Azure dosya eşitleme Windows Update'ten en son güncelleştirmeleri ile güncel ile kullandığınız tüm sunucuların tutma öneririz. 

### <a name="file-system-features"></a>Dosya sistemi özellikleri
| Özellik | Destek durumu | Notlar |
|---------|----------------|-------|
| Erişim denetim listeleri (ACL'ler) | Tam olarak desteklenir | Windows ACL Azure dosya eşitleme tarafından korunur ve Windows Server tarafından sunucu uç noktalarda uygulanır. Windows ACL değil (henüz) doğrudan buluta erişilen dosyaları varsa Azure dosyaları tarafından desteklenir. |
| Sabit bağlantılar | Atlandı | |
| Sembolik bağlantılar | Atlandı | |
| Bağlama noktaları | Kısmen destekleniyor | Bağlama noktaları sunucusu uç noktası kökündeki olabilir, ancak bir sunucu uç noktanın ad alanında içeriyorsa atlanır. |
| Kavşakları | Atlandı | Örneğin, dağıtılmış dosya sistemi DfrsrPrivate ve DFSRoots klasörler. |
| Yeniden ayrıştırma noktaları | Atlandı | |
| NTFS sıkıştırması | Tam olarak desteklenir | |
| Seyrek dosyalar | Tam olarak desteklenir | Seyrek dosyalar eşitleme (engellenmez), ancak bir tam dosya olarak buluta eşitlemenin. Dosya içeriğini bulutta (veya başka bir sunucuda) değiştirirseniz, değişikliğin yüklendiğinde artık seyrek dosyasıdır. |
| Alternatif veri akışlarını (ADS) | Korunur, ancak eşitlendi | Örneğin, dosya sınıflandırma altyapısı tarafından oluşturulan sınıflandırma etiketlerini eşitlenmedi. Dosyaları her sunucu uç noktaları var olan sınıflandırma etiketlerini dokunulmadan kalır. |

> [!Note]  
> Yalnızca NTFS birimleri desteklenir. ReFS, FAT, FAT32 ve diğer dosya sistemleri desteklenmez.

### <a name="files-skipped"></a>Atlanan Dosyaları
| Dosya/klasör | Not |
|-|-|
| Desktop.ini | Dosya sistemine özgü |
| ethumbs.db$ | Küçük resimleri için geçici dosya |
| ~$\*.\* | Office geçici dosya |
| \*.tmp | Geçici dosya |
| \*.laccdb | Dosyayı kilitleme erişim DB|
| 635D02A9D91C401B97884B82B3BCDAEA.* | İç eşitleme dosya|
| \\Sistem birimi bilgileri | Birime belirli klasör |
| $RECYCLE. DEPO| Klasör |
| \\SyncShareState | Eşitleme için klasör |

### <a name="failover-clustering"></a>Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi için "Genel kullanım için dosya sunucusu" dağıtım seçeneği Azure dosya eşitleme tarafından desteklenir. Yük Devretme Kümelemesi desteklenmiyor "Uygulama verileri için genişleme dosya sunucusu" (SOFS) veya kümelenmiş paylaşılan Birimler'in (CSV).

> [!Note]  
> Azure dosya eşitleme Aracısı düzgün çalışması için yük devretme kümesi eşitleme için kümedeki her düğüm yüklenmelidir.

### <a name="data-deduplication"></a>Yinelenen verileri kaldırma
Bulut etkin katmanlama yok birimler için Windows Server yinelenen verileri birimde etkinleştiriliyor kaldırma Azure dosya eşitleme destekler. Şu anda etkin katmanlama bulut ile Azure dosya eşitleme ve yinelenen verileri kaldırma birlikte çalışabilirliği desteklemiyoruz.

### <a name="distributed-file-system-dfs"></a>Dağıtılmış dosya sistemi (DFS)
Azure dosya eşitleme destekleyen DFS ad alanlarını (DFS-N) ve DFS Çoğaltma (DFS-R) ile başlayan birlikte çalışma [Azure dosya eşitleme Aracısı 1.2](https://go.microsoft.com/fwlink/?linkid=864522).

**DFS ad alanlarını (DFS-N)**: Azure dosya eşitleme DFS-N sunucularında tam olarak desteklenir. Bir veya daha fazla DFS-N üye sunucu uç noktaları ve bulut uç noktası arasında eşitleme verileri için Azure dosya eşitleme Aracısı'nı yükleyebilirsiniz. Daha fazla bilgi için bkz: [DFS ad alanları genel bakış](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS Çoğaltma (DFS-R)**: bu yana DFS-R ile Azure dosya eşitleme hem de çoğaltma çözümler, çoğu durumda, DFS-R ile Azure dosya eşitleme değiştirme öneririz. Ancak, istediğiniz birkaç senaryo DFS-R ve Azure dosya eşitleme birlikte kullanmak vardır:

- Bir Azure dosya eşitleme dağıtımına bir DFS-R dağıtımından geçirdiğiniz. Daha fazla bilgi için bkz: [DFS Çoğaltma (DFS-R) dağıtımı için Azure dosya eşitleme geçirmek](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Dosya verilerinin bir kopyasını gereken her şirket içi sunucu doğrudan internet'e bağlı.
- Şube sunucuları Azure dosya eşitleme kullanmak istediğiniz tek bir hub sunucusu, oturum verilerini bir araya.

Azure dosya eşitleme ve DFS-R için yan yana çalışmak:

1. Azure dosya eşitleme bulut katmanı olmalıdır DFS-R çoğaltılmış klasörler birimlerle tarihinde devre dışı bırakılacak.
2. Sunucu uç noktaları DFS-R Salt okunur çoğaltma klasörlerde yapılandırılmamalıdır.

Daha fazla bilgi için bkz: [DFS Çoğaltmaya genel bakış](https://technet.microsoft.com/library/jj127250).

### <a name="windows-search"></a>Windows arama
Bulut katmanlama bir sunucu uç noktasında etkinleştirildiğinde, musunuz dosya atlandı ve Windows Search tarafından dizinli değil. Olmayan katmanlı dosyaları düzgün bir şekilde dizine alınır.

### <a name="antivirus-solutions"></a>Virüsten koruma çözümleri
Virüsten koruma bilinen kötü amaçlı kod dosyaları tarayarak çalıştığından, bir virüsten koruma yazılımı katmanlı dosyaların geri çağırma neden olabilir. Katmanlı dosyaları "Çevrimdışı" özniteliği olmadığından, çevrimdışı dosyaları okuma atlamak için kendi çözümünü yapılandırma konusunda bilgi edinmek için yazılım satıcınıza danışmanlık öneririz. 

Aşağıdaki çözümlerde, çevrimdışı dosyalar atlanıyor desteklemek için bilinmektedir:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee EndPoint Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) ("Tarama yalnızca gerekenleri" PDF sayfa 90 bakın)
- [Kaspersky virüsten koruma](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Yedekleme çözümleri
Virüsten koruma çözümleri gibi yedekleme çözümleri katmanlı dosyaların geri çağırma neden olabilir. Bir şirket içi yedek ürün yerine Azure dosya paylaşımı yedeklemek için bir bulut yedekleme çözümünün kullanmanızı öneririz.

Bir şirket içi yedekleme çözümü kullanıyorsanız, yedeklemeleri devre dışı katmanlama bulut olan eşitleme grubundaki bir sunucuda yapılmalıdır. Dosya sunucusu uç noktası konum içinde geri yüklerken dosya düzeyinde geri yükleme seçeneğini kullanın. Geri yüklenen dosyaları eşitleme grubundaki tüm uç noktalara eşitlenir ve mevcut dosyaları yedekten geri sürüm ile değiştirilecek.

> [!Note]  
> Uygulama algılayan birim düzeyi ve tam kurtarma (BMR) geri yükleme seçenekleri beklenmeyen sonuçlara neden olabilir ve şu anda desteklenmiyor. Bu geri yükleme seçenekleri gelecekteki bir sürümde desteklenir.

### <a name="encryption-solutions"></a>Şifreleme çözümleri
Şifreleme çözümleri için destek nasıl uygulanana üzerinde bağlıdır. Azure dosya eşitleme çalışmak bilinmektedir:

- BitLocker şifrelemesi
- Azure Information Protection, Azure Rights Management Services (Azure RMS) ve Active Directory RMS

Azure dosya eşitleme ile çalışmıyor bilinmektedir:

- NTFS dosya sistemi (EFS) şifreli

Genel olarak, Azure dosya eşitleme BitLocker'ı gibi dosya sistemi aşağıda sit şifreleme çözümleri ve BitLocker gibi dosya biçiminde uygulanan çözümleri ile birlikte çalışabilirlik desteklemelidir. Dosya sistemini (NTFS EFS gibi) yukarıda sit çözümleri için hiçbir özel birlikte çalışabilirlik yapıldı.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Diğer hiyerarşik Depolama Yönetimi (HSM) çözümleri
Diğer bir HSM çözümleri Azure dosya eşitleme ile kullanılmalıdır.

## <a name="region-availability"></a>Bölge kullanılabilirliği
Azure dosya eşitleme yalnızca önizleme aşağıdaki bölgelerde kullanılabilir:

| Bölge | Veri merkezinin konumu |
|--------|---------------------|
| Avustralya Doğu | New South Wales |
| Avustralya Güneydoğu | Victoria |
| Orta Kanada | Toronto |
| Doğu Kanada | Quebec City |
| Orta ABD | Iowa |
| Doğu Asya | Hong Kong |
| Doğu ABD | Virginia |
| Doğu US2 | Virginia |
| Kuzey Avrupa | İrlanda |
| Güneydoğu Asya | Singapur |
| Birleşik Krallık Güney | Londra |
| Birleşik Krallık Batı | Cardiff |
| Batı Avrupa | Hollanda |
| Batı ABD | Kaliforniya |

Önizleme'de depolama eşitleme hizmeti ile aynı bölgede olan bir Azure dosya paylaşımı ile eşitleme destekler.

## <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik Duvarı ve proxy ayarlarını göz önünde bulundurun](storage-sync-files-firewall-and-proxy.md)
* [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
* [Azure dosyaları dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md)
