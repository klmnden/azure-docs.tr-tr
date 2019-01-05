---
title: Azure Cosmos DB'deki yapay bölüm anahtarları
description: Azure Cosmos DB kapsayıcıları yapay bölüm anahtarlarını kullanmayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: mjbrown
ms.openlocfilehash: 37d220a13aec99de94afa3357db1462d11f8662c
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043385"
---
# <a name="create-a-synthetic-partition-key"></a>Yapay bölüm anahtarı oluşturma

Yüzlerce veya binlerce gibi birçok farklı değerlere sahip bir bölüm anahtarı için en iyi bir uygulamadır. Bu bölüm anahtarı değerlerine ile ilişkili öğeleri arasında verileri ve iş yükü'nın eşit olarak dağıtmak için kullanılan hedeftir. Yapay bölüm anahtarı, verilerinizi böyle bir özellik mevcut değilse oluşturulabilir. Aşağıdaki bölümlerde, kapsayıcınız için yapay bir bölüm anahtarı oluşturmak için birkaç temel teknikten açıklanmaktadır.

## <a name="concatenating-multiple-properties-of-an-item"></a>Birden çok öğenin özelliklerini birleştirme

Birden çok özellik değerleri ile birleştirerek tek bir yapay bir bölüm anahtarı oluşturabilir `partitionKey` özelliği. Bu anahtarları yapay anahtar olarak adlandırılır. Örneğin, aşağıdaki örnek belgeyi göz önünde bulundurun:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Önceki belge için bir seçenek kapsayıcınız cihaz kimliği ya da tarih bölümü istiyorsanız /deviceId veya /date bölüm anahtarı olarak ayarlamaktır. Bu iki değerden bir Sentetik birleştirmek için başka bir seçenektir `partitionKey` bölüm anahtarı olarak kullanılan özellik.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Gerçek zamanlı senaryolarda, bir veritabanında belgeler binlerce olabilir. böylece yapay anahtar el ile eklemek yerine, değerleri birleştirebilir ve belgelere yapay anahtar eklemek için istemci tarafı mantığını tanımlamanız gerekir.

## <a name="using-a-partition-key-with-random-suffix"></a>Rastgele son eki ile bir bölüm anahtarının kullanılması

İş yükü daha eşit bir şekilde dağıtmak için başka bir olası rastgele bir sayı bölüm anahtarı değeri sonuna eklenecek stratejisidir. Yazma işlemleri bölümler arasında dağıtılmasını öğeler paralel olarak gerçekleştirdiğiniz bu şekilde etkinleştirir.

Örneğin, bir bölüm anahtarı bir tarihi temsil ediyorsa, 1 ile 400 arasında rastgele bir sayı seçin ve tarih sonek olarak birleştirmek. Bu yöntem bölüm anahtarı değerlerine 2018-08-09.1, 2018-08-09.2 gibi vb. 2018-08-09.400 ile sonuçlanır. Bölüm anahtarı'nu rasgeleleştirilirken çünkü yazma işlemleri kapsayıcı üzerindeki her gün birden çok bölümler arasında eşit olarak yayılır. Bu yöntem, genel olarak daha yüksek iş hacmi ve daha iyi paralellik ile sonuçlanır.

## <a name="using-a-partition-key-with-pre-calculated-suffixes"></a>Önceden hesaplanmış öneklerle bir bölüm anahtarının kullanılması 

Stratejisi rasgeleleştirilirken yazma üretimi büyük ölçüde artırabilir ancak belirli bir öğeyi öğesi yazılırken kullanılan sonek değeri bilmiyor okumak zordur. Bireysel öğeleri okunmasını kolaylaştırmak için önceden hesaplanmış sonekleri stratejiyi kullanabilirsiniz. Öğeler bölümler arasında dağıtmak için rastgele bir sayı kullanmak yerine, sorgulamak istediğiniz şeye göre hesaplayan bir sayı kullanın.

Burada bir kapsayıcı bir tarih bölüm anahtarı kullanan önceki örnekte, göz önünde bulundurun. Artık her öğenin erişilebilir bir Toplamıdır (araç kimlik numarası) özniteliği vardır ve çoğunlukla Toplamıdır, öğeleri tarihe ayrıca bulmak için sorguları çalıştırırsanız varsayalım. Uygulamanız için kapsayıcı öğe Yazar önce üzerinde Toplamıdır göre bir karma soneki hesaplamak ve bölüm anahtarı tarihe ekleyin. Hesaplama eşit olarak dağıtılmış, benzer şekilde rastgele stratejisi yöntemi tarafından üretilen sonuç 1 ile 400 arasında bir sayı üretebilir. Bölüm anahtarı değeri, sonra hesaplanan sonucu ile birleştirilmiş tarih olur.

Bu strateji ile bölüm anahtarı değerlerine ve bölümler arasında yazma eşit olarak yayılır. Belirli bir araç-kimlik-sayı için bölüm anahtarı değeri hesaplayabilir çünkü belirli bir öğe ve tarih kolayca okuyabilir. Bu yöntemin avantajı, tek sıcak bölüm anahtarı (tüm iş yükünün geçen bölüm anahtarı) oluşturma kaçınabilirsiniz içindir. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde bölümleme kavramı hakkında daha fazla bilgi edinebilirsiniz:

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md)
* Daha fazla bilgi edinin [Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* Bilgi [Cosmos kapsayıcısında aktarım hızını sağlamasını yapma](how-to-provision-container-throughput.md)
* Bilgi [Cosmos veritabanı aktarım hızını sağlamasını yapma](how-to-provision-database-throughput.md)
