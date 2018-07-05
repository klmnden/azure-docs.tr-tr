---
title: 'Azure Analysis Services öğreticisi ek ders: Ayrıntı Satırları | Microsoft Docs'
description: Azure Analysis Services öğreticisinde bir Ayrıntı Satırları İfadesinin nasıl oluşturulacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 626258488afec4b3c3f025ae85bd3b5866aa0cf3
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443986"
---
# <a name="supplemental-lesson---detail-rows"></a>Ek ders - Ayrıntı Satırları

Bu ek derste DAX Düzenleyicisi'ni kullanarak özel bir Ayrıntı Satırları İfadesi tanımlayacaksınız. Ayrıntı Satırları İfadesi, son kullanıcılar için ölçünün toplu sonuçlarıyla ilgili daha fazla bilgi sağlayan bir ölçü özelliğidir. 
  
Bu dersin tahmini tamamlanma süresi: **10 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu ek ders bir tablo modelleme öğreticisinin parçasıdır. Bu ek dersteki görevleri gerçekleştirmeden önce tüm önceki dersleri tamamlamış veya bir Adventure Works İnternet Satışları örnek model projesini tamamlamış olmanız gerekir.  
  
## <a name="whats-the-issue"></a>Sorun nedir?
Ayrıntı satırları ifadesi eklemeden önce Internettotalsales ölçü ayrıntılarını da gözden geçirelim.

1.  SSDT’de **Model** menüsü > **Excel'de çözümleme**’ye tıklayarak Excel'i açın ve boş bir PivotTable oluşturun.
  
2.  **PivotTable Alanları**’nda FactInternetSales tablosundaki **InternetTotalSales** ölçüsünü **Değerler**, DimDate tablosundaki **CalendarYear** ölçüsünü **Sütunlar**, **EnglishCountryRegionName** ölçüsünü **Satırlar**’a ekleyin. PivotTable artık bölge ve yıla göre Internettotalsales ölçü gelen bir toplu sonuçları verir. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. PivotTable'da bir yıl ve bölge adı için toplu değere çift tıklayın. Avustralya ve 2014 yılının için değer. Verileri (yararlı olmayan verileri) içeren yeni bir sayfa açılır.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Buradaki amaç, Internettotalsales ölçü toplu sonucuna katkıda bulunan veri satırları ve sütunları içeren bir tablodur. Bunu yapmak için ölçünün bir özelliği ayrıntı satırları ifadesi ekleyin.

## <a name="add-a-detail-rows-expression"></a>Ayrıntı Satırları İfadesi ekleme

#### <a name="to-create-a-detail-rows-expression"></a>Ayrıntı Satırları İfadesi oluşturmak için 
  
1. FactInternetSales tablosunun ölçü kılavuzunda **InternetTotalSales** ölçüsüne tıklayın. 

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

  
## <a name="see-also"></a>Ayrıca bkz.  

[SELECTCOLUMNS İşlevi (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Ek Ders - dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Ek ders - Düzensiz hiyerarşiler](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
 