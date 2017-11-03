---
title: "Araç durumu tahmin etmek ve alışkanlık - Azure yürüten | Microsoft Docs"
description: "Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: af8b3d5bf891c93c30a05c5f02d86639a466dde5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Araç telemetri analizi çözüm kitabı
Bu **menü** bu playbook bölümlerde bağlanır. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Genel Bakış
Süper bilgisayarlar Laboratuvarın dışına taşınmış ve şimdi bizim Garaj yerleşmiş durumdayken! Bu modern otomobiller algılayıcılar, izlemek ve saniyede milyonlarca olayı izleme olanağı veren çok sayıda içerir. 2020 tarafından bu araba çoğunu Internet'e bağlı, bekliyoruz. Bu bol miktarda büyük güvenlik, güvenilirlik ve daha iyi yönlendirmeli bir deneyim sağlamak için veri içine dokunma düşünün! Microsoft, bu gerçekte Cortana Intelligence ile düş yapmıştır.

Microsoft'un Cortana Intelligence tam olarak yönetilen büyük veri ve akıllı eyleme verilerinizi dönüştürmenizi sağlar Gelişmiş analytics suite ' dir. Cortana Intelligence araç Telemetri analizi çözüm şablonu tanıtmak istiyoruz. Bu çözümün nasıl araba dealerships, otomobil üreticileri ve sigorta şirketler Cortana Intelligence yeteneklerini gerçek zamanlı kazanmak için kullanabilir ve araç sistem durumu ve yürüten Tahmine dayalı Öngörüler alışkanlıklarınıza gösterir. 

Çözüm olarak uygulanan bir [lambda mimarisi deseni](https://en.wikipedia.org/wiki/Lambda_architecture) tam için Cortana Intelligence platformun olası gösteren gerçek zamanlı ve toplu işleme. Çözüm: 

* bir araç telematik simulator sağlar
* Event Hubs Azure'da sanal araç telemetri olayları milyonlarca alma yararlanır 
* araç sistem üzerinde gerçek zamanlı Öngörüler elde etmek için Stream Analytics kullanır
* verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder. 
* Machine Learning avantajlarından anomali algılama gerçek zamanlı yararlanır getirin ve Tahmine dayalı Öngörüler elde etmek için işleme toplu.
* ölçekli veri ve orchestration, planlama, kaynak yönetimi ve izleme toplu işleme ardışık işlemek için veri fabrikası dönüştürmek için Hdınsight yararlanır 
* Bu çözüm gerçek zamanlı veri ve Power BI kullanarak Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sağlar

## <a name="architecture"></a>Mimari
![Çözüm mimarisi diyagramı](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Şekil 1 – araç Telemetri analizi çözüm mimarisi*

Bu çözümü şunlardır **Cortana Intelligence bileşenleri** ve bunların uçtan uca tümleştirme genişletilebileceğini gösterir:

* **Olay hub'ları** Azure'da araç telemetri olayları milyonlarca alma için.
* **Akış analizi** araç sistem üzerinde gerçek zamanlı Öngörüler öğrenilmesi ve bu verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder.
* **Machine Learning** anomali algılama gerçek zamanlı ve toplu işleme Tahmine dayalı Öngörüler elde edin.
* **Hdınsight** ölçeğinde veri dönüştürme için de
* **Veri Fabrikası** planlama, kaynak yönetimi ve izleme toplu işleme ardışık orchestration işler.
* **Power BI** gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano bu çözümü sunar.

Bu çözüm iki farklı erişen **veri kaynakları**: 

* **Benzetimli araç sinyalleri ve tanılama**: araç telematik simulator tanılama bilgileri ve karşılık gelen sinyalleri araç ve belirli bir noktada yönlendirmeli düzeni durumuna zamanında yayar. 
* **Araç katalog**: Toplamıdır modeli eşleme içeren bir başvuru veri kümesi.

