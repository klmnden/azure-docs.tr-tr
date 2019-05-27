---
title: Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızı
description: Azure Cosmos kapsayıcılarınızdaki ve veritabanları için sağlanan aktarım hızı ayarlama konusunda bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rimman
ms.openlocfilehash: 598a1562127a67c78f67cdd02b00d83d4a606739
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953549"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Kapsayıcı ve veritabanlarına aktarım hızı sağlama

Bir Azure Cosmos veritabanı, bir dizi kapsayıcıları için yönetim birimidir. Bir veritabanı şemadan kapsayıcıların bir kümesinden oluşur. Bir Azure Cosmos kapsayıcı hem aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı makineler bir Azure bölgesi içinde bir dizi yatay olarak bölümlenir ve Azure Cosmos hesabınızla ilişkili tüm Azure bölgelerine dağıtılır.

Azure Cosmos DB ile aktarım hızını iki ayrıntı düzeylerinde sağlayabilirsiniz:
 
- Azure Cosmos kapsayıcıları
- Azure Cosmos veritabanları

## <a name="set-throughput-on-a-container"></a>Bir kapsayıcıda aktarım hızı ayarlama  

Bir Azure Cosmos kapsayıcıdaki sağlanmış olan aktarım hızı bu kapsayıcı için özel olarak ayrılmış. Kapsayıcı, sağlanan aktarım hızı her zaman alır. Bir kapsayıcı sağlanan aktarım hızını finansal SLA'ları tarafından desteklenmektedir. Bir kapsayıcıdaki aktarım hızı yapılandırma konusunda bilgi için bkz: [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).

Ayar, bir kapsayıcıda sağlanan aktarım hızı en sık kullanılan bir seçenektir. Kullanarak herhangi bir üretilen iş miktarını sağlayarak bir kapsayıcının aktarım hızını esnek olarak ölçeklendirebilirsiniz [istek birimi (RU)](request-units.md). 

Bir Azure Cosmos kapsayıcıdaki sağlanmış olan aktarım hızı, tüm mantıksal bölümler kapsayıcının olarak eşit dağıtılır. Aktarım mantıksal bölümler için seçmeli olarak belirtemezsiniz. Bir fiziksel bölüm tarafından barındırılan bir veya daha fazla mantıksal bölümleri olan bir kapsayıcı olduğundan, fiziksel bölümler yalnızca kapsayıcıya ait ve kapsayıcıdaki sağlanmış olan aktarım hızı destekler. 

Mantıksal bölüm üzerinde çalışan bir iş yükünü birden çok, mantıksal birime ayrılmış aktarım hızı kullanırsa, işlemlerinizin oranı sınırlı alın. Hız sınırlama ortaya çıktığında, tüm kapsayıcı için sağlanan aktarım hızı artırabilir veya işlemleri yeniden deneyin. Bölümleme hakkında daha fazla bilgi için bkz: [mantıksal bölümler](partition-data.md).

Kapsayıcı için garantili performans istediğinizde işleme kapsayıcı ayrıntı düzeyinde yapılandırmanızı öneririz.

Aşağıdaki resimde, bir fiziksel bölüm bir veya daha fazla mantıksal bölümleri olan bir kapsayıcı nasıl barındıran gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Bir veritabanında aktarım hızı ayarlama

Bir Azure Cosmos veritabanı aktarım hızını sağladığınızda, aktarım hızı veritabanında tüm kapsayıcılar arasında paylaşılır. Belirli kapsayıcılarına veritabanında bir sağlanan aktarım hızı belirttiyseniz, bir özel durumdur. Veritabanı düzeyinde sağlanan aktarım hızı, kapsayıcılar arasında paylaşımı makine kümesi üzerinde bir veritabanını barındırmak için benzer. Tüm kapsayıcıları bir veritabanı içinde bir makinede kullanılabilir kaynakları paylaştığından, doğal olarak tahmin edilebilir performans üzerinde belirli bir kapsayıcı almıyor. Bir veritabanı üzerinde sağlanan aktarım hızı yapılandırma konusunda bilgi için bkz: [yapılandırma sağlanan aktarım hızını bir Azure Cosmos veritabanı](how-to-provision-database-throughput.md).

