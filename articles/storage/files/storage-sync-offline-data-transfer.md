---
title: Azure dosya eşitleme ile çevrimdışı alımı için Data Box ve diğer yöntemleri kullanın.
description: İşlem ve eşitleme uyumlu toplu geçişi etkinleştirmek için en iyi uygulamaları destekler.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.component: files
ms.openlocfilehash: a184e0563d1ad26671c38cabe07f42d97cbe2885
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213624"
---
# <a name="migrate-to-azure-file-sync"></a>Azure dosya eşitleme geçirme
Azure dosya eşitleme için taşımak için birkaç seçenek vardır:

### <a name="uploading-files-via-azure-file-sync"></a>Azure dosya eşitleme ile dosya karşıya yükleme
Dosyalarınızı yerel olarak bir Windows Server 2012 R2 veya sonraki bir sürüme taşıyın ve Azure dosya eşitleme aracısını yüklemek için en basit seçenek var. Eşitleme yapılandırıldıktan sonra dosyalarınızı sunucudan yükleyeceksiniz. Biz şu anda bir ortalama karşıya yükleme hızınızı tüm müşterilerimizin hakkında 2 her gün 1 TB arasında görüyorsanız.
Göz önünde bulundurun bir [bant genişliği azaltma zamanlama](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter) sunucunuzun iyi vatandaşı veri merkezinizdeki olduğundan emin olun.

