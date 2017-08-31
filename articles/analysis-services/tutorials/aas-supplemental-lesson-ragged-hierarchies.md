---
title: "Azure Analysis Services öğreticisi ek ders: Düzensiz hiyerarşiler | Microsoft Docs"
description: "Azure Analysis Services öğreticisinde düzensiz hiyerarşilerin nasıl düzeltileceğini açıklar."
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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 0f02ff73eb126cc397312e87bde50b3ee2d6ce53
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Ek ders - Düzensiz hiyerarşiler

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu ek derste, farklı düzeylerde boş değerler (üyeler) içeren hiyerarşileri özetlerken karşılaşılan ortak bir sorunu çözümlersiniz. Örneğin, bir üst düzey yöneticiye bağlı departman yöneticilerinin ve yönetici olmayan çalışanların olduğu bir kuruluşta. Veya, Ülke-Bölge-Şehirden oluşan ve Washington D.C., Vatikan gibi bazı şehirlerde üst Eyalet veya Bölgenin olmadığı coğrafi hiyerarşilerde. Bir hiyerarşinin boş üyeleri olduğunda, hiyerarşi genellikle farklı veya düzensiz düzeylere iner.

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

1400 uyumluluk düzeyindeki tablo modelleri, hiyerarşiler için ek bir **Hide Members** özelliğine sahiptir. **Varsayılan** ayarı, hiçbir düzeyde boş üye olmadığını varsayar. **Boş üyeleri gizle** ayarı, bir PivotTable veya rapora eklendiğinde boş üyeleri hiyerarşinin dışında tutar.  
  
Bu dersi tamamlamak için tahmini süre: **20 dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu ek ders konusu bir tablo modelleme öğreticisinin parçasıdır. Bu ek dersteki görevleri gerçekleştirmeden önce tüm önceki dersleri tamamlamış veya bir Adventure Works İnternet Satışları örnek model projesini tamamlamış olmanız gerekir. 

Öğreticinin bir parçası olarak AW İnternet Satışları projesini oluşturduysanız, modeliniz henüz düzensiz veriler veya hiyerarşiler içermemektedir. Bu ek dersi tamamlamak için bazı ek tablolar ilave ederek sorunu oluşturmanız, ilişki, hesaplanmış sütunlar, bir ölçü ve yeni bir Kuruluş hiyerarşisi oluşturmanız gerekir. Bu kısım yaklaşık 15 dakika sürer. Ardından, sorunu yalnızca birkaç dakika içinde çözersiniz.  

## <a name="add-tables-and-objects"></a>Tablo ve nesne ekleme
  
### <a name="to-add-new-tables-to-your-model"></a>Modelinize yeni tablolar eklemek için
  
1.  Tablo Modeli Gezgini’nde **Veri Kaynakları**’nı genişletin, ardından bağlantınıza sağ tıklayın > **Yeni Tabloları İçeri Aktar**’a tıklayın.
  
2.  Gezgin’de **DimEmployee** ve **FactResellerSales**’i seçip **Tamam**’a tıklayın.

3.  Sorgu Düzenleyicisi'nde **İçeri Aktar**’a tıklayın

4.  Aşağıdaki [ilişkileri](../tutorials/aas-lesson-4-create-relationships.md) oluşturun:

    | Tablo 1           | Sütun       | Filtre Yönü   | Tablo 2     | Sütun      | Etkin |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Varsayılan            | DimDate     | Tarih        | Evet    |
    | FactResellerSales | DueDate      | Varsayılan            | DimDate     | Tarih        | Hayır     |
    | FactResellerSales | ShipDateKey  | Varsayılan            | DimDate     | Tarih        | Hayır     |
    | FactResellerSales | ProductKey   | Varsayılan            | DimProduct  | ProductKey  | Evet    |
    | FactResellerSales | EmployeeKey  | Her İki Tabloya | DimEmployee | EmployeeKey | Evet    |

5. **DimEmployee** tablosunda aşağıdaki [hesaplanmış sütunları](../tutorials/aas-lesson-5-create-calculated-columns.md) oluşturun: 

    **Path** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **FullName** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Level1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  **DimEmployee** tablosunda **Kuruluş** adlı bir [hiyerarşi](../tutorials/aas-lesson-9-create-hierarchies.md) oluşturun. Sırayla şu sütunları ekleyin: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.

7.  **FactResellerSales** tablosunda aşağıdaki [ölçüyü](../tutorials/aas-lesson-6-create-measures.md) oluşturun:

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  [Excel'de Çözümleme](../tutorials/aas-lesson-12-analyze-in-excel.md) özelliğini kullanarak Excel'i açın ve otomatik olarak bir PivotTable oluşturun.

9.  **PivotTable Alanları** alanında, **DimEmployee** tablosundaki **Kuruluş** değerini **Satırlar** ve **FactResellerSales** tablosundaki **ResellerTotalSales** ölçüsünü **Değerler** alanına ekleyin.

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    PivotTable'da görebileceğiniz gibi hiyerarşi, düzensiz olan satırları gösterir. Boş üyelerin gösterildiği çok sayıda satır vardır.

## <a name="to-fix-the-ragged-hierarchy-by-setting-the-hide-members-property"></a>Hide members özelliğini ayarlayarak düzensiz hiyerarşiyi düzeltmek için

1.  **Tablo Modeli Gezgini**’nde **Tablolar** > **DimEmployee** > **Hiyerarşiler** > **Kuruluş**’u genişletin.

2.  **Özellikler** > **Üyeleri Gizle** menüsünden **Boş üyeleri gizle**’yi seçin. 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Excel'e geri dönerek PivotTable'ı yenileyin. 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Şimdi çok daha iyi görünüyor!

## <a name="see-also"></a>Ayrıca Bkz.   
[9. Ders: Hiyerarşi oluşturma](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Ek Ders - Dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Ek Ders - Ayrıntı satırları](../tutorials/aas-supplemental-lesson-detail-rows.md)  
