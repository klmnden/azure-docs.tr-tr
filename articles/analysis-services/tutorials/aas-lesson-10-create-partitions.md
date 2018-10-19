---
title: 'Azure Analysis Services öğreticisi 10. Ders: Bölüm oluşturma | Microsoft Docs'
description: Azure Analysis Services öğretici projesinde bölümlerin nasıl oluşturulacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5aaaee6f9a69f9cb619935f18f614d7572a755d7
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429728"
---
# <a name="create-partitions"></a>Bölüm oluşturma

Bu derste, FactInternetSales tablosunu diğer bölümlerden bağımsız olarak işlenebilecek (yenilenebilecek) daha küçük mantıksal kısımlara ayıran bölümler oluşturursunuz. Varsayılan olarak, modelinize eklediğiniz her tablonun tablodaki tüm sütun ve satırları içeren tek bir bölümü vardır. FactInternetSales tablosu için verileri yıla göre bölmek, yani tablodaki beş yılın her biri için birer bölüm oluşturmak istiyoruz. Daha sonra her bölüm bağımsız olarak işlenebilir. Daha fazla bilgi edinmek için bkz. [Bölümler](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular). 
  
Bu dersin tahmini tamamlanma süresi: **15 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [9. Ders: Hiyerarşi Oluşturma](../tutorials/aas-lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Bölüm oluşturma  
  
#### <a name="to-create-partitions-in-the-factinternetsales-table"></a>FactInternetSales tablosunda bölümler oluşturmak için  
  
1.  Tablosal Model Gezgini’nde **Tablolar**’ı genişletin ve **FactInternetSales** > **Bölümler**’e sağ tıklayın.  
  
2.  Bölüm Yöneticisi’nde **Kopyala**’ya tıklayın ve adı **FactInternetSales2010** olarak değiştirin.
  
    Bölümün yalnızca belirli bir döneme ait satırları içermesini istediğinizden, 2010 yılı için sorgu ifadesini değiştirmeniz gerekir.
  
4.  **Tasarım**’a tıklayarak Sorgu Düzenleyicisi’ni açın ve sonra **FactInternetSales2010** sorgusuna tıklayın.

5.  Önizlemede **OrderDate** sütun başlığındaki aşağı oka ve ardından **Tarih/Saat Filtreleri** > **Arasında**’ya tıklayın.

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  Satırları Filtrele iletişim kutusunda, **Şu satırları göster: OrderDate** bölümünde **şundan sonra veya şuna eşit** seçeneğini değiştirmeyin ve tarih alanına **1/1/2010** yazın. **And** işlecini seçili halde bırakıp **şundan önce:** seçeneğini belirleyin ve tarih alanına **1/1/2011** yazıp **Tamam**’a tıklayın.

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    Sorgu Düzenleyicisi’ndeki UYGULANAN ADIMLAR’da Filtrelenen Satırlar adlı başka bir adım göründüğüne dikkat edin. Bu filtre yalnızca 2010’a it sipariş tarihlerini seçmek içindir.

8.  **İçeri Aktar**’a tıklayın.

    Bölüm Yöneticisi’nde sorgu ifadesinin artık ek bir Filtrelenen Satırlar yan tümcesi içerdiğine dikkat edin.

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    Bu deyim, filtrelenen satırlar yan tümcesinde belirtildiği gibi bu bölümün yalnızca OrderDate değerinin 2010 takvim yılında olduğu satırlardaki verileri içermesi gerektiğini belirtir.  
  
  
#### <a name="to-create-a-partition-for-the-2011-year"></a>2011 yılına ait bir bölüm oluşturmak için  
  
1.  Bölüm listesinde, oluşturduğunuz **FactInternetSales2010** bölümüne ve ardından **Kopyala**’ya tıklayın.  Bölüm adını **FactInternetSales2011** olarak değiştirin. 

    Yeni bir filtrelenen satırlar yan tümcesi oluşturmak için Sorgu Düzenleyicisi’ni kullanmanız gerekmez. 2010 için sorgunun bir kopyasını oluşturduğunuzdan, 2011 sorgusunda tek yapmanız gereken küçük bir değişiklik yapmaktır.
  
2.  Bu bölümün yalnızca 2011 yılına ait satırları içermesi için **Sorgu İfadesi**’nde, Filtrelenen Satırlar yan tümcesindeki yılları sırasıyla **2011** ve **2012** olarak değiştirin. Örneğin:  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="to-create-partitions-for-2012-2013-and-2014"></a>2012, 2013 ve 2014 yılına ait bölümler oluşturmak için.  
  
- Önceki adımları izleyerek 2012, 2013 ve 2014 için bölümler oluşturun ve Filtrelenen Satırlar yan tümcesindeki yılları yalnızca ilgili yıla ait satırları içerecek şekilde değiştirin. 
  

## <a name="delete-the-factinternetsales-partition"></a>FactInternetSales bölümünü silin
Artık her yıl için birer bölümünüz olduğuna göre FactInternetSales bölümünü silebilir ve bölümleri işlerken Tümünü işle’yi seçtiğinizde çakışmalardan kaçınabilirsiniz.

#### <a name="to-delete-the-factinternetsales-partition"></a>FactInternetSales bölümünü silmek için
-  FactInternetSales bölümüne ve ardından **Sil**’e tıklayın.



## <a name="process-partitions"></a>İşlem bölümleri  
Bölüm Yöneticisi’nde, oluşturduğunuz her yeni bölüme ait **Son İşlenme** sütununda bu bölümlerin hiç işlenmemiş olarak gösterildiğine dikkat edin. Bölüm oluştururken bu bölümlerdeki verileri yenilemek için bir Bölümleri İşle veya Tabloyu İşle işlemi çalıştırmalısınız.  
  
#### <a name="to-process-the-factinternetsales-partitions"></a>FactInternetSales bölümlerini işlemek için  
  
1.  Bölüm Yöneticisi’ni kapatmak için **Tamam**’a tıklayın.  
  
2.  **FactInternetSales** tablosuna ve sonra **Model** menüsü > **İşle** > **Bölümleri İşle**’ye tıklayın.  
  
3.  Bölümleri İşle iletişim kutusunda, **Mod**’un **İşlem Varsayılan** olarak ayarlandığını doğrulayın.  
  
4.  Oluşturduğunuz beş bölümün her biri için **İşlem** sütunundaki onay kutusunu seçin ve **Tamam**’a tıklayın.  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    Kimliğe Bürünme kimlik bilgileri istenirse, 2. Ders’te belirttiğiniz Windows kullanıcı adını ve parolasını girin.  
  
    **Veri İşleme** iletişim kutusu görünür ve her bölüm için işlem ayrıntılarını görüntüler. Her bölüm için farklı sayıda satır aktarıldığına dikkat edin. Her bölüm, yalnızca SQL Deyimi’nin WHERE yan tümcesinde belirtilen yıla ait olan satırları içerir. İşleme tamamlandığında, Veri İşleme iletişim kutusunu kapatın.  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>Sırada ne var?
Sonraki derse gidin: [11. Ders: Rol Oluşturma](../tutorials/aas-lesson-11-create-roles.md). 