Bir Azure Cosmos veritabanı ayarı aktarım hızını, her zaman bu veritabanı için sağlanan aktarım hızı alma garanti eder. Veritabanı içinde tüm kapsayıcıları sağlanan aktarım hızı paylaştığından, Azure Cosmos DB veritabanındaki belirli bir kapsayıcı için öngörülebilir bir üretilen iş hacmi garanti sağlamaz. Belirli bir kapsayıcı alabileceği aktarım hızı kısmı bağlıdır:

* Kapsayıcı sayısı.
* Bölüm anahtarları çeşitli kapsayıcılar için seçimi.
* İş yükü kapsayıcıların çeşitli mantıksal bölümler arasında dağıtımı. 

Birden çok kapsayıcıya aktarım hızı paylaşmak istediğiniz, ancak belirli bir kapsayıcı için aktarım hızı ayrılması istemiyorsanız, aktarım hızı bir veritabanı üzerinde yapılandırmanızı öneririz. 

Aşağıdaki örnekler, burada, aktarım hızı ve veritabanı düzeyinde sağlamak tercih edilen göstermektedir:

* Bir veritabanının sağlanan aktarım hızı kapsayıcıları kümesi paylaşımı, çok kiracılı bir uygulama için yararlıdır. Her bir kullanıcı farklı bir Azure Cosmos kapsayıcı tarafından temsil edilebilir.

* MongoDB veya Azure Cosmos DB'ye bir kümesi sanal makinelerinin ya da şirket içi fiziksel sunucuları üzerinde barındırılan, Cassandra gibi bir NoSQL veritabanı geçiş yaptığınızda, kapsayıcıları kümesi arasında bir veritabanının sağlanan aktarım hızı paylaşımı yararlıdır. Sağlanan aktarım hızı, Azure Cosmos veritabanı bir mantıksal eşdeğeri olarak yapılandırılmış ancak daha düşük maliyetli ve esnek, MongoDB veya Cassandra kümenizi işlem kapasitesinin düşünün.  

Sağlanan aktarım hızına sahip bir veritabanı içinde oluşturulan tüm kapsayıcıları ile oluşturulması gereken bir [bölüm anahtarı](partition-data.md). Belirli bir anda zaman, tüm mantıksal bölümler bu kapsayıcı, bir veritabanı içinde bir kapsayıcıya ayrılan aktarım hızı dağıtılır. Aktarım hızı seçmeli olarak bir veritabanı üzerinde yapılandırılan ve sağlanan aktarım hızı paylaşan kapsayıcılar varsa, belirli bir kapsayıcı veya mantıksal bir bölümü için uygulanamıyor. 

Bir mantıksal bölüm iş yüküne en fazla aktarım hızı belirli bir mantıksal birime atanan kullanırsa, işlemlerinizin oranı sınırlı. Hız sınırlama ortaya çıktığında, tüm veritabanı verimliliğini artırmak veya işlemleri yeniden deneyin. Bölümleme hakkında daha fazla bilgi için bkz: [mantıksal bölümler](partition-data.md).

Bir veritabanı için sağlanan işleme paylaşan farklı kapsayıcılar ait birden çok mantıksal bölümleri tek bir fiziksel bölüm üzerinde barındırılabilir. Tek bir mantıksal bölüm kapsayıcısının her zaman bir fiziksel bölüm kapsamlı sırada *"L"* mantıksal bölümler arasında *"C"* sağlanan aktarım hızı bir veritabanını paylaşan kapsayıcıları eşlenen ve barındırılan *"R"* fiziksel bölümler. 

Aşağıdaki resimde, bir veritabanı içinde farklı kapsayıcılar ait bir veya daha fazla mantıksal bölümler bir fiziksel bölüm nasıl barındırabilir gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Bir veritabanı ve kapsayıcı aktarım hızı ayarlama

İki model birleştirebilirsiniz. Hem veritabanı hem de kapsayıcı sağlama işleme izin verilmiyor. Aşağıdaki örnek, bir Azure Cosmos veritabanı ve kapsayıcı aktarım hızına gösterilmektedir:

