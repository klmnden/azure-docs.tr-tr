---
title: Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL veritabanını dışarı aktarma
description: U-SQL veritabanını dışarı aktarma ve otomatik olarak yerel bir hesap almak için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: fe28aa8b88f557d4bbcdabf1de1c4bc6491743ce
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60628582"
---
# <a name="export-a-u-sql-database"></a>U-SQL veritabanını dışarı aktarma

Bu makalede, kullanmayı öğrenin [Visual Studio için Azure Data Lake Araçları](https://aka.ms/adltoolsvs) bir U-SQL veritabanı tek bir U-SQL betiği ve indirilen kaynakları dışarı aktarmak için. Aynı işlemde yerel bir hesap için dışa aktarılan veritabanını içeri aktarabilirsiniz.

Müşteriler, geliştirme, test ve üretim için birden çok ortamı genellikle korur. Bu ortamların, hem yerel bir hesapta, bir geliştiricinin yerel bilgisayarda ve azure'da bir Azure Data Lake Analytics hesabında barındırılır. 

Geliştirin ve U-SQL sorguları geliştirme ve test ortamları ayarlamak, geliştiriciler genellikle bir üretim veritabanında işlerini yeniden oluşturmanız gerekir. Veritabanı Dışarı Aktarma Sihirbazı'nı bu işlem artırmanıza yardımcı olur. Sihirbazı kullanarak, geliştiricilerin mevcut veritabanı ortam ve diğer Data Lake Analytics hesapları için örnek veri kopyalayabilirsiniz.

## <a name="export-steps"></a>Dışarı aktarma adımları

### <a name="step-1-export-the-database-in-server-explorer"></a>1\. adım: Sunucu Gezgini'nde veritabanını dışarı aktarma

Sunucu Gezgini'nde izinlerine sahip olduğunuz tüm Data Lake Analytics hesapları listelenir. Veritabanı dışarı aktarmak için:

1. Sunucu Gezgini'nde, dışarı aktarmak istediğiniz veritabanını içeren hesabını genişletin.
2. Veritabanına sağ tıklayın ve ardından **dışarı**. 
   
    ![Sunucu Gezgini - veritabanı bir dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Varsa **dışarı** menü seçeneği kullanılabilir değilse, yapmanız [en son sürümünü sürümüne güncelleştirme aracı](https://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>2\. adım: Dışarı aktarmak istediğiniz nesneleri yapılandırın

Büyük bir veritabanı yalnızca küçük bir bölümünü gerekiyorsa, Dışarı Aktarma Sihirbazı'nda dışarı aktarmak istediğiniz nesne kümesini yapılandırabilirsiniz. 

Verme eylemi bir U-SQL işi tamamlandı. Bu nedenle, bir Azure hesabından diğerine verme bazı maliyetler doğurur.

![Veritabanı Dışarı Aktarma Sihirbazı - seçim nesneleri dışarı aktarın.](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>3\. adım: Nesne listesi ve diğer yapılandırmaları denetleyin

Bu adımda seçilen nesneleri doğrulayabilirsiniz **nesne listesini dışarı aktarma** kutusu. Herhangi bir hata varsa, seçin **önceki** geri dönün ve doğru şekilde dışarı aktarmak istediğiniz nesneleri yapılandırın.

Dışarı aktarma hedefi için diğer ayarları da yapılandırabilirsiniz. Yapılandırma açıklamaları aşağıdaki tabloda listelenmiştir:

|Yapılandırma|Açıklama|
|-------------|-----------|
|Hedef adı|Bu ad, dışarı aktarılan Veritabanı kaynaklarından tasarruf etmek istediğiniz gösterir. Derlemeleri, ek dosyaları ve örnek verileri verilebilir. Bu ada sahip bir klasör yerel veri kökü klasörünüzün altında oluşturulur.|
|Proje dizini|Bu yol, dışarı aktarılan U-SQL komut dosyasını kaydetmek istediğiniz tanımlar. Tüm veritabanı nesne tanımları, bu konuma kaydedilir.|
|Yalnızca şema|Bu seçeneği belirlerseniz, tek veritabanı tanımları ve kaynaklar (gibi derlemeleri ve ek dosyaları) dışarı aktarılır.|
|Şema ve veri|Bu seçeneği seçerseniz, veritabanı tanımları, kaynakları ve veri dışarı aktarılır. Tablo üst N satırlarını dışarı aktarılır.|
|Yerel veritabanına otomatik olarak içeri aktarma|Bu seçeneği belirlerseniz, dışarı aktarılan veritabanıdır verirken yerel veritabanınızı otomatik olarak içeri aktarılan tamamlandı.|

![Veritabanı Dışarı Aktarma Sihirbazı - nesne listesini dışarı aktarma ve diğer yapılandırmaları](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>4\. Adım: Dışarı aktarma sonuçlarını denetleyin

Dışarı aktarma tamamlandığında, Sihirbazı günlük pencerede dışarı aktarılan sonuçları görüntüleyebilirsiniz. Aşağıdaki örnek, derlemeleri, ek dosyaları ve örnek veriler dahil olmak üzere dışarı aktarılan U-SQL betiği ve veritabanı kaynakları bulmak gösterilmektedir:

![Veritabanı Dışarı Aktarma Sihirbazı - dışarı aktarma sonuçları](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>İçeri aktarma dışarı aktarılan veritabanına yerel bir hesap

Dışarı aktarılan veritabanını içeri aktarmak için en uygun yolu seçmektir **alma yerel veritabanı otomatik olarak için** adım 3'te dışa aktarma işlemi sırasında onay kutusu. İlk olarak, bu kutuyu işaretlerseniz alamadık, dışarı aktarılan U-SQL betiğini dışarı aktarma günlüğünde bulun. Ardından, U-SQL betiğini yerel olarak veritabanını yerel hesabınızda içeri aktarmak için çalıştırın.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>İçeri aktarma dışarı aktarılan veritabanına bir Data Lake Analytics hesabı

Veritabanı için farklı bir Data Lake Analytics hesap içeri aktarmak için:

1. Derlemeleri ve ek dosyaları almak istediğiniz Data Lake Analytics hesabı, varsayılan Azure Data Lake Store hesabına örnek veriler dahil olmak üzere dışarı aktarılan kaynakların karşıya yükleyin. Dışarı aktarılan kaynak klasörünü yerel veri kök klasörü altında bulabilirsiniz. Varsayılan Data Lake Store hesabının kök klasörün tamamını yükleyin.
2. Karşıya yükleme tamamlandığında, dışarı aktarılan bir U-SQL betiği veritabanına içeri aktarmak istediğiniz Data Lake Analytics hesabına gönderin.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu anda seçerseniz **şema ve veri** adım 3, araç seçeneği tablolarında depolanan verileri dışarı aktarmak için bir U-SQL işi çalıştırır. Bu nedenle, dışa aktarma işlemi verileri yavaş olabilir ve ücrete neden olabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [U-SQL veritabanları hakkında bilgi edinin](/u-sql/data-definition-language-ddl-statements) 
* [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)


