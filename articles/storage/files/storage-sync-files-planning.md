---
title: "Bir Azure dosya eşitleme (Önizleme) dağıtımı için planlama | Microsoft Docs"
description: "Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: d626f71aa21cea562ef6c9554c05e6de027e7f4d
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Bir Azure dosya eşitleme (Önizleme) dağıtımı için planlama
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu kılavuzda Azure dosya eşitleme dağıtırken dikkat edilmesi gerekenler açıklanmaktadır. Okumanız önerilir [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) test Kılavuzu. 

## <a name="understanding-azure-file-sync-terminology"></a>Azure dosya eşitleme terimleri anlama
Azure dosya eşitleme ayrıntılara gitmeden önce terminoloji anlamak önemlidir.

### <a name="storage-sync-service"></a>Depolama eşitleme hizmeti
Depolama eşitleme hizmeti, Azure dosya eşitleme temsil eden üst düzey Azure kaynaktır. Depolama hesabı kaynak eşi depolama eşitleme hizmeti kaynaktır ve similarily Azure Resouce gruplar halinde dağıtılabilir. Depolama hesabı kaynak farklı bir üst düzey kaynaktan gerekir çünkü depolama eşitleme hizmeti birden çok eşitleme grubu aracılığıyla birden çok depolama hesapları ile eşitleme ilişkisi oluşturabilirsiniz. Bir abonelik dağıtılan birden çok depolama eşitleme hizmeti kaynak olabilir.

### <a name="sync-group"></a>Eşitleme grubu
Eşitleme grubu bir Grup dosyası için eşitleme topolojisi tanımlar. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş olarak saklanacaktır. Örneğin, AFS ile yönetmek istediğiniz dosyaları iki ayrı kümesi varsa, iki eşitleme grubu oluşturun ve farklı uç noktalar her birine ekleyin. Bir depolama eşitleme hizmeti gerektiği kadar eşitleme grubu barındırabilir.  

### <a name="registered-server"></a>Kayıtlı sunucu
Kayıtlı sunucu nesnesi sunucunuz (veya küme) arasında bir güven ilişkisi temsil eder ve depolama eşitleme hizmeti. Bir depolama eşitleme hizmeti örneğine istediğiniz kadar sunucusu kaydedebilirsiniz. Ancak, bir sunucu (veya küme) yalnızca bir depolama eşitleme hizmeti ile herhangi bir anda kaydedilebilir.

### <a name="azure-file-sync-agent"></a>Azure dosya eşitleme aracı
Azure dosya eşitleme Aracısı, Windows Server'ın bir Azure dosyasıyla eşitlenecek bir hangi etkinleştirir paylaşmak indirilebilir bir pakettir. Azure dosya eşitleme Aracısı üç ana bileşenden oluşur: 
- **FileSyncSvc.exe**: arka plan Windows hizmeti sunucu uç noktalarda değişiklikleri izleme ve Azure Eşitleme oturumları başlatma sorumludur.
- **StorageSync.sys**: Azure dosya eşitleme dosya sistemi filtre, Azure dosyaları sorumlu katmanlama dosyaları (zaman bulut katmanlandırma etkin).
- **PowerShell Yönetimi cmdlet'leri**: Microsoft.StorageSync Azure kaynak sağlayıcısı ile etkileşim için PowerShell cmdlet'leri. Bunlar (varsayılan) aşağıdaki konumlarda bulunabilir:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Sunucusu uç noktası
Sunucusu uç noktası sunucusunda bir kayıtlı sunucu birim veya birim kök klasörü gibi belirli bir konumu temsil eder. Kendi ad alanları (örneğin F:\sync1 ve F:\sync2) çakışan, birden çok sunucu bitiş noktaları aynı birimde bulunabilir. Bulut katmanlama ilkeleri her sunucusu uç noktası için ayrı ayrı yapılandırabilirsiniz. Bir sunucu konumu ile var olan bir dosya sunucusu uç noktası eşitleme grubuna eklerseniz, bu dosyaları eşitleme grubundaki diğer uç nokta zaten bulunan diğer dosyaları ile birleştirilir.

