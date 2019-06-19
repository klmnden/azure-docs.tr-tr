---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: a3f75b7273164abc5318f16e9ab8d9883ff0c0aa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188153"
---
## <a name="test"></a>Azure'da işlevi test etme

Dağıtılan işlevi test etmek için cURL kullanın. Kullanarak önceki adımda kopyaladığınız URL'ye sorgu dizesi ekleme `&name=<yourname>` aşağıdaki örnekteki gibi bir URL:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

![Azure'da işlevi çağırmak için cURL kullanarak.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

Ayrıca, web tarayıcınızın adres için kopyalanan URL'yi yapıştırabilirsiniz. Yeniden sorgu dizesini URL'ye `&name=<yourname>` isteği yürütmeden önce URL'si.

![İşlev çağrısı için bir web tarayıcısı kullanma.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
