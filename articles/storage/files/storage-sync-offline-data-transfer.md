---
title: Azure Data Box ve diğer yöntemleri kullanarak Azure dosya eşitleme ile veri taşıma
description: Azure dosya eşitleme ile uyumlu bir şekilde toplu veri geçirin.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 04b13c1e511f54c1fcf7b632d3a368fde16bf319
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549052"
---
# <a name="migrate-bulk-data-to-azure-file-sync"></a>Azure dosya eşitleme için toplu verileri geçirme
Azure dosya eşitleme için iki şekilde toplu veri geçirebilirsiniz:

* **Azure dosya eşitleme'yi kullanarak dosyaları yükleyin.** En basit yöntem budur. Dosyalarınızı yerel olarak Windows Server 2012 R2 veya sonraki bir sürüme taşıyın ve Azure dosya eşitleme aracısını yükleyin. Eşitlemeyi ayarladıktan sonra sunucudan dosyalarınızı karşıya yüklenir. (Müşterilerimize şu anda 1 TiB hakkında iki günde bir ortalama karşıya yükleme hızını karşılaşırsınız.) Sunucunuz çok fazla bant genişliği için veri merkezinizin kullanmaz, emin olmak için ayarlamak isteyebileceğiniz bir [bant genişliği azaltma zamanlama](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter).
* **Dosyalarınızı çevrimdışı aktarın.** Yeterli bant genişliği yoksa, dosyalar, makul bir sürede Azure'a yüklemek mümkün olmayabilir. İlk eşitleme dosyalarının tüm küme zorluktur. Bu zorluğun üstesinden gelmek için çevrimdışı toplu geçiş gibi araçları kullanın [Azure Data Box ailesi](https://azure.microsoft.com/services/storage/databox). 

