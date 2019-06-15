---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 87fd6b626efb60c7fc7ec8896f2c3758ae5cc33c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66132891"
---
### <a name="app-service-plan"></a>App Service planı
Web uygulamasını barındırmak için service planı oluşturur. Planınızla adını sağlayın **hostingPlanName** parametresi. Plan kaynak grubu için kullandığınız konumun aynısını konumdur. Fiyatlandırma katmanı ve çalışan boyutu belirtilir **sku** ve **workerSize** parametreleri

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

