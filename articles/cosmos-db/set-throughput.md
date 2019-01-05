---
title: Azure Cosmos DB için sağlama aktarım hızı
description: Azure Cosmos DB kapsayıcıları ve veritabanları için sağlanan aktarım hızı ayarlama konusunda bilgi edinin.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: 3e992dd8ab24e4e60b81c6565ea4ec3971a9336b
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036451"
---
# <a name="provision-throughput-on-azure-cosmos-containers-and-databases"></a>Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızı

Bir Azure Cosmos veritabanı, bir dizi kapsayıcıları için yönetim birimidir. Bir veritabanı şemadan kapsayıcıların bir kümesinden oluşur. Bir Azure Cosmos kapsayıcı hem aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı makineler bir Azure bölgesi içinde bir dizi yatay olarak bölümlenir ve Azure Cosmos hesabınızla ilişkili tüm Azure bölgelerine dağıtılır.

Azure Cosmos DB aktarım hızı iki ayrıntı düzeylerinde - yapılandırmanıza olanak tanır **Azure Cosmos kapsayıcıları** ve **Azure Cosmos veritabanı**.

## <a name="setting-throughput-on-a-container"></a>Bir kapsayıcısında aktarım hızını ayarlama  

Bir Azure Cosmos kapsayıcıdaki sağlanmış olan aktarım hızı için kapsayıcı özel olarak ayrılmış. Kapsayıcı, sağlanan aktarım hızı her zaman alır. Bir kapsayıcı sağlanan aktarım hızını finansal SLA'ları tarafından desteklenmektedir. Bir kapsayıcıdaki aktarım hızı yapılandırmak için bkz [nasıl sağlanacağı bir Azure Cosmos kapsayıcısında aktarım hızını](how-to-provision-container-throughput.md).

Bir kapsayıcı sağlanan aktarım hızı ayarlama yaygın olarak kullanılan bir seçenektir. Herhangi bir miktarda işleme (RU) sağlayarak bir kapsayıcının aktarım hızını esnek olarak ölçeklendirebilirsiniz, ancak mantıksal bölümleri için aktarım hızı seçmeli olarak belirtemezsiniz. Mantıksal bölüm üzerinde çalışan bir iş yükünü birden çok belirli mantıksal bölüm için ayrılan aktarım hızı tüketir, işlemlerinizin oranı sınırlı alırsınız. Hız sınırlama ortaya çıktığında, kapsayıcının tamamı verimliliğini artırmak veya işlemi yeniden deneyin. Bölümleme hakkında daha fazla bilgi için bkz: [mantıksal bölümler](partition-data.md).

Garantili performans kapsayıcısı için istediğiniz zaman, kapsayıcı ayrıntı düzeyinde aktarım hızı yapılandırmanız önerilir.

Bir Azure Cosmos kapsayıcı sağlanan aktarım hızı, tüm mantıksal bölümler kapsayıcının olarak eşit dağıtılır. Bir fiziksel bölüm tarafından barındırılan bir veya daha fazla mantıksal bölümleri olan bir kapsayıcı olduğundan, fiziksel bölümler yalnızca kapsayıcıya ait ve kapsayıcıdaki sağlanmış olan aktarım hızı destekler. Aşağıdaki resimde, bir fiziksel bölüm bir veya daha fazla mantıksal bölümleri olan bir kapsayıcı nasıl barındıran gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition.png)

## <a name="setting-throughput-on-a-database"></a>Veritabanı aktarım hızını ayarlama

