---
title: SQL Data Warehouse ile Azure Machine Learning kullanma | Microsoft Docs
description: Çözüm geliştirmek üzere Azure SQL Data Warehouse ile Azure Machine Learning kullanma öğreticisi.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: ''
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: c19860c6b5b1c15d1e29ddc67f9cf9ad4618725b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31435277"
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>SQL Data Warehouse ile Azure Machine Learning kullanın
Azure Machine Learning, SQL Data Warehouse verilerinizi karşı Tahmine dayalı modelleri oluşturmak için kullanın ve ardından tüketen hazır web Hizmetleri olarak yayımlama tam olarak yönetilen Tahmine dayalı analiz hizmetidir. Tahmine dayalı analiz temel bilgileri öğrenmek ve makine öğrenme okuyarak [azure'da Machine Learning'e giriş][Introduction to Machine Learning on Azure].  Daha sonra oluşturma, eğitme, Puanlama ve machine learning modelini kullanarak test öğrenin [oluşturma deneme öğretici][Create experiment tutorial].

Bu makalede, aşağıdakileri kullanarak yapılacağı hakkında bilgi edineceksiniz [Azure Machine Learning Studio][Azure Machine Learning Studio]:

* Oluşturmak, eğitmek ve Tahmine dayalı bir modeli Puanlama veritabanından verileri okuyamadı
* Veritabanınız için veri yazma

## <a name="read-data-from-sql-data-warehouse"></a>SQL veri ambarından veri okuma
Biz AdventureWorksDW veritabanında ürün tablodan veri okur.

### <a name="step-1"></a>1. Adım
Tıklayarak yeni bir deneme başlatın + yeni Machine Learning Studio penceresinin alt DENEMEYİ seçin ve ardından boş denemeler'ı seçin. Tuvale üstündeki varsayılan deneme adını seçin ve anlamlı, örneğin, bir bisiklet fiyat tahmini yeniden adlandırın.

### <a name="step-2"></a>2. Adım
Veri kümeleri paletindeki okuyucu modülü ve modülleri deneme tuvalinin sol arayın. Modülünü deneme tuvaline sürükleyin.
![][drag_reader]

### <a name="step-3"></a>3. Adım
Okuyucu modülü seçin ve Özellikler bölmesi doldurun.

1. Azure SQL veritabanı veri kaynağı olarak seçin.
2. Veritabanı sunucusu adı: sunucu adını yazın. Kullanabileceğiniz [Azure portal] [ Azure portal] bu bulunamıyor.

![][server_name]

1. Veritabanı adı: yalnızca belirtilen sunucuda bir veritabanının adını yazın.
2. Sunucu kullanıcı hesabı adı: veritabanı için erişim izinlerine sahip bir hesabın kullanıcı adını yazın.
3. Sunucu kullanıcı hesabı parolası: Belirtilen kullanıcı hesabı için parola sağlayın.
4. Herhangi bir sunucu sertifikayı kabul: verilerinizi okumadan önce site sertifikası gözden geçirme atlamak istiyorsanız bu seçeneği (daha az güvenli) kullanın.
5. Veritabanı Sorgusu: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. Bu durumda, biz aşağıdaki sorguyu kullanarak Ürün tablosundan verileri okur.

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>4. Adım
1. Deneme tuvalinin altında Çalıştır tıklayarak denemeyi çalıştırın.
2. Deneme bittiğinde, okuyucu modülü başarıyla tamamlandığını göstermek için yeşil bir onay işareti sahip olur. Ayrıca sağ üst köşedeki çalışma durumu tamamlandı dikkat edin.

![][run]

1. İçeri aktarılan verileri görmek için otomobil veri kümesinin altındaki çıkış bağlantı noktasına tıklayın ve görsel öğe seçin.

## <a name="create-train-and-score-a-model"></a>Oluşturmak, eğitmek ve bir modeli Puanlama
Şimdi bu veri kümesi için kullanabilirsiniz:

* Bir Model oluşturmak: verileri işlemek ve özellikleri tanımlar
* Modeli eğitmek: seçin ve bir öğrenme algoritmasını Uygula
* Puanlama ve test modeli: yeni bir bisiklet fiyatını tahmin etme

![][model]

Nasıl oluşturulacağı hakkında daha fazla bilgi için eğitme, Puanlama ve machine learning modelini kullanın test [oluşturma deneme öğretici][Create experiment tutorial].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için veri yazma
Biz sonuç AdventureWorksDW veritabanında ProductPriceForecast tabloya kümesini yazacaksınız.

### <a name="step-1"></a>1. Adım
Veri kümeleri paletini yazan modülünde ve modülleri deneme tuvalinin sol arayın. Modülünü deneme tuvaline sürükleyin.

![][drag_writer]

### <a name="step-2"></a>2. Adım
Yazıcı modülü seçin ve Özellikler bölmesi doldurun.

1. Azure SQL veritabanı veri hedef olarak seçin.
2. Veritabanı sunucusu adı: sunucu adını yazın. Kullanabileceğiniz [Azure portal] [ Azure portal] bu bulunamıyor.
3. Veritabanı adı: yalnızca belirtilen sunucuda bir veritabanının adını yazın.
4. Sunucu kullanıcı hesabı adı: veritabanı için yazma izinlerine sahip bir hesabın kullanıcı adını yazın.
5. Sunucu kullanıcı hesabı parolası: Belirtilen kullanıcı hesabı için parola sağlayın.
6. Herhangi bir sunucu sertifikası (güvensiz) kabul edin: sertifikayı görüntülemek istemiyorsanız bu seçeneği belirleyin.
7. Kaydedilecek sütunun virgülle ayrılmış listesini: çıktısını almak istediğiniz veri kümesi veya sonuç sütun listesini sağlar.
8. Veri tablosu adı: veri tablonun adını belirtin.
9. Datatable sütunlar virgülle ayrılmış listesi: Yeni tabloda kullanmak için sütun adlarını belirtin. Sütun adlarını kaynak veri kümesinde gördüğünüzden farklı olabilir, ancak çıkış tablosu için tanımladığınız aynı sayıda sütun burada listelemeniz gerekir.
10. SQL Azure işlem yazılan satır sayısı: SQL veritabanı tek bir işlemde yazılan satır sayısını yapılandırabilirsiniz.

![][writer_properties]

### <a name="step-3"></a>3. Adım
1. Deneme tuvalinin altında Çalıştır tıklayarak denemeyi çalıştırın.
2. Deneme bittiğinde, tüm modüllerin bunlar başarıyla tamamlandığını göstermek için yeşil bir onay işareti sahip olur.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Veri Ambarı’nda geliştirmeye genel bakış][SQL Data Warehouse development overview].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction to machine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
