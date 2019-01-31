---
title: Veritabanı işlemleri ve Azure Cosmos DB'de iyimser eşzamanlılık denetimi
description: Bu makalede veritabanı işlemleri ve Azure Cosmos DB'de iyimser eşzamanlılık denetimi
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 044fe3de265d298ecd366a50b9db77eeea32bbb7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457947"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>İşlemler ve iyimser eşzamanlılık denetimi

Veritabanı işlemleri verileri eşzamanlı değişikliklerle başa çıkmak için bir güvenli ve öngörülebilir programlama modeli sağlar. Geleneksel ilişkisel veritabanları, SQL Server gibi tetiklenir ve/veya depolanan yordamları kullanarak iş mantığı yazmanıza izin, doğrudan veritabanı altyapısının içinde yürütme için sunucuya gönderin. İki farklı programlama dilleri - ile (işlem olmayan) uygulama programlama dili gibi JavaScript, Python, işlem için gerekli olan geleneksel veri tabanlarına C#, Java vb. ve işlem programlama dili (T-SQL) olan yerel olarak veritabanı tarafından yürütülen.

Anlık görüntü yalıtımıyla ACID (kararlılık, tutarlılık, yalıtım, dayanıklılık) uyumlu işlemler tam veritabanı altyapısı, Azure Cosmos DB destekler. Tüm veritabanı işlemlerinin kapsamı içinde bir kapsayıcının mantıksal bölüm işlemsel olarak bölümün çoğaltması tarafından barındırılır veritabanı altyapısının içinde yürütülür. Bu işlemlerin her ikisi de dahil (mantıksal bölüm içindeki bir veya daha fazla öğe güncelleştirme) yazma ve okuma işlemleri. Aşağıdaki tabloda, farklı işlemler ve işlem kapsamı türleri gösterilmektedir:

| **İşlem**  | **İşlem türü** | **Tek veya çoklu öğe işlem** |
|---------|---------|---------|
| (Ön/son tetikleyici) Ekle | Yazma | İşlem tek öğe |
| (Bir ön/son tetikleyici) Ekle | Yazma ve okuma | Birden çok öğe işlem |
| (Ön/son tetikleyici) değiştirin | Yazma | İşlem tek öğe |
| (Bir ön/son tetikleyici) değiştirin | Yazma ve okuma | Birden çok öğe işlem |
| Upsert (olmadan, bir ön/son tetikleyici) | Yazma | İşlem tek öğe |
| Upsert (ile ön/son tetikleyici) | Yazma ve okuma | Birden çok öğe işlem |
| (Ön/son tetikleyici) Sil | Yazma | İşlem tek öğe |
| (Bir ön/son tetikleyici) Sil | Yazma ve okuma | Birden çok öğe işlem |
| Saklı yordamı yürüt | Yazma ve okuma | Birden çok öğe işlem |
| Sistem tarafından başlatılan bir birleştirme yordamının yürütülmesi | Yazma | Birden çok öğe işlem |
| Sistem tarafından başlatılan bir öğe süre sonu (TTL) alarak öğeleri silme yürütme | Yazma | Birden çok öğe işlem |
| Okuma | Okuma | Tek öğeli işlem |
| Değişiklik akışı | Okuma | Birden çok öğe işlem |
| Sayfalandırılmış okuma | Okuma | Birden çok öğe işlem |
| Sayfalandırılmış sorgu | Okuma | Birden çok öğe işlem |
| UDF sayfalandırılmış sorgunun bir parçası olarak yürütün | Okuma | Birden çok öğe işlem |

## <a name="multi-item-transactions"></a>Birden çok öğe işlemleri

Azure Cosmos DB yazma depolanan yordamlar, ön/son tetikleyicileri, kullanıcı-tanımlı-işlevler (UDF'ler) ve JavaScript birleştirme yordamları sağlar. Azure Cosmos DB, veritabanı altyapısının içinde JavaScript yürütme yerel olarak destekler. Saklı yordamlar kaydedebilirsiniz, ön/son Tetikleyicileri ve kullanıcı-tanımlı-işlevler (UDF'ler) birleştirme yordamları bir kapsayıcı ve daha sonra bunları işlemsel olarak Azure Cosmos veritabanı altyapısının içinde yürütün. İçinde JavaScript uygulama mantığının yazma doğal ifade denetim akışı, değişken kapsamı, atama ve özel durum işleme temelleri JavaScript dilinde doğrudan veritabanı işlemleri içinde tümleştirme sağlar.

JavaScript tabanlı saklı yordamlar, Tetikleyiciler, UDF'ler ve birleştirme yordamları anlık görüntü yalıtımıyla çevresel ACID işlemi içinde mantıksal bölüm içindeki tüm öğelerde sarmalanır. Yürütme sürecinde JavaScript programı bir özel durum oluşturursa tüm işlem iptal edilir ve geri alındı. Sonuçta elde edilen programlama modeli basittir güçlü. JavaScript geliştiricileri yine de tanıdık dillerini kullanarak oluşturur ancak kalıcı bir programlama modeli ve kitaplık temelleri alın.

