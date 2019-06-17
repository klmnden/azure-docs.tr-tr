---
title: Azure Data Factory ile denetim tablo kullanarak bir veritabanı kopyalama toplu | Microsoft Docs
description: Azure Data Factory kullanarak kaynak tablo bölüm listesi depolamak için bir dış denetim tablosunu kullanarak bir veritabanından toplu veri kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.openlocfilehash: c4224693642e8c9f76deedc0c8ad8586e122cc23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60635454"
---
# <a name="bulk-copy-from-a-database-with-a-control-table"></a>Veritabanı denetim tablosu ile toplu kopyalama

Oracle Server, Netezza, Teradata veya SQL Server veri ambarında verileri Azure SQL veri ambarı'na kopyalamak için birden çok tablodan büyük miktarda veriyi yüklemek zorunda. Genellikle, tek bir tablodan satır ile birden çok iş parçacığı paralel yükleyebilir, böylece her tabloda bölümlenecek verilere sahip. Bu makalede, bu gibi durumlarda kullanılacak bir şablon açıklanır.

 >! Az sayıda tablo nispeten küçük veri birimi ile verileri SQL veri ambarı'na kopyalamak istiyorsanız, bunu kullanmak için daha verimli Not [Azure Data Factory veri kopyalama aracını](copy-data-tool.md). Bu makalede açıklanan senaryo için gereksinim duyduğunuz kadar çok şablonudur.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, bir dış denetim tablosundan kopyalamak için kaynak veritabanı bölümlerini bir listesini alır. Ardından kaynak veritabanındaki her bölüm üzerinden yinelenir ve veriler hedefe kopyalar.

Şablon üç etkinlikleri içerir:
- **Arama** emin veritabanı bölümlerin listesine bir dış denetim tablosundan alır.
- **ForEach** arama etkinliğinden bölüm listesi alır ve her bölüm kopyalama etkinliğine yinelenir.
- **Kopyalama** her bölüm kaynak veritabanı deposundan hedef deposuna kopyalar.

Şablon beş parametreleri tanımlar:
- *Control_Table_Name* kaynak veritabanı için bölüm listesi depolar, dış denetim tablo.
- *Control_Table_Schema_PartitionID* dış denetim tablonuzdaki her bölüm kimliğini depolar sütun adı adıdır Bölüm kimliği kaynak veritabanındaki her bölüm için benzersiz olduğundan emin olun.
- *Control_Table_Schema_SourceTableName* her tablo adı kaynak veritabanından depolar, dış denetim tablodur.
- *Control_Table_Schema_FilterQuery* kaynak veritabanındaki her bölüm veri alma filtre sorgusunu depolayan dış denetim tablosunda sütun adı. Verileri yıla göre bölümlenmiş, örneğin, her satır için depolanan sorguyu benzer olabilir ' seçin * datasource öğesinden burada LastModifytime > = '' 2015-01-01: 00:00:00 '' ve LastModifytime < '' 2015-12-31 23:59:59.999'' ='.
- *Data_Destination_Folder_Path* verileri hedef deponuza kopyalanıp burada yoludur. Bu parametre yalnızca, seçtiğiniz hedef dosya tabanlı depolama ise görülebilir. SQL veri ambarı hedef depo olarak seçerseniz, bu parametre gerekli değildir. Ancak tablo adları ve SQL veri ambarı şema olanlara kaynak veritabanındaki ile aynı olmalıdır.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Denetim tabloya toplu kopyalama kaynak veritabanı bölüm listesi depolamak için SQL Server'ı veya Azure SQL veritabanı oluşturun. Aşağıdaki örnekte, kaynak veritabanında beş bölümler vardır. Üç bölüm içindir *datasource_table*, ve iki *project_table*. Sütun *LastModifytime* tablodaki verileri bölümlemek için kullanılan *datasource_table* kaynak veritabanından. İlk bölüm okumak için kullanılan sorgu ' seçin * datasource_table gelen burada LastModifytime > = '' 2015-01-01 00:00:00 '' ve LastModifytime < '' 2015-12-31 23:59:59.999'' ='. Benzer bir sorgu verilerini diğer bölümlerden okumak için kullanabilirsiniz.

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. Git **toplu kopyalama veritabanından** şablonu. Oluşturma bir **yeni** 1. adımda oluşturduğunuz denetimi dış tablo bağlantısı.

    ![Denetim tablosuna yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Oluşturma bir **yeni** verilerden Kopyalamakta kaynak veritabanı bağlantısı.

     ![Kaynak veritabanı için yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Oluşturma bir **yeni** bağlantı hedef veri depolamak için verileri kopyaladığınız.

    ![Hedef depo için yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Seçin **bu şablonu kullan**.

    ![Bu şablonu kullanın.](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Aşağıdaki örnekte gösterildiği gibi işlem hattını görürsünüz:

    ![İşlem hattı gözden geçirin](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Seçin **hata ayıklama**, girin **parametreleri**ve ardından **son**.

    ![Tıklayın ** hata ayıklama **](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Aşağıdaki örneğe benzer sonuçlar görürsünüz:

    ![Sonucu gözden geçir](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (İsteğe bağlı) SQL veri ambarı veri hedef olarak seçerseniz, hazırlama, SQL veri ambarı Polybase gerektirdiği için Azure Blob depolama alanına bir bağlantı girmelisiniz. Blob depolamadaki bir kapsayıcıda zaten oluşturulduğundan emin olun.
    
    ![Polybase ayarı](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
