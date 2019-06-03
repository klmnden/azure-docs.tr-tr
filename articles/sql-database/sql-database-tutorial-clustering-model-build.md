---
title: 'Öğretici: R ile kümeleme modeli oluşturun'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bölümünde üç bölümlü öğretici serisinin bu iki R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde kümelemeyi gerçekleştirmek için bir K-ortalamaları modeli oluşturacaksınız.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 05/17/2019
ms.openlocfilehash: 12738b63be92420c5f3afea6c133522cbd97f849
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419765"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile bir kümeleme modeli oluşturun

Bölümünde üç bölümlü öğretici serisinin bu iki R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde kümelemeyi gerçekleştirmek için bir K-ortalamaları modeli oluşturacaksınız.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * K-ortalamaları algoritması için küme sayısını tanımlayın
> * Kümeleme gerçekleştirin
> * Sonuçları analiz edin

İçinde [Birinci bölümde](sql-database-tutorial-clustering-model-prepare-data.md), r'de kümelemeyi gerçekleştirmek için bir Azure SQL veritabanından verileri hazırlama işleminin nasıl yapılacağını öğrendiniz

İçinde [üçüncü kısmı](sql-database-tutorial-clustering-model-deploy.md), yeni verileri temel alan kümeleme yapabilmek için bir Azure SQL veritabanında bir saklı yordam oluşturma öğreneceksiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticide iki parçası varsayar tamamladığınızdan [ **Birinci bölümde** ](sql-database-tutorial-clustering-model-prepare-data.md) ve önkoşullarını.

## <a name="define-the-number-of-clusters"></a>Küme sayısı tanımlayın

Müşteri verilerinizin küme için kullanacağınız **K-ortalamaları** gruplandırma veri en basit ve en iyi bilinen yollarından biri, kümeleme algoritması.
Daha fazla bilgi edinebilirsiniz K-ortalamaları hakkında [K-ortalamaları kümeleme algoritması için tam bir kılavuz](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

Algoritma, iki giriş kabul eder: Veri ve önceden tanımlanmış birtakım "*k*" oluşturulacak küme sayısını temsil eden.
Çıktı *k* kümeler arasında bölümlenmiş giriş veri kümeleri.

Kümeler için kullanılacak algoritmayı sayısını belirlemek için bir kullanın. grupları ayıklanan küme sayısı tarafından kareler toplamı içinde. Uygun kullanılacak kümeleri dirseği veya çizimin "dirsek" sayısıdır.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted. 
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Dirsek grafiği](./media/sql-database-tutorial-clustering-model-build/elbow-graph.png)

Graf üzerinde temel, gibi görünüyor *bin = 4* denemek için iyi bir değer olacaktır. Olduğunu *k* değerine müşterilere dört kümeler halinde gruplandırmak.

## <a name="perform-clustering"></a>Kümeleme gerçekleştirin

İşlev aşağıdaki R betiği kullanacaksınız **rxKmeans**, RevoScaleR paket K-ortalamaları işlevinde olduğu.

```r
# Output table to hold the customer group mappings.
# This is a table where the cluster mappings will be saved in the database.
return_cluster = RxSqlServerData(table = "return_cluster", connectionString = connStr);
# Set the seed for the random number generator for predictability
set.seed(10);
# Generate clusters using rxKmeans and output key / cluster to a table in SQL database
# called return_cluster
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters=4,
                   outFile=return_cluster,
                   outColName="cluster",
                   extraVarsToWrite=c("customer"),
                   overwrite=TRUE);

# Read the customer returns cluster table from the database
customer_cluster <- rxDataStep(return_cluster);
```

## <a name="analyze-the-results"></a>Sonuçları analiz edin

K-ortalamaları kullanarak kümeleme yaptığınızdan, sonuç çözümleyin ve eyleme dönüştürülebilir bilgilerden bulup bulamayacağınızı öğrenmek için sonraki adım olan.

**Clust** nesne K-ortalamaları kümeleme sonuçları içerir.

```r
#Look at the clustering details to analyze results
clust
```

```results
Call:
rxKmeans(formula = ~orderRatio + itemsRatio + monetaryRatio + 
    frequency, data = customer_returns, outFile = return_cluster, 
    outColName = "cluster", extraVarsToWrite = c("customer"), 
    overwrite = TRUE, numClusters = 4)
Data: customer_returns
Number of valid observations: 37336
Number of missing observations: 0 
Clustering algorithm:  

K-means clustering with 4 clusters of sizes 31675, 671, 2851, 2139
Cluster means:
   orderRatio   itemsRatio monetaryRatio frequency
1 0.000000000 0.0000000000    0.00000000  0.000000
2 0.007451565 0.0000000000    0.04449653  4.271237
3 1.008067345 0.2707821817    0.49515232  1.031568
4 0.000000000 0.0004675082    0.10858272  1.186068
Within cluster sum of squares by cluster:
         1          2          3          4
    0.0000  1329.0160 18561.3157   363.2188
```

Dört küme yol değişkenleri kullanarak verilen tanımlanan [birinci bölüm](sql-database-tutorial-clustering-model-prepare-data.md#separate-customers):

* *orderRatio* = dönüş sırası oranı (kısmen veya tamamen siparişler toplam sayısı döndürülen siparişler toplam sayısı)
* *itemsRatio* dönüş öğesi oranı (toplam satın alınan öğelerin sayısı ile döndürülen öğe sayısını) =
* *monetaryRatio* = iade miktarı oranı (toplam parasal miktarını satın alınan miktarı döndürülen öğe)
* *Sıklık* dönüş sıklığı =

Veri madenciliği K-ortalamaları genellikle kullanarak daha fazla analiz sonuçlarını ve her kümeyi daha iyi anlamak için ek adımlar gerektirir, ancak bazı iyi müşteri adayları sağlayabilir.
Bu sonuçları yorumlayan birkaç yolu vardır:

* Etkin olmayan müşteriler, bir grup kümesi 1 (en büyük kümesi) gibi görünüyor (tüm sıfır değerler).
* Küme 3 dönüş davranışı açısından öne çıkar bir grubu gibi görünüyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme

***Bu öğretici ile devam etmek için gideceğinizi değil,***, Azure SQL veritabanı sunucunuzdan tpcxbb_1gb veritabanını silin.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **tpcxbb_1gb**ve aboneliğinizi seçin.
1. Seçin, **tpcxbb_1gb** veritabanı.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serisinin kısmında iki, şu adımları tamamlandı:

* K-ortalamaları algoritması için küme sayısını tanımlayın
* Kümeleme gerçekleştirin
* Sonuçları analiz edin

Oluşturduğunuz machine learning modelini dağıtma hakkında bilgi için Bu öğretici serisinin üç bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile kümeleme model dağıtma](sql-database-tutorial-clustering-model-deploy.md)