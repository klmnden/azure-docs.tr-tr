---
title: "Araç durumu tahmin etmek ve alışkanlık - Azure yürüten | Microsoft Docs"
description: "Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: bradsev
ms.openlocfilehash: 02b3e0e0808cb9a1a8a2186b1abe6da7dd13e56e
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Araç Telemetri analiz çözümü playbook
Bu playbook bölümlerde bu menü bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Genel Bakış
Laboratuvarın dışına taşınmış ve şimdi garages içinde yerleşmiş durumdayken Süper bilgisayarlar. Bunlar artık çok algılayıcılar içeren modern otomobiller içinde gönderilme. Bu algılayıcılar bunları izleyebilir ve saniyede milyonlarca olayı izleme olanağı verir. 2020 tarafından çoğu bu araçlar, Internet'e bağlanır. Büyük güvenlik, güvenilirlik, bu bol miktarda veri içine dokunma sağlar ve bu nedenle daha iyi yürüten deneyimi. Microsoft bu gerçekte Cortana Intelligence ile düş hale getirir.

Cortana Intelligence tam olarak yönetilen büyük veri ve akıllı eyleme, verilerinizi dönüştürmek için kullanabileceğiniz Gelişmiş analytics suite ' dir. Cortana Intelligence araç Telemetri analizi çözüm şablonu nasıl araba dealerships, otomobil üreticileri ve sigorta şirketler gerçek zamanlı elde etmiş ve Tahmine dayalı Öngörüler araç sistem durumu ve yürüten alışkanlıklarınıza gösterir.

Çözüm olarak uygulanan bir [lambda mimarisi deseni](https://en.wikipedia.org/wiki/Lambda_architecture), gösterilir ve tam için Cortana Intelligence platformun olası gerçek zamanlı ve toplu işleme.

## <a name="architecture"></a>Mimari
Araç Telemetri analizi çözüm mimarisi Bu diyagramda gösterilmiştir:

![Çözüm mimarisi diyagramı](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Bu çözüm, aşağıdaki Cortana Intelligence bileşenleri içerir ve kendi tümleştirme genişletilebileceğini gösterir:

* **Azure Event Hubs** Azure'da araç telemetri olayları milyonlarca alır.
* **Azure Stream Analytics** araç sistem durumunu gerçek zamanlı Öngörüler sağlar ve bu verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder.
* **Azure Machine Learning** anormallikleri gerçek zamanlı olarak algılar ve toplu işleme Tahmine dayalı Öngörüler sağlamak için kullanır.
* **Azure Hdınsight** ölçeğinde veri dönüştürür.
* **Azure Data Factory** işler orchestration, planlama, kaynak yönetimi ve toplu işleme ardışık düzeni izleme.
* **Power BI** gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano bu çözümü sunar.

Bu çözüm iki farklı veri kaynaklarına erişiyor: 

* **Benzetimli araç sinyalleri ve tanılama**: araç telematik simulator tanılama bilgileri ve karşılık gelen sinyalleri araç ve belirli bir noktada yönlendirmeli düzeni durumuna zamanında yayar. 
* **Araç katalog**: Bu başvuru veri kümesi Toplamıdır numaraları modellerine eşler.

