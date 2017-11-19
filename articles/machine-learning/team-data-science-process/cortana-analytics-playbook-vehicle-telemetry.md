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
ms.openlocfilehash: bec5066a2e1ba0e4e5e81c4e1be28ed8eb93ceed
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Araç Telemetri analiz çözümü playbook
Bu playbook bölümlerde bu menü bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Genel Bakış
Süper bilgisayarlar Laboratuvarın dışına taşınmış ve şimdi garages içinde yerleşmiş durumdayken. Bu modern otomobiller izlemek ve saniyede milyonlarca olayı izleme olanağı vermek çok algılayıcılar içerir. 2020 tarafından çoğu bu araçlar, Internet'e bağlanır. Bu bol miktarda veri içine dokunma büyük güvenlik, güvenilirlik ve daha iyi yönlendirmeli deneyimi sağlar. Microsoft bu gerçekte Cortana Intelligence ile düş hale getirir.

Cortana Intelligence tam olarak yönetilen büyük veri ve akıllı eyleme, verilerinizi dönüştürmek için kullanabileceğiniz Gelişmiş analytics suite ' dir. Cortana Intelligence araç Telemetri analizi çözüm şablonu nasıl araba dealerships, otomobil üreticileri ve sigorta şirketler gerçek zamanlı kazanmadan ve Tahmine dayalı Öngörüler araç sistem durumu ve yürüten alışkanlıklarınıza gösterir. 

Çözüm olarak uygulanan bir [lambda mimarisi deseni](https://en.wikipedia.org/wiki/Lambda_architecture), gösterilir ve tam için Cortana Intelligence platformun olası gerçek zamanlı ve toplu işleme.

## <a name="architecture"></a>Mimari
Araç Telemetri analizi çözüm mimarisi Bu diyagramda gösterilmiştir:

![Çözüm mimarisi diyagramı](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Bu çözüm, aşağıdaki Cortana Intelligence bileşenleri içerir ve bunların uçtan uca tümleştirme genişletilebileceğini gösterir:

* **Azure Event Hubs** Azure'da araç telemetri olayları milyonlarca alır.
* **Azure Stream Analytics** araç sistem durumunu gerçek zamanlı Öngörüler sağlar ve bu verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder.
* **Azure Machine Learning** anormallikleri gerçek zamanlı olarak algılar ve toplu işleme Tahmine dayalı Öngörüler sağlamak için kullanır.
* **Azure Hdınsight** ölçeğinde veri dönüştürür.
* **Azure Data Factory** işler orchestration, planlama, kaynak yönetimi ve toplu işleme ardışık düzeni izleme.
* **Power BI** gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano bu çözümü sunar.

Bu çözüm iki farklı veri kaynaklarına erişiyor: 

* **Benzetimli araç sinyalleri ve tanılama**: araç telematik simulator tanılama bilgileri ve karşılık gelen sinyalleri araç ve belirli bir noktada yönlendirmeli düzeni durumuna zamanında yayar. 
* **Araç katalog**: Bu başvuru veri kümesi VINs modellerine eşler.

