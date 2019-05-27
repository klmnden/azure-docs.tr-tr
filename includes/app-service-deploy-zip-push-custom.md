---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 79fb8517ec6880e8a3eae0e74275567a24644b87
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132575"
---
## <a name="deployment-customization"></a>Dağıtım özelleştirme

Dağıtım işlemi, anında iletme .zip dosyasını çalıştırılmaya hazır uygulamasını içerdiğini varsayar. Varsayılan olarak, hiçbir özelleştirmeleri çalıştırılır. Sürekli Tümleştirme ile aldığınız aynı yapı işlemlerini etkinleştirmek için uygulama ayarlarınızı aşağıdakileri ekleyin:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

.Zip anında dağıtım kullandığınızda, bu ayardır **false** varsayılan olarak. Varsayılan değer **true** sürekli tümleştirme dağıtımları için. Ayarlandığında **true**, dağıtımıyla ilgili ayarlarınızı dağıtımı sırasında kullanılır. Uygulama ayarları olarak veya .zip dosyasının kök dizininde bulunan bir yapılandırma dosyası, bu ayarları yapılandırabilirsiniz. Daha fazla bilgi için [deposu ve dağıtım ile ilgili ayarları](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) dağıtım başvurusu.