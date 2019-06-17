---
title: Verileri ve iş yükü eşit bir şekilde dağıtmak için Azure Cosmos DB'de yapay bölüm anahtarı oluşturun.
description: Azure Cosmos kapsayıcılarınızı yapay bölüm anahtarlarını kullanmayı öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 1fd436746dcd2e93a1699ac5c68965213c74580e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65978875"
---
# <a name="create-a-synthetic-partition-key"></a>Yapay bölüm anahtarı oluşturma

Yüzlerce veya binlerce gibi birçok farklı değerlere sahip bir bölüm anahtarı için en iyi uygulamadır. Bu bölüm anahtarı değerlerine ile ilişkili öğeleri arasında verileri ve iş yükü'nın eşit olarak dağıtmak için kullanılan hedeftir. Böyle bir özellik verilerinizi mevcut değilse oluşturabilirsiniz bir *yapay bölüm anahtarı*. Bu belgede, Cosmos kapsayıcınız için yapay bir bölüm anahtarı oluşturmak için birkaç temel teknikten açıklanmaktadır.

## <a name="concatenate-multiple-properties-of-an-item"></a>Birden çok öğenin özelliklerini birleştirme

Birden çok özellik değerleri ile birleştirerek tek bir yapay bir bölüm anahtarı oluşturabilir `partitionKey` özelliği. Bu anahtarları yapay anahtar olarak adlandırılır. Örneğin, aşağıdaki örnek belgeyi göz önünde bulundurun:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Önceki belge için bir seçenek /deviceId veya /date bölüm anahtarı olarak ayarlamaktır. Kapsayıcınız cihaz kimliği ya da tarih bölümü istiyorsanız bu seçeneği kullanın. Bu iki değerden bir Sentetik birleştirmek için başka bir seçenektir `partitionKey` bölüm anahtarı olarak kullanılan özellik.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Gerçek zamanlı senaryolarda binlerce veritabanında öğeleri olabilir. Yapay anahtar el ile eklemek yerine, değerleri birleştirebilir ve yapay anahtar Cosmos kapsayıcılarınızı öğeleri eklemek için istemci tarafı mantığını tanımlayın.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Bir bölüm anahtarı ile rastgele bir sonek kullanın

İş yükü daha eşit bir şekilde dağıtmak için başka bir olası rastgele bir sayı bölüm anahtarı değeri sonuna eklenecek stratejisidir. Bu şekilde öğeleri dağıttığınızda, bölümler arasında paralel yazma işlemleri gerçekleştirebilirsiniz.

Bir bölüm anahtarı bir tarihi temsil edip etmediğini bir örnektir. 1 ile 400 arasında rastgele bir sayı seçin ve tarih sonek olarak birleştirmek. Bölüm anahtarı değerleri gibi bu yöntem sonuçlanır `2018-08-09.1`,`2018-08-09.2`ve benzeri aracılığıyla `2018-08-09.400`. Bölüm anahtarı rastgele yap çünkü yazma işlemleri kapsayıcı üzerindeki her gün birden çok bölümler arasında eşit olarak yayılır. Bu yöntem, genel olarak daha yüksek iş hacmi ve daha iyi paralellik ile sonuçlanır.

## <a name="use-a-partition-key-with-pre-calculated-suffixes"></a>Bir bölüm anahtarı ile önceden hesaplanmış soneklerini kullanacak 

Rastgele soneki stratejisi yazma üretimi büyük ölçüde artırabilir, ancak belirli bir öğeyi okuma zordur. Öğe yazdığınız zaman kullanıldığını sonek değeri bilinmiyor. Bireysel öğeleri okunmasını kolaylaştırmak için önceden hesaplanmış sonekleri stratejisi kullanın. Öğeler bölümler arasında dağıtmak için rastgele bir sayı kullanmak yerine, sorgulamak istediğiniz şeye göre hesaplanan bir numara kullanın.

Önceki örnekte, burada bir kapsayıcı bölüm anahtarı olarak bir tarih kullanır göz önünde bulundurun. Şimdi her bir öğe olduğunu varsayalım. bir `Vehicle-Identification-Number` (`VIN`) erişmek için istediğimiz özniteliği. Ayrıca, genellikle öğeleri bulmak için sorguları çalıştırma varsayalım `VIN`, tarih yanı sıra. Uygulamanız için kapsayıcı öğe Yazar önce üzerinde Toplamıdır göre bir karma soneki hesaplamak ve bölüm anahtarı tarihe ekleyin. Hesaplama, eşit olarak dağıtılır ve 1 ile 400 arasında bir sayı üretebilir. Bu sonuç rastgele soneki stratejisi yöntemi tarafından üretilen sonuçlar benzer. Bölüm anahtarı değeri, sonra hesaplanan sonucu ile birleştirilmiş tarihtir.

Bu strateji ile bölüm anahtarı değerlerine ve bölümler arasında yazma eşit olarak yayılır. Belirli bir bölüm anahtarı değeri hesaplayabilir olduğundan kolayca maddeyi ve tarih okuyabilirsiniz `Vehicle-Identification-Number`. Bu yöntemin avantajı, tek sıcak bölüm anahtarı, başka bir deyişle, tüm iş yükünün geçen bölüm anahtarı oluşturma kaçınabilirsiniz içindir. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde bölümleme kavramı hakkında daha fazla bilgi edinebilirsiniz:

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
* Kullanma hakkında daha fazla bilgi edinin [Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızını](set-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).