### <a name="cloud-endpoint"></a>Bulut uç noktası
Bulut uç noktasına bir eşitleme grubunun parçası olan bir Azure dosya paylaşımıdır. Yalnızca tüm Azure dosya paylaşımı eşitlemeler ve bir Azure dosya paylaşımı bir bulut uç noktası ve bu nedenle, bir eşitleme grubunun üyesi olabilir. Bir Azure dosya paylaşımının dosyaları var olan bir bulut uç noktası olarak bir eşitleme grubuna eklerseniz, bu dosyaları eşitleme grubundaki diğer uç nokta zaten bulunan diğer dosyaları ile birleştirilir.

> [!Important]  
> Azure dosya değişiklikleri doğrudan paylaşmak azure dosya eşitleme destekler ancak unutmayın Azure dosya paylaşımında yapılan tüm değişiklikler önce her 24 saatte bir bulut uç noktası için yalnızca initatiated olan bir Azure dosya eşitleme değişiklik algılama işi tarafından bulunacak gerektiğini. Bkz: [Azure dosyaları ile ilgili SSS](storage-files-faq.md#afs-change-detection) daha fazla bilgi için.

### <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulut katmanlama hangi etkinleştirir sık kullanılan veya erişmek için Azure dosyaları katmanlı dosyaları Azure dosya eşitleme, isteğe bağlı bir özellik değil. Bir dosya katmanlı, Azure dosya eşitleme dosya sistemi (StorageSync.sys) değiştirir dosyanın yerel olarak işaretçiyle filtre uygulayabilir veya yeniden ayrıştırma noktası, bir URL Azure dosyaları dosyasında temsil eden. Katmanlı bir dosyanın üçüncü taraf uygulamalar katmanlı dosyaları tanımlayabilmeniz NTFS Ayarla "Çevrimdışı" özniteliği var. Bir kullanıcı bir katmanlı dosya açıldığında, Azure dosya eşitleme Azure dosyaları dosya verilerinden kullanıcının dosya bilmenize gerek olmadan sistemde depolanmadığını geri sorunsuz bir şekilde çağırır. Bu işlev hiyerarşik Depolama Yönetimi (HSM) de denir.

## <a name="azure-file-sync-interoperability"></a>Azure dosya eşitleme birlikte çalışabilirlik 
Bu bölüm, Windows Server özellikleri ve rolleri ve üçüncü taraf çözümlerle birlikte çalışabilirlik Azure dosya eşitleme kapsar.

### <a name="supported-versions-of-windows-server"></a>Windows Server'ın desteklenen sürümleri
Şu anda Azure dosya eşitleme tarafından Windows Server'ın desteklenen sürümleri şunlardır:

| Sürüm | Desteklenen SKU'ları | Desteklenen dağıtım seçenekleri |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi olan sunucu) |
| Windows Server 2012 R2 | Datacenter ve Standard | Tam (bir kullanıcı Arabirimi olan sunucu) |

Windows Server'ın gelecek sürümleri yayımlanır yayımlanmaz ve Windows'un eski sürümlerini kullanıcı geri bildirim göre eklenebilir eklenecektir.

> [!Important]  
> Tüm Windows Azure dosya eşitleme Windows Update'ten en son güncelleştirmeleri ile güncel ile kullanılan sunucuları tutma öneririz. 

### <a name="file-system-features"></a>Dosya sistemi özellikleri
| Özellik | Destek durumu | Notlar |
|---------|----------------|-------|
| Erişim denetim listeleri (ACL'ler) | Tam olarak desteklenir | Windows ACL Azure dosya eşitleme tarafından korunur ve Windows Server'da sunucu uç noktaları tarafından uygulanır. Windows ACL değil (henüz) doğrudan buluta erişilen dosyaları varsa Azure dosyaları tarafından desteklenir. |
| Sabit bağlantılar | Atlandı | |
| Sembolik bağlantılar | Atlandı | |
| Bağlama noktaları | Kısmen destekleniyor | Bağlama noktaları sunucusu uç noktası kökündeki olabilir, ancak sunucu uç noktanın ad alanında içeriyorsa atlanacak. |
| Kavşakları | Atlandı | |
| Yeniden ayrıştırma noktaları | Atlandı | |
| NTFS sıkıştırması | Tam olarak desteklenir | |
| Seyrek dosyalar | Tam olarak desteklenir | Seyrek dosyaların eşitlendiğini (engellenen değil), ancak bulut tam bir dosya olarak eşitleme yapar. Dosya içeriğini bulutta (veya başka bir sunucuda) değiştirirseniz, değişikliğin yüklendiğinde dosyası artık seyrek olarak olacaktır |
| Alternatif veri akışlarını (ADS) | Korunur, ancak eşitlendi | |

> [!Note]  
> Yalnızca NTFS birimleri desteklenir.

### <a name="failover-clustering"></a>Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi için "Genel kullanım için dosya sunucusu" dağıtım seçeneği Azure dosya eşitleme tarafından desteklenir. Yük Devretme Kümelemesi desteklenmiyor "Uygulama verileri için genişleme dosya sunucusu" (SOFS) ya da, kümelenmiş paylaşılan birimleri (CSV).

> [!Note]  
> Azure dosya eşitleme Aracısı düzgün çalışması için yük devretme kümesi eşitleme için kümedeki her düğüm yüklenmelidir.

### <a name="windows-server-data-deduplication"></a>Windows Server yinelenen verileri kaldırma
Etkin katmanlama bulut olmadan birimler için Azure dosya eşitleme birimde etkinleştiriliyor yinelenen verileri kaldırma destekler. Azure dosya eşitleme etkin katmanlama bulut ile ve şu anda yinelenen verileri kaldırma arasında birlikte desteklemez.

### <a name="anti-virus-solutions"></a>Virüsten koruma çözümleri
Virüsten koruma bilinen kötü amaçlı kod dosyaları tarayarak çalıştığından, virüsten koruma katmanlı dosyaların geri çağırma neden olabilir. Katmanlı dosyaları "Çevrimdışı" özniteliği kümesine sahip olduğundan, çevrimdışı dosyaları okuma atlamak için yazılım satıcınızla kendi çözümünü yapılandırma ile ilgili danışmanlık öneririz. 

Aşağıdaki çözümlerde, çevrimdışı dosyalar atlanıyor desteklemek için bilinmektedir:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee EndPoint Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (bkz: "yalnızca gerekenleri tara" bölümü sayfasında 90)
- [Kaspersky virüsten koruma](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Yedekleme çözümleri
Virüsten koruma çözümleri gibi yedekleme çözümleri katmanlı dosyaların geri çağırma neden olabilir. Bir şirket içi yedek ürün kullanmak yerine yedekleme Azure dosya paylaşımı için bir bulut yedekleme çözümü kullanarak öneririz.

### <a name="encryption-solutions"></a>Şifreleme çözümleri
Şifreleme çözümleri için destek nasıl uygulanana üzerinde bağlıdır. Azure dosya eşitleme çalışmak bilinmektedir:

- BitLocker şifrelemesi
- Azure RMS (ve eski Active Directory RMS)

Azure dosya eşitleme ile çalışmıyor bilinmektedir:

- NTFS dosya sistemi (EFS) şifreli

Genel olarak, Azure dosya eşitleme BitLocker ve dosya biçimi, BitLocker'ı gibi uygulanan çözümleri gibi dosya sistemi aşağıda sit şifreleme çözümleri ile birlikte çalışma görüyor olmalısınız ancak hiçbir özel birlikte çalışabilirliği sit çözümleriyle birlikte çalışabilirliği yaptı dosya sistemini (NTFS dosya sistemi şifrelenmiş gibi) yukarıda.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Diğer hiyerarşik Depolama Yönetimi (HSM) çözümleri
Başka bir hiyerarşik depolama yönetimi çözümü ile Azure dosya eşitleme kullanılmalıdır.

## <a name="region-availability"></a>Bölge kullanılabilirliği
Azure dosya eşitleme yalnızca önizleme aşağıdaki bölgelerde kullanılabilir:

| Bölge | Veri merkezinin konumu |
|--------|---------------------|
| Batı ABD | California, ABD |
| Batı Avrupa | Hollanda |
| Güneydoğu Asya | Singapur |
| Avustralya Doğu | Yeni Güney İngiltere, Avustralya |

Önizleme'de, yalnızca depolama eşitleme hizmeti ile aynı bölgede bir Azure dosya paylaşımı ile eşitleme destekliyoruz.

## <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure dosyaları dağıtımını planlama](storage-files-planning.md)
* [Azure dosyaları dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md)
