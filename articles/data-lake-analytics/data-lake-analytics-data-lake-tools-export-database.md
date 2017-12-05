---
title: "Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL veritabanı dışarı | Microsoft Docs"
description: "U-SQL veritabanını dışarı aktarma ve otomatik olarak yerel bir hesap içeri aktarmak için Visual Studio için Azure Data Lake araçları kullanmayı öğrenin."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 441606258f9541c9552925e7c0cbc9b3a9effb4d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="export-a-u-sql-database"></a>U-SQL veritabanını dışarı aktarma

Bu makalede, nasıl kullanılacağını öğrenirsiniz [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) U-SQL veritabanını tek bir U-SQL betiği ve indirilen kaynakları dışarı aktarma. Dışa aktarılan veritabanını aynı işlemde yerel bir hesap içeri aktarabilirsiniz.

Müşteriler genellikle birden çok ortamları geliştirme, test ve üretim için korur. Bu ortamlar, hem yerel bir hesap, bir geliştiricinin yerel bilgisayarda ve bir Azure Data Lake Analytics hesabı Azure üzerinde barındırılır. 

Geliştirmek ve U-SQL sorguları geliştirme ve test ortamları ayarlama geliştiriciler genellikle bir üretim veritabanında işlerini yeniden oluşturmanız gerekir. Veritabanı Dışarı Aktarma Sihirbazı'nı bu işlem artırmanıza yardımcı olur. Sihirbazı kullanarak, geliştiriciler var olan veritabanı ortamı ve diğer Data Lake Analytics hesaplarına örnek veri kopyalayabilirsiniz.

## <a name="export-steps"></a>Dışarı aktarma adımları

### <a name="step-1-export-the-database-in-server-explorer"></a>1. adım: Sunucu Gezgininde veritabanını dışarı aktarma

Server Explorer'da izniniz tüm Data Lake Analytics hesapları listelenir. Veritabanı dışarı aktarmak için:

1. Sunucu Gezgini'nde, dışarı aktarmak istediğiniz veritabanını içeren hesabı'nı genişletin.
2. Bir veritabanını sağ tıklatın ve ardından **verme**. 
   
    ![Sunucu Gezgini - veritabanı bir dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Varsa **verme** menü seçeneği kullanılabilir değilse, yapmanız [en son sürümünü sürümüne güncelleştirme aracı](http://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>2. adım: dışarı aktarmak istediğiniz nesneler yapılandırma

Büyük bir veritabanı yalnızca küçük bir bölümünü gerekiyorsa, Dışarı Aktarma Sihirbazı'nda dışarı aktarmak istediğiniz nesneler kümesinin yapılandırabilirsiniz. 

Verme eylemi bir U-SQL işi tamamlandı. Bu nedenle, bir Azure hesabının dışarı aktarma bazı maliyetler doğurur.

![Veritabanı Dışarı Aktarma Sihirbazı - seçin nesneleri dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>3. adım: nesne listesi ve diğer yapılandırmalar denetleyin

Bu adımda, seçilen nesneleri doğrulayabilirsiniz **verme nesne listesi** kutusu. Herhangi bir hata varsa, seçin **önceki** geri dönün ve doğru şekilde dışarı aktarmak istediğiniz nesneler yapılandırın.

Dışarı aktarma hedefi için diğer ayarları da yapılandırabilirsiniz. Yapılandırma açıklamaları aşağıdaki tabloda listelenmiştir:

|Yapılandırma|Açıklama|
|-------------|-----------|
|Hedef adı|Bu ad, dışarı aktarılan veritabanı kaynaklarını kaydetmek istediğiniz yeri gösterir. Derlemeler, ek dosyalar ve örnek verileri örnektir. Bu ada sahip bir klasör, yerel veri kök klasörünün altında oluşturulur.|
|Proje dizini|Bu yol, dışarı aktarılan U-SQL komut dosyasını kaydetmek istediğiniz yeri tanımlar. Tüm veritabanı nesne tanımları bu konuma kaydedilir.|
|Yalnızca Şema|Bu seçeneği seçerseniz, yalnızca veritabanı tanımlarını ve kaynaklar (örneğin, derlemeler ve ek dosyalar) dışarı aktarılır.|
|Şema ve Veri|Bu seçeneği seçerseniz, veritabanı tanımları, kaynakları ve veri dışarı aktarılır. Tablolar üst N satırlarını dışarı aktarılır.|
|Yerel Veritabanına Otomatik Olarak Aktar|Bu seçeneği belirlerseniz, dışa aktarılan veritabanını yerel veritabanınızı verilirken otomatik olarak içeri aktarılan tamamlanmış olmasıdır.|

![Veritabanı Dışarı Aktarma Sihirbazı - verme nesne listesi ve diğer yapılandırmalar](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>4. adım: dışarı aktarma sonuçlarını denetleyin

Dışarı aktarma tamamlandıktan sonra Sihirbazı'nda günlük penceresinde dışarı aktarılan sonuçları görüntüleyebilirsiniz. Aşağıdaki örnek, derlemeleri, ek dosyalar ve örnek verileri de dahil olmak üzere dışarı aktarılan U-SQL komut dosyası ve veritabanı kaynakları bulmak gösterilmektedir:

![Veritabanı Dışarı Aktarma Sihirbazı - verme sonuçları](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>Dışa aktarılan veritabanını yerel bir hesap içeri aktarma

Dışa aktarılan veritabanını almak için en kolay yolu seçmektir **alma yerel veritabanı otomatik olarak için** adım 3'te dışa aktarma işlemi sırasında onay kutusu. İlk olarak, bu onay kutusunu alamadık, dışarı aktarılan U-SQL betiği verme günlüğünde bulun. Ardından, veritabanını yerel hesabınıza yerel olarak almak için U-SQL betiğini çalıştırın.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>Dışa aktarılan veritabanını Data Lake Analytics hesabı için içeri aktarma

Veritabanı için farklı bir Data Lake Analytics hesap içeri aktarmak için:

1. Derlemeler, ek dosyalar ve almak istediğiniz Data Lake Analytics hesabının varsayılan Azure Data Lake Store hesabına örnek verileri de dahil olmak üzere dışarı aktarılan kaynakların karşıya yükleyin. Dışa aktarılan kaynak klasörünü yerel veri kök klasörü altında bulabilirsiniz. Tüm klasörü varsayılan Data Lake Store hesabı köküne karşıya yükleyin.
2. Karşıya yükleme tamamlandığında, dışarı aktarılan U-SQL betiği veritabanına içeri aktarmak istediğiniz Data Lake Analytics hesabı gönderin.

## <a name="known-limitations"></a>Bilinen sınırlamaları

Şu anda seçerseniz **şeması ve verisi** seçenek 3. adımında, aracı tablolarda depolanan verileri dışarı aktarmak için bir U-SQL işi çalıştırır. Bu nedenle, işlem dışarı aktarma veri yavaş olabilir ve ücrete neden. 

## <a name="next-steps"></a>Sonraki adımlar

* [U-SQL veritabanları hakkında bilgi edinin](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)


