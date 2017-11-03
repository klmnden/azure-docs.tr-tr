---
title: "Veri alımı ve takım veri bilimi işlem yaşam döngüsü - Azure aşaması anlama | Microsoft Docs"
description: "Hedefler, görevler ve veri alma ve veri bilimi projelerinizi anlama aşaması için teslim edilebilir."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: eea3c357ebf6ad920c0ddebdb979aa07aece4794
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="data-acquisition-and-understanding"></a>Veri edinme ve anlama

Hedefler, görevler, bu konuda özetlenir ve sonuçlara ilişkili **veri alımı ve anlama aşama** takım veri bilimi işleminin. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Uygun analytics ortamında, model oluşturmak hazır bulunan hedef değişkenleri olan ilişkileri anlaşılır bir temiz, yüksek kaliteli veri kümesi.
* Çözüm mimarisi yenileyip verileri düzenli olarak puan veri ardışık geliştirilmiştir.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele üç ana görevleri şunlardır:

* **Veri alma** hedef analitik ortamına.
* **Verileri araştırmak** veri kalitesini soruyu yanıtlamak yeterli olup olmadığını belirlemek için. 
* **Veri ardışık ayarlamak** yeni ya da düzenli olarak Puanlama amacıyla verilerin yenilenmesi.

### <a name="21-ingest-the-data"></a>2.1 veri alma
Veri kaynağı konumlardan tahminleri yürütülecek olan ve burada analizi işlemlerini eğitim gibi hedef konumlara taşımak için işlemini ayarlayın. Teknik Ayrıntılar ve bunun çeşitli Azure Veri Hizmetleri ile nasıl seçenekleri için bkz: [veri analizi için depolama ortamlara yükleme](ingest-data.md). 

### <a name="22-explore-the-data"></a>2.2 verileri keşfedin
Modellerinizi eğitmek önce verileri ses bir anlayış geliştirmek gerekir. Gerçek veri kümeleri genellikle gürültülü veya değerleri eksik veya diğer tutarsızlıklar barındırmasını. Veri özetleme ve görselleştirme, verilerinizin kalitesini denetleme ve modelleme için hazır hale gelmeden önce verileri işlemek için gerekli bilgileri sağlamak için kullanılabilir. Bu işlem genellikle yinelemelidir.

TDSP sağlar adlı otomatik bir yardımcı program [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) görselleştirmek ve veri Özet Raporları Hazırlama yardımcı olacak. İlk hiçbir kodlama ile etkileşimli olarak ilk veri anlamak ve ardından veri keşfi ve görselleştirme için özel kod yazmanıza yardımcı olmak için verileri araştırmak için IDEAR ile başlamanızı öneririz. Veri temizleme ile ilgili yönergeler için bkz: [veri Gelişmiş için hazırlamak üzere görevleri makine öğrenme](prepare-data.md).  

Cleansed veri kalitesi memnun olduktan sonra sonraki adıma daha iyi yardımcı verileri devredilen desenleri seçin ve hedef için uygun bir Tahmine dayalı modeli geliştirmek anlamaktır. Bulgu verileri ne kadar iyi bağlı hedef için ve sonraki modelleme adımlara ilerlemek için yeterli veri olup arayın. Yeniden, bu işlem genellikle yinelemelidir. Başlangıçta önceki aşamasında tanımlanan veri kümesi artırmak için daha doğru veya daha çok ilgili veriler yeni veri kaynaklarıyla bulmak gerekebilir.  

### <a name="23-set-up-a-data-pipeline"></a>2.3 veri ardışık ayarlayın
İlk alımı ve verilerini temizleme ek olarak, genellikle yeni verilerinizi puanlamada veya devam eden öğrenme işleminin bir parçası düzenli aralıklarla verileri yenilemek için bir işlem ayarlamanız gerekir. Bu, bir veri ardışık veya iş akışı ayarlayarak yapılabilir. Burada bir [örnek](move-sql-azure-adf.md) sahip işlem hattı ayarlama konusunda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Bu aşamada veri ardışık çözüm mimarisini geliştirilir. Ardışık Düzen veri bilimi projesi aşağıdaki aşamaları ile paralel de geliştirilir. Ardışık Düzen toplu tabanlı veya akış/real-zamanlı veya iş gereksinimlerinize ve bu çözümü tümleştirilmekte mevcut sistemlerinizi kısıtlamaları bağlı olarak karma olabilir. 

## <a name="artifacts"></a>Yapıtlar
Sonuçlara bu aşamada verilmiştir.

* [**Veri Kalitesi raporu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Bu rapor, veri özetleri, her özniteliği ve hedef arasındaki ilişkileri içerir değişken derecelendirme vs. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) TDSP parçası hızlı bir şekilde bir CSV dosyası veya ilişkisel bir tablo gibi herhangi bir tablo veri kümesi üzerinde bu raporu oluşturabilmesi olarak sağlanan aracı. 
* **Çözüm mimarisi**: bir model oluşturduktan sonra bu diyagramı veya Puanlama çalıştırmak için kullanılan veri ardışık veya tahminleri yeni verilerin açıklaması olabilir. Ayrıca, yeni verilere dayalı modelinizi yeniden eğitme için ardışık düzeni içerir. Belge depolanan [proje](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) TDSP dizin yapısı şablon kullanırken dizin.
* **Denetim noktası karar**: tam özellik mühendislik ve model oluşturmanın başlamadan önce beklenen değeri Bunun yapılması devam etmek yeterli olup olmadığını belirlemek için projeyi yeniden değerlendirene. Örneğin, devam etmek için daha fazla veri toplamak veya soruyu yanıtlamak için veri yok gibi proje abandon gerek hazır olması olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her takım veri bilimi işlemi yaşam döngüsü adımda bağlantıları verilmiştir:

* [1. İş anlama](lifecycle-business-understanding.md)
* [2. Veri alımı ve anlama](lifecycle-data.md)
* [3. Modelleme](lifecycle-modeling.md)
* [4. Dağıtım](lifecycle-deployment.md)
* [5. Müşteri kabulü](lifecycle-acceptance.md)

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir.  

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.