Bu makalede Azure dosya eşitleme ile uyumlu bir şekilde çevrimdışı dosyaları geçirme açıklanmaktadır. Dosya çakışmalarını önlemek için ve eşitleme etkinleştirdikten sonra dosya ve klasör erişim denetim listeleri (ACL'ler) ve zaman damgaları korumak için bu yönergeleri izleyin.

## <a name="online-migration-tools"></a>Çevrimiçi Geçiş Araçları
İşlemi yalnızca Data Box için aynı zamanda diğer çevrimdışı Geçiş Araçları için Biz bu makalede çalışır açıklanmaktadır. AzCopy, Robocopy, veya iş ortağı araçları ve Hizmetleri gibi çevrimiçi araçları için de çalışır. Ancak ilk üstesinden gelmek sınama karşıya yükleme, Azure dosya eşitleme ile uyumlu bir şekilde bu araçları kullanabilmek için bu makaledeki adımları izleyin.


## <a name="benefits-of-using-a-tool-to-transfer-data-offline"></a>Çevrimdışı veri aktarmak için bir araç kullanmanın avantajları
Aktarım aracı Data Box gibi çevrimdışı geçiş için kullanmanın başlıca avantajları şunlardır:

- Tüm dosyaları ağ üzerinden karşıya yükleme gerekmez. Büyük ad alanları için bu aracı, önemli ölçüde ağ bant genişliği ve saat tasarruf sağlayabilirsiniz.
- Azure dosya eşitleme kullandığınızda, Canlı sunucunuzun verileri Azure'a taşıma işlemi sonrasında, değişen dosyaları karşıya yükler, hangi aktarım aracı ne olursa olsun (Data Box, Azure içeri/dışarı aktarma hizmeti vb.) kullanın.
- Azure dosya eşitleme, çevrimdışı toplu geçiş aracı ACL'leri aktarım değil olsa bile, dosya ve klasör ACL'leri eşitler.
- Veri kutusu ve Azure dosya eşitleme kapalı kalma süresi gerektirir. Azure'a veri aktarmayı Data Box'ı kullandığınızda, ağ bant genişliği verimli bir şekilde kullanmak ve dosya uygunluğunu korumak. Sizin de ad alanınızı verileri Azure'a taşıma işlemi sonrasında, değişen dosyaları karşıya yükleyerek güncel tutun.

## <a name="prerequisites-for-the-offline-data-transfer"></a>Çevrimdışı veri aktarımı için Önkoşullar
Çevrimdışı veri aktarımınız tamamlamadan önce geçirdiğiniz sunucuda eşitleme etkinleştirmemeniz gerekir. Başlamadan önce dikkate alınması gereken diğer noktalar aşağıdaki gibidir:

- Data Box için geçişiniz toplu kullanmayı planlıyorsanız: Gözden geçirme [Data Box için dağıtım önkoşulları](../../databox/data-box-deploy-ordered.md#prerequisites).
- Son Azure dosya eşitleme topolojinizi planlama: [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- Eşitlemek istediğiniz dosya paylaşımlarını barındıracak bir Azure depolama hesabı seçin. Toplu geçişinizi geçici hazırlama paylaşımları aynı depolama hesapları yapıldığından emin olun. Toplu geçiş, yalnızca bir son - ve aynı depolama hesabında yer alan bir hazırlama-share yararlanarak etkinleştirilebilir.
- Bir toplu geçiş, yalnızca bir sunucu konumu ile yeni bir eşitleme ilişkisinin oluşturduğunuzda yararlanılabilir. Mevcut bir eşitleme ilişkisi olan bir toplu geçiş etkinleştirilemiyor.


## <a name="process-for-offline-data-transfer"></a>Çevrimdışı veri aktarım işlemi
Azure dosya eşitleme ', Azure Data Box gibi toplu Geçiş Araçları ile uyumlu bir şekilde ayarlamak nasıl aşağıda verilmiştir:

![Azure dosya eşitleme ' ayarlama işlemini gösteren diyagram](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Adım | Ayrıntı |
|---|---------------------------------------------------------------------------------------|
| ![1. Adım](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Data Box'ınızı sipariş](../../databox/data-box-deploy-ordered.md). Data Box ailesi teklifleri [birden çok ürünlerin](https://azure.microsoft.com/services/storage/databox/data) gereksinimlerinizi karşılayacak şekilde. Data Box'ınızı aldığınızda izleyin, [verilerinizi kopyalamak için belgeleri](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box) Data box'taki bu UNC yolu:  *\\< DeviceIPAddres\>\<StorageAccountName_AzFile\> \<ShareName\>*. Burada, *ShareName* hazırlama paylaşım adıdır. Data Box Azure'a geri gönderin. |
| ![2. Adım](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Geçici hazırlama paylaşımları olarak seçtiğiniz Azure dosya paylaşımlarını dosyalarınızı görünmesini bekleyin. *Bu paylaşımlara eşitleniyor etkinleştirmeyin.* |
| ![3. Adım](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Data Box oluşturduğunuz her dosya paylaşımına yönelik yeni ve boş bir paylaşım oluşturun. Bu yeni paylaşıma Data Box paylaşım olarak aynı depolama hesabında olmalıdır. [Yeni bir Azure dosya paylaşımı oluşturma işlemini](storage-how-to-create-file-share.md). |
| ![4. Adım](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Bir eşitleme grubu oluşturma](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) depolama eşitleme hizmetinde. Bulut uç noktası olarak boş paylaşım başvurusu. Her Data Box dosya paylaşımı için bu adımı yineleyin. [Azure dosya eşitleme ayarı](storage-sync-files-deployment-guide.md). |
| ![5. Adım](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Sunucu uç noktası Canlı sunucusuna dizininize eklemek](storage-sync-files-deployment-guide.md#create-a-server-endpoint). İşleminde, Azure'a dosyaların taşınmış belirtin ve hazırlama paylaşımları başvuru. Etkinleştirmek veya Bulut gerektiği şekilde katmanlama devre dışı bırakabilirsiniz. Canlı sunucunuzda bir sunucu uç noktası oluşturulurken hazırlama paylaşımı başvuru. Üzerinde **sunucusu uç noktası ekleme** dikey altında **çevrimdışı veri aktarımı**seçin **etkin**ve ardından bulut olarak aynı depolama hesabında olmalıdır hazırlama paylaşımı seçin uç nokta. Burada, kullanılabilir paylaşımları depolama hesabı ve eşitleme olmayan paylaşımları tarafından filtrelenir. |

![Azure portal kullanıcı arabirimi, yeni bir sunucu uç noktası oluşturulurken çevrimdışı veri aktarımı etkinleştirme gösteren ekran görüntüsü](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Eşitleme paylaşımı
Sunucu uç noktanızı oluşturmanızın ardından, eşitleme başlar. Eşitleme işlemi, her dosya sunucusunda dosyaları burada borç olarak Data Box hazırlama paylaşımda var olup olmadığını belirler. Dosya varsa, eşitleme işlemi dosya sunucudan yüklemek yerine hazırlama paylaşımı kopyalar. Dosya Paylaşımı hazırlama yoksa veya yerel sunucuda daha yeni bir sürüm varsa, eşitleme işlemi yerel sunucudan dosyayı yükler.

> [!IMPORTANT]
> Sunucu uç noktası oluştururken toplu geçiş modunu etkinleştirebilirsiniz. Sunucu uç noktası kurduktan sonra ad alanına bir zaten eşitleme sunucusu toplu geçirilen verilerden tümleştiremezsiniz.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL ve zaman damgaları dosyaları ve klasörleri
Azure dosya eşitleme kullandığınız toplu geçiş aracı başlangıçta ACL'leri aktarım yaramadı bile dosya ve klasör ACL'leri Canlı sunucusundan eşitlenen sağlar. Bu nedenle, hazırlama paylaşımı dosyalar ve klasörler için herhangi bir ACL içermesi gerekmez. Yeni bir sunucu uç noktası oluşturma, çevrimdışı veri geçiş özelliğini etkinleştirdiğinizde, tüm dosya EDL'leri sunucuda eşitlenir. Yeni oluşturulan ve değiştirilen zaman damgaları de eşitlenir.

## <a name="shape-of-the-namespace"></a>Ad alanının şekli
Eşitleme etkinleştirdiğinizde, sunucunun içeriği ad alanı şeklini belirler. Data Box anlık görüntü ve geçiş işlemini tamamladıktan sonra dosyaları yerel sunucudan silinirse canlı, eşitleme ad alanına bu dosyaları taşımayın. Hazırlama paylaşımda kalır, ancak bunlar kopyalanmaz. Eşitleme ad alanı Canlı sunucusuna göre tuttuğundan, bu gereklidir. Data Box *anlık görüntü* verimli dosya kopyalama için yalnızca hazırlama bir zemin olduğu. Şekil dinamik ad alanı için yetkili değil.

## <a name="cleaning-up-after-bulk-migration"></a>Toplu geçişten sonra Temizleme 
Sunucu ad alanı, ilk eşitleme tamamlandığında hazırlama dosya paylaşımı toplu geçişi Data Box dosyalarını kullanın. Üzerinde **sunucu uç noktası özellikleri** Azure portalındaki dikey penceresinde, **çevrimdışı veri aktarımı** bölümünde durumu değişir **sürüyor** için **tamamlandı** . 

![Çevrimdışı veri aktarımı için durumu ve devre dışı bırak denetimleri bulunduğu sunucu uç noktası özellikleri dikey penceresinin ekran görüntüsü](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Artık maliyet tasarrufu için hazırlama paylaşım temizleyebilirsiniz:

1. Üzerinde **sunucu uç noktası özellikleri** durumu olduğunda dikey penceresinde **tamamlandı**seçin **çevrimdışı veri aktarımını devre dışı**.
2. Maliyetlerden tasarruf etmek için hazırlama paylaşım silmeyi düşünün. Bu nedenle çok kullanışlı değildir hazırlama paylaşımı büyük olasılıkla dosya ve klasör ACL içermiyor. Yedekleme zaman içinde nokta amacıyla gerçek oluşturun [eşitlenirken Azure dosya paylaşımının anlık görüntüsünü](storage-snapshots-files.md). Yapabilecekleriniz [anlık görüntülerini almak için Azure Yedekleme'yi ayarlayın]( ../../backup/backup-azure-files.md) bir zamanlamaya göre.

Yalnızca durumu olduğunda çevrimdışı veri aktarım modunu devre dışı **tamamlandı** veya yanlış yapılandırma nedeniyle iptal etmek istediğinizde. Dağıtım sırasında modu devre dışı bırakırsanız, dosyaları hazırlama paylaşımınızın hala kullanılabilir olsa bile sunucudan karşıya yükleme başlar.

> [!IMPORTANT]
> Çevrimdışı veri aktarım modunu devre dışı bıraktıktan sonra toplu geçiş hazırlama paylaşımından hala kullanılabilir olsa bile yeniden etkinleştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
