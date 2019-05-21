---
title: 'Öğretici: Eğitim ve r ile Tahmine dayalı modelleri karşılaştırın'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bölümünde üç bölümlü öğretici serisinin bu iki r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile iki tahmine dayalı modeller oluşturun ve sonra en doğru modeli seçin.
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
ms.date: 05/02/2019
ms.openlocfilehash: 3d336d6a53b6d234048c56d8492d278bef6fed64
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957613"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir model oluşturma

Bölümünde üç bölümlü öğretici serisinin bu iki r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile iki tahmine dayalı modeller oluşturun ve sonra en doğru modeli seçin.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * İki makine öğrenimi modellerini eğitin
> * Her iki modelinden tahminlerde
> * En doğru model seçmek için sonuçlarını karşılaştırın

İçinde [Birinci bölümde](sql-database-tutorial-predictive-model-prepare-data.md), örnek veritabanını bir Azure SQL veritabanına içeri aktarmak ve ardından r'deki Tahmine dayalı bir modeli eğitmek için kullanılacak veri hazırlama hakkında bilgi edindiniz

İçinde [üçüncü kısmı](sql-database-tutorial-predictive-model-deploy.md), model bir veritabanında saklamak ve ardından yeni verileri temel alan tahmin yapabileceği bir saklı yordam oluşturma hakkında bilgi edineceksiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticide iki parçası varsayar tamamladığınızdan [ **Birinci bölümde** ](sql-database-tutorial-predictive-model-prepare-data.md) ve önkoşullarını.

## <a name="train-two-models"></a>İki modeli eğitme

En iyi modeli kayak kiralama verilerini bulmak için iki farklı modelleri (doğrusal regresyon ve karar ağacı) oluşturun ve hangisinin daha doğru bir şekilde tahmin bakın. Veri çerçevesinin kullanacağınız `rentaldata` bu serinin birinci oluşturuldu.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];

#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use rxLinMod to create a linear regression model, trained with the training data set
model_linmod <- rxLinMod(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rxDTree to create a decision tree model, trained with the training data set
model_dtree  <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Her iki modelinden tahminlerde

Predıct kiralama her eğitilen modelini kullanarak sayıları için bir predıct işlevi kullanın.

```r
#Use both models to make predictions using the test data set.
predict_linmod <- rxPredict(model_linmod, test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

predict_dtree  <- rxPredict(model_dtree,  test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_linmod);
head(predict_dtree);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Sonuçları karşılaştırma

Şimdi en iyi tahmin modellerinin sağlayan görmek istiyorsunuz. Bunu yapmanın hızlı ve kolay bir yolu, eğitim verilerinizi gerçek değerler ve tahmin edilen değerler arasındaki farkı görmek için temel bir çizim işlev kullanmaktır.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(2, 1));
plot(predict_linmod$RentalCount_Pred - predict_linmod$RentalCount, main = "Difference between actual and predicted. rxLinmod");
plot(predict_dtree$RentalCount_Pred  - predict_dtree$RentalCount,  main = "Difference between actual and predicted. rxDTree");
```

![İki model karşılaştırma](./media/sql-database-tutorial-predictive-model-build-compare/compare-models.png)

Karar ağacı modeli iki model daha doğru gibi görünüyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle devam etmeyecekseniz, Azure SQL veritabanı sunucunuzdan Tutorialdb'yi veritabanını silin.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **Tutorialdb'yi**ve aboneliğinizi seçin.
1. Tutorialdb'yi veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serisinin kısmında iki, şu adımları tamamlandı:

* İki makine öğrenimi modellerini eğitin
* Her iki modelinden tahminlerde
* En doğru model seçmek için sonuçlarını karşılaştırın

Oluşturduğunuz machine learning modelini dağıtma hakkında bilgi için Bu öğretici serisinin üç bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı model dağıtma](sql-database-tutorial-predictive-model-deploy.md)
