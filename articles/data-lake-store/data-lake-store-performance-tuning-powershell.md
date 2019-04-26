---
title: Performans ayarlama Kılavuzu, Azure Data Lake depolama Gen1 ile Powershell kullanma | Microsoft Docs
description: Azure PowerShell ile Azure Data Lake depolama Gen1 kullanırken performansı konusunda ipuçları
services: data-lake-store
documentationcenter: ''
author: stewu
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: stewu
ms.openlocfilehash: 1c554b0eee844a632e6412b6f8a285c7a2573326
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60195870"
---
# <a name="performance-tuning-guidance-for-using-powershell-with-azure-data-lake-storage-gen1"></a>Performans ayarlama Kılavuzu, Azure Data Lake depolama Gen1 ile PowerShell kullanma

Bu makalede Azure Data Lake depolama Gen1 ile çalışmak için PowerShell'i kullanırken daha iyi bir performans elde etmek için ayarlanabilecek özellikler listelenmektedir:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="performance-related-properties"></a>Performans ile ilgili Özellikler

| Özellik            | Varsayılan | Açıklama |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Bu parametre, her bir dosya karşıya yüklenirken veya indirilirken kaç paralel iş parçacığı kullanılacağını seçmenize olanak tanır. Bu sayı dosya ayrılabilecek en fazla iş parçacıklarını gösterir, ancak senaryonuza bağlı olarak daha az iş parçacığı elde edebilirsiniz (1 KB'lık dosya karşıya yüklüyorsanız 20 iş parçacığı isteseniz bile Örneğin, bir iş parçacığı olursunuz).  |
| ConcurrentFileCount | 10      | Bu parametre özellikle klasörlerin karşıya yüklenmesi ve indirilmesi içindir. Bu parametre, karşıya yüklenebilecek veya indirilebilecek eş zamanlı dosya sayısını belirler. Bu sayı en fazla karşıya yüklenen veya indirilen tek seferde eş zamanlı dosya sayısını temsil eder, ancak senaryonuza bağlı olarak daha az Eş zamanlılık elde edebilirsiniz (iki dosya yüklüyorsanız, isteseniz bile gibi iki eş zamanlı dosya yüklemeleri alma 15 için). |

**Örnek**

Bu komut, kullanıcının yerel sürücüsüne dosya ve 100 eşzamanlı dosya başına 20 iş parçacığı kullanarak Data Lake depolama Gen1 dosyaları indirir.

    Export-AzDataLakeStoreItem -AccountName <Data Lake Storage Gen1 account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

## <a name="how-do-i-determine-the-value-for-these-properties"></a>Bu özellikleri için değer nasıl belirlerim?

Sonraki soruya sahip olabileceğiniz için performans ile ilgili özellikler sağlamak için hangi değerin nasıl belirlenir. Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. adım: Toplam iş parçacığı sayısını belirleyin** -kullanılacak toplam iş parçacığı sayısını hesaplayarak başlamanız gerekir. Genel bir kural olarak her fiziksel çekirdek için altı iş parçacığı kullanmanız gerekir.

        Total thread count = total physical cores * 6

    **Örnek**

    PowerShell komutlarını 16 çekirdekli bir D14 VM’den çalıştırdığınız varsayılmıştır

        Total thread count = 16 cores * 6 = 96 threads


* **2. adım: Perfilethreadcount değerini hesaplayın** -biz dosyaların boyutuna hesaplıyoruz. 2,5 GB'den küçük dosyalar için varsayılan değer olan 10 yeterli olduğundan bu parametreyi değiştirmek için gerek yoktur. 2,5 GB'den büyük olan dosyalar için 10 iş parçacığı ilk 2,5 GB için temel olarak kullanın ve dosya boyutundaki her ek 256 MB'lık artış için 1 iş parçacığı ekleyin. Birçok farklı boyutta dosya içeren bir klasörü kopyalıyorsanız dosyaları benzer dosya boyutları halinde gruplandırmayı göz önünde bulundurun. Dosya boyutlarının benzer olmaması performansın düşmesine neden olabilir. Benzer boyutlu dosyaları gruplandırmanız mümkün değilse PerFileThreadCount değerini en büyük dosya boyutuna göre ayarlamanız gerekir.

        PerFileThreadCount = 10 threads for the first 2.5 GB + 1 thread for each additional 256 MB increase in file size

    **Örnek**

    1 GB ile 10 GB arasında değişen 100 dosyanız olduğunu varsayarsak, 10 GB'ın en büyük dosya boyutu aşağıdaki gibi okuduğunuz Denklem için kullanırız.

        PerFileThreadCount = 10 + ((10 GB - 2.5 GB) / 256 MB) = 40 threads

* **3. adım: Concurrentfilecount değerini hesaplayın** - toplam iş parçacığı sayısı ve perfilethreadcount değerini kullanarak concurrentfilecount değerini hesaplayın aşağıdaki denklemi temel alarak:

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Örnek**

    Kullandığımız örnek değerler temel alınmıştır

        96 = 40 * ConcurrentFileCount

    Sonuç olarak **ConcurrentFileCount** değeri **2,4** ve bunu **2**’ye yuvarlayabiliriz.

## <a name="further-tuning"></a>Daha fazla ayar

Üzerinde çalışılan dosyaların boyutu çeşitlilik gösterdiğinden daha fazla ayar yapmanız gerekebilir. Yukarıdaki hesaplama iyi daha büyük ve 10 GB aralığından yakın tüm veya çoğu dosyaları çalışır. Bunun yerine çoğu dosya küçük olacak şekilde birçok farklı dosya boyutu varsa PerFileThreadCount değerini azaltabilirsiniz. PerFileThreadCount değerini azaltarak ConcurrentFileCount değerini artırabiliriz. Bu nedenle bizim dosyaları çoğunu 5 GB aralığında daha küçük olduğunu varsayıyoruz, biz hesaplamamızın Yinele:

    PerFileThreadCount = 10 + ((5 GB - 2.5 GB) / 256 MB) = 20

Bu nedenle, **ConcurrentFileCount** sonucunda 4.8 olarak 96/20 haline gelir için yuvarlanır **4**.

Dosya boyutlarınızın dağılımına göre **PerFileThreadCount** değerini artırıp azaltarak bu ayarları değiştirmeye devam edebilirsiniz.

### <a name="limitation"></a>Sınırlama

* **Dosya sayısı ConcurrentFileCount değerinden az olduğu**: Karşıya yüklediğiniz dosyalarının sayısını daha küçük olup olmadığını **ConcurrentFileCount** hesaplanmış ve ardından azaltmanız gerekir **ConcurrentFileCount** dosya sayısına eşit olacak şekilde. **PerFileThreadCount** değerini artırmak için kalan iş parçacıklarından dilediğinizi kullanabilirsiniz.

* **Çok fazla iş parçacığı**: Çok fazla kümenizin boyutunu artırmadan iş parçacığı sayısını artırmak için performansın düşmesine neden riskini çalıştırın. CPU’da bağlam değişimi sırasında çekişme sorunları olabilir.

* **Yetersiz eşzamanlılık**: Eşzamanlılık yeterli değilse kümeniz çok küçük olabilir. Daha fazla eşzamanlılık sağlar, kümedeki düğüm sayısını artırabilirsiniz.

* **Azaltma hataları**: Eşzamanlılığınız çok yüksekse azaltma hataları görebilirsiniz. Azaltma hataları görüyorsanız eşzamanlılığı azaltmanız veya bize ulaşmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Büyük veri gereksinimleri için Azure Data Lake depolama Gen1 kullanın](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)

