---
title: "Performans Data Lake Store ile Powershell kullanmaya yönelik kılavuz ayarlama | Microsoft Docs"
description: "Data Lake Store ile Azure PowerShell kullanırken, performansı artırmak ipuçları"
services: data-lake-store
documentationcenter: 
author: stewu
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: stewu
ms.openlocfilehash: 63e1114d49b7bcb8910e8cd8205f10d1e8587f61
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="performance-tuning-guidance-for-using-powershell-with-azure-data-lake-store"></a>Performans Azure Data Lake Store ile PowerShell kullanmaya yönelik kılavuz ayarlama

Bu makalede, Data Lake Store ile çalışmak için PowerShell kullanırken bir daha iyi performans almak için ayarlanmış özellikler listelenmektedir:

## <a name="performance-related-properties"></a>Performansla ilgili özellikleri

| Özellik            | Varsayılan | Açıklama |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Bu parametre, her bir dosya karşıya yüklenirken veya indirilirken kaç paralel iş parçacığı kullanılacağını seçmenize olanak tanır. Bu sayı, dosya başına ayrılabilecek en fazla iş parçacığı temsil eder, ancak senaryonuza bağlı olarak daha az iş parçacığı alabilirsiniz (bir 1 KB dosya yüklüyorsanız, 20 iş parçacığı isteyin olsa bile Örneğin, bir iş parçacığı size).  |
| ConcurrentFileCount | 10      | Bu parametre özellikle klasörlerin karşıya yüklenmesi ve indirilmesi içindir. Bu parametre, karşıya yüklenebilecek veya indirilebilecek eş zamanlı dosya sayısını belirler. Bu sayı, karşıya veya aynı anda indirilen eşzamanlı dosya en büyük sayısını temsil eder, ancak senaryonuza bağlı olarak daha az eşzamanlılık alabilirsiniz (iki dosya yüklüyorsanız, sorun olsa bile Örneğin, iki eş zamanlı dosyaları karşıya elde için 15). |

**Örnek**

Bu komut, dosya başına 20 iş parçacığı ve 100 eşzamanlı dosya kullanarak dosyaları Azure Data Lake Store’dan kullanıcının yerel sürücüsüne indirir.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

## <a name="how-do-i-determine-the-value-for-these-properties"></a>Bu özellikler için değer nasıl belirlerim?

Sahip olabileceğiniz sonraki soruya için performans ile ilgili özellikler sağlamak için hangi değerin nasıl belirlenir. Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. Adım: Toplam iş parçacığı sayısını belirleyin** - İlk olarak kullanılması gereken toplam iş parçacığı sayısını hesaplayabilirsiniz. Genel kural olarak, her fiziksel çekirdek için altı iş parçacığı kullanmanız gerekir.

        Total thread count = total physical cores * 6

    **Örnek**

    PowerShell komutlarını 16 çekirdekli bir D14 VM’den çalıştırdığınız varsayılmıştır

        Total thread count = 16 cores * 6 = 96 threads


* **2. Adım: PerFileThreadCount değerini hesaplayın**  - Kendi PerFileThreadCount değerimizi dosyaların boyutunu temel olarak hesaplıyoruz. 2,5 GB'den küçük dosyalar için varsayılan olarak 10 yeterli olduğu için bu parametreyi değiştirmenize gerek yoktur. 2,5 GB'den büyük olan dosyalar için 10 iş parçacığı için ilk 2,5 GB temel olarak kullanın ve dosya boyutu 1 iş parçacığı her ek 256 MB Boyutunda artış için ekleyin. Birçok farklı boyutta dosya içeren bir klasörü kopyalıyorsanız dosyaları benzer dosya boyutları halinde gruplandırmayı göz önünde bulundurun. Dosya boyutlarının benzer olmaması performansın düşmesine neden olabilir. Benzer boyutlu dosyaları gruplandırmanız mümkün değilse PerFileThreadCount değerini en büyük dosya boyutuna göre ayarlamanız gerekir.

        PerFileThreadCount = 10 threads for the first 2.5 GB + 1 thread for each additional 256 MB increase in file size

    **Örnek**

    1 GB ile 10 GB arasında 100 dosyalarınız varsayıldığında, 10 GB en büyük dosya boyutu aşağıdaki gibi okuduğunuz eşitlik için kullanırız.

        PerFileThreadCount = 10 + ((10 GB - 2.5 GB) / 256 MB) = 40 threads

* **3. adım: ConcurrentFilecount hesaplamak** - toplam iş parçacığı sayısı kullanın ve ConcurrentFileCount hesaplamak için PerFileThreadCount dayalı deneylerin:

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Örnek**

    Kullandığımız örnek değerler temel alınmıştır

        96 = 40 * ConcurrentFileCount

    Sonuç olarak **ConcurrentFileCount** değeri **2,4** ve bunu **2**’ye yuvarlayabiliriz.

## <a name="further-tuning"></a>Daha fazla ayar

Üzerinde çalışılan dosyaların boyutu çeşitlilik gösterdiğinden daha fazla ayar yapmanız gerekebilir. Daha büyük ve 10 GB aralığında yakın iyi tüm veya çoğu dosyaların önceki hesaplama çalışır. Bunun yerine çoğu dosya küçük olacak şekilde birçok farklı dosya boyutu varsa PerFileThreadCount değerini azaltabilirsiniz. PerFileThreadCount değerini azaltarak ConcurrentFileCount değerini artırabiliriz. Bu nedenle, bizim dosyaları çoğunu 5 GB aralığında küçük olduğunu varsayıyoruz, biz bizim hesaplama Yinele:

    PerFileThreadCount = 10 + ((5 GB - 2.5 GB) / 256 MB) = 20

Bu nedenle, **ConcurrentFileCount** 4.8 olan 96/20 hale için yuvarlanmış **4**.

Dosya boyutlarınızın dağılımına göre **PerFileThreadCount** değerini artırıp azaltarak bu ayarları değiştirmeye devam edebilirsiniz.

### <a name="limitation"></a>Sınırlama

* **Dosya sayısı ConcurrentFileCount değerinden az**: Karşıya yüklemekte olduğunuz dosyaların sayısı hesapladığınız **ConcurrentFileCount** değerinden azsa **ConcurrentFileCount** değerini dosya sayısına eşit olacak şekilde azaltmanız gerekir. **PerFileThreadCount** değerini artırmak için kalan iş parçacıklarından dilediğinizi kullanabilirsiniz.

* **Çok fazla iş parçacığı**: Kümenizin boyutunu artırmadan iş parçacığı sayısını çok fazla artırırsanız düşük performans riskiyle karşılaşabilirsiniz. CPU’da bağlam değişimi sırasında çekişme sorunları olabilir.

* **Yetersiz eşzamanlılık**: Eşzamanlılık yeterli değilse kümeniz çok küçük olabilir. Daha fazla eşzamanlılık sağlar, kümedeki düğüm sayısını artırabilirsiniz.

* **Azaltma hataları**: Eşzamanlılığınız çok yüksekse azaltma hataları görebilirsiniz. Azaltma hataları görüyorsanız eşzamanlılığı azaltmanız veya bize ulaşmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store'u büyük veri gereksinimleri için kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)

