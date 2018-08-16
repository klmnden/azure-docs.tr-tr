---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: b8b8b1a593328197c61f8edba9f8f849518dd2e5
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129131"
---
## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

