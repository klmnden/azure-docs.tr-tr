---
title: "Azure Analysis Services öğreticisi - 6. Ders: Ölçü oluşturma | Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde ölçülerin nasıl oluşturulacağını açıklar."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: fa47d4ea9aa019464e465c051b016dac7c224dc9
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="create-measures"></a>Ölçü oluşturma

Bu derste, modelinize eklenecek ölçüleri oluşturursunuz. Oluşturduğunuz hesaplanan sütunlara benzer şekilde, ölçü de bir DAX formülü kullanılarak oluşturulmuş bir hesaplamadır. Bununla birlikte, hesaplanan sütunlardan farklı olarak ölçüler kullanıcı tarafından seçilen bir *filtre* temel alınarak değerlendirilir. Örneğin, bir PivotTable’da Satır Etiketleri alanına eklenen belirli bir sütun veya dilimleyici. Daha sonra, uygulanan ölçü tarafından filtredeki her hücre için bir değer hesaplanır. Ölçüler, sayısal veriler üzerinde dinamik hesaplamalar gerçekleştirmek için neredeyse tüm tablosal modellere eklemek isteyeceğiniz güçlü, esnek hesaplamalardır. Daha fazla bilgi edinmek için bkz. [Ölçüler](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).
  
Ölçü oluşturmak için *Ölçü Kılavuzu*’nu kullanırsınız. Varsayılan olarak, her tablonun boş bir ölçüm kılavuzu vardır; bununla birlikte, genellikle her tablo için ölçü oluşturmazsınız. Ölçü kılavuzu, Veri Görünümünde model tasarımcısındaki bir tablonun altında görünür. Bir tablonun ölçü kılavuzunu gizlemek veya göstermek için **Tablo** menüsüne ve ardından **Ölçü Kılavuzunu Göster**’e tıklayın.  
  
Ölçü kılavuzundaki boş bir hücreye tıklayıp formül çubuğuna bir DAX formülü yazarak ölçü oluşturabilirsiniz. Formülü tamamlamak için ENTER tuşuna bastığınızda, ölçü hücrede görünür. Bir sütuna tıklayıp araç çubuğundaki Otomatik Toplam düğmesine (**∑**) tıklayarak standart bir toplama işleviyle de ölçü oluşturabilirsiniz. Otomatik Toplam özelliği kullanılarak oluşturulan ölçüler, sütunun hemen altındaki ölçü kılavuzu hücresinde görünür, ancak taşınamaz.  
  
Bu derste, hem formül çubuğuna bir DAX formülü girerek hem de Otomatik Toplam özelliğini kullanarak ölçüler oluşturursunuz.  
  
Bu dersin tahmini tamamlanma süresi: **30 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [5. Ders: Hesaplanan sütunlar oluşturma](../tutorials/aas-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Ölçü oluşturma  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a>DimDate tablosunda DaysCurrentQuarterToDate ölçüsü oluşturmak için  
  
1.  Model tasarımcısında **DimDate** tablosuna tıklayın.  
  
2.  Ölçü kılavuzunda, sol üstteki boş hücreye tıklayın.  
  
3.  Formül çubuğuna aşağıdaki formülü yazın:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Sol üstteki hücrenin artık **DaysCurrentQuarterToDate** şeklinde bir adı olduğuna ve adın sonuna sonucun (**92**) eklendiğine dikkat edin. Kullanıcı filtresi uygulanmadığından sonuç bu noktada alakalı değildir.
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    Hesaplanan sütunlardan farklı olarak, ölçü formülleriyle ölçü adını yazabilir ve adın sonuna bir virgül koyup formül ifadesini yazabilirsiniz.

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a>DimDate tablosunda DaysInCurrentQuarter ölçüsü oluşturmak için  
  
1.  Model tasarımcısında **DimDate** tablosu hala etkinken, ölçü kılavuzunda oluşturduğunuz ölçünün altında bulunan boş hücreye tıklayın.  
  
2.  Formül çubuğuna aşağıdaki formülü yazın:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Tamamlanmamış bir dönemle önceki dönem arasında karşılaştırma oranı oluştururken. Formül tarafından geçen dönemin oranı hesaplanmalı ve bulunan değer bir önceki döneme ait aynı oranla karşılaştırılmalıdır. Bu durumda, [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] sonucu, geçerli dönemde geride bırakılan orandır.  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a>FactInternetSales tablosunda InternetDistinctCountSalesOrder ölçüsü oluşturmak için  
  
1.  **FactInternetSales** tablosuna tıklayın.   
  
2.  **SalesOrderNumber** sütun başlığına tıklayın.  
  
3.  Araç çubuğunda Otomatik Toplam (**∑**) düğmesinin yanındaki aşağı oka tıklayıp **DistinctCount**’u seçin.  
  
    Otomatik Toplam özelliği, DistinctCount standart toplama formülünü kullanarak seçili sütun için otomatik olarak bir ölçü oluşturur.  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  Ölçü kılavuzunda yeni ölçüye tıklayın ve **Özellikler** penceresindeki **Ölçü Adı**, bölümünde, ölçüyü **InternetDistinctCountSalesOrder** olarak yeniden adlandırın. 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a>FactInternetSales tablosunda ek ölçüler oluşturmak için  
  
1.  Otomatik Toplam özelliğini kullanarak aşağıdaki ölçüleri oluşturun ve adlandırın:  

    |Sütun|Ölçü adı|Otomatik Toplam (∑)|Formül|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Sayı|=COUNTA([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Toplam|=SUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Toplam|=SUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Toplam|=SUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Toplam|=SUM([SalesAmount])|  
    |Marj|InternetTotalMargin|Toplam|=SUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Toplam|=SUM([TaxAmt])|  
    |Nakliye|InternetTotalFreight|Toplam|=SUM([Freight])|  
  
2.  Ölçü kılavuzundaki boş bir hücreye tıklayıp formül çubuğunu kullanarak aşağıdaki özel ölçüleri sırasıyla oluşturun:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
FactInternetSales tablosu için oluşturulan ölçüler, kullanıcının seçtiği filtre tarafından tanımlanan öğeler için satış, maliyet ve kar marjı gibi kritik finansal verileri analiz etmek için kullanılabilir.  
  
## <a name="whats-next"></a>Sırada ne var?
[7. Ders: Önemli Performans Göstergeleri oluşturma](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  

  
