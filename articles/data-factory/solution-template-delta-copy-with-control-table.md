---
title: Azure Data Factory ile denetim tabloyla veritabanından delta kopya | Microsoft Docs
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
ms.openlocfilehash: 23e1255013cd5e52166fe0e59a8931dd9ecd81a0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967677"
---
# <a name="delta-copy-from-database-with-control-table"></a>Veritabanı denetim tablosu ile Delta Kopyala

Artımlı değişiklikleri (yeni veya güncelleştirilmiş satır) bir veritabanındaki bir tablodan yalnızca Azure'a bir dış denetim Tablo üst eşik değerini depolamak yüklemek istediğinizde.  Mevcut bir şablonu, o durum için tasarlanmıştır. 

Kaynak veritabanı şeması yeni veya güncelleştirilmiş satırları tanımlamak için içeren zaman damgası sütunu veya artırılmasının anahtarı gerekir. Bu şablon gerektirir.

Yeni veya güncelleştirilmiş satırları tanımlamak için kaynak veritabanında zaman damgası sütununa sahip, ancak delta kopya etkinleştirmek için bir dış denetim tablo oluşturmak istemiyorsanız, bir tetikleyici zamanlanmış süre okumak için bir değişken olarak kullanan bir işlem hattı almak için veri kopyalama aracını kullanabilirsiniz yalnızca kaynak veritabanından yeni satır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, her zaman önce eski eşik değerini alır ve ardından geçerli eşik değeri ile karşılaştırır. Bundan sonra bunu yalnızca değişiklikleri 2 eşik değer arasında karşılaştırma göre kaynak veritabanından kopyalar.  Tamamlandığında, delta veri yükleme sonraki açışınızda bir dış denetim tablosuna yeni üst eşik değeri depolar.

Şablon dört etkinlikleri içerir:
-   A **arama** bir dış denetim tablosunda depolanan eski üst eşik değerini almak için etkinlik.
-   A **arama** kaynak veritabanından geçerli üst eşik değerini almak için etkinlik.
-   A **kopyalama** değişiklikleri yalnızca kaynak veritabanından hedef deposuna kopyalamak için etkinlik. Kopyalama etkinliği kaynak veritabanında değişiklikleri tanımlamak için kullanılan sorgu olarak benzer ' seçin * ÖĞESİNDEN Data_Source_Table burada TIMESTAMP_Column > "son üst eşik" ve TIMESTAMP_Column < = "geçerli üst eşik" '.
-   A **SqlServerStoredProcedure** sonraki açışınızda bir delta kopya için dış denetim tabloya geçerli üst eşik değeri yazmak için etkinlik.

Şablon beş parametreleri tanımlar:
-   Parametre *Data_Source_Table_Name* istediğiniz verileri yüklemek için kaynak veritabanından tablo adı olduğu.
-   Parametre *Data_Source_WaterMarkColumn* yeni veya güncelleştirilmiş satırları tanımlamak için kullanılan kaynak tablodaki sütun adı. Normalde, bu sütunun türü datetime veya INT vb. olabilir.
-   Parametre *Data_Destination_Folder_Path* veya *Data_Destination_Table_Name* verileri hedef deponuza kopyalanıp burada yerdir.
-   Parametre *Control_Table_Table_Name* üst eşik değerini depolamak için dış denetim tablo adıdır.
-   Parametre *Control_Table_Column_Name* üst eşik değerini depolamak için dış denetim tablodaki sütun adı.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Yüklemek istediğiniz kaynak tablosu keşfedin ve yeni veya güncelleştirilmiş satırların dilimlemek için kullanılabilen üst eşik sütunu tanımlayın. Normalde, bu sütunun türü datetime veya INT vb. ve yeni satır eklendiğinde artırma tutma verisini olabilir.  Örnek kaynak tablosundan (tablo adı: data_source_table) sütun aşağıda kullanabiliriz *LastModifytime* üst eşik sütunu olarak.

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
    
2. Bir SQL server veya SQL Azure, delta veri yükleme için üst eşik değerini depolamak için bir denetim tablo oluşturun. Örnekte aşağıdaki, Denetim tablonun adı görebileceğiniz *watermarktable*. İçinde üst eşik değerini depolamak için sütun adı olduğu *WatermarkValue* ve kendi türünün *datetime*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Aynı SQL Server'da saklı yordam oluşturmak veya SQL Azure denetim tablo oluşturmak için kullanılır. Saklı yordam, delta veri yükleme sonraki için dış denetim tabloya yeni üst eşik değeri yazmak için kullanılır.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Şablon gidin **Delta kopya veritabanından**, oluşturup bir **yeni bağlantı** Kaynak veritabanınıza verileri nerede olarak kopyalamaktır.

    ![Kaynak tablosu için yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Oluşturma bir **yeni bağlantı** burada veri kopyalamak için hedef veri deposuna.

    ![Hedef tablo için yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Oluşturma bir **yeni bağlantı** dış denetimine tablo ve saklı yordam.  Burada denetim tablo oluşturduğunuz ve #2 ve #3. adımda saklı yordamı veritabanına bağlanıyor.

    ![Denetim tablo veri deposunda yeni bir bağlantı oluşturun](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Tıklayın **bu şablonu kullan**.

     ![Bu şablonu kullan](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir işlem hattı bakın:

     ![Gözden geçirme işlem hattı](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Saklı yordam etkinliği tıklatın, **saklı yordam adı**, tıklayın **içeri aktarma parametresi** tıklatıp **dinamik içerik Ekle**.  

     ![Saklı yordam etkinliği](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. İçerik yazma **@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** tıklatıp **son**.  

     ![Saklı yordam için parametre içeriğini yazma](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Tıklayın **hata ayıklama**, giriş parametreleri ve tıklayın **son**.

    ![Hata ayıklama tıklayın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir sonuç bakın:

    ![Sonucu gözden geçir](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Kaynak tablosunda yeni satırlar oluşturabilirsiniz.  Yeni satırlar oluşturmak için örnek sql olabilir şu şekilde:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
13. Tıklayarak işlem hattını yeniden çalıştırma **hata ayıklama**, giriş parametreleri ve tıklayın **son**.

    ![Hata ayıklama tıklayın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

14. Yalnızca yeni satırlar hedefe kopyalanan görürsünüz.

15. (İsteğe bağlı) SQL veri ambarı veri hedef olarak seçerseniz, ayrıca SQL veri ambarı Polybase tarafından gereken bir hazırlık olarak Azure blob depolama bağlantısı giriş gerekir.  Blob depolamadaki bir kapsayıcıda zaten oluşturulduğundan emin olun.  
    
    ![Polybase yapılandırın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)