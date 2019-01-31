---
title: Azure Cosmos DB'yi dizine ekleme
description: Azure Cosmos DB'de dizinleme nasıl çalıştığını anlayın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: 9e1ae9faecea67e1abb2a1b08d5641bc25c9f38a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469099"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>-Azure Cosmos DB'de dizinleme genel bakış

Azure Cosmos DB şemadan veritabanıdır ve şema ya da dizin yönetimiyle ilgilenmenize gerek kalmadan uygulamanızı üzerinde hızla yineleme olanak tanır. Varsayılan olarak, Azure Cosmos DB otomatik olarak tüm öğelerini kapsayıcınızda şema veya ikincil dizinler geliştiricilerden gerek kalmadan oluşturur.

## <a name="items-as-trees"></a>Öğeleri olarak ağaçları

JSON belgeleri olarak bir kapsayıcı içindeki öğeleri yansıtma ve bunları ağaçları temsil eden, Azure Cosmos DB hem yapısını hem de örnek değerleri öğeler arasında birleştirme kavramını normalleştirir bir **yolu yapısı'dinamik olarak kodlanmış** . Bu gösteriminde bir ağaç düğümünü hem özellik adlarını ve değerlerini içeren bir JSON belgesinde her etiket haline gelir. Ağacın bırakır gerçek değerleri içeren ve Ara düğümleri şema bilgileri içerir. Aşağıdaki görüntüde oluşturulan iki için ağaçları temsil eden bir kapsayıcı içindeki öğeleri (1 ve 2):

![Bir Azure Cosmos kapsayıcısında iki farklı öğe ağacı temsili](./media/index-overview/indexing-as-tree.png)

Sahte kök düğümü belge etiketleri için karşılık gelen gerçek düğümlerine üst öğe olarak oluşturulur. İç içe veri yapılarını hiyerarşi ağacında sürücü. Ara (örneğin, 0, 1,...) sayısal değerlerle etiketli yapay düğümler numaralandırmaları temsil etmek için kullanılan ve dizi dizinleri.

## <a name="index-paths"></a>Dizin yolları

Azure Cosmos DB, JSON belgeleri olarak öğeleri ve dizin olarak ağaçları projeleri. Ağaçtaki yolları için ilkeleri ayarlayabilirsiniz. Dahil etmek veya yolları dizine elmadan hariç tutmak seçebilirsiniz. Geliştirilmiş yazma performansı sunar ve dizin depolaması burada ahead sorgu desenleri bilinen senaryoları için daha düşük olabilir. Daha fazla bilgi için bkz. [dizin yolları](index-paths.md).

## <a name="indexing-under-the-hood"></a>Dizin oluşturma: Başlık altında

Azure Cosmos veritabanı, belirli yollarını dışlanacak yapılandırmadığınız sürece her bir ağaç yolu olduğu dizine veri için otomatik dizin oluşturma geçerlidir.

Azure Cosmos veritabanı kullanan her bir öğenin bilgileri depolamak ve sorgulamak için verimli gösterimi kolaylaştırmak için dizin veri yapısı ters. Dizin ağacı ile kapsayıcı içindeki öğeleri temsil eden ağaçları birleşimiyle oluşturulan bir belgedir. Dizin ağacında, yeni öğeler eklendiğinde veya var olan öğeleri kapsayıcıda güncelleştirilir zamanla artar. İlişkisel veritabanı dizini oluşturma aksine Azure Cosmos DB, yeni alanlar sunulan olarak sıfırdan dizinleme yeniden başlatmadığını yeni öğeleri mevcut yapıya eklenir. 

Terim adlı etiket ve konum değerleri içeren bir dizin girdisi dizin ağacında her bir düğümü olduğu ve aktarımlar adlı öğeleri kimliklerini. Aktarımlar içine süslü ayraçlar (örneğin {1,2}) ters dizin şekilde Document1 ve Document2 gibi öğelere karşılık gelen belirli bir etiket değeri içeren. Her şeyi büyük bir dizin içinde paketlenmiş şema etiketleri ve örnek değerleri eşit değerlendirmesini olduğu ile ilgili önemli bir çıkarımında olur. Bırakır hala bir örnek değer olmayan yinelenir, farklı şemasını etiketlerle öğeleri arasında farklı rolleri olabilir ancak değerin aynısıdır. Aşağıdaki görüntüde, ters farklı öğeler için dizin oluşturma gösterilmektedir:

![Başlık altında dizin, dizin ters](./media/index-overview/inverted-index.png)

> [!NOTE]
> Ters dizini bir arama motoru bilgi alma etki alanında kullanılan dizin oluşturma yapıları benzer görünebilir. Bu yöntemde Azure Cosmos DB veritabanınıza şema yapısını bağımsız olarak herhangi bir öğe için aranacak sağlar.

Normalleştirilmiş yol için dizin değeri, değerin türü bilgilerle birlikte tüm kökünden ileriye doğru yol kodlar. Yolunu ve değer aralığı gibi uzamsal tür dizin çeşitli türleri sağlamak için kodlanır. Değer kodlama, benzersiz bir değer veya bir dizi yol bileşimi sağlamak için tasarlanmıştır.

## <a name="querying-with-indexes"></a>Dizinler ile sorgulama

Ters dizini, hızlı sorgu koşulu karşılayan belgelerini tanımlamak için bir sorgu sağlar. Birörnek yolları açısından hem şema hem de örnek değerleri kabul ederek, ters ayrıca bir ağaç dizinidir. Bu nedenle, dizin ve sonuçları için geçerli bir JSON belgesi sıralanabilir ve ağaç gösteriminde döndürülen gibi belgeleri olarak kendilerini döndürdü. Bu yöntem, ek sorgulamak için sonuçları recursing sağlar. Aşağıdaki görüntüde, dizin noktası sorguda bir örnek gösterilmektedir:  

![Noktası sorgusu örneği](./media/index-overview/index-point-query.png)

Bir aralık sorgusu için GermanTax sorgu işleme bir parçası olarak yürütülen bir kullanıcı tanımlı işlev var. Kullanıcı tanımlı işlevi sorguyu tümleştirilmiş zengin programlama mantığı sağlayan tüm kayıtlı, bir Javascript işlevidir. Aşağıdaki görüntüde, dizin içinde bir aralığı sorgusu bir örnek gösterilmektedir:

![Aralık sorgusu örneği](./media/index-overview/index-range-query.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma ilkesi](index-policy.md)
- [Dizin türleri](index-types.md)
- [Dizin yolları](index-paths.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
