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
ms.openlocfilehash: 673f4935dce28b30c10e6abf4c7d22e00c1dd73a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60762244"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Stream Analytics araçları yükleme
Azure Stream Analytics araçları Visual Studio 2017, 2015 ve 2013'ü destekler. Bu makalede, yükleme ve kaldırma araçları açıklar.

Araçları kullanarak daha fazla bilgi için bkz: [Visual Studio için Stream Analytics Araçları](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Yükleme
### <a name="recommended-visual-studio-2019-and-2017"></a>Önerilen: Visual Studio 2019 ve 2017
* İndirme [Visual Studio 2019 (Önizleme 2 veya üzeri) ve Visual Studio 2017 (15.3 veya üzeri)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez. Visual Studio 2017'de Mac desteklenmiyor. 
* Stream Analytics araçları parçası olan **Azure geliştirme** ve **veri depolama ve işleme** Visual Studio 2017 iş yükleri. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.

Etkinleştirme **veri depolama ve işleme** gösterildiği gibi iş yükü:

![Veri depolama ve işleme iş yükü seçili](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Etkinleştirme **Azure geliştirme** gösterildiği gibi iş yükü:

![Azure geliştirme iş yükü seçili](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)

* Araçlar menüsünde **Uzantılar ve güncelleştirmeler**. Yüklü uzantılar tıklayıp Bul Azure Data Lake ve Stream Analytics Araçları **güncelleştirme** son uzantıyı yüklemek için. 

![Visual Studio uzantıları ve güncelleştirmeler](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-extensions-updates.png)

### <a name="visual-studio-2015-2013"></a>Visual Studio 2015, 2013
* Visual Studio 2015 veya Visual Studio 2013 güncelleştirme 4 yükleyin. Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez. 
* .NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK'yı yükleme kullanarak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
* Yükleme [Visual Studio için Azure Stream Analytics Araçları](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Güncelleştirme

### <a name="visual-studio-2019-and-2017"></a>Visual Studio 2019 ve 2017
Yeni sürüm anımsatıcı Visual Studio bildirimde gösterilir.

![Visual Studio yeni sürüm anımsatıcı](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-new-version-reminder-vs-tools.png)

### <a name="visual-studio-2015-and-2013"></a>Visual Studio 2015 ve 2013
Visual Studio için yüklü olan Stream Analytics araçları, yeni sürümleri için otomatik olarak denetleyin. En son sürümünü yüklemek için açılır penceredeki yönergeleri izleyin. 


## <a name="uninstall"></a>Kaldırma

### <a name="visual-studio-2019-and-2017"></a>Visual Studio 2019 ve 2017
Visual Studio Yükleyicisi ve seçin **Değiştir**. NET **Azure Data Lake ve Stream Analytics Araçları** ya da onay kutusundan **veri depolama ve işleme** iş yükü veya **Azure geliştirme** iş yükü.

### <a name="visual-studio-2015-and-2013"></a>Visual Studio 2015 ve 2013
Denetim Masası'na gidin ve kaldırma **Visual Studio için Microsoft Azure Data Lake ve Stream Analytics Araçları**.





