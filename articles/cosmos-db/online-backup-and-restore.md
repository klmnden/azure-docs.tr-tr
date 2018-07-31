---
title: Çevrimiçi Yedekleme ve geri yükleme işlemi Azure Cosmos DB | Microsoft Docs
description: Otomatik yedekleme ve bir Azure Cosmos DB veritabanını geri yükleme hakkında bilgi edinin.
keywords: Yedekleme ve geri yükleme, çevrimiçi yedekleme
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 66b4f63e75773aa0c1857dfcc19e22b48a0c3537
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343171"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Otomatik çevrimiçi yedekleme ve geri yükleme işlemi Azure Cosmos DB
Azure Cosmos DB, tüm verilerinizin yedeklerini düzenli aralıklarla otomatik olarak alır. Otomatik yedeklemeler, performans veya veritabanı işlemlerinizi kullanılabilirliğini etkilemeden alınır. Tüm yedeklemeler ayrı olarak başka bir depolama hizmetinde depolanır ve bu yedekleri bölgesel felaketlere karşı dayanıklılık için genel olarak çoğaltılır. Cosmos DB kapsayıcınız kaza ve daha sonra veri kurtarma veya bir olağanüstü durum kurtarma çözümü gerektiğinde otomatik yedekleme senaryoları için tasarlanmıştır.  

Bu makalede bilgiler Cosmos DB'de kullanılabilirliği ve veri yedekliliği ile başlar ve sonra yedeklemeler açıklanır. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Cosmos DB - yeniden anımsamak ile yüksek kullanılabilirlik
Cosmos DB olacak şekilde tasarlanmıştır [Global olarak dağıtılmış](distribute-data-globally.md) – ilke yük devretme ve saydam çok girişli API'lerini kullanan yanı sıra birden çok Azure bölgeleri arasında aktarım hızını ölçeklendirme sağlar. Azure Cosmos DB sunar [% 99,99 kullanılabilirlik SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db) tek bölgeli tüm hesaplar ve çok bölgeli tüm hesaplar tutarlılıkla ve kullanılabilirliğine çok bölgeli tüm veritabanı hesaplarında % 99,999 okuyun. Azure Cosmos DB'de tüm yazma işlemlerini istemciye bildirmeden önce yerel disklere onaylanmadan yerel veri merkezinde bir çekirdeği tarafından. Cosmos DB yüksek kullanılabilirliğini yerel depolama alanında kullanır ve herhangi bir harici depolama teknolojilerini temel bağlı değildir. Ayrıca, veritabanı hesabınız ile birden fazla Azure bölgesine ilişkiliyse, okumalarınızın diğer bölgeler arasında çoğaltılır. Birçok dilediğiniz, veritabanı hesabıyla ilişkili bölge okuma gibi düşük gecikme süreleri ve erişim verilerinizi ölçeklendirmek için sahip olabilir. Okuma her bölgede (çoğaltılan) verilerin depolanmasına çoğaltma kümesi arasında kalıcıdır.  

Aşağıdaki diyagramda gösterildiği gibi tek bir Cosmos DB kapsayıcısı olan [yatay olarak bölünmüş](partition-data.md). "Bölüm", aşağıdaki diyagramda bir daire gösterilir ve her bölüm çoğaltma kümesi yüksek oranda kullanılabilir hale gelir. Yerel dağıtım (X ekseni tarafından gösterilen) tek bir Azure bölgesi içinde budur. Ayrıca, her bölüm (sahip, karşılık gelen çoğaltma kümesi) daha sonra genel olarak (örneğin, bölgelerde Bu çizim üç: Doğu ABD, Batı ABD ve orta Hindistan) veritabanı hesabınızla ilişkili birden çok bölgede dağıtılır. "Bölüm kümesi" Global olarak dağıtılmış olan varlık (Y ekseni ile gösterilir) her bölgede verilerinizin birden çok kopya kapsayan. Veritabanı hesabıyla ilişkili bölge öncelik atayabilirsiniz ve Cosmos DB şeffaf bir şekilde üzerinden olağanüstü durum yaşanması sonraki bölgeye başarısız olur. El ile uygulamanızın uçtan uca kullanılabilirliğini sınamak için yük devretme benzetimi yapabilirsiniz.  

Aşağıdaki görüntüde, yedeklilik Cosmos DB ile yüksek düzeyde gösterilmektedir.

![Yüksek düzeyde artıklık Cosmos DB ile](./media/online-backup-and-restore/redundancy.png)

