---
title: Azure Data Factory OMS ile izleme | Microsoft Docs
description: Azure Data Factory tarafından analiz için verilerin Operations Management Suite (OMS) için Yönlendirme izlemeyi öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: douglasl
ms.openlocfilehash: fb5da75d0e42dd0700cad008d348ff846d602409
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42062131"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Operations Management Suite (OMS) ile Azure Data factory'yi izleme

Operations Management Suite (OMS) için rota verileri için Azure İzleyici ile Azure Data Factory tümleştirme kullanabilirsiniz. Bu tümleştirme, aşağıdaki senaryolarda kullanışlıdır:

1.  Oms'ye veri fabrikası tarafından yayımlanan ölçümleri zengin bir dizi karmaşık sorgular yazmak istediğiniz. Ayrıca, OMS aracılığıyla bu sorguları özel uyarılar oluşturabilirsiniz.

2.  Veri fabrikaları arasında izlemek istediğiniz. Birden çok veri fabrikaları veri tek bir OMS çalışma alanınıza yönlendirebilir.

Yedi dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

## <a name="get-started"></a>Başlarken

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Tanılama ayarları ve OMS çalışma alanı yapılandırma

Veri fabrikanızın tanılama ayarlarını etkinleştirme.

1.  Seçin **Azure İzleyici** -> **tanılama ayarları** -> data factory tanılamayı Aç -> seçin.

    ![İzleyici oms image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  OMS çalışma alanının yapılandırılması dahil olmak üzere tanılama ayarları sağlar.

    ![İzleyici oms image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Azure Market'te Azure Data Factory Analytics OMS paketi yükleyin

![İzleyici oms image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![İzleyici oms image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Tıklayın **Oluştur** ve OMS çalışma alanı ayarlarını ve OMS çalışma alanı seçin.

![İzleyici oms image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>OMS kullanarak Azure Data Factory ölçümleri izleme

Yükleme **Azure Data Factory Analytics** OMS paketi aşağıdaki ölçümler sağlayan görünümü varsayılan kümesi oluşturur:

- Veri fabrikası tarafından ADF çalıştırır-1) işlem hattı çalıştırmaları

- Veri fabrikası tarafından etkinlik çalıştırmalarını ADF çalıştırır-2)

- Veri fabrikası tarafından ADF çalıştırır-3) tetikleyici çalıştırmaları

- ADF hataları-1) ilk 10 veri fabrikası işlem hattı hataları

- Veri fabrikası tarafından ADF hataları-2) ilk 10 etkinlik çalıştırmaları

- Veri fabrikası tarafından ADF hataları-3) ilk 10 tetikleyici hataları

- Türe göre etkinlik çalıştırmalarını ADF İstatistikleri-1)

- Türe göre ADF istatistikleri-2) tetikleyici çalıştırmaları

- ADF istatistikleri-3) en fazla işlem hattı süresi çalıştırır.

![İzleyici oms image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![İzleyici oms image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Yukarıdaki ölçümlerini görselleştirmek, bu ölçümleri arkasında sorguları bakmak, sorguları Düzenle, uyarı oluşturma ve VS.

![İzleyici oms image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [izleme ve işlem hatlarını programlama yoluyla yönetme](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) izleme ve işlem hatları, betik çalıştırarak yönetme hakkında bilgi edinmek için.