* Adlı bir Azure Cosmos veritabanı oluşturabileceğiniz *Z* sağlanan aktarım hızı ile *"K"* RU. 
* Ardından, adlı beş kapsayıcıları oluşturma *A*, *B*, *C*, *D*, ve *E* veritabanı içinde.
* Açıkça yapılandırabileceğiniz *"P"* RU adlı kapsayıcıda sağlanan aktarım hızının *B*.
* *"K"* RU aktarım hızı arasında dört kapsayıcımız paylaşılan *A*, *C*, *D*, ve *E*. Aktarım hızı için kullanılabilir miktarda *A*, *C*, *D*, veya *E* değişir. Tek tek her kapsayıcının aktarım hızını SLA'sı vardır.
* Adlı kapsayıcıyı *B* kesindir *"P"* RU işleme her zaman. Bu SLA'ları tarafından desteklenmektedir.

## <a name="update-throughput-on-a-database-or-a-container"></a>Bir veritabanı veya bir kapsayıcının aktarım hızını güncelleştir

Bir Azure Cosmos kapsayıcısı veya bir veritabanı oluşturduktan sonra sağlanan aktarım hızı güncelleştirebilirsiniz. Veritabanı veya kapsayıcı yapılandırabilirsiniz en fazla sağlanan aktarım hızı sınırı yoktur. En düşük sağlanan aktarım hızı, aşağıdaki etkenlere bağlıdır: 

* Şimdiye kadar kapsayıcıda depoladığınız en yüksek boyut
* Kapsayıcıdaki her zamankinden sağlama en yüksek aktarım
* Paylaşılan aktarım hızı olan bir veritabanında oluşturduğunuz Azure Cosmos kapsayıcı sayısı. 

SDK'ları kullanarak program aracılığıyla bir kapsayıcı veya bir veritabanının en düşük aktarım hızı alma veya Azure portalında değerini görüntüleyin. .NET SDK'sı kullanırken [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) yöntemi, sağlanan aktarım hızı değerinde ölçeklendirmenize olanak sağlar. Java SDK'sı kullanırken [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) yöntemi, sağlanan aktarım hızı değerinde ölçeklendirmenize olanak sağlar. 

.NET SDK'sı kullanırken [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) yöntemi bir kapsayıcı veya bir veritabanının en düşük aktarım hızı almanızı sağlar. 

Herhangi bir zamanda, bir kapsayıcı veya bir veritabanı sağlanan aktarım hızını ölçeklendirebilirsiniz. 

## <a name="comparison-of-models"></a>Modellerin karşılaştırması

|**Parametre**  |**Bir veritabanı üzerinde sağlanan aktarım hızı**  |**Bir kapsayıcı sağlanan aktarım hızı**|
|---------|---------|---------|
|En az ru |400 (sonra ilk dört kapsayıcılar, her ek kapsayıcı en az 100 RU / saniye gerektirir.) |400|
|Kapsayıcı başına en az ru|100|400|
|1 GB depolama alanı kullanmak için gereken en düşük ru|40|40|
|En fazla ru|Veritabanında sınırsız.|Kapsayıcıda sınırsız.|
|RUs atanan veya belirli bir kapsayıcı için kullanılabilir|Hiçbir garanti eder. Belirli bir kapsayıcıya atanan RU özelliklerine bağlıdır. Bölüm anahtarlarını işleme paylaşan kapsayıcılar, kapsayıcı sayısı ve iş yükü dağılımını seçimi özellikler olabilir. |Kapsayıcı üzerinde yapılandırılmış tüm RU kapsayıcı için özel olarak ayrılmıştır.|
|Bir kapsayıcı için en fazla depolama|Sınırsız.|Sınırsız.|
|Bir kapsayıcının mantıksal bölüm başına en fazla aktarım hızı|10 bin ru|10 bin ru|
|Bir kapsayıcının mantıksal bölüm başına en fazla depolama (veri + dizin)|10 GB|10 GB|

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).

