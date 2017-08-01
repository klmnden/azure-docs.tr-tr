---
title: "Azure Analysis Services öğreticisi ek ders: Ayrıntı Satırları | Microsoft Docs"
description: "Azure Analysis Services öğreticisinde bir Ayrıntı Satırları İfadesinin nasıl oluşturulacağını açıklar."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: fde5cd9a9efc3a13e731a91962ced5c086a72355
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---
# <a name="supplemental-lesson---detail-rows"></a>Ek ders - Ayrıntı Satırları

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu ek derste DAX Düzenleyicisi'ni kullanarak özel bir Ayrıntı Satırları İfadesi tanımlayacaksınız. Ayrıntı Satırları İfadesi, son kullanıcılar için ölçünün toplu sonuçlarıyla ilgili daha fazla bilgi sağlayan bir ölçü özelliğidir. 
  
Bu dersin tahmini tamamlanma süresi: **10 dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu ek ders konusu bir tablo modelleme öğreticisinin parçasıdır. Bu ek dersteki görevleri gerçekleştirmeden önce tüm önceki dersleri tamamlamış veya bir Adventure Works İnternet Satışları örnek model projesini tamamlamış olmanız gerekir.  
  
## <a name="what-do-we-need-to-solve"></a>Neyi çözmemiz gerekiyor?
Bir Ayrıntı Satırları İfadesi eklemeden önce InternetTotalSales ölçümüzün ayrıntılarına bakalım.

1.  SSDT’de **Model** menüsü > **Excel'de çözümleme**’ye tıklayarak Excel'i açın ve boş bir PivotTable oluşturun.
  
2.  **PivotTable Alanları**’nda FactInternetSales tablosundaki **InternetTotalSales** ölçüsünü **Değerler**, DimDate tablosundaki **CalendarYear** ölçüsünü **Sütunlar**, **EnglishCountryRegionName** ölçüsünü **Satırlar**’a ekleyin. PivotTable bu durumda bölge ve yıla göre InternetTotalSales ölçüsünden toplu sonuçları verir. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. PivotTable'da bir yıl ve bölge adı için toplu değere çift tıklayın. Burada Avustralya ve 2014 yılının değerine çift tıkladık. Verileri (yararlı olmayan verileri) içeren yeni bir sayfa açılır.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Burada görmek istediğimiz, InternetTotalSales ölçümüzün toplu sonucuna katkıda bulunan veri sütunlarını ve satırlarını içeren bir tablodur. Bunu yapmak için, ölçünün bir özelliği olarak Ayrıntı Satırları İfadesini ekleyebiliriz.

## <a name="add-a-detail-rows-expression"></a>Ayrıntı Satırları İfadesi ekleme

#### <a name="to-create-a-detail-rows-expression"></a>Ayrıntı Satırları İfadesi oluşturmak için 
  
1. SSDT’deki FactInternetSales tablosunun ölçü kılavuzunda **InternetTotalSales** ölçüsüne tıklayın. 

2. **Özellikler** > **Ayrıntı Satırları İfadesi** menüsünde düzenleyici düğmesine tıklayarak DAX Düzenleyicisi’ni açın.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. DAX Düzenleyicisi'nde aşağıdaki ifadeyi girin:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Bu ifade FactInternetSales tablosundaki ad, sütun ve ölçü sonuçlarını belirtir ve bir kullanıcı PivotTable veya raporda toplu bir sonuca çift tıkladığında ilgili tablolar döndürülür.

4. Excel'e geri dönerek 3. Adımda oluşturulan sayfayı silin ve sonra bir toplu değere çift tıklayın. Bu kez, ölçü için tanımlanan Ayrıntı Satırları İfadesi özelliğiyle birlikte birçok yararlı veri içeren yeni bir sayfa açılır.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Modelinizi yeniden dağıtın.

  
## <a name="see-also"></a>Ayrıca Bkz.  
[SELECTCOLUMNS İşlevi (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Ek Ders - Dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Ek Ders - Düzensiz hiyerarşiler](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  

