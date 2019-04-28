---
title: Azure Cosmos DB'de otomatik, çevrimiçi yedekleme ve isteğe bağlı veri geri yükleme
description: Bu makalede Azure Cosmos DB'de works otomatik, çevrimiçi yedekleme ve isteğe bağlı veri geri yükleme nasıl.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 6ed968b1613a96a2f4ab449c7b52488e066a38ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60930259"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Azure Cosmos DB'de çevrimiçi yedekleme ve isteğe bağlı veri geri yükleme

Azure Cosmos DB, verilerinizin yedeklerini düzenli aralıklarla otomatik olarak alır. Otomatik yedeklemeler, performans veya kullanılabilirlik veritabanı işlemleri etkilemeden alınır. Tüm yedeklemeler ayrı ayrı bir depolama hizmetinde depolanır ve bu yedekleri bölgesel felaketlere karşı dayanıklılık için genel olarak çoğaltılır. Otomatik yedeklemeler, yanlışlıkla sildiğinizde veya, Azure Cosmos hesabı, veritabanı ve kapsayıcı güncelleştirin ve daha sonra veri kurtarma gerektiren senaryolarda yararlı olur.

## <a name="automatic-and-online-backups"></a>Otomatik ve çevrimiçi yedeklemeler

Azure Cosmos DB ile yalnızca verilerinizi, ancak ayrıca verilerinizin yedeklerini düzeyde yedekli ve bölgesel felaketlere karşı dayanıklı değildir. Aşağıdaki adımlar, Azure Cosmos DB veri yedekleme performansını gösterir:

* Azure Cosmos DB otomatik olarak 4 saatte veritabanınızın bir yedeğine alır ve herhangi bir noktada zaman yalnızca en son 2 yedeklemeler depolanır. Ancak, kapsayıcı ya da veritabanını sildiyseniz Azure Cosmos DB belirli bir kapsayıcı veya veritabanı mevcut anlık görüntü 30 gün boyunca tutar.

* Gerçek verileri yerel olarak Azure Cosmos DB içinde bulunduğu ise azure Cosmos DB bu yedeklemeler Azure Blob Depolama alanında depolar.

*  Düşük gecikme süresi garanti için anlık görüntü yedeklemenizin geçerli yazma bölgesine aynı bölgede Azure Blob storage'da depolanır (veya yazma bölgeleri biri çok yöneticili yapılandırma varsa), Azure Cosmos veritabanı hesabı. Bölgesel bir olağanüstü durum karşı dayanıklılık için her anlık görüntü yedekleme verilerinin Azure Blob depolama alanındaki başka bir bölgede coğrafi olarak yedekli depolama (GRS) aracılığıyla yeniden çoğaltılır. Yedekleme için çoğaltılır bölge, kaynak bölge ve kaynak bölgeyle ilişkili bölgesel çift temel alır. Daha fazla bilgi için bkz. [Azure bölgelerinin coğrafi olarak yedekli çiftleri listesi](../best-practices-availability-paired-regions.md) makalesi. Bu yedekleme doğrudan erişemez. Azure Cosmos DB Bu yedekleme kullanırsanız yalnızca bir yedekleme geri yükleme başlatılır.

* Yedeklemelerin performansını veya uygulamanızın kullanılabilirliğini etkilemeden alınır. Azure Cosmos DB veri yedekleme herhangi ek sağlanan aktarım hızına (RU) kullanan veya veritabanınızın kullanılabilirliğini ve performansı etkileyen arka planda gerçekleştirir.

