---
title: Toplu kopyalama ile Azure Data Factory ile kontrol tablosu veritabanından | Microsoft Docs
description: Tam olarak verileri toplu olarak Azure Data Factory ile kaynak tabloların bölüm listesi depolamak için bir dış denetim tablo kullanarak bir veritabanını kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
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
ms.openlocfilehash: b267da18f2537e462ecda0ac265eac07a069c293
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967699"
---
# <a name="bulk-copy-from-database-with-control-table"></a>Denetim tabloyla veritabanından toplu kopyalama

Azure'da Oracle Sunucusu, Netezza sunucusu, Teradata sunucusunda veya SQL Server gibi veri ambarı'nızı verileri kopyalamak istediğiniz zaman birden çok tablodan veri kaynaklarında çok büyük miktarda veri yüklemek zorunda değilsiniz. Çoğu durumda, böylece tek bir tablodan satır ile birden çok iş parçacığı paralel yükleyebilir her tabloda daha ayrıntılı bölümlenecek veri yok. Mevcut bir şablonu, o durum için tasarlanmıştır. 

Az sayıda tablo verilerinin küçük boyutlu verileri kopyalamak isterseniz, sizin için "veri kopyalama aracı" gitmek daha verimli olur kopyalama etkinliği ya da foreach etkinliği +, işlem hattında kopyalama etkinliği tek tek sağlamak için. Bu şablon çalışması için bu basit kullanmanız daha büyük.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon bir dış denetim tablosundan hedef deponun üzerinden kopyalanması gereken kaynak veritabanı bölümlerini listesini alır ve ardından kaynak veritabanındaki her bölüm üzerinden yinelenir ve veri kopyalama işlemini gerçekleştirir.

Şablon üç etkinlikleri içerir:
-   A **arama** bir dış denetim tablosundan kaynak veritabanı bölümlerini listesini almak için etkinlik.
-   A **ForEach** arama etkinliğinden bölüm listesi almak ve her birinin kopyalama etkinliği için yinelemek için etkinlik.
-   A **kopyalama** için her bölüm kaynak veritabanı deposundan hedef depolama alanına kopyalama etkinliği.

Şablon beş parametreleri tanımlar:
-   Parametre *Control_Table_Name* dış denetim tablonuzun tablo adıdır. Denetim tablo, kaynak veritabanı için bölüm listesi depolamak için kullanılır.
-   Parametre *Control_Table_Schema_PartitionID* her bölüm kimliği depolamak için dış denetim tablodaki sütun adı Bölüm kimliği kaynak veritabanında her bölüm için benzersiz olduğundan emin olun.
-   Parametre *Control_Table_Schema_SourceTableName* dış denetim tablonuzdaki her tablo adı kaynak veritabanından depolamak için sütun adı.
-   Parametre *Control_Table_Schema_FilterQuery* dış denetim tablonuzdaki her bölüm kaynak veritabanında veri alma filtre sorgusunu depolamak için sütun adı. Veriler her yıla göre bölümlenir, örneğin, her satır için depolanan sorgu olarak benzer olabilir ' seçin * datasource öğesinden burada LastModifytime > = '' 2015-01-01: 00:00:00 '' ve LastModifytime < '' 2015-12-31 23:59:59.999'' ='
-   Parametre *Data_Destination_Folder_Path* verileri hedef deponuza kopyalanıp burada klasör yoludur.  Bu parametre yalnızca, seçtiğiniz hedef bir dosya tabanlı depolama olan mağazanınsa görülebilir.  SQL veri ambarı hedef depo olarak seçerseniz, burada girilen için gerekli parametre yoktur. Ancak tablo adları ve SQL veri ambarı şema olanlara kaynak veritabanında aynı olmalıdır.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Bir SQL server veya SQL Azure ' ın toplu kopyalama kaynak veritabanı bölümü listesini depolamak için bir denetim tablo oluşturun.  Örnekten kaynak veritabanınızda beş bölümleri olan üç bölüm için bir tablo nerede görebilirsiniz:*datasource_table* ve iki bölüm için başka bir tablo:*project_table*. Sütun *LastModifytime* tablodaki verileri bölümlemek için kullanılan *datasource_table* kaynak veritabanından. İlk bölüm okumak için kullanılan sorgu ' seçin * datasource_table gelen burada LastModifytime > = '' 2015-01-01 00:00:00 '' ve LastModifytime < '' 2015-12-31 23:59:59.999'' ='.  Ayrıca, diğer bölümlerden verileri okumak için benzer sorgu görebilirsiniz. 

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

2. Şablon gidin **toplu kopyalama veritabanından**, oluşturup bir **yeni bağlantı** dış denetim tablonuzun için.  Bu bağlantıyı nerede denetim Tablo #1. adımda oluşturmuştunuz veritabanına bağlanıyor.

    ![Denetim tablosuna yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Oluşturma bir **yeni bağlantı** Kaynak veritabanınıza verileri nerede olarak kopyalamaktır.

     ![Kaynak veritabanı için yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Oluşturma bir **yeni bağlantı** burada veri kopyalamak için hedef veri deposuna.

    ![Hedef depo için yeni bir bağlantı oluşturun](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Tıklayın **bu şablonu kullan**.

    ![Bu şablonu kullan](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir işlem hattı bakın:

    ![İşlem hattı gözden geçirin](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Tıklayın **hata ayıklama**, giriş parametreleri ve ardından **son**

    ![Hata ayıklama tıklayın](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir sonuç bakın:

    ![Sonucu gözden geçir](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (İsteğe bağlı) SQL veri ambarı veri hedef olarak seçerseniz, ayrıca SQL veri ambarı Polybase tarafından gereken bir hazırlık olarak Azure blob depolama bağlantısı giriş gerekir.  Blob depolamadaki bir kapsayıcıda zaten oluşturulduğundan emin olun.  
    
    ![Polybase ayarı](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)