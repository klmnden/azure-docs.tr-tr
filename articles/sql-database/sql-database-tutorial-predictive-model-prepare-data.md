---
title: 'Öğretici: R ile Tahmine dayalı bir modeli eğitmek için verileri hazırlama'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir modeli eğitmek için bir Azure SQL veritabanından veri bölümünde bu üç bölümlü öğretici serisinin birinci hazırlamanız.
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
ms.openlocfilehash: aa9c41ee34a50ab9b1409357bfe7d123166601bf
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978741"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir modeli eğitmek için verileri hazırlama

R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir modeli eğitmek için bir Azure SQL veritabanından veri bölümünde bu üç bölümlü öğretici serisinin birinci hazırlamanız.

Bu öğretici serisinin bir kayak kiralama işletme sahibi ve gelecekteki bir tarihte gerekir bisiklet sayısını tahmin etmek istediğiniz hayal edin. Bu bilgiler, stok, personel ve tesis hazır olun yardımcı olur.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Örnek bir veritabanı bir Azure SQL veritabanı'na aktarma
> * Verileri Azure SQL veritabanı'ndan R kullanarak bir veri çerçevesine yükleyin.
> * Bazı sütunlar kategorik olarak belirleyerek verileri hazırlama

İçinde [bölüm iki](sql-database-tutorial-predictive-model-build-compare.md), oluşturma ve birden çok modeli eğitmek ve sonra en doğru olanı seçin öğreneceksiniz.

İçinde [üçüncü kısmı](sql-database-tutorial-predictive-model-deploy.md), model bir veritabanında saklamak ve ardından yeni verileri temel alan tahmin yapabileceği bir saklı yordam oluşturma hakkında bilgi edineceksiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa, azure aboneliği - [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

* Machine Learning Hizmetleri ile Azure SQL veritabanı sunucusu etkin - genel Önizleme süresince Microsoft ekleme ve mevcut veya yeni veritabanlarınız için makine öğrenimi etkinleştir. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

* RevoScaleR package - bakın [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) için bu paketi yerel olarak yüklemek Seçenekler.

* R IDE - Bu öğreticide [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Bu öğreticide SQL sorgu aracı - varsayılır kullanmakta olduğunuz [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="import-the-sample-database"></a>Örnek veritabanını içeri aktarma

Bu öğreticide kullanılan örnek veri kümesi için kaydedilmiş bir **.bacpac** veritabanı yedek dosyasını indirin ve kullanın.

1. Dosya indirme [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac).

1. Bölümündeki yönergeleri izleyin [bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-import), bu ayrıntıları kullanarak:

   * Alınacak **TutorialDB.bacpac** indirdiğiniz dosya
   * Genel Önizleme sırasında seçin **5. nesil/sanal çekirdek** yeni veritabanı için yapılandırma
   * "Tutorialdb'yi" Yeni veritabanı adı

## <a name="load-the-data-into-a-data-frame"></a>Verileri bir veri çerçevesine yükleyin

R verilerini kullanmak için verileri Azure SQL veritabanı'ndan veri çerçevesine yükleme (`rentaldata`).

RStudio içinde yeni bir RScript dosyası oluşturun ve aşağıdaki betiği çalıştırın. Değiştirin **sunucu**, **UID**, ve **PWD** kendi bağlantı bilgileriyle.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

Aşağıdakine benzer bir sonuç görmeniz gerekir.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Verileri hazırlama

Bu örnek veritabanındaki hazırlık çoğunu zaten yapıldığını, ancak burada bir daha fazla hazırlık yaparsınız.
Üç sütun olarak tanımlamak için aşağıdaki R betiğini kullanın *kategorileri* veri türlerini değiştirerek *faktörü*.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

Aşağıdakine benzer bir sonuç görmeniz gerekir.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Eğitim verileri artık hazır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle devam etmeyecekseniz, Azure SQL veritabanı sunucunuzdan Tutorialdb'yi veritabanını silin.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **Tutorialdb'yi**ve aboneliğinizi seçin.
1. Tutorialdb'yi veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Serinin Bu öğretici serisinde, bu adımlar tamamlandı:

* Bir veritabanı yedekleme dosyasını bir Azure SQL veritabanı'na aktarma
* Verileri Azure SQL veritabanı'ndan R kullanarak bir veri çerçevesine yükleyin.
* Bazı sütunlar kategoriler olarak belirleyerek verileri hazırlama

Bir machine learning Tutorialdb'yi veritabanından veri kullanan modeli oluşturmak için Bu öğretici serisinin oluşan izleyin:

> [!div class="nextstepaction"]
> [Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir model oluşturma](sql-database-tutorial-predictive-model-build-compare.md)