![Yüksek düzeyde artıklık Cosmos DB ile](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Tam, otomatik ve çevrimiçi yedeklemeler
HAY aksi, kapsayıcı veya veritabanı cihazımı sildim! Cosmos DB ile yalnızca verilerinizi ancak verilerinizin yedeklerini da bölgesel bir olağanüstü durumlar için yüksek düzeyde yedekli ve dayanıklı yapılır. Bu otomatik yedeklemeler şu anda yaklaşık dört saat alınır ve her zaman en son iki yedekleme depolanır. Verileri yanlışlıkla bırakılan veya bozulursa, başvurun [Azure Destek](https://azure.microsoft.com/support/options/) sekiz saat içinde. 

Yedeklemelerin performansını veya veritabanı işlemlerinizi kullanılabilirliğini etkilemeden alınır. Cosmos DB yedekleme kullanılırken, sağlanan RU veya performansını ve veritabanınızın kullanılabilirliğini etkileyen olmadan arka planda gerçekleştirir. 

Cosmos DB içinde depolanan, verilerinizi farklı olarak, otomatik yedekleri Azure Blob Depolama hizmetinde depolanır. Düşük gecikme süresi/verimli karşıya yükleme sağlamak için yedekleme anlık görüntüsünü aynı bölgede geçerli yazma bölgesine Cosmos DB veritabanı hesabınızı Azure Blob storage'da örneğine yüklenir. Bölgesel bir olağanüstü durum karşı dayanıklılık için her anlık görüntü yedekleme verilerinizi Azure Blob depolama alanındaki başka bir bölgede coğrafi olarak yedekli depolama (GRS) aracılığıyla yeniden çoğaltılır. Aşağıdaki diyagramda, Batı ABD'deki uzak bir Azure Blob Depolama hesabındaki tüm Cosmos DB kapsayıcısı (üç birincil'ındaki tüm bölümler Batı ABD, bu örnekte) ile yedeklenir ve ardından GRS için Doğu ABD kopyalandığı gösterilmektedir. 

Aşağıdaki görüntüde tüm Cosmos DB varlıkların GRS Azure depolama alanında düzenli tam yedeklemeler gösterilmektedir.

![GRS Azure depolama alanındaki tüm Cosmos DB varlıklarını, düzenli aralıklarla tam yedekler](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Yedekleme bekletme süresi
Yukarıda açıklandığı gibi Azure Cosmos DB bölüm düzeyinde dört saatte verilerinizin anlık görüntüleri alır. Herhangi bir zamanda yalnızca son iki anlık görüntüleri korunur. Ancak, kapsayıcı/veritabanı silinirse, Azure Cosmos DB tüm verilen kapsayıcı/veritabanı silinmiş bölümleri için mevcut anlık görüntü 30 gün boyunca tutar.

Kendi anlık görüntüleri tutmak istiyorsanız, SQL API'si için JSON seçeneği ver Azure Cosmos DB'de kullanabileceğiniz [veri geçiş aracı](import-data.md#export-to-json-file) ek Yedeklemeler zamanlamak için.

> [!NOTE]
> Geri yükleme tam veritabanı hesap düzeyinde olur, "Veritabanı düzeyinde kapsayıcı bir dizi için Aktarım sağlama –" Lütfen unutmayın. Ayrıca Destek ekibine 8 saat içinde kapsayıcınızı yanlışlıkla silinmiş ulaşmak için emin olmanız gerekir. 8 saat içinde Destek ekibine başvurun yoksa varsa veri geri yüklenemez. 


## <a name="restoring-a-database-from-an-online-backup"></a>Bir veritabanı çevrimiçi bir yedekten geri yükleme

Veritabanı veya kapsayıcı kaza ile silerseniz, şunları yapabilirsiniz [bir destek bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya [Azure destek çağrısı](https://azure.microsoft.com/support/options/) veri otomatik en son yedeklemeden geri yüklemek için. Azure desteği yalnızca standart, geliştirici gibi seçili planları için kullanılabilir, destek, temel plan ile kullanılamaz. Farklı destek planları hakkında bilgi edinmek için [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfası. 

Veritabanınızı (burada bir kapsayıcı içindeki belgeler silinir çalışmaları içerir) veri bozulma sorunu nedeniyle geri yüklemek gerekirse bkz [veri bozulması işleme](#handling-data-corruption) bozuk veriler önlemek için ek adımlar atmanız gereken şekilde Mevcut yedeklemeler üzerine yazmasını. Belirli bir anlık görüntüye geri yüklenmesi, yedekleme için Cosmos DB, verileri bu anlık görüntü için yedekleme döngüsü boyunca kullanılabilir olduğunu gerektirir.

## <a name="handling-data-corruption"></a>Veri bozulması işleme

Azure Cosmos DB veritabanı hesabının her bölümün son iki yedeklerini korur. Bu model iyi bir kapsayıcı (belge, grafik, tablo koleksiyonu) çalışır veya son sürümleri birini geri yüklenebilir olduğundan bir veritabanı yanlışlıkla silinmiş. Ancak, durumda kullanıcılar bir veri bozulma sorunu neden olabilir, Azure Cosmos DB veri bozulmasını farkında olabilir ve bozulma mevcut yedeklemeler üzerine mümkündür. 

Bozulma algılandı hemen sonra yaklaşık süreyi bozulma ile veritabanı hesabı ve kapsayıcı bilgilerle müşteri hizmetlerine ulaşın. Başka bir eylem durumunda, kullanıcının gerçekleştirebilirsiniz (veri silme/updation) bozuk bozuk verilerle üzerine yedeklemeleri korunması kullanıcı bozuk kapsayıcıyı (koleksiyon/grafik/table) silmeniz gerekir.  

Aşağıdaki görüntüde verilerin yanlışlıkla silinmesini veya bir kapsayıcı içindeki veri güncelleştirmek için Azure portal aracılığıyla container(collection/graph/table) geri yüklemek için destek isteği oluşturma gösterilmektedir.

![Hatalı güncelleştirme için bir kapsayıcı geri yükleyin veya Cosmos DB'de veri silme](./media/online-backup-and-restore/backup-restore-support.png)

Bu tür senaryolara - için geri yükleme gerçekleştirildiğinde veriler başka bir hesaba geri (soneki ile "-geri") ve kapsayıcı. Bu geri yükleme, Müşteri verilerinin doğrulama yapmak ve verileri gereken şekilde taşımak için bir fırsat sağlamak için yerinde yapılmaz. Geri yüklenen aynı RU'ları ve dizin oluşturma ilkeleri ile aynı bölgede kapsayıcıdır. 

## <a name="next-steps"></a>Sonraki adımlar

Birden çok veri merkezinde veritabanınızda çoğaltmak için bkz: [verilerinizi Cosmos DB ile küresel olarak dağıtmak](distribute-data-globally.md). 

Azure desteği, dosya kişiye [Azure portalından bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

