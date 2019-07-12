---
title: Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB ile çalışma
description: Bu makalede, saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri Azure Cosmos DB'de gibi kavramlar tanıtılmaktadır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 53ff318dcc034fb11e2d554f9ad8e8814eb32879
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672580"
---
# <a name="stored-procedures-triggers-and-user-defined-functions"></a>Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler

Azure Cosmos DB, JavaScript dil ile tümleşik, işlem yürütülmesini sağlar. Azure Cosmos DB SQL API'sini kullanarak, yazabileceğiniz **saklı yordamlar**, **Tetikleyicileri**, ve **kullanıcı tanımlı işlevler (UDF'ler)** JavaScript dilinde. Veritabanı altyapısının içinden yürütülen JavaScript'te mantığınızı yazabilirsiniz. Oluşturabilir ve tetikleyicileri, saklı yordamlar ve UDF'ler kullanarak yürütme [Azure portalında](https://portal.azure.com/), [JavaScript dil tümleşik Azure Cosmos DB'de sorgu API'si](javascript-query-api.md) veya [Cosmos DB SQL API istemcisi SDK'ları](how-to-use-stored-procedures-triggers-udfs.md).

## <a name="benefits-of-using-server-side-programming"></a>Sunucu tarafı programlama kullanmanın avantajları

Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) JavaScript dilinde yazılırken zengin uygulamalar oluşturmanıza olanak tanır ve aşağıdaki avantajları sahiptirler:

* **Yordam mantıksal:** JavaScript hızlı iş mantığı için zengin ve tanıdık bir arabirim sunan bir üst düzey programlama dili olarak. Bir dizi veri üzerinde karmaşık işlemleri gerçekleştirebilirsiniz.

* **Atomik işlemler:** Azure Cosmos DB, tek bir saklı yordam ya da bir tetikleyici içinde gerçekleştirilen veritabanı işlemleri atomiktir garanti eder. Tüm işlemlerin başarılı ya da bunların hiçbiri başarılı tek bir toplu iş ilgili işlemlere birleştirerek uygulama atomik bu işlevselliği sağlar.

* **Performans:** JSON verilerini doğası gereği JavaScript dil türü sisteme eşlenir. Birkaç en iyi duruma getirme gibi JSON belgelerinin arabellek havuzu ve yürütme kodu için istek üzerine ulaşılabilir yönetilmelerini yavaş gerçekleştirmesi için bu eşleme sağlar. İş mantığı içeren veritabanına aktarma ile ilgili diğer performans avantajı vardır:

   * *Toplu işleme:* Grup ekleme gibi işlemleri ve bunları toplu olarak gönderin. Ağ trafiğinin gecikme süresini maliyetleri ve ayrı işlemler oluşturmak için mağaza ek yükünü önemli ölçüde azaltılır.

   * *Önceden derleme:* Saklı yordamlar, tetikleyiciler ve UDF'ler her betik çağırmayı zamanında derleme maliyet önlemek için örtük olarak bayt kod biçimine önceden derlenmiş. Önceden derleme nedeniyle saklı yordamlar çağırmayı hızlı ve düşük bir ayak izine sahip.

   * *Sıralama:* Bazen operations birini gerçekleştirebilir tetikleyici bir mekanizma gerekir veya verilere ek güncelleştirmeler. Kararlılık yanı sıra, ayrıca performans avantajları vardır sunucu tarafında yürütülürken.

* **Saklama:** Saklı yordamlar, tek bir yerde mantıksal gruplandırmak için kullanılabilir. Kapsülleme veri öğesinden bağımsız olarak uygulamalarınızı gelişmesi sağlar verileri en üstünde bir soyutlama katmanı ekler. Bu Soyutlama Katmanı, doğrudan uygulamanıza ek mantık ekleme yönetmek zorunda olmadığınız ve şemasız verileri gerektiğinde yararlıdır. Özet, Canlı betikleri erişimden kolaylaştırarak inovasyonu verilerin güvenliğini sağlar.

> [!TIP]
> Saklı yordamlar, yazma yoğunluklu ve bölüm anahtarı değeri arasında bir işlem gerektiren işlemler için en uygun seçenektir. Saklı yordamlar kullanma karar verirken, yazma olası en uzun süreyi Kapsüllenen etrafında iyileştirin. Genel olarak bakıldığında, saklı yordamlar, çok sayıda okuma veya sorgu işlemleri yapmak için en verimli yol değildir, yordamları okuma istemciye döndürmek için çok sayıda batch istenen avantajı sunulur, bu saklı kullanma biçimde değil. En iyi performans için istemci-tarafı Cosmos SDK'sını kullanarak üzerinde bu okuma yoğunluklu işlemleri yapılması gerekir. 

## <a name="transactions"></a>İşlemler

Tipik bir veritabanındaki işlem, tek bir mantıksal birim iş olarak gerçekleştirilen işlemleri öğesinin bir dizisi olarak tanımlanabilir. Her işlem sağlar **özelliği ACID garantileri**. ACID anlamına gelen iyi bilinen bir kısaltma şöyledir: **A**tomicity, **C**uyumluluk, **miyim**solation, ve **D**urability. 

