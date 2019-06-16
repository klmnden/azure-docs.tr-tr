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
ms.openlocfilehash: 68a208af1a9aa9e73f2af99021d195f264fb21f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66156001"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid kaynak sağlayıcısını etkinleştirme

Azure aboneliğinizde daha önce Event Grid kullanmadıysanız Event Grid kaynak sağlayıcısına kaydolmanız gerekebilir. Şu komutu çalıştırın:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Kayıt işleminin tamamlanması biraz sürebilir. Durumu denetlemek için şunu çalıştırın:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

`RegistrationStatus` `Registered` olduğu zaman devam edebilirsiniz.
