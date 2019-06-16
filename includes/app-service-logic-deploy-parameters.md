---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: f188f2c7bea511f1109d37ef49563e0f745a770e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66167429"
---
Azure Resource Manager ile şablonu dağıtırken kullanılacak değerler için parametre tanımlayabilirsiniz. Şablonu içeren bir `parameters` tüm parametre değerlerini içeren bölümü. Her parametre değeri, dağıtmak istediğiniz kaynakları tanımlamak için şablon tarafından kullanılır.

> [!NOTE]
> Her zaman aynı kalan değerler için parametre tanımlamayın. Dağıtmakta olduğunuz projeye veya dağıttığınız ortama bağlı farklılık gösteren, değerler için parametre tanımlayın.

Parametreler tanımladığınızda:

* Bir kullanıcı dağıtım sırasında sağlayabileceğiniz izin verilen değerleri belirtmek için kullanın **allowedValues** alan.

* Dağıtım sırasında hiçbir değer sağlandığında parametresi için varsayılan değerleri atamak için kullanın **defaultValue** alan. 
