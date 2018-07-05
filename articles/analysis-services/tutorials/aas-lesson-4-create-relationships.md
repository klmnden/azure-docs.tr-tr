---
title: 'Azure Analysis Services öğreticisi 4. Ders: İlişki oluşturma | Microsoft Docs'
description: Azure Analysis Services öğretici projesinde ilişkilerin nasıl oluşturulacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5007e18db0af40621ab4b30a16d705d3a5b3915c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443826"
---
# <a name="create-relationships"></a>İlişki oluşturma

Bu derste, verileri içeri aktarıp farklı tablolar arasında yeni ilişkiler oluşturduğunuzda otomatik olarak oluşturulan ilişkileri doğrularsınız. İlişki, iki tablo arasında kurulan ve bu tablolardaki verilerin nasıl bağıntılı olması gerektiğini belirleyen bir bağlantıdır. Örneğin, DimProduct tablosu ile DimProductSubcategory tablosu arasında, her ürünün bir alt kategoriye ait olması temelinde bir ilişki mevcuttur. Daha fazla bilgi edinmek için bkz. [İlişkiler](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Bu dersin tahmini tamamlanma süresi: **10 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [3. Ders: Tarih Tablosu olarak işaretleme](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Mevcut ilişkileri gözden geçirme ve yeni ilişki ekleme  
Veri Al seçeneğini kullanarak verileri içeri aktardığınızda, AdventureWorksDW2014 veritabanından yedi tablo aldınız. İlişkisel bir veritabanındaki verileri içeri aktardığınızda, verilerle birlikte genellikle mevcut ilişkiler de otomatik olarak içeri aktarılır. Veri Al seçeneğinin veri modelindeki ilişkileri otomatik olarak oluşturması için veri kaynağındaki tabloların arasında ilişki olması gerekir.

Kendi modelinizi yazmaya başlamadan önce tablolar arasındaki bu ilişkilerin düzgün oluşturulduğunu doğrulamanız gerekir. Bu öğretici için üç yeni ilişki de ekleyeceksiniz.  

  
#### <a name="to-review-existing-relationships"></a>Mevcut ilişkileri gözden geçirmek için  
  
1.  **Model** menüsü > **Model Görünümü** > **Diyagram Görünümü**’ne tıklayın.  

    Model tasarımcısı artık içeri aktardığınız tüm tabloları ve bunlar arasındaki çizgileri görüntüleyen dikey bir grafik biçimi olan Diyagram Görünümü’nde açılır. Tablolar arasındaki çizgiler, verileri içeri aktardığınızda otomatik olarak oluşturulan ilişkileri gösterir.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    > [!NOTE]
    > Tablolar arasında ilişki olmaması veri kaynağındaki tabloların arasında da ilişki olmadığı anlamına gelir.

    Model tasarımcısının sağ alt köşesindeki mini eşlem denetimlerini kullanarak olabildiğince fazla sayıda tabloyu dahil edin. Ayrıca, tabloları tıklayarak farklı konumlara sürükleme, tabloları birbirine yaklaştırma ya da belirli bir sıralamaya sokma seçeneğiniz de vardır. Tabloların taşınması, tablolar arasında mevcut olan ilişkileri etkilemez. Belirli bir tablodaki tüm sütunları görüntülemek için bir tablonun kenarını tıklayıp sürükleyerek genişletin veya küçültün.  
  
2.  **DimCustomer** tablosu ile **DimGeography** tablosu arasındaki kesiksiz çizgiye tıklayın. Bu iki tablo arasındaki kesiksiz çizgi, bu ilişkinin etkin olduğunu, yani DAX formülleri hesaplanırken varsayılan olarak kullanıldığını gösterir.  
  
    Artık hem **DimCustomer** tablosundaki **GeographyKey** sütununun hem de **DimGeography** tablosundaki **GeographyKey** sütununun bir kutu içinde göründüğüne dikkat edin. İlişkide bu sütunlar kullanılır. İlişkinin özellikleri artık **Özellikler** penceresinde de görünür.  
  
    > [!TIP]  
    > Model tasarımcısını diyagram görünümünde kullanmaya ek olarak, tüm tablolar arasındaki ilişkileri bir tablo biçiminde görüntülemek için İlişkileri Yönet iletişim kutusunu da kullanabilirsiniz. Tablosal Model Gezgini’nde **İlişkiler** > **İlişkileri Yönet**’e sağ tıklayın.
  
3.  AdventureWorksDW veritabanından tabloların her biri içeri aktarılırken aşağıdaki ilişkilerin oluşturulduğunu doğrulayın:  
  
    |Etkin|Tablo|İlişkili Arama Tablosu|  
    |----------|---------|------------------------|  
    |Evet|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Evet|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Evet|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Evet|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Evet|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    İlişkilerden herhangi biri eksikse modelinizin şu tabloları içerdiğini doğrulayın: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory ve FactInternetSales. Aynı veri kaynağı bağlantısından alınan tablolar farklı zamanlarda içeri aktarılmışsa, varsa bu tablolar arasındaki ilişkiler oluşturulmaz ve ilişkilerin el ile oluşturulması gerekir. İlişki olmaması, veri kaynağında da ilişki olmadığı anlamına gelir. Bunları veri modelinde el ile oluşturmanız gerekir.

### <a name="take-a-closer-look"></a>Daha yakından bakın
Diyagram Görünümü’nde tablolar arasındaki ilişkiyi gösteren çizgilerde bir ok, bir yıldız ve bir rakam olduğuna dikkat edin.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

Ok, filtre yönünü gösterir. Yıldız işareti, tablonun ilişki kardinalitesinde çoklu tarafta olduğunu gösterirken, bir rakamı tablonun ilişkinin bir tarafı olduğunu gösterir. Bir ilişkiyi düzenlemeniz gerekiyorsa, örneğin ilişki filtresinin yönünü veya kardinalitesini değiştirmeniz gerekiyorsa, ilişki çizgisine çift tıklayarak İlişkiyi Düzenle iletişim kutusunu açın.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Bu özellikler gelişmiş veri modellemesine yöneliktir ve bu öğreticinin kapsamı dışındadır. Daha fazla bilgi edinmek için bkz. [Analysis Services’da tablosal modeller için çift yönlü çapraz filtreler](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services).

Bazı durumlarda, zincir iş mantığını desteklemek için modelinizdeki tablolar arasında ek ilişkiler oluşturmanız gerekebilir. Bu öğretici için FactInternetSales tablosu ile DimDate tablosu arasında üç ek ilişki oluşturmanız gerekir.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Tablolar arasında yeni ilişkiler eklemek için  
  
1.  Model tasarımcısında, **FactInternetSales** tablosundan **OrderDate** sütununa tıklayıp basılı tutun, sonra imleci **DimDate** tablosundaki **Tarih** sütununa sürükleyin ve bırakın.  

    **İnternet Satışları** tablosundaki **OrderDate** sütunu ile **Tarih** tablosundaki **Tarih** sütunu arasında etkin bir ilişki oluşturduğunuz gösteren kesiksiz bir çizgi görünür. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > İlişki oluşturulurken, birincil tablo ile ilgili arama tablosu arasındaki kardinalite ve filtre yönü otomatik olarak seçilir.  
  
2.  **FactInternetSales** tablosunda **DueDate** sütununa tıklayıp basılı tutun, sonra imleci **DimDate** tablosundaki **Tarih** sütununa sürükleyin ve bırakın.  
  
    **FactInternetSales** tablosundaki **DueDate** sütunu ile **DimDate** tablosundaki **Tarih** sütunu arasında etkin olmayan bir ilişki oluşturduğunuzu gösteren bir noktalı çizgi görünür. Tablolar arasında birden çok ilişki oluşturabilirsiniz, ancak aynı anda yalnızca bir ilişki etkin olabilir. Özel DAX deyimlerinde özel toplamalar gerçekleştirmek amacıyla etkin olmayan ilişkiler etkinleştirilebilir.  
  
3.  Son olarak bir tane daha ilişki oluşturun. **FactInternetSales** tablosunda **ShipDate** sütununa tıklayıp basılı tutun, sonra imleci **DimDate** tablosundaki **Tarih** sütununa sürükleyin ve bırakın.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Sırada ne var?
[5. Ders: Hesaplanan sütun oluşturma](../tutorials/aas-lesson-5-create-calculated-columns.md).
  
  
  
