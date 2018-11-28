---
title: Veri edinme ve anlama aşamasına Team Data Science Process yaşam döngüsü - Azure | Microsoft Docs
description: Hedefleri, görevleri ve teslim edilebilirleri veri alımı ve veri bilimi projelerinizi anlama aşaması
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
ms.openlocfilehash: 3d80759b401dc8c3514736a3de20af5d7b2fe63c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242322"
---
# <a name="data-acquisition-and-understanding"></a>Veri edinme ve anlama

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirleri veri edinme ve anlama aşama Team Data Science işlem (TDSP) ile ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Hedef değişkenlere ilişkilerini anladım temiz, yüksek kaliteli veri kümesi üretir. Modeli hazır olacak şekilde veri kümesi uygun analytics ortamında bulun.
* Bir çözüm mimarisini yenilenir ve verileri düzenli olarak puanlar veri işlem hattı geliştirin.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada yönelik üç ana görev vardır:

   * **Veri alma** hedef analitik ortamına.
   * **Verileri keşfedin** veri kalitesi soruyu yanıtlamak yeterli olup olmadığını belirlemek için. 
   * **Bir veri işlem hattı ayarlayın** yeni veya düzenli olarak puanlamak için veriler yenilenir.

### <a name="ingest-the-data"></a>Veri alma
Verileri eğitim ve tahmin gibi analiz işlemlerini çalıştırdığınız hedef konumlara kaynak konumları taşıma işlemini ayarlayın. Teknik Ayrıntılar ve çeşitli Azure Veri Hizmetleri ile veri taşıma konusunda seçenekleri için bkz. [analiz için depolama ortamlarına veri yükleme](ingest-data.md). 

### <a name="explore-the-data"></a>Verileri keşfetme
Modellerinizi eğitmek önce verileri ses bir anlayış geliştirmek gerekir. Gerçek veri kümeleri genellikle gürültülü, değerleri eksik veya diğer tutarsızlıklar barındırmasını. Veri özetleme ve görselleştirme, veri kalitesini denetlemek ve modelleme için hazır hale gelmeden önce veriyi işlemek gereken bilgileri sağlamak için kullanabilirsiniz. Bu işlem genellikle yinelemelidir.

TDSP adlı bir otomatik yardımcı programını sağlar [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), verileri Görselleştirme ve veri Özet Raporları Hazırlama yardımcı olmak için. İlk veri kodlama ile etkileşimli olarak ilk veri anlama geliştirmenize yardımcı olmak için keşfetmek için IDEAR ile başlamanızı öneririz. Ardından veri keşfi ve görselleştirme için özel kod yazabilirsiniz. Verilerin temizlenmesi ile ilgili yönergeler için bkz. [için Gelişmiş veri hazırlama görevleri makine öğrenimi](prepare-data.md).  

Temizlenen veri kalitesi yaptıktan sonra sonraki verileri belirlidir desenleri daha iyi anlamak için adımdır. Bu, seçin ve hedef için uygun bir Tahmine dayalı model geliştirmenize yardımcı olur. Bulgu verileri ne kadar iyi bağlı hedef için bakın. Ardından sonraki modelleme adımlarla ileri taşımak için yeterli veri olup olmadığını belirler. Yeniden, bu işlem genellikle yinelemelidir. Başlangıçta önceki aşamada tanımlanan veri kümesi genişletmek için daha doğru ya da daha fazla ilgili verileri içeren yeni veri kaynaklarını bulmak gerekebilir. 

### <a name="set-up-a-data-pipeline"></a>Bir veri işlem hattı ayarlayın
İlk alımı ve veri temizleme ek olarak, genellikle yeni verilerinizi puanlamada veya verileri düzenli aralıklarla sürekli öğrenme sürecinin parçası olarak yenilemek için bir işlem ayarlamanız gerekir. Bir veri işlem hattı veya iş akışı ayarlayarak bunu yapabilirsiniz. [Veri taşıma bir şirket içi SQL Server örneğinden Azure Data Factory ile Azure SQL veritabanı](move-sql-azure-adf.md) makalede sahip bir işlem hattı kurmak nasıl bir örnek verilmektedir [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Bu aşamada, veri işlem hattı bir çözüm mimarisini geliştirin. İşlem hattı ile veri bilimi proje sonraki aşaması paralel geliştirme. İş gereksinimlerinize ve kısıtlamaları, bu çözüm tümleştiriliyor mevcut sistemlerinize bağlı olarak, işlem hattı aşağıdakilerden biri olabilir: 

   * Toplu işleme dayalı
   * Akış ve gerçek zamanlı 
   * Karma 

## <a name="artifacts"></a>Yapıtlar
Aşağıda, bu aşamada teslim edilebilirleri verilmiştir:

   * [Veri Kalitesi raporunu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Bu rapor, veri özetleri, her bir öznitelik ve hedef arasındaki ilişkileri içerir değişken derecelendirme ve daha fazlası. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) aracı gibi TDSP bir parçası olarak bir CSV dosyası veya bir ilişkisel tabloya gibi tüm tablosal veri kümesi Bu raporda hemen oluşturabilirsiniz. 
   * **Çözüm mimarisi**: verilerinizi açıklamasını işlem hattı, Puanlama çalıştırmak için kullanın veya yeni veriler hakkında Öngörüler bir modeli oluşturduktan sonra veya çözüm mimarisini diyagram olabilir. Ayrıca, yeni verileri temel alan, modeli yeniden eğitme için işlem hattı içerir. Belgede Store [proje](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) TDSP dizin yapısı şablonunu kullandığınızda dizin.
   * **Denetim noktası karar**: tam özellik Mühendisliği ve model oluşturmaya başlamadan önce beklenen değeri Bunun yapılması devam etmek yeterli olup olmadığını belirlemek için projeyi yeniden değerlendirene. Örneğin, devam etmek için daha fazla veri toplayabilir veya verileri soruyu yanıtlamak için mevcut olmadığından, projeyi iptal gerek hazır hale getirebilir.

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İşin gereksinimlerini anlama](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantıları ve küçük resim açıklamaları senaryolarıyla bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Adımlar Azure Machine Learning Studio'nun TDSPs yürütmek nasıl bir örnekleri için bkz: [Azure Machine Learning ile TDSP kullanma](https://aka.ms/datascienceprocess).
