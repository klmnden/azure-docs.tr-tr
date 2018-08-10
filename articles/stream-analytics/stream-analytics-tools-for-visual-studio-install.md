---
title: Visual Studio için Azure Stream Analytics araçları ayarlayın
description: Bu makalede, yükleme gereksinimleri ve Visual Studio için Azure Stream Analytics araçları ayarlama açıklanır.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: e87fc5b91e9e1d5f4f8449e84b17bcdab9c0b6b2
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713603"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Stream Analytics araçları yükleme
Azure Stream Analytics araçları, Visual Studio 2017, 2015 ve 2013 destekler. Bu makalede, yükleme ve kaldırma araçları açıklar.

Araçları kullanarak daha fazla bilgi için bkz: [Visual Studio için Stream Analytics Araçları](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Yükleme
### <a name="visual-studio-2017"></a>Visual Studio 2017
* İndirme [Visual Studio 2017 (15.3 veya üzeri)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez. 
* Stream Analytics araçları parçası olan **Azure geliştirme** ve **veri depolama ve işleme** Visual Studio 2017 iş yükleri. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.

Etkinleştirme **veri depolama ve işleme** gösterildiği gibi iş yükü:

![Veri depolama ve işleme iş yükü seçili](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Etkinleştirme **Azure geliştirme** gösterildiği gibi iş yükü:

![Azure geliştirme iş yükü seçili](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)


### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Visual Studio 2015 veya Visual Studio 2013 güncelleştirme 4 yükleyin. Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez. 
* .NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK'yı yükleme kullanarak [Web Platformu yükleyicisi](http://www.microsoft.com/web/downloads/platform.aspx).
* Yükleme [Visual Studio için Azure Stream Analytics Araçları](http://aka.ms/asatoolsvs).

## <a name="update"></a>Güncelleştirme

### <a name="visual-studio-2017"></a>Visual Studio 2017
Yeni sürüm anımsatıcı Visual Studio bildirimde gösterilir. 

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 ve Visual Studio 2015
Visual Studio için yüklü olan Stream Analytics araçları, yeni sürümleri için otomatik olarak denetleyin. En son sürümünü yüklemek için açılır penceredeki yönergeleri izleyin. 


## <a name="uninstall"></a>Kaldırma

### <a name="visual-studio-2017"></a>Visual Studio 2017
Visual Studio Yükleyicisi ve seçin **Değiştir**. NET **Azure Data Lake ve Stream Analytics Araçları** ya da onay kutusundan **veri depolama ve işleme** iş yükü veya **Azure geliştirme** iş yükü.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 ve Visual Studio 2015
Denetim Masası'na gidin ve kaldırma **Visual Studio için Microsoft Azure Data Lake ve Stream Analytics Araçları**.