* Kararlılık, tüm bir işlem içinde gerçekleştirilen işlemleri tek bir birim ve ya da tümü ölçümlerimizde, kabul edilir ya da bunların hiçbiri'nın olmasını sağlar. 

* Tutarlılık veri işlemleri arasında her zaman geçerli bir durumda olduğundan emin olur. 

* Yalıtım iki işlem birbiriyle müdahale – kullanılabilecek birden fazla yalıtım düzeyleri uygulama ihtiyaçlarına göre birçok ticari sisteme sağlamak garanti eder. 

* Dayanıklılık, bir veritabanında kaydedilen herhangi bir değişikliği her zaman mevcut olmasını sağlar.

Azure Cosmos DB'de JavaScript çalışma zamanı veritabanı altyapısının içinden barındırılır. Bu nedenle, veritabanı oturumu ile aynı kapsamda saklı yordamlar ve tetikleyiciler içinde yapılan istekleri yürütün. Bu özellik bir saklı yordam ya da bir tetikleyici parçası olan tüm işlemler için ACID özellikleri sağlamak Azure Cosmos DB sağlar. Örnekler için bkz [işlemleri uygulamak üzere nasıl](how-to-write-stored-procedures-triggers-udfs.md#transactions) makalesi.

### <a name="scope-of-a-transaction"></a>Bir işlem kapsamı

Ardından bir saklı yordam bir Azure Cosmos kapsayıcı ile ilişkili ise, saklı yordamı bir mantıksal bölüm anahtarı işlem kapsamı içinde yürütülür. Her bir saklı yordam yürütme işlemi kapsamına karşılık gelen bir mantıksal bölüm anahtarı değerini içermelidir. Daha fazla bilgi için [Azure Cosmos DB bölümleme](partition-data.md) makalesi.

### <a name="commit-and-rollback"></a>Kaydetme ve geri alma

İşlem, yerel olarak Azure Cosmos DB JavaScript programlama modeline tümleştirilmiştir. Bir JavaScript işlevi içinde tüm işlemleri otomatik olarak tek bir işlem altında sarmalanır. JavaScript mantığının bir saklı yordam içinde özel durumlar tamamlanırsa, işlem içinde tüm işlemleri veritabanına kararlıyız. Deyimleri ister `BEGIN TRANSACTION` ve `COMMIT TRANSACTION` Azure Cosmos DB'de örtük (tanıdık bir ilişkisel veritabanları). Komut dosyası özel durumlar varsa, Azure Cosmos DB JavaScript çalışma zamanı tüm işlemin döndürülmesine neden olur. Bu nedenle, bir özel durum etkili bir şekilde değerine eşdeğer olan bir `ROLLBACK TRANSACTION` Azure Cosmos DB'de.

### <a name="data-consistency"></a>Veri tutarlılığı

