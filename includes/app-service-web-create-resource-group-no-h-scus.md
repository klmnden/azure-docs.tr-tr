---
title: include dosyası
description: include dosyası
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
ms.date: 09/18/2018
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: 458782011623721bb44771d42caa32130e23bbc9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66139288"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell içinde [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *Orta Güney ABD* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Ücretsiz** katmanda App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.