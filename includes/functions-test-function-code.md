---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: a3f75b7273164abc5318f16e9ab8d9883ff0c0aa
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64867358"
---
## <a name="test"></a>Azure'da işlevi test etme

Dağıtılan işlevi test etmek için cURL kullanın. Kullanarak önceki adımda kopyaladığınız URL'ye sorgu dizesi ekleme `&name=<yourname>` aşağıdaki örnekteki gibi bir URL:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

![Azure'da işlevi çağırmak için cURL kullanarak.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

Ayrıca, web tarayıcınızın adres için kopyalanan URL'yi yapıştırabilirsiniz. Yeniden sorgu dizesini URL'ye `&name=<yourname>` isteği yürütmeden önce URL'si.

![İşlev çağrısı için bir web tarayıcısı kullanma.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