Saklı yordamları ve tetikleyicileri, bir Azure Cosmos kapsayıcı birincil Çoğaltmada her zaman yürütülür. Bu özellik, saklı yordamlar teklifinden okuyan sağlar [güçlü tutarlılık](consistency-levels-tradeoffs.md). Kullanıcı tanımlı işlevleri kullanarak sorguları, birincil veya ikincil bir çoğaltmaya üzerinde çalıştırılabilir. Bu arada, salt okunur mantığı en iyi uygulama tarafından mantıksal uygulanır ve kullanarak sorgular saklı yordamları ve Tetikleyicileri işlem yazma desteklemeye yöneliktir – [Azure Cosmos DB SQL API SDK'ları](sql-api-dotnet-samples.md), yardımcı olacak saturate Veritabanı aktarım hızını. 

## <a name="bounded-execution"></a>Sınırlanmış yürütme

Tüm Azure Cosmos DB işlemler belirtilen zaman aşımı süresi içinde tamamlamanız gerekir. Bu kısıtlama için JavaScript işlevleri - geçerli saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler. Bir işlem bu süre sınırı içinde tamamlanmazsa, işlem geri alındı.

JavaScript işlevlerinizin süre sınırı içinde son veya toplu iş/sürdürme yürütme için bir devamlılık tabanlı modeli uygulamak ya da emin olabilirsiniz. Tüm saklı yordamları ve Tetikleyicileri süre sınırları işlemek için geliştirilmesini basitleştirmek için Azure Cosmos kapsayıcısı altında çalışır (örneğin, oluşturma, okuma, güncelleştirme ve öğelerin Sil) Bu işlem olacak olup olmadığını gösteren bir Boole değeri döndürür tamamlayın. Bu değer, false ise, kodun daha fazla zaman ya da sağlanan aktarım hızı yapılandırılan değerden tüketiyor olduğundan, yordamı yürütme sarmalamanız gerekir göstergesidir. İşlemler kuyruğa alınmış önceki ilk kabul edilmeyen deposu işlemi için saklı yordam sürede tamamlanır ve başka istek kuyruğa almadığından tamamlanması garanti edilir. Bu nedenle, işlemler, betiğin denetim akışı yönetmek için JavaScript'in geri çağırma kuralını kullanarak sıraya alınan birer birer olmalıdır. Komut dosyalarını bir sunucu tarafı ortamda çalıştırıldığından, kesin olarak yönetilir. Sürekli yürütme sınırlarının ihlal betikleri devre dışı olarak işaretlenmiş ve yürütülemez ve bunlar yürütme sınırları uymanız oluşturulması.

JavaScript işlevleri getirilmiştir konusu [sağlanan aktarım hızı kapasitesine](request-units.md). JavaScript işlevleri olabilecek çok sayıda kısa süre içinde istek birimleri kullanılarak son ve sağlanan aktarım hızı kapasite sınırına ulaşılırsa oranı-sınırlı olabilir. Betikleri ek işleme harcanan aktarım hızı yürütülen veritabanı işlemlerinin yanı sıra kullanmak bu veritabanı işlemleri istemciden aynı işlemleri yürütülürken değerinden biraz daha düşük maliyetli olmasına rağmen dikkat edin önemlidir.

## <a name="triggers"></a>Tetikleyiciler

Azure Cosmos DB, iki tür tetikleyiciyi destekler:

### <a name="pre-triggers"></a>Öncesi Tetikleyicileri

Azure Cosmos DB, bir Azure Cosmos DB öğe üzerinde bir işlemi gerçekleştirerek çağrıldı Tetikleyiciler sağlar. Örneğin, bir öğe oluştururken öncesi bir tetikleyici belirtebilirsiniz. Bu durumda, öğe oluşturulmadan önce ön tetikleyici çalıştırılır. Öncesi tetikleyici, giriş parametreleri bulunamaz. Gerekirse, istek nesnesi özgün istek gövdesinin güncelleştirmek için kullanılabilir. Tetikleyiciler kaydettiğinizde, kullanıcılar ile çalışabilmesi için işlemleri belirtebilir. Bir tetikleyici ile oluşturulmuşsa `TriggerOperation.Create`, bu tetikleyici, bir değiştirme işlemi kullanılarak izin verilmediği anlamına gelir. Örnekler için bkz [nasıl yazılacağını Tetikleyicileri](how-to-write-stored-procedures-triggers-udfs.md#triggers) makalesi.

### <a name="post-triggers"></a>Sonrası Tetikleyicileri

Benzer şekilde ön tetikleyicileri, sonrası tetikleyicileri, ayrıca bir Azure Cosmos DB öğe üzerinde bir işlem ile ilişkili ve bunlar tüm giriş parametrelerini gerekmez. Çalışan *sonra* işleminin tamamlandığını ve istemciye gönderilen yanıt iletisi erişebilir. Örnekler için bkz [nasıl yazılacağını Tetikleyicileri](how-to-write-stored-procedures-triggers-udfs.md#triggers) makalesi.

> [!NOTE]
> Kayıtlı Tetikleyicileri olduğunda otomatik olarak çalıştırma ilgili işlemleri (oluşturma / silme / değiştirmek / güncelleştirme) gerçekleşir. Bu işlem yürütülürken özel olarak çağrılması gerekir. Daha fazla bilgi için bkz. [Tetikleyicileri çalıştırma](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) makalesi.

## <a id="udfs"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı işlevler (UDF'ler) SQL API'sine sorgu dili sözdizimi genişletmek ve özel iş mantığı bir kolayca uygulamak için kullanılır. Yalnızca sorgulara çağrılabilir. UDF bağlam nesnesi için erişimi yoktur ve yalnızca JavaScript işlem olarak kullanılmak üzere tasarlanmıştır. Bu nedenle, UDF'ler ikincil çoğaltmalarda çalıştırılabilir. Örnekler için bkz [kullanıcı tanımlı işlevler yazma](how-to-write-stored-procedures-triggers-udfs.md#udfs) makalesi.

## <a id="jsqueryapi"></a>JavaScript dil ile tümleşik sorgu API'si

SQL API sorgu söz dizimi kullanarak sorgu göndermeye ek olarak [sunucu tarafı SDK](https://azure.github.io/azure-cosmosdb-js-server) SQL bilgileri olmadan bir JavaScript arabirimi kullanarak sorguları yapmanıza olanak tanır. JavaScript sorgu API'si koşul işlevler işlev çağrılarının dizisine geçirerek sorguları programlı olarak oluşturmanıza olanak tanır. Sorgular JavaScript çalışma zamanı tarafından ayrıştırılan ve verimli bir şekilde Azure Cosmos DB içinde yürütülür. JavaScript API sorgu desteği hakkında bilgi edinmek için [çalışma JavaScript dil ile tümleşik sorgu API'si](javascript-query-api.md) makalesi. Örnekler için bkz [saklı yordamları ve Tetikleyicileri Javascript sorgu API'si kullanarak yazma](how-to-write-javascript-query-api.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB'de ile aşağıdaki makaleleri kullanın ve yazma işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yazma](how-to-write-stored-procedures-triggers-udfs.md)

* [Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler kullanma](how-to-use-stored-procedures-triggers-udfs.md)

* [Çalışma JavaScript dil ile tümleşik sorgu API'si](javascript-query-api.md)
