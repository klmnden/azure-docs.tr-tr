---
title: Azure Stream Analytics araçları Visual Studio için Kurulum
description: Bu makalede, yükleme gereksinimlerini ve Azure akış analizi araçları Visual Studio için Kurulum nasıl açıklanmaktadır.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/19/2017
ms.openlocfilehash: 511658fc0e2b480987455007dac5f55cd7850feb
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Stream Analytics araçlarını yükleme
Azure Stream Analytics araçları, Visual Studio 2017, 2015 ve 2013 artık destekler. Bu belge, yükleme ve kaldırma araçları açıklar.

Araçları kullanarak daha fazla bilgi için bkz: [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="install"></a>Yükleme
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Karşıdan [Visual Studio 2017 (15.3 veya üstü)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional ve topluluk sürümleri desteklenir. Express sürümü desteklenmiyor. 
* Stream Analytics araçları parçası olan **Azure geliştirme** ve **veri depolama ve işleme** Visual Studio 2017 iş yükleri. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.

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