Doğrudan veritabanı altyapısının içinde JavaScript yürütme yeteneği, performans ve veritabanı işlemleri bir kapsayıcı öğeler karşı işlem tabanlı olarak yürütülmesini sağlar. Ayrıca, Azure Cosmos veritabanı altyapısı, yerel olarak JSON ve JavaScript desteklediğinden, bir uygulamanın türü sistemleri ve veritabanı arasında hiçbir empedans uyuşmazlığı yoktur.

## <a name="optimistic-concurrency-control"></a>İyimser eşzamanlılık denetimi 

İyimser eşzamanlılık denetimi, kayıp güncelleştirmelerden sağlar ve siler. Eş zamanlı, çakışan işlemleri normal kötümser öğesine sahip mantıksal bölüm tarafından barındırılan Veritabanı Altyapısı'nın kilitleme için tabi. Mantıksal bölüm içindeki bir öğenin en son sürüme güncelleştirmek iki eş zamanlı işlem denediğinizde, bunlardan birini kazanma ve diğer başarısız olur. Daha eski bir öğenin değerini okuma aynı anda aynı öğesi güncelleştirilmeye çalışılıyor, bir veya iki işlemleri daha önce, ancak veritabanı ya da daha önce okunan değerle çakışan bir işlem olup olmadığını gerçekten bilmez öğesinin en son değeri. Neyse ki, veritabanı altyapısının içinde işlem sınırı girin iki işlem izin vermeden önce bu durum, iyimser eşzamanlılık denetimini (OCC) ile'e algılanabilir. OCC yanlışlıkla başkaları tarafından yapılan değişikliklerin üzerine yazmasını verilerinizi korur. Ayrıca diğerleri kendi değişikliklerini yanlışlıkla üzerine yazılmasını engeller.

Bir öğenin eş zamanlı güncelleştirmelerin OCC için Azure Cosmos DB'nin iletişim protokolü katmanı tarafından tabi. Azure Cosmos veritabanı, (silinmesi ya da güncelleştirmekte) öğenin istemci tarafı sürümü Azure Cosmos kapsayıcısında öğenin sürümü ile aynı olmasını sağlar. Bu, başkalarının ve yazar tarafından yanlışlıkla üzerine yazma korunmasını garanti eder. Çok kullanıcılı bir ortamda, iyimser eşzamanlılık denetimi yanlışlıkla silme veya öğenin yanlış sürümünü güncelleştirme korur. Bu nedenle, öğeleri Meşhur "kayıp güncelleştirmesi" veya "kayıp Sil" sorunlara karşı korunur.

Bir Azure Cosmos kapsayıcısında depolanan her öğe sahip sistem tanımlı `_etag` özelliği. Değerini `_etag` otomatik olarak oluşturulur ve öğesi her güncelleştirildiğinde, sunucu tarafından güncelleştirilir. `_etag` ile sağlanan istemci IF-match istek üst öğeyi koşullu olarak güncelleştirilip güncelleştirilemeyeceğini karar vermek sunucu izin vermek için kullanılabilir. IF-match üst bilgisi değeri değeri ile eşleşen `_etag` sunucuda öğesi sonra güncelleştirilir. IF-match istek üstbilgisi değeri artık geçerli değilse, sunucu işlemi reddeder bir "HTTP 412 önkoşul hatası" yanıt iletisi. İstemci sunucuda öğesinin geçerli sürümü almak veya kendi Server'daki öğenin sürümü geçersiz kılmak için öğesini daha sonra yeniden getirmesi `_etag` öğe için bir değer. Ayrıca, `_etag` IF-none-match üst bilgisi ile yeniden getirmesi kaynağının gerekli olup olmadığını belirlemek için kullanılabilir. 

Öğesi her güncelleştirildiğinde öğenin _etag değeri değiştirir. Değiştirme öğesi işlemlerini için IF-match açıkça istek seçenekleri bir parçası olarak ifade edilmelidir. Örnek kodda bir örnek için bkz [GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs#L398-L446). `_etag` değerleri bir saklı yordam tarafından dokunulan tüm yazılmış öğeler için örtük olarak denetlenir. Herhangi bir çakışma algılandı saklı yordam işlem döndürülmesine neden olur ve bir özel durum oluşturur. Bu yöntemde tüm ya da hiçbir yazma saklı yordam içinde atomik olarak uygulanır. Bu güncelleştirmeleri yeniden uygulayın ve özgün istemci isteği yeniden denemek için uygulamaya bir sinyaldir.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla veritabanı işlemleri ve iyimser eşzamanlılık denetimi için aşağıdaki makalelere bakın:

- [Azure Cosmos veritabanı, kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md)
- [Tutarlılık düzeyleri](consistency-levels.md)
- [Çakışma türlerini ve çözümleme ilkeleri](conflict-resolution-policies.md)
