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
ms.openlocfilehash: 8dbc90c3888a9c53db7d2ebeea2dd5f3ee5746a0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944706"
---
## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

