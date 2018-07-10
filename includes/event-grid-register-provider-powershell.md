---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 0946b703b45f69ede409167213f2629a796f18b5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869636"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid kaynak sağlayıcısını etkinleştirme

Azure aboneliğinizde daha önce Event Grid kullanmadıysanız Event Grid kaynak sağlayıcısına kaydolmanız gerekebilir. Şu komutu çalıştırın:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Kayıt işleminin tamamlanması biraz sürebilir. Durumu denetlemek için şunu çalıştırın:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

`RegistrationStatus` `Registered` olduğu zaman devam edebilirsiniz.