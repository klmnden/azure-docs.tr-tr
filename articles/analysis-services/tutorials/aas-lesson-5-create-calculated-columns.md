---
title: "Azure Analysis Services öğreticisi 5. Ders: Hesaplanan sütunlar oluşturma | Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde hesaplanan sütunların nasıl oluşturulacağını açıklar."
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
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 893371145d77e156843271907aeef0c3756d0403
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---
<a id="lesson-5-create-calculated-columns" class="xliff"></a>

# 5. Ders: Hesaplanan sütunlar oluşturma

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste, hesaplanan sütunlar ekleyerek modelinizde veri oluşturursunuz. Verileri Al’ı kullanırken Sorgu Düzenleyicisi’ni kullanarak veya daha sonra, burada yaptığınız gibi model tasarımcısına giderek hesaplanan sütunlar (özel sütunlar olarak) oluşturabilirsiniz. Daha fazla bilgi edinmek için bkz. [Hesaplanan sütunlar](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Üç farklı tabloda beş yeni hesaplanan sütun oluşturabilirsiniz. Sütunları oluşturmak, yeniden adlandırmak ve tabloda farklı yerlere eklemek için farklı yollar olduğunu göstermek amacıyla, her görev için gerçekleştirilmesi gereken adımlar biraz farklıdır.  

Veri Çözümleme İfadeleri’ni (DAX) de ilk olarak bu derste kullanırsınız. DAX, tablosal modeller için yüksek oranda özelleştirilebilen formül ifadeleri oluşturmaya yönelik özel bir dildir. Bu öğreticide, DAX dilini kullanarak hesaplanan sütunlar, ölçüler ve rol filtreleri oluşturursunuz. Daha fazla bilgi edinmek için bkz. [Tablosal modellerde DAX](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
Bu dersin tahmini tamamlanma süresi: **15 dakika**  
  
<a id="prerequisites" class="xliff"></a>

## Ön koşullar  
Bu konu, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin bir parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [4. Ders: İlişki oluşturma](../tutorials/aas-lesson-4-create-relationships.md). 
  
<a id="create-calculated-columns" class="xliff"></a>

## Hesaplanan sütunlar oluşturma  
  
<a id="create-a-monthcalendar-calculated-column-in-the-dimdate-table" class="xliff"></a>

#### DimDate tablosunda bir MonthCalendar hesaplanan sütunu oluşturma  
  
1.  **Model** menüsü > **Model Görünümü** > **Veri Görünümü**’ne tıklayın.  
  
    Hesaplanan sütunlar yalnızca Veri Görünümü'ndeki model tasarımcısı kullanılarak oluşturulabilir.  
  
2.  Model tasarımcısında **DimDate** tablosuna (sekme) tıklayın.  
  
3.  **CalendarQuarter** sütun başlığına sağ tıklayıp **Sütun Ekle**’ye tıklayın.  
  
    **Takvim Çeyreği** sütununun sol tarafına **Hesaplanan Sütun 1** adlı yeni bir sütun eklenir.  
  
4.  Tablonun üstündeki formül çubuğuna aşağıdaki DAX formülünü yazın: Otomatik Tamamlama, sütun ve tabloların tam adlarını yazmanıza yardımcı olur ve kullanılabilen işlevleri listeler.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Daha sonra, hesaplanan sütundaki tüm satırlar için değerler doldurulur. Tabloyu aşağı kaydırırsanız, her bir satırdaki verilere bağlı olarak satırlarda bu sütun için farklı değerler girilebildiğini görürsünüz.    
  
5.  Bu sütunu **MonthCalendar** olarak yeniden adlandırın. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
MonthCalendar hesaplanan sütunu, Ay için sıralanabilen bir ad sağlar.  
  
<a id="create-a-dayofweek-calculated-column-in-the-dimdate-table" class="xliff"></a>

#### DimDate tablosunda DayOfWeek hesaplanan sütunu oluşturma  
  
1.  **DimDate** tablosu hala etkinken **Sütun** menüsüne ve sonra **Sütun Ekle**’ye tıklayın.  
  
2.  Formül çubuğuna aşağıdaki formülü yazın:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Formülü oluşturmayı tamamladığınızda, ENTER tuşuna basın. Yeni sütun tablonun en sağına eklenir.  
  
3.  Sütunu **DayOfWeek** olarak yeniden adlandırın.  
  
4.  Sütun başlığına tıklayın, sütunu **EnglishDayNameOfWeek** sütunu ile **DayNumberOfMonth** sütunu arasına sürükleyin.  
  
    > [!TIP]  
    > Tablonuzdaki sütunların taşınması, gezintiyi kolaylaştırır.  
  
DayOfWeek hesaplanan sütunu, haftanın günü için sıralanabilen bir ad sağlar.  
  
<a id="create-a-productsubcategoryname-calculated-column-in-the-dimproduct-table" class="xliff"></a>

#### DimProduct tablosunda ProductSubcategoryName hesaplanan sütunu oluşturma  
  
  
1.  **DimProduct** tablosunda, tablonun en sağına gidin. En sağdaki sütunun **Sütun Ekle** (italik) olarak adlandırıldığına dikkat edin ve sütun başlığına tıklayın.  
  
2.  Formül çubuğuna aşağıdaki formülü yazın:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Sütunu **ProductSubcategoryName** olarak yeniden adlandırın.  
  
ProductSubcategoryName hesaplanan sütunu, DimProductSubcategory tablosundaki EnglishProductSubcategoryName sütunundan veriler içeren DimProduct tablosunda bir hiyerarşi oluşturmak için kullanılır. Hiyerarşiler birden fazla tabloya yayılamaz. Hiyerarşileri 9. Derste oluşturacaksınız.  
  
<a id="create-a-productcategoryname-calculated-column-in-the-dimproduct-table" class="xliff"></a>

#### DimProduct tablosunda ProductCategoryName hesaplanan sütunu oluşturma  
  
1.  **DimProduct** tablosu hala etkinken **Sütun** menüsüne ve sonra **Sütun Ekle**’ye tıklayın.  
  
2.  Formül çubuğuna aşağıdaki formülü yazın:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Sütunu **ProductCategoryName** olarak yeniden adlandırın.  
  
ProductCategoryName hesaplanan sütunu, DimProductCategory tablosundaki EnglishProductCategoryName sütunundan veriler içeren DimProduct tablosunda bir hiyerarşi oluşturmak için kullanılır. Hiyerarşiler birden fazla tabloya yayılamaz.  
  
<a id="create-a-margin-calculated-column-in-the-factinternetsales-table" class="xliff"></a>

#### FactInternetSales tablosunda Margin hesaplanan sütunu oluşturma  
  
1.  Model tasarımcısında **FactInternetSales** tablosunu seçin.  
  
2.  **SalesAmount** sütunu ile **TaxAmt** sütunu arasında yeni bir hesaplanan sütun oluşturun.  
  
3.  Formül çubuğuna aşağıdaki formülü yazın:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Sütunu **Margin** olarak yeniden adlandırın.  
 
      ![aas lesson5 newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    Margin hesaplanan sütunu, her satış için kar marjını analiz etmek için kullanılır.  
  
<a id="whats-next" class="xliff"></a>

## Sırada ne var?
[6. Ders: Ölçü oluşturma](../tutorials/aas-lesson-6-create-measures.md).
  
  
  

