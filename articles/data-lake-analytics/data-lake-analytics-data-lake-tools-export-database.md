---
title: "Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL veritabanları dışarı aktarma | Microsoft Docs"
description: "U-SQL veritabanını dışarı aktarma ve aynı anda yerel hesap içeri aktarmak için Visual Studio için Azure Data Lake araçları kullanmayı öğrenin."
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
ms.openlocfilehash: 9f11e07f7fbaf2b708d6fbc8a746238745132bda
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-export-u-sql-database"></a>U-SQL veritabanı dışarı aktarma

Bu belgede, nasıl kullanılacağını öğreneceksiniz [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) U-SQL veritabanını tek bir U-SQL betiği ve indirilen kaynakları dışarı aktarma. Dışarı aktarılan veritabanı için yerel hesap içeri aktarma aynı işlem içinde de desteklenir.

Müşteriler genellikle birden çok ortamları geliştirme, test ve üretim için korur. Bu ortamlar geliştiricilerinin yerel makinede yerel hesap ve Azure Data Lake Analytics hesabı Azure üzerinde barındırılan. Geliştirme ve U-SQL sorgularında geliştirme ve test ortamları ayarlama, geliştiricilerin her şeyi üretim veritabanını yeniden oluşturmanız gerekecek yaygındır. **Veritabanı Dışarı Aktarma Sihirbazı** bu işlem artırmanıza yardımcı olur. Sihirbazı kullanarak, geliştiriciler var olan veritabanı ortamı ve diğer Azure Data Lake Analytics hesaplarına örnek veri kopyalayabilirsiniz.

## <a name="export-steps"></a>Dışarı aktarma adımları

### <a name="step-1-right-click-the-database-in-server-explorer-and-click-export"></a>1. adım: Sunucu Gezgininde veritabanını sağ tıklatın ve "Dışarı aktar..." düğmesini tıklatın

Tüm Azure Data Lake Analytics hesaplarını, listelenen Server Explorer'da iznine sahip. Genişletme bir dışarı aktarma ve seçmek için veritabanını sağ tıklatın veritabanını içeren **dışarı aktar...** . Bağlam menüsü bulamazsanız, gerek [en son sürümünü sürümüne güncelleştirme aracı](http://aka.ms/adltoolsvs).

![Data Lake Analytics verme veritabanı araçları](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

### <a name="step-2-configure-the-objects-you-want-to-export"></a>2. adım: dışarı aktarmak istediğiniz nesneler yapılandırma

Bazen bir veritabanı büyük ancak küçük bir bölümünü yeterlidir, ardından dışa aktarma Sihirbazı'nda dışarı aktarmak istediğiniz nesneler kümesinin yapılandırabilirsiniz. U-SQL işlemini çalıştırarak, verme eylemi tamamlandığını not alın ve bu nedenle Azure hesabından dışarı aktarma bazı maliyet oluşturur.

![Veri Gölü analizi araçları verme Veritabanı Sihirbazı](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-more-configurations"></a>3. adım: nesne listesi ve daha fazla yapılandırmalarını denetleyin

Bu adımda, seçilen nesneler iletişim kutusunun üstündeki çift kontrol edebilirsiniz. Bazı hatalar varsa, geri dönün ve yeniden dışarı aktarmak istediğiniz nesneler yapılandırmak için önceki'ni tıklatın.

Dışarı aktarma hedefi hakkında diğer yapılandırmaları da yapabilirsiniz, bu yapılandırmalar açıklamalarını tabloda listelenir:

|Yapılandırma|Açıklama|
|-------------|-----------|
|Hedef adı|Bu ad, derlemeleri, ek dosyalar ve örnek verileri gibi verilen veritabanı kaynaklarını kaydetmek istediğiniz yeri gösterir. Bu ada sahip bir klasör, yerel veri kök klasörünün altında oluşturulur.|
|Proje dizini|Bu yol, tüm veritabanı nesne tanımlarını içeren dışarı aktarılan U-SQL komut dosyasını kaydetmek istediğiniz yeri tanımlar.|
|Yalnızca Şema|Bu seçeneğin belirlenmesi sonuçları yalnızca veritabanı tanımlarını ve kaynakları (örneğin, derlemeler ve ek dosyalar) dışarı aktarılan.|
|Şema ve Veri|Bu seçeneğin belirlenmesi, veritabanı tanımları, kaynakları ve verilecek veri ile sonuçlanır. Tablolar üst N satırlarını dışarı aktarılır.|
|Yerel Veritabanına Otomatik Olarak Aktar|Dışarı aktarma tamamlandıktan sonra bu yapılandırmayı dışarı aktarılan veritabanı anlamına gelmektedir denetimi yerel veritabanınızı otomatik olarak içeri aktarılır.|

![Veri Gölü analizi araçları verme Veritabanı Sihirbazı yapılandırması](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>4. adım: dışarı aktarma sonuçlarını denetleyin

Tüm bu ayarları ve verme durumu sonra günlük penceresinden dışarı aktarılan Sonuçları Sihirbazı'nda bulabilirsiniz. Kırmızı dikdörtgen bDüşük ekran görüntüsü tarafından işaretlenen üzerinden oturum derlemeler, ek dosyalar ve örnek verileri de dahil olmak üzere dışarı aktarılan U-SQL komut dosyası ve veritabanı kaynaklarını konumunu bulabilirsiniz.

![Data Lake Analytics araçları verme Veritabanı Sihirbazı Tamamlandı](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="how-to-import-the-exported-database-to-local-account"></a>Dışarı aktarılan veritabanı için yerel hesap içeri aktarma

Bu içeri aktarma bunu yapmanın en kolay yolu denetimi **otomatik yerel veritabanı alma** adım 3'te verme ilerleme sırasında. Bunu yapmak unuttuysanız, dışarı aktarılan U-SQL betiği verme günlük aracılığıyla bulun ve yerel olarak veritabanını yerel hesabınıza almak için U-SQL betiğini çalıştırın.

## <a name="how-to-import-the-exported-database-to-azure-data-lake-analytics-account"></a>Dışarı aktarılan veritabanı için Azure Data Lake Analytics hesap içeri aktarma

Veritabanı için başka bir Azure Data Lake Analytics hesap içeri aktarmak için iki adım gerekir:

1. Derlemeler, ek dosyalar ve örnek verileri almak istediğiniz Azure Data Lake Analytics hesabının varsayılan Azure Data Lake Store hesabına dahil olmak üzere dışarı aktarılan kaynakların karşıya yükleyin. Yerel veri kök klasörü altında dışa aktarılan kaynak klasörü bulun ve varsayılan depolama hesabı köküne tüm klasörü yükleyin.
2. Dışarı aktarılan U-SQL betiği tamamlandı dosyalarını karşıya yükledikten sonra veritabanına içeri aktarmak istediğiniz Azure Data Lake Analytics hesabı gönderin.

## <a name="known-limitation"></a>Bilinen sınırlama

Şu anda seçtiyseniz **şeması ve verisi** Sihirbazı'nda aracı tablolarda depolanan verileri dışarı aktarmak için bir U-SQL işi çalıştırır. İşte bu nedenle işlem dışarı aktarma veri yavaş ve maliyet doğurur. 

## <a name="next-steps"></a>Sonraki adımlar

* [U-SQL veritabanı anlama](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [test etme ve yerel çalıştırma ve Azure Data Lake U-SQL SDK'sını kullanarak hata ayıklama U-SQL işleri](data-lake-analytics-data-lake-tools-local-run.md)