### <a name="offline-bulk-transfer"></a>Çevrimdışı toplu aktarımı
Karşıya yükleme kesinlikle en basit seçenek olmakla birlikte, kullanılabilir bant genişliğinizi dosyalarınızı Azure makul bir sürede eşitlemek için izin vermez, bu sizin için işe yaramayabilir. Burada üstesinden gelmek için tam kümesini dosyaları ilk eşitlemesi zorluktur. Daha sonra genellikle daha az bant genişliği kullanan ad alanında oluşunca Azure dosya eşitleme yalnızca değişiklikleri taşıyın.
Bu ilk karşıya yükleme zorluk, Azure gibi çevrimdışı toplu Geçiş Araçları üstesinden gelme [Data Box ailesi](https://azure.microsoft.com/services/storage/databox) kullanılabilir. Aşağıdaki makalede bir Azure dosya eşitleme uyumlu şekilde çevrimdışı geçiş işleminden yararlanmak için izlemeniz gereken işlemi ele alınmaktadır. Bu, çakışma dosyaları ve eşitleme etkinleştirdikten sonra dosya ve klasör ACL'leri ve zaman damgaları korumak yardımcı olacak en iyi yöntemler açıklanmıştır.

### <a name="online-migration-tools"></a>Çevrimiçi Geçiş Araçları
Aşağıda açıklanan işlemi yalnızca Data Box'a çalışmaz. Herhangi bir çevrimdışı Geçiş Aracı (örneğin, Data Box) veya AzCopy, Robocopy veya üçüncü taraf araçları ve Hizmetleri gibi çevrimiçi araçları için çalışır. İlk yükleme zorluğun üstesinden gelmek için kullandığınız yöntemden bağımsız olarak, bir eşitleme'deki bu araçları kullanmak için aşağıda açıklanan adımları önemli, bu nedenle uyumlu şekilde.


## <a name="offline-data-transfer-benefits"></a>Çevrimdışı veri aktarımı avantajları
Data Box'ı kullanırken çevrimdışı geçiş başlıca avantajları şunlardır:

- Dosyalar, veri kutusu gibi bir çevrimdışı toplu aktarım işlemi aracılığıyla Azure'a geçirirken sunucunuza tüm dosyaları ağ üzerinden karşıya yükleme gerekmez. Büyük ad alanları için bu ağ bant genişliğine ve zaman içinde önemli ölçüde tasarruf anlamına gelebilir.
- Azure dosya eşitleme kullanın ve ardından ulaşım bağımsız olarak kullanılan (Data Box, Azure içeri aktarma, vb.), Canlı sunucunuzun yalnızca verileri azure'a sevk bu yana değişmiş olan dosyaları karşıya yükler.
- Çevrimdışı toplu geçiş ürün ACL'leri aktarım değil olsa bile, dosya ve klasör ACL'leri de - eşitlenen azure dosya eşitleme sağlar.
- Azure Data Box'ı ve Azure dosya eşitleme kullanırken sıfır kapalı kalma süresi yoktur. Verileri Azure'a aktarmak için Data Box'ı kullanarak dosyayı uygunluğundan korurken verimli ağ bant genişliği kullanımını sağlar. Ayrıca, ad alanı güncel Data Box gönderildikten sonra değiştirilen dosyaları karşıya yükleyerek tutar.

## <a name="plan-your-offline-data-transfer"></a>Çevrimdışı veri aktarımınız planlama
Başlamadan önce aşağıdaki bilgileri gözden geçirin:

- Bir veya birden çok Azure dosya paylaşımlarını Azure dosya eşitleme ile eşitleme etkinleştirilmeden önce toplu geçişinizi tamamlayın.
- Data Box için geçişiniz toplu kullanmayı planlıyorsanız: Gözden geçirme [Data Box için dağıtım önkoşulları](../../databox/data-box-deploy-ordered.md#prerequisites).
- Son Azure dosya eşitleme topolojinizi planlama: [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- Eşitlemek istediğiniz dosya paylaşımlarını barındıracak bir Azure depolama hesabı seçin. Toplu geçişinizi geçici hazırlama paylaşımları aynı depolama hesapları yapıldığından emin olun. Toplu geçiş, yalnızca bir son - ve aynı depolama hesabında yer alan bir hazırlama-share yararlanarak etkinleştirilebilir.
- Bir toplu geçiş, yalnızca bir sunucu konumu ile yeni bir eşitleme ilişkisinin oluşturduğunuzda yararlanılabilir. Mevcut bir eşitleme ilişkisi olan bir toplu geçiş etkinleştirilemiyor.

## <a name="offline-data-transfer-process"></a>Çevrimdışı veri aktarım işlemi
Bu bölümde, Azure dosya eşitleme ' Azure Data Box gibi toplu Geçiş Araçları ile uyumlu bir şekilde ayarlama işlemi açıklanmaktadır.

![Ayrıca aşağıdaki paragrafta ayrıntılı açıklanmıştır işlem adımları Görselleştirme](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Adım | Ayrıntı |
|---|---------------------------------------------------------------------------------------|
| ![İşlem adımı 1](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Data Box'ınızı sipariş](../../databox/data-box-deploy-ordered.md). Vardır [Data Box ailesi içinde birkaç teklifleri](https://azure.microsoft.com/services/storage/databox/data) ihtiyaçlarınıza uyum sağlayacak. Data Box'ınızı almak ve Data Box'ı izleyin [verilerinizi kopyalamak için belgeleri](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box). Veri Data box'taki bu UNC yoluna kopyalandığından emin olun: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>` burada `ShareName` hazırlama paylaşım adıdır. Data Box Azure'a geri gönderin. |
| ![İşlem 2. adım](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Geçici hazırlama paylaşımları olarak belirlenen Azure dosya paylaşımlarını dosyalarınızı görünmesini bekleyin. **Bu paylaşımlar eşitleme etkinleştirmeyin!** |
| ![İşlem adımı 3](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Boş Data Box oluşturduğunuz her bir dosya paylaşımı için yeni bir paylaşım oluşturun. Bu yeni paylaşıma Data Box paylaşımı ile aynı depolama hesabındaki olduğundan emin olun. [Yeni bir Azure dosya paylaşımı oluşturma işlemini](storage-how-to-create-file-share.md). |
| ![İşlem 4. adım](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Bir eşitleme grubu oluşturma](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) depolama eşitleme hizmeti ve bir bulut uç noktası olarak boş paylaşım başvurusu. Her Data Box dosya paylaşımı için bu adımı yineleyin. Gözden geçirme [dağıtma Azure dosya eşitleme](storage-sync-files-deployment-guide.md) Kılavuzu ve Azure dosya eşitleme ' için gerekli adımları izleyin. |
| ![İşlem 5. adım](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Sunucu uç noktası Canlı sunucusuna dizininize eklemek](storage-sync-files-deployment-guide.md#create-a-server-endpoint). Dosyalar zaten Azure'a taşıdık işlemde belirtin ve hazırlama paylaşımları başvurun. Bu, etkinleştirme veya devre dışı gerektiği şekilde katmanlama bulut sizin tercihinize bağlıdır. Canlı sunucunuzda bir sunucu uç noktası oluşturulurken hazırlama paylaşımı başvurmanız gerekir. Yeni Sunucu uç noktası dikey penceresinde "Çevrimdışı veri aktarımı" (aşağıdaki görüntüde) etkinleştirmek ve bulut uç noktası ile aynı depolama hesabındaki bulunmalıdır hazırlama paylaşım başvurun. Kullanılabilir paylaşımları, depolama hesabı ve değil zaten eşitleme paylaşımları tarafından filtrelenir. |

![Çevrimdışı veri aktarımı, yeni bir sunucu uç noktası oluşturulurken etkinleştirmek için Azure portal kullanıcı arabirimini görselleştirme.](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Eşitleme paylaşımı
Sunucu uç noktanızı oluşturulduktan sonra eşitleme ettiğiniz günde başlatılır. Eşitleme sunucusunda bulunan her dosya için dosya paylaşımında hazırlama Data Box dosyaları burada borç olarak varsa ve bu nedenle, eşitleme dosya sunucudan karşıya yerine hazırlama paylaşımı kopyalar belirler. Dosya Paylaşımı hazırlama yok veya yerel sunucuda daha yeni bir sürümü kullanılabilir, eşitleme dosyanın yerel sunucudan yükleyin.

> [!IMPORTANT]
> Sunucu uç noktası oluşturma sırasında yalnızca toplu yükseltme modu etkinleştirebilirsiniz. Sunucu uç noktası kurulduktan sonra şu anda toplu tümleştirmek için hiçbir yolu yoktur ad alanına zaten eşitleme bir sunucudan veri geçişi.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL ve zaman damgaları dosyaları ve klasörleri
Azure dosya eşitleme bile kullanılan toplu geçiş aracı ACL'leri başlangıçta aktarım değil dosya ve klasör ACL'leri Canlı sunucusundan eşitlenen sağlayacaktır. Bu dosyalar ve klasörler üzerindeki herhangi bir ACL içermiyor için hazırlama paylaşımı Tamam olduğu anlamına gelir. Yeni bir sunucu uç noktası oluşturma, çevrimdışı veri geçiş özelliğini etkinleştirdiğinizde, o anda sunucusundaki tüm dosyaları için ACL'leri eşitlenecektir. Aynı oluşturma - ve değiştiren-zaman damgaları için geçerlidir.

## <a name="shape-of-the-namespace"></a>Ad alanının şekli
Ad alanı şeklini eşitleme etkinleştirildiğinde sunucuda nedir tarafından belirlenir. Data Box sonra silinen dosyaları yerel sunucuda varsa "-anlık görüntü" ve - geçiş ve ardından bu dosyalar olmaz canlı, eşitleme ad alanına. Bunlar, hazırlama paylaşımda olmaya devam eder ancak hiçbir zaman kopyalanır. Eşitleme ad alanı Canlı sunucusuna göre tutar, istenen davranışı aynıdır. Data Box "snapshot" yalnızca hazırlama bir zemin verimli dosya kopyalama ve dinamik ad alanı şekli için yetkili değil ' dir.

## <a name="finishing-bulk-migration-and-clean-up"></a>Toplu geçişi sonlandırma ve temizleme
Aşağıdaki ekran görüntüsünde, Azure portalında sunucu uç noktası özellikleri dikey penceresinde gösterir. Çevrimdışı veri aktarımı bölümünde işleminin durumunu gözlemleyebilirsiniz. "Sürüyor" veya "Tamamlandı" ya da gösterilir.

Sunucu tüm ad alanı, ilk eşitleme tamamlandıktan sonra hazırlama dosyası yararlanarak tamamladınız Data Box toplu paylaşımıyla dosyaları geçirildi. Durum "Tamamlandı" için ile değişiyor çevrimdışı veri aktarımı için sunucu uç noktası özelliklerinde gözlemleyin. Şu anda maliyetinden tasarruf etmek hazırlama paylaşım temizleyebilirsiniz:

1. Durumu "tamamlandığında,", "Çevrimdışı veri aktarımı devre dışı bırak" sunucu uç noktası özelliklerinde basın.
2. Maliyetinden tasarruf etmek hazırlama paylaşım silmeyi düşünün. Hazırlama paylaşım, dosya ve klasör ACL'leri bulunma olasılığı düşüktür ve bu nedenle sınırlı bir kullanımdır. Yedekleme "zamanda nokta" doğrultusunda, bunun yerine gerçek oluşturma [eşitlenirken Azure dosya paylaşımının anlık görüntüsünü](storage-snapshots-files.md). Yapabilecekleriniz [anlık görüntülerini almak Azure yedeklemeyi etkinleştirme]( ../../backup/backup-azure-files.md) bir zamanlamaya göre.

![Çevrimdışı veri aktarımı için durumu ve devre dışı bırak denetimleri yer aldığı Azure portalı kullanıcı arabirimini sunucu uç noktası özellikleri için görselleştirme.](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Yalnızca bu modu durum "tamamlandı" veya gerçekten yanlış yapılandırmadan kaynaklanan durdurmak istediğinizde devre dışı bırakmanız gerekir. Yasal bir dağıtım modu Orta şekilde devre dışı bırakma, hazırlama paylaşımınızın hala kullanılabilir olsa bile dosyaları sunucudan karşıya yükleme başlar.

> [!IMPORTANT]
> Çevrimdışı veri aktarımı devre dışı bırakıldıktan sonra yeniden etkinleştirmek için hiçbir yolu yoktur toplu geçiş hazırlama paylaşımından hala kullanılabilir olsa bile.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
