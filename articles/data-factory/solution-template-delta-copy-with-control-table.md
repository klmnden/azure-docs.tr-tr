---
title: Azure Data Factory ile denetim tablosunu kullanarak bir veritabanından delta kopya | Microsoft Docs
description: Artımlı olarak Azure Data Factory ile bir veritabanından yalnızca yeni veya güncelleştirilmiş bir satır kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
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
ms.date: 12/24/2018
ms.openlocfilehash: c32592ce539eeb2dec71792e4a6eb31e7d904eff
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60312516"
---
# <a name="delta-copy-from-a-database-with-a-control-table"></a>Veritabanı denetim tablosu ile Delta kopya

Bu makalede, artımlı olarak yeni veya güncelleştirilmiş satırları bir veritabanı tablosundan Azure'a üst eşik değerini depolayan bir dış denetim tablosunu kullanarak yüklemek kullanılabilir olan bir şablon açıklanır.

Bu şablon, kaynak veritabanı şemasını yeni veya güncelleştirilmiş satırları tanımlamak için bir zaman damgası sütunu veya artırılmasının anahtar içermesi gerekir.

>[!NOTE]
> Yeni veya güncelleştirilmiş satırları tanımlamak için kaynak veritabanında bir zaman damgası sütununa sahip ancak delta kopya için kullanmak üzere bir dış denetim tablo oluşturmak istemiyorsanız, bunun yerine kullanabileceğiniz [Azure Data Factory veri kopyalama aracını](copy-data-tool.md) bir işlem hattı alınamıyor. Bu araç, yeni satır kaynak veritabanından okumak için bir değişken olarak bir tetikleyici zamanlanmış süre kullanır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, önce eski eşik değerini alır ve geçerli eşik değeri ile karşılaştırır. Bundan sonra değişiklikleri yalnızca kaynak veritabanından iki eşik değeri arasında bir karşılaştırma göre kopyalar. Son olarak, delta veri yükleme sonraki açışınızda bir dış denetim tablosuna yeni üst eşik değeri depolar.

Şablon dört etkinlikleri içerir:
- **Arama** bir dış denetim tablosunda depolanan eski üst eşik değerini alır.
- Başka bir **arama** etkinlik kaynak veritabanından geçerli üst eşik değerini alır.
- **Kopyalama** değişiklikleri yalnızca kaynak veritabanından hedef deposuna kopyalar. Kaynak veritabanındaki değişiklikleri tanımlayan sorgu benzer ' seçin * ÖĞESİNDEN Data_Source_Table burada TIMESTAMP_Column > "son üst eşik" ve TIMESTAMP_Column < = "geçerli üst eşik" '.
- **SqlServerStoredProcedure** sonraki geçerli üst eşik değerini bir delta kopya için dış denetim tabloya yazar.

Şablon beş parametreleri tanımlar:
- *Data_Source_Table_Name* verilerden yüklemek istediğiniz kaynak veritabanında tablo.
- *Data_Source_WaterMarkColumn* yeni tanımlamak için kullanılan veya satır güncelleştirilmiş olan kaynak tablodaki sütunun adıdır. Bu sütunun türü genellikle *datetime*, *INT*, veya benzer.
- *Data_Destination_Folder_Path* veya *Data_Destination_Table_Name* nerede verileri kopyalanır hedef deponuzda yerdir.
- *Control_Table_Table_Name* dış denetim tablo, üst eşik değeri depolar.
- *Control_Table_Column_Name* üst eşik değerini depolayan dış denetim tablodaki sütundur.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Kaynak keşfedin yüklemek istediğiniz tablo ve yeni veya güncelleştirilmiş satırları tanımlamak için kullanılan üst eşik sütunu tanımlayın. Bu sütunun türü olabilir *datetime*, *INT*, veya benzer. Yeni satırlar eklendikçe bu sütunun değeri artırır. Aşağıdaki örnek kaynak tablosundan (data_source_table) kullanabiliriz *LastModifytime* sütun üst eşik sütunu olarak.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Delta veri yükleme için üst eşik değerini depolamak için SQL Server'ı veya Azure SQL veritabanı denetimi tablo oluşturun. Aşağıdaki örnekte denetim tablo adıdır. *watermarktable*. Bu tabloda *WatermarkValue* üst eşik değerini depolayan bir sütundur ve türünün *datetime*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Denetim tablo oluşturmak için kullanılan aynı SQL Server veya Azure SQL veritabanı örneğinde bir saklı yordamı oluşturun. Saklı yordam, delta veri yükleme sonraki dış denetim tablosuna yeni üst eşik değeri yazmak için kullanılır.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Git **Delta kopya veritabanından** şablonu. Oluşturma bir **yeni** veri kopyalama istediğiniz kaynak veritabanı bağlantısı.

    ![Kaynak tablosu için yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Oluşturma bir **yeni** verileri kopyalamak istediğiniz hedef veri deposuna bağlantı.

    ![Hedef tablo için yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Oluşturma bir **yeni** 2 ve 3. adımda oluşturduğunuz saklı yordam ve denetim dış tablo bağlantısı.

    ![Denetim tablo veri deposunda yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Seçin **bu şablonu kullan**.

     ![Bu şablonu kullanın.](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Aşağıdaki örnekte gösterildiği gibi kullanılabilir işlem hattı bakın:

     ![İşlem hattı gözden geçirin](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Seçin **saklı yordamı**. İçin **saklı yordam adı**, seçin **[update_watermark]** . Seçin **parametreyi içeri aktar**ve ardından **dinamik içerik Ekle**.  

     ![Saklı yordam etkinliği ayarlayın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. İçerik yazma  **\@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** ve ardından **son**.  

     ![Saklı yordamın parametreleri için içerik yazma](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Seçin **hata ayıklama**, girin **parametreleri**ve ardından **son**.

    ![Seçin ** hata ayıklama **](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Aşağıdaki örneğe benzer sonuçlar görüntülenir:

    ![Sonucu gözden geçir](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Kaynak tablosunda yeni satırlar oluşturabilirsiniz. Yeni satırlar oluşturmak için örnek SQL dil şu şekildedir:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
14. İşlem hattını yeniden çalıştırmak için seçin **hata ayıklama**, girin **parametreleri**ve ardından **son**.

    ![Seçin ** hata ayıklama **](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

    Yalnızca yeni satırlar hedefe kopyalandığını görürsünüz.

15. (İsteğe bağlı:) SQL veri ambarı veri hedefi olarak seçtiyseniz, SQL veri ambarı Polybase tarafından gerekli olan hazırlama için aynı zamanda Azure Blob Depolama için bir bağlantı sağlamanız gerekir. Kapsayıcıya Blob Depolama alanında zaten oluşturulmuş olduğundan emin olun.
    
    ![Polybase yapılandırın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile denetim tablosunu kullanarak bir veritabanından toplu kopyalama](solution-template-bulk-copy-with-control-table.md)
- [Azure Data Factory ile birden çok kapsayıcı dosyaları Kopyala](solution-template-copy-files-multiple-containers.md)
