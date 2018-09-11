---
title: Veri bilimi projeleri - Azure Machine Learning yürütülmesini | Microsoft Docs
description: Nasıl bir veri Bilimcisi bir veri bilimi projenizin ilerlemesini izleyebilirsiniz.
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
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: 32390b05d2ec258a68ed4f53135399675105a7e9
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302094"
---
# <a name="track-progress-of-data-science-projects"></a>Veri bilimi projeleri ilerlemesini

Veri bilimi grup yöneticileri, takım Liderleri ve proje liderleri gerek projelerini ilerlemesini izlemek için hangi iş bunları ve kim tarafından yapılan ve yapılacaklar listelerinde kalır. 

## <a name="azure-devops-dashboards"></a>Azure DevOps panolar
Azure DevOps kullanıyorsanız, etkinlikler ve belirli bir Çevik proje ile ilişkili iş öğelerini izlemek için panolar oluşturma olanağına sahip olursunuz. 

Oluşturma ve pencere öğeleri ve panolarda Azure DevOps üzerine özelleştirme hakkında daha fazla bilgi için aşağıdaki yönergeler kümesini bakın:

- [Ekleme ve panoları Yönet](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [Panoya pencere öğeleri ekleme](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Örnek Pano

İlişkili depolara işleme sayısını yanı sıra bir Çevik veri bilimi proje sprint etkinliklerini izlemek için yerleşik bir Basit örnek Pano aşağıda verilmiştir. **Üst sol** panelinde gösterir:

- geri sayım geçerli sprint 
- Son 7 gün içindeki her bir depo işleme sayısını
- belirli kullanıcılar için iş öğesi. 

Paneller kalan birikmeli akış diyagramı (CFD), burndown ve bir projenin tamamlanma durumunu göster:

- **Alt sol**: CFD gri mavi olarak kabul edilen ve yeşil renkte Bitti, onaylanan gösteren belirli bir durumdaki iş miktarı.
- **Sağ üst**: burndown grafiği ve kalan süre tamamlamak için kalan iş).
- **Sağ alt**: toplam miktarı iş tamamlanan iş tamamlanma durumunu grafik.

![pano](./media/track-progress/dashboard.png)

Hızlı başlangıçlar ve öğreticiler, bu grafikler oluşturmak nasıl bir açıklaması için bkz: [panolar](https://docs.microsoft.com/azure/devops/report/dashboards/).
 
## <a name="next-steps"></a>Sonraki adımlar

İşlem için tüm adımları gösteren talimatlara **belirli senaryoları** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 
