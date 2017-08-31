---
title: "Azure Analysis Services öğreticisi 2. Ders: Verileri alma| Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde verilerin nasıl alınacağını ve içeri aktarılacağını açıklar."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: e77de4b9a74b528fa8a7ce86424fc14628b2cacc
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---

# <a name="lesson-2-get-data"></a>2. Ders: Verileri alma

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste, SSDT’deki Verileri Al özelliğini kullanarak AdventureWorksDW2014 örnek veritabanına erişir, verileri seçer, önizleme ve filtreleme uygular ve sonra model çalışma alanınıza aktarırsınız.  
  
Verileri Al’ı kullanarak birçok farklı kaynaktaki verileri içeri aktarabilirsiniz: Azure SQL Veritabanı, Oracle, Sybase, OData Feed, Teradata, dosyalar ve daha fazlası. Veriler bir Power Query M formül ifadesi kullanılarak da sorgulanabilir.
  
Bu dersin tahmini tamamlanma süresi: **10 dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu konu, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin bir parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [1. Ders: Yeni tablosal model projesi oluşturma](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Bağlantı oluşturma  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a>AdventureWorksDW2014 veritabanına bir bağlantı oluşturmak için  
  
1.  Tablosal Model Gezgini'nde **Veri Kaynakları** > **Veri Kaynağından İçeri Aktar**’a sağ tıklayın.  
  
    Bunu yaptığınızda Verileri Al açılır ve bir veri kaynağına bağlanma konusunda size rehberlik eder. **Çözüm Gezgini**’nde Tablosal Model Gezgini’ni görmüyorsanız, **Model.bim**’e çift tıklayarak modeli tasarımcıda açın. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  Verileri Al’da **Veritabanı** > **SQL Server Veritabanı** > **Bağlan**’a tıklayın.  
  
3.  **SQL Server Veritabanı** iletişim kutusunda, **Sunucu** bölümüne AdventureWorksDW2014 veritabanını yüklediğiniz sunucunun adını yazın ve **Bağlan**’a tıklayın.  

4.  Kimlik bilgilerini girmeniz istendiğinde, Analysis Services’ın verileri içeri alırken ve işlerken veri kaynağına bağlanmak için kullandığı kimlik bilgilerini belirtmeniz gerekir. **Kimliğe Bürünme Modu**’nda **Hesap Kimliğine Bürün**’ü seçip kimlik bilgilerini girin ve **Bağlan**’a tıklayın. Parola kullanım süresinin sona ermediği bir hesap kullanmanız önerilir.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > Windows kullanıcı hesabı ve parolasını kullanma, bir veri kaynağına bağlanmanın en güvenli yolunu sağlar.
  
5.  Gezgin’de **AdventureWorksDW2014** veritabanını seçip **Tamam**’a tıklayın. Bunu yaptığınızda veritabanı bağlantısı oluşturulur. 
  
6.  Gezgin’de şu tabloların onay kutusunu seçin: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** ve **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
Tamam'a tıkladığınızda Sorgu Düzenleyicisi açılır. Sonraki bölümde yalnızca içeri aktarmak istediğiniz verileri seçersiniz.

  
## <a name="filter-the-table-data"></a>Tablo verilerini filtreleme  
AdventureWorksDW2014 örnek veritabanındaki tablolar, modelinize eklenmesi gerekmeyen veriler içeriyor. Modelin kullandığı bellek içi alandan tasarruf etmek için gereksiz verileri mümkün olduğunca filtrelemek istersiniz. Tablolardaki bazı sütunların çalışma alanı veritabanında ya da oluşturulduktan sonra model veritabanında içeri aktarılmaması için bu sütunları filtrelersiniz. 
  
#### <a name="to-filter-the-table-data-before-importing"></a>İçeri aktarmadan önce tablo verilerini filtrelemek için  
  
1.  Sorgu Düzenleyicisi’nde **DimCustomer** tablosunu seçin. Veri kaynağında (AdventureWorksDWQ2014 örnek veritabanınız) DimCustomer tablosunun bir görünümü açılır. 
  
2.  Çoklu seçimi (Ctrl+tıklama) kullanarak **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**’ı seçin, sağ tıklayın ve **Sütunları Kaldır**’a tıklayın. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Bu sütunlardaki değerler İnternet satışları analiziyle ilgili olmadığından, bu sütunların içeri aktarılması gerekmez. Gereksiz sütunların dışarıda bırakılması, modelinizin daha küçük ve daha verimli olmasını sağlar.  
  
4.  Her tabloda aşağıdaki sütunları kaldırarak geriye kalan tabloları da filtreleyin:  
    
    **DimDate**
    
      |Sütun|  
      |--------|  
      |DateKey|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Sütun|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Sütun|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Sütun|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Sütun|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      |Sütun|  
      |------------------|  
      |**OrderDateKey**|  
      |**DueDateKey**|  
      |**ShipDateKey**|   
  
## <a name="Import"></a>Seçilen tabloları ve sütun verilerini içeri aktarma  
Gereksiz verilerin önizlemesini yapıp bunları filtrelediğinize göre, artık geriye kalan ve istediğiniz verileri içeri aktarabilirsiniz. Sihirbaz, tablo verilerinin yanı sıra varsa tablolar arasındaki ilişkileri içeri aktarır. Modelde yeni tablolar ile sütunlar oluşturulur ve filtrelediğiniz veriler içeri aktarılmaz.  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a>Seçilen tabloları ve sütun verilerini içeri aktarmak için  
  
1.  Seçimlerinizi gözden geçirin. Her şey yolundaysa **İçeri Aktar**’a tıklayın. Veri İşleme iletişim kutusu, veri kaynağınızdan çalışma alanı veritabanınıza aktarılmakta olan verilerin durumunu gösterir.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  **Kapat**’a tıklayın.  

  
## <a name="save-your-model-project"></a>Model projenizi kaydetme  
Model projenizi sık sık kaydetmeniz önemlidir.  
  
#### <a name="to-save-the-model-project"></a>Model projesini kaydetmek için  
  
-   **Dosya** > **Tümünü Kaydet**’e tıklayın.  
  
## <a name="whats-next"></a>Sırada ne var?
[3. Ders: Tarih Tablosu Olarak İşaretleme](../tutorials/aas-lesson-3-mark-as-date-table.md).

  
  

