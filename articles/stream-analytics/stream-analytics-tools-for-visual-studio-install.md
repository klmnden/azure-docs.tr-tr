---
title: "Visual Studio için yükleme yönergeleri için Azure Stream Analytics araçları | Microsoft Docs"
description: "Visual Studio için yükleme yönergeleri için Azure Stream Analytics araçları"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: su-jie
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 9/19/2017
ms.author: sujie
ms.openlocfilehash: 307270a25545a0388e67c37656057f81535d8d3b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="installation-instructions-for-stream-analytics-tools-for-visual-studio"></a>Visual Studio için yükleme yönergeleri için Stream Analytics araçları
Azure Stream Analytics araçları, Visual Studio 2017, 2015 ve 2013 artık destekler. Bu belgede, yükleme ve Araçları'nı kaldırın tanıtır.

Nasıl kullanacağınızı öğrenin [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="install"></a>Yükleme
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Karşıdan [Visual Studio 2017 (15.3 veya üstü)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional ve topluluk sürümleri desteklenir. Express sürümü desteklenmiyor. 
* Stream Analytics araçları parçası olan **Azure geliştirme** ve **veri depolama ve işleme** Visual Studio 2017 iş yükleri. Bu iki iş yükleri birini Visual Studio yüklemenizin bir parçası olarak etkinleştirin.

Etkinleştirme **veri depolama ve işleme** gösterildiği gibi iş yükü:

![Veri depolama ve işlem iş yükü](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-2017-install-01.png)

Etkinleştirme **Azure geliştirme** gösterildiği gibi iş yükü:

![Azure geliştirme iş yükü](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-2017-install-02.png)


### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Visual Studio 2015 veya Visual Studio 2013 güncelleştirme 4 yükleyin. Enterprise (Ultimate/Premium), Professional ve topluluk sürümleri desteklenir. Express sürümü desteklenmiyor. 
* .NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK'yı yükleme kullanarak [Web Platformu yükleyicisi](http://www.microsoft.com/web/downloads/platform.aspx).
* Yükleme [Visual Studio için Azure Stream Analytics Araçları](http://aka.ms/asatoolsvs).



## <a name="update"></a>Güncelleştirme

### <a name="visual-studio-2017"></a>Visual Studio 2017
Yeni sürüm anımsatıcı Visual Studio bildiriminde gösterir. 

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
Visual Studio için yüklü Stream Analytics araçları için yeni sürümleri otomatik olarak kontrol edin. En son sürümünü yüklemek için açılır pencere'ndaki yönergeleri izleyin. 


## <a name="uninstall"></a>Kaldırma

### <a name="visual-studio-2017"></a>Visual Studio 2017
Visual Studio Yükleyicisi'ni çift tıklatın ve seçin **Değiştir**. Clear **Azure Data Lake ve akış analiz araçları** ya da onay kutusundan **veri depolama ve işleme** iş yükü veya **Azure geliştirme** iş yükü.

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
Denetim Masası'na gidin ve Kaldır **Visual Studio için Microsoft Azure Data Lake ve akış analizi Araçları**.