Bir Azure Cosmos veritabanı aktarım hızını sağladığınızda, aktarım hızı belirli kapsayıcılarında sağlanan işleme belirtilmediği sürece veritabanında tüm kapsayıcılar arasında paylaşılır. Veritabanı aktarım hızını kapsayıcılarında arasında paylaşımı makine kümesi üzerinde bir veritabanını barındırmak için benzer. Tüm kapsayıcıları bir veritabanı içinde bir makinede kullanılabilir kaynakları paylaşarak olduğundan, doğal olarak tahmin edilebilir performans üzerinde herhangi bir belirli kapsayıcısına almıyor. Aktarım hızı bir veritabanını yapılandırmak için bkz [nasıl bir Azure Cosmos veritabanı'nda sağlanan aktarım hızı yapılandırma](how-to-provision-database-throughput.md).

Bir Azure Cosmos veritabanı ayarı aktarım hızını, her zaman sağlanan aktarım hızı alma garanti eder. Veritabanı paylaşım sağlanan aktarım hızı içindeki tüm kapsayıcıları beri Azure Cosmos DB veritabanındaki belirli bir kapsayıcı için öngörülebilir bir üretilen iş hacmi garanti sağlamaz. Belirli bir kapsayıcı alabileceği aktarım hızı kısmı bağlıdır:

* Kapsayıcı sayısı
* Bölüm anahtarları çeşitli kapsayıcılar için seçimi ve
* İş yükü kapsayıcıların çeşitli mantıksal bölümler arasında dağıtımı. 

Birden çok kapsayıcıya aktarım hızı paylaşmak istediğiniz, ancak belirli bir kapsayıcı için aktarım hızı ayrılması istemediğiniz aktarım hızı bir veritabanı'nda yapılandırma önerilir. Veritabanı düzeyinde sağlama aktarım hızı için tercih edilen olduğu bazı örnekler aşağıda verilmiştir:

* Bir veritabanının sağlanan aktarım hızı kapsayıcıları kümesi paylaşımı, çok kiracılı bir uygulama için yararlıdır. Her bir kullanıcı farklı bir Azure Cosmos kapsayıcı tarafından temsil edilebilir.

* Bir VM kümesi ya da şirket içi fiziksel sunucuları Azure Cosmos DB için barındırılan bir NoSQL veritabanı (örneğin, MongoDB, Cassandra) geçirilirken kapsayıcıları kümesi arasında bir veritabanının sağlanan aktarım hızı paylaşımı yararlı olur. (Ancak, daha uygun maliyetli ve esnek), MongoDB veya Cassandra kümenizi işlem kapasitesinin bir mantıksal eşdeğeri olarak, Azure Cosmos veritabanı üzerinde yapılandırılan ve sağlanan aktarım hızı düşünebilirsiniz.  

Sağlanan aktarım hızına sahip bir veritabanı içinde oluşturulan tüm kapsayıcıları bir bölüm anahtarı ile oluşturulması gerekir. Belirli bir anda zaman, tüm mantıksal bölümler bu kapsayıcı, bir veritabanı içinde bir kapsayıcıya ayrılan aktarım hızı dağıtılır. Aktarım hızı seçmeli olarak sağlanan aktarım hızı bir veritabanında paylaşımı kapsayıcılar varsa, belirli bir kapsayıcı veya mantıksal bir bölümü için uygulanamıyor. Bir mantıksal bölüm iş yüküne en fazla aktarım hızı belirli bir mantıksal birime atanan kullanırsa, işlemlerinizin oranı sınırlı olur. Hız sınırlama ortaya çıktığında, kapsayıcının tamamı verimliliğini artırmak veya işlemi yeniden deneyin. Bölümleme hakkında daha fazla bilgi için bkz: [mantıksal bölümler](partition-data.md).

Bir veritabanı için sağlanan işleme paylaşımı birden çok mantıksal bölümleri tek bir fiziksel bölüm üzerinde barındırılabilir. Tek bir mantıksal bölüm kapsayıcısının her zaman içinde bir fiziksel bölüm kapsamlıdır, ancak 'L' 'C' kapsayıcıları için sağlanan aktarım hızına bir veritabanının paylaşımı arasında mantıksal bölümler eşlenen ve 'R' fiziksel bölümler barındırılan. Aşağıdaki resimde, bir veritabanı içinde farklı kapsayıcılar ait bir veya daha fazla mantıksal bölümler bir fiziksel bölüm nasıl barındırabilir gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition2.png)

## <a name="setting-throughput-on-a-database-and-a-container"></a>Bir veritabanı ve kapsayıcı aktarım hızı ayarlama

İki model birleştirebilir, hem veritabanı hem de kapsayıcı aktarım hızı sağlama izin verilir. Aşağıdaki örnek, bir Azure Cosmos veritabanı ve kapsayıcı aktarım hızına gösterilmektedir:

* 'K' RU sağlanan aktarım hızı ile 'Z' adlı bir Azure Cosmos veritabanı oluşturabilirsiniz. 
* Ardından, A adlı beş kapsayıcıları oluşturma B, C, D ve veritabanı içinde E.
* Bu gibi durumlarda, sağlanan aktarım hızının 'P' RU açıkça 'B' kapsayıcıdaki yapılandırabilirsiniz.
* Dört kapsayıcımız arasında – A, C, D ve E. paylaşılan 'K' RU aktarım hızı Aktarım hızı miktarda kullanılabilir a, C, D ya da E değişir ve tek tek her kapsayıcının aktarım hızını SLA'sı vardır.
* ' % S'kapsayıcı 'B' her zaman 'P' RU aktarım hızı alma sağlanır ve SLA'lar ile desteklenir.

## <a name="comparison-of-models"></a>Modellerin karşılaştırması

|**Kota**  |**Bir veritabanı üzerinde sağlanan aktarım hızı**  |**Bir kapsayıcı sağlanan aktarım hızı**|
|---------|---------|---------|
|En az ru |400 (sonra ilk dört kapsayıcılar, her ek kapsayıcı en az 100 RU/sn gerektirir.) |400|
|Kapsayıcı başına en az ru|100|400|
|1 GB depolama alanı kullanmak için gereken en düşük ru|40|40|
|En fazla ru|Veritabanında sınırsız|Kapsayıcıda sınırsız|
|RUs atanan/kullanılabilir belirli bir kapsayıcı|Hiçbir garanti eder. Belirli bir kapsayıcıya atanan RU - bölüm anahtarları paylaşan aktarım hızı, iş yükü dağılımını kapsayıcı sayısı kapsayıcıların seçimi gibi özellikleri bağlıdır. |Kapsayıcı üzerinde yapılandırılmış tüm RU kapsayıcı için özel olarak ayrılmıştır.|
|Bir kapsayıcı için en fazla depolama|Sınırsız|Sınırsız|
|Bir kapsayıcının mantıksal bölüm başına en fazla aktarım hızı|10 bin ru|10 bin ru|
|Bir kapsayıcının mantıksal bölüm başına en fazla depolama (veri + dizin)|10 GB|10 GB|

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md)
* Bilgi [bir Azure Cosmos kapsayıcısında aktarım hızını sağlamasını yapma](how-to-provision-container-throughput.md)
* Bilgi [bir Azure Cosmos veritabanı aktarım hızını sağlamasını yapma](how-to-provision-database-throughput.md)

