---
title: Azure Data Factory OMS ile izleme | Microsoft Docs
description: Azure Data Factory çözümleme için yönlendirme verileri Operations Management Suite (OMS) tarafından izlemek öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: douglasl
ms.openlocfilehash: 4275a4ddcee51d58949b5bd83e4a898cb3dbb389
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304426"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Operations Management Suite (OMS) ile İzleyici Azure veri fabrikası

Operations Management Suite (OMS) için rota verileri için Azure İzleyicisi ile Azure Data Factory tümleştirme kullanabilirsiniz. Bu tümleştirme, aşağıdaki senaryolarda kullanışlıdır:

1.  Veri Fabrikası için OMS tarafından yayımlanan ölçümleri zengin bir dizi karmaşık sorgular yazmak istiyorum. Ayrıca, özel uyarılar OMS aracılığıyla bu sorgular oluşturabilirsiniz.

2.  Veri fabrikaları arasında izlemek istersiniz. Tek bir OMS çalışma birden çok veri fabrikaları veri yönlendirebilirsiniz.

## <a name="get-started"></a>Başlarken

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Tanılama ayarları ve OMS çalışma alanını yapılandırma

Veri fabrikanızın tanılama ayarlarını etkinleştirin.

1.  Seçin **Azure İzleyici** -> **tanılama ayarları** data factory -> Aç tanılamayı Seç ->.

    ![İzleyici oms image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  OMS çalışma yapılandırması dahil olmak üzere tanılama ayarlarını sağlayın.

    ![İzleyici oms image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Azure Marketi'nden Azure veri fabrikası Analytics OMS paketi yükle

![İzleyici oms image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![İzleyici oms image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Tıklatın **oluşturma** ve OMS çalışma alanı ayarları ve OMS çalışma alanını seçin.

![İzleyici oms image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Azure veri fabrikası OMS kullanarak ölçümleri izleyin

Yükleme **Azure Data Factory Analytics** OMS paketi, aşağıdaki ölçümleri sağlayan varsayılan bir görünüm oluşturur:

- Veri fabrikası tarafından ADF çalışır-1) ardışık düzen çalıştırır

- Veri fabrikası tarafından ADF çalışır-2) etkinlik çalışması

- Veri fabrikası tarafından ADF çalışır-3) tetikleyici çalıştırır

- Veri Fabrikası ADF hataları-1) en iyi 10 ardışık düzen hataları

- Veri fabrikası tarafından ADF hataları-2) ilk 10 etkinlik çalışması

- Veri Fabrikası ADF hataları-3) ilk 10 tetikleyici hataları

- Türe göre etkinlik çalışması ADF İstatistikleri-1)

- Türe göre ADF istatistikleri-2) tetikleyici çalıştırır

- ADF istatistikleri-3) en fazla ardışık düzen süre çalışır

![İzleyici oms image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![İzleyici oms image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Yukarıdaki ölçümleri görselleştirme, bu ölçümleri arkasında sorguları bakmak, sorguları düzenleme, uyarılar oluşturabilir ve benzeri.

![İzleyici oms image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [İzleyici ve ardışık düzen programlı olarak yönetmek](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) izleme ve komut dosyaları çalıştırarak ardışık düzen yönetme hakkında bilgi edinmek için.
