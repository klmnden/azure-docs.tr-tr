---
title: Tahmin araç durumu ve sürüş alışkanlıkları - Azure | Microsoft Docs
description: Araç durumu ve sürüş üzerinde gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence'nın özelliklerini kullanmaya alışkanlıkları.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.openlocfilehash: 0594b9b40fdf8647f99946a31dbb47a95c9694ac
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300362"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Araç Telemetri analizi çözüm kitabı

Süper bilgisayarlar laboratuvarlardan ve şimdi garages unutulmuş. Bunlar artık çok sensörlerden içeren son teknoloji ürünü otomobiller içinde gönderilme. Bu sensörlerden bunları izleyebilir ve her saniyede milyonlarca olayı izleme olanağı sağlayacak. 2020 yılına gelindiğinde, bu araçlar çoğu Internet'e bağlanır. Daha fazla güvenlik, güvenilirlik, bu verileri zengin olarak sağlar ve böylece daha iyi bir sürüş deneyimi. Microsoft Cortana Intelligence ile gerçeğe dream bunu yapar.

Cortana Intelligence tam olarak yönetilen büyük veri ve verilerinizi akıllı eylemlere dönüştürmenizi kullanabileceğiniz Gelişmiş analiz özellikleri sunan. Cortana Intelligence araç Telemetri analizi çözüm şablonuyla araba bayileri, otomobil üreticileri ve sigorta şirketlerinin elde gerçek zamanlı ve Tahmine dayalı Öngörüler araç durumu ve sürüş alışkanlıkları nasıl gösterir.

Çözüm olarak uygulanan bir [lambda mimari yapısı](https://en.wikipedia.org/wiki/Lambda_architecture), gösterilir ve tam için Cortana Intelligence platformunun olası gerçek zamanlı ve toplu işlem.

## <a name="architecture"></a>Mimari
Araç Telemetri analizi çözüm mimarisi Bu diyagramda gösterilmiştir:

![Çözüm mimarisi diyagramı](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Bu çözüm, Cortana Intelligence aşağıdaki bileşenleri içerir ve kendi tümleştirme gösterir:

* **Azure Event Hubs** Azure'a milyonlarca araç telemetri alır.
* **Azure Stream Analytics** araç durumu üzerinde gerçek zamanlı Öngörüler sağlar ve bu verileri daha ayrıntılı toplu analizler için uzun vadeli depolamaya devam ettirir.
* **Azure Machine Learning** anormallikleri gerçek zamanlı olarak algılar ve toplu işlem, Tahmine dayalı Öngörüler sağlamak için kullanır.
* **Azure HDInsight** uygun ölçekte verileri dönüştürür.
* **Azure Data Factory** düzenleme, planlama, kaynak yönetimini ve toplu işlem hattının işler.
* **Power BI** Bu çözüm, gerçek zamanlı verileri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sağlar.

Bu çözüm, iki farklı veri kaynaklarına erişiyor: 

* **Benzetimli vehicle sinyaller ve tanılama**: bir araç telematik simülatörü zamanında tanılama bilgilerini ve karşılık gelen sinyalleri için araç ve belirli bir noktada sürüş deseni durumunu yayar. 
* **Araç Kataloğu**: Bu başvuru veri kümesi Toplamıdır numaraları modellere eşler.

## <a name="next-steps"></a>Sonraki Adımlar

Bu çözümün daha ayrıntılı incelemek için bkz: [araç Telemetri analizi çözüm kitabı: çözümün içine yakından](cortana-analytics-playbook-vehicle-telemetry-deep-dive.md).

Power BI raporları ve panolar Bu çözüm için yapılandırma konusunda bilgi için bkz: [araç Telemetri analizi çözüm şablonu Power BI Panosu kurulum yönergeleri](cortana-analytics-playbook-vehicle-telemetry-powerbi.md).
