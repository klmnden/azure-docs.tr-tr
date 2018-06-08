---
title: Veri alımı ve takım veri bilimi işlemi yaşam döngüsü - Azure aşaması anlama | Microsoft Docs
description: Hedefler, görevler ve sonuçlara veri alım ve veri bilimi projelerinizi anlama aşaması
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: af295b5fb0afca03f33f65fd3b0a9fb5b8165bba
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837319"
---
# <a name="data-acquisition-and-understanding"></a>Veri edinme ve anlama

Bu makalede hedefleri, görevleri ve veri alım ve anlama aşama takım veri bilimi işlem (TDSP) ilişkili sonuçlara özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

   1. **İş anlama**
   2. **Veri alımı ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

Görsel bir TDSP yaşam döngüsü şöyledir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Hedef değişkenlere ilişkilerini anladım temiz, yüksek kaliteli veri kümesi üretir. Model için hazır olması için veri kümesi uygun analytics ortamında bulun.
* Çözüm mimarisi yeniler ve verileri düzenli olarak puanlar veri ardışık geliştirin.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele üç ana görevleri şunlardır:

   * **Veri alma** hedef analitik ortamına.
   * **Verileri araştırmak** veri kalitesini soruyu yanıtlamak yeterli olup olmadığını belirlemek için. 
   * **Veri ardışık ayarlamak** yeni ya da düzenli olarak Puanlama amacıyla verilerin yenilenmesi.

### <a name="ingest-the-data"></a>Veri alma
Verileri eğitim ve tahminleri gibi analizi işlemlerini çalıştırdığı hedef konumlara kaynak konumlardan taşıma işlemini ayarlayın. Teknik Ayrıntılar ve çeşitli Azure Veri Hizmetleri ile veri taşıma seçenekleri için bkz: [veri analizi için depolama ortamlara yükleme](ingest-data.md). 

### <a name="explore-the-data"></a>Verileri keşfetme
Modellerinizi eğitmek önce verileri ses bir anlayış geliştirmek gerekir. Gerçek veri kümeleri genellikle gürültülü, değerler eksik ya da diğer tutarsızlıklar barındırmasını. Verilerinizi kalitesini denetleme ve modelleme için hazır hale gelmeden önce verileri işlemek için gereken bilgileri sağlamak için veri özetleme ve görselleştirme kullanabilirsiniz. Bu işlem genellikle yinelemelidir.

TDSP adlı bir otomatik yardımcı program sunar [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), verileri görselleştirmek ve veri Özet Raporları Hazırlama yardımcı olacak. İlk hiçbir kodlama ile etkileşimli olarak ilk veri anlama geliştirmeye yardımcı olmak için verileri araştırmak için IDEAR ile başlamanızı öneririz. Ardından, veri keşfi ve görselleştirme için özel kod yazabilirsiniz. Veri temizleme ile ilgili yönergeler için bkz: [veri Gelişmiş için hazırlamak üzere görevleri makine öğrenme](prepare-data.md).  

Cleansed veri kalitesi yaptıktan sonra sonraki adıma daha iyi verileri devredilen desenleri anlamaktır. Bu, seçin ve hedef için uygun bir Tahmine dayalı modeli geliştirmek yardımcı olur. Bulgu verileri ne kadar iyi bağlı hedef için bakın. Ardından sonraki modelleme adımlarla ilerlemek için yeterli veri olup olmadığını belirler. Yeniden, bu işlem genellikle yinelemelidir. Başlangıçta önceki aşamasında tanımlanan veri kümesi artırmak için daha doğru veya daha çok ilgili veriler yeni veri kaynaklarıyla bulmak gerekebilir. 

### <a name="set-up-a-data-pipeline"></a>Veri ardışık ayarlayın
İlk alımı ve verilerini temizleme ek olarak, genellikle yeni verilerinizi puanlamada veya devam eden öğrenme işleminin bir parçası düzenli aralıklarla verileri yenilemek için bir işlem ayarlamanız gerekir. Veri ardışık veya iş akışı ayarlayarak bunu yapabilirsiniz. [Veri taşıma bir şirket içi SQL Server örneğinden Azure Data Factory ile Azure SQL veritabanına](move-sql-azure-adf.md) makalede sahip işlem hattı ayarlama konusunda bir örnek sunulmaktadır [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Bu aşamada, veri ardışık çözüm mimarisini geliştirin. Ardışık Düzen veri bilimi projesi sonraki aşaması ile paralel geliştirin. İş gereksinimlerinize ve bu çözümü tümleştirilmekte mevcut sistemlerinizi kısıtlamaları bağlı olarak, ardışık düzen aşağıdakilerden biri olabilir: 

   * Toplu işlem tabanlı
   * Akış veya gerçek zamanlı 
   * Bir karma 

## <a name="artifacts"></a>Yapıtlar
Sonuçlara bu aşamada şunlardır:

   * [Veri Kalitesi raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Bu rapor, veri özetleri, her özniteliği ve hedef arasındaki ilişkileri içerir ve değişken derecelendirme. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) TDSP parçası hızlı bir şekilde bir CSV dosyası veya ilişkisel bir tablo gibi tablolu bir veri kümesinde, bu raporu oluşturabilmesi olarak sağlanan aracı. 
   * **Çözüm mimarisi**: verilerinizi açıklaması potansiyel satış, Puanlama çalıştırmak için kullanın veya yeni verilerin tahminleri model oluşturduktan sonra veya çözüm mimarisi diyagramı olabilir. Ayrıca, yeni verilere dayalı modelinizi yeniden eğitme için ardışık düzeni içerir. Belgede saklamak [proje](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) TDSP dizin yapısı şablonunu kullandığınızda, dizin.
   * **Denetim noktası karar**: tam özellikli mühendislik ve model oluşturmanın başlamadan önce beklenen değeri Bunun yapılması devam etmek yeterli olup olmadığını belirlemek için projeyi yeniden değerlendirene. Örneğin, devam etmek için daha fazla veri toplamak veya soruyu yanıtlamak için veri yok gibi proje abandon gerek hazır olması olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her adımda TDSP yaşam döngüsü bağlantıları verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alımı ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımlar gösteren baştan sona tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantılar ve küçük resim açıklamaları senaryolarla listesini sağlar. İzlenecek yollar bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir. 

Azure Machine Learning Studio kullanan TDSPs adımları yürütmek nasıl örnekleri için bkz: [TDSP Azure Machine Learning ile kullanmak](http://aka.ms/datascienceprocess).