* Yanlışlıkla silinmiş veya verilerinizi bozuk, sizinle iletişim kuralım [Azure Destek](https://azure.microsoft.com/support/options/) Azure Cosmos DB takıma yardımcı olur böylece 8 saat içinde verileri yedeklerden geri.

Aşağıdaki görüntüde, nasıl bir Azure Cosmos kapsayıcısı ile üç birincil fiziksel'ındaki tüm bölümler Batı ABD Batı ABD uzak bir Azure Blob Depolama hesabında yedeklenebilir ve ardından Doğu ABD olarak kopyalandığı gösterilmektedir:

![GRS Azure depolama alanındaki tüm Cosmos DB varlıklarını, düzenli aralıklarla tam yedekler](./media/online-backup-and-restore/automatic-backup.png)

## <a name="options-to-manage-your-own-backups"></a>Kendi yedeklemelerinizi yönetmek için seçenekleri

Azure Cosmos DB SQL API hesaplarıyla, aşağıdaki yaklaşımlardan birini kullanarak da kendi yedekleme işlemlerinizi kendi başınıza koruyabilirsiniz:

* Kullanım [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) verileri düzenli aralıklarla, tercih ettiğiniz bir depoya taşıma.

* Azure Cosmos DB kullanmayı [değişiklik akışını](change-feed.md) verileri düzenli aralıklarla artımlı değişiklikler yanı sıra, tam yedeklemeler için okuma ve kendi depolama alanında depolayın.

## <a name="backup-retention-period"></a>Yedekleme bekletme süresi

Azure Cosmos DB, dört saatte verilerinizin anlık görüntüleri alır. Herhangi bir zamanda yalnızca son iki anlık görüntüleri korunur. Ancak, kapsayıcı ya da veritabanını sildiyseniz Azure Cosmos DB belirli bir kapsayıcı veya veritabanı mevcut anlık görüntü 30 gün boyunca tutar.

## <a name="restoring-data-from-online-backups"></a>Çevrimiçi yedeklemelerden veri geri yükleme

Yanlışlıkla silinmesi veya değiştirilmesi veri aşağıdaki senaryolardan birinde oluşabilir:  

* Tüm Azure Cosmos hesabı silindi

* Bir veya daha fazla Azure Cosmos veritabanı silindi

* Bir veya daha fazla Azure Cosmos kapsayıcıları silindi

* Azure Cosmos öğeleri (örneğin, belgeleri) bir kapsayıcı içindeki silinmiş veya değiştirilmiş. Bu belirli durumda, genellikle "veri bozulması" da denir.

* Paylaşılan teklif veritabanı veya paylaşılan teklif veritabanı kapsayıcılara silinir veya bozulursa

Azure Cosmos DB, yukarıdaki senaryolarda tüm verileri geri yükleyebilirsiniz. Geri yükleme işlemi, her zaman geri yüklenen verileri tutmak için yeni bir Azure Cosmos hesabı oluşturur. Biçimde yeni hesap adı belirtilmezse, olan `<Azure_Cosmos_account_original_name>-restored1`. Birden çok geri yüklemeler denenirse, son basamak artırılır. Verileri önceden oluşturulmuş bir Azure Cosmos hesabınıza geri yükleyemezsiniz.

Bir Azure Cosmos hesabı silindiğinde, sağlanan hesap adı kullanımda olmadığından emin ediyoruz verileri aynı ada sahip bir hesaba geri yükleyebilirsiniz. Böyle durumlarda, yalnızca aynı adı kullanmak için geri yüklenen verilerin engeller, ancak ayrıca uygun hesabı öğesinden daha zor geri yüklemek için keşfetmek yapar çünkü hesap silindikten sonra yeniden oluşturma için önerilir. 

Bir Azure Cosmos veritabanı silindiğinde, tüm veritabanı veya veritabanı kapsayıcılara kümesini geri yüklemek mümkündür. Veritabanlarında kapsayıcılar'ı seçin ve tüm geri yüklenen veriler, yeni bir Azure Cosmos hesabında yerleştirilir ve bunları geri yüklemek mümkündür.

Bir kapsayıcı içindeki bir veya daha fazla öğe yanlışlıkla silinen veya (veri bozulması olay) değiştirilmiş, geri yüklemek için gereken zamanı belirtmek gerekir. Bu durum için essence zaman önemlidir. Kapsayıcı dinamik olduğundan, yedekleme bekletme süresi (sekiz saat varsayılandır) beklerseniz belirttikleri şekilde yedekleme hala çalışıyor. Yedekleme döngüsü tarafından üzerine gerekmez çünkü siler söz konusu olduğunda, verileriniz artık depolanır. Silinen veritabanları veya kapsayıcıları için yedeklemeler 30 gün boyunca kaydedilir.

Aktarım hızı ve veritabanı düzeyinde sağlarsanız (diğer bir deyişle, burada bir dizi kapsayıcıları sağlanan aktarım hızı paylaşır), yedekleme ve geri yükleme işlemini bu durumda tüm veritabanı düzeyinde ve kapsayıcılara düzeyinde gerçekleşir. Bu gibi durumlarda, kapsayıcılar, geri yüklemek için bir alt kümesi seçerek bir seçenek değildir.

## <a name="migrating-data-to-the-original-account"></a>Verileri özgün hesabına geçirme

Verileri geri yükleme işleminin birincil amacı, silmek veya yanlışlıkla değiştirebilen herhangi bir veri kurtarmak için bir yol sağlamaktır. Bu nedenle, içeriği kurtarılan veriler ne beklediğiniz içerdiği emin olmak için önce İnceleme öneririz. Ardından verileri birincil hesabın geçirmeyi üzerinde çalışır. Geri yüklenen hesabı Canlı hesabı olarak kullanmak mümkün olsa da, üretim iş yükleri varsa, önerilen bir seçenek değil.  

Verileri özgün Azure Cosmos hesabına geçirmek için farklı yollar şunlardır:

* Kullanarak [Cosmos DB veri geçiş aracı](import-data.md)
* Kullanarak [Azure veri fabrikası]( ../data-factory/connector-azure-cosmos-db.md)
* Kullanarak [değişiklik akışını](change-feed.md) Azure Cosmos DB'de 
* Özel kod yazma

Silme geri yüklenen hesapları bitirdikten hemen sonra geçirme, çünkü bunlar ücret ödemeye devam.

## <a name="next-steps"></a>Sonraki adımlar

Ardından verileri bir Azure Cosmos hesaptan geri yüklemek veya bir Azure Cosmos hesabına veri geçirmeyi öğrenin hakkında bilgi edinebilirsiniz

* İstek, Azure desteği'ne başvurun bir geri yükleme yapmak için [Azure portalından bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Bir Azure Cosmos hesabından veri geri yükleme](how-to-backup-and-restore.md)
* [Kullanım Cosmos DB değişiklik akışı](change-feed.md) verilerini Azure Cosmos DB'ye taşımak için.
* [Azure Data factory'yi](../data-factory/connector-azure-cosmos-db.md) verilerini Azure Cosmos DB'ye taşımak için.

