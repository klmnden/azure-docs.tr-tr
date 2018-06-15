---
title: Azure CLI Redis önbelleği örnekleri | Microsoft Docs
description: Azure Redis önbelleği için Azure CLI örneklerini içerir.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 8d2de145-50c0-4f76-bf8f-fdf679f03698
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.date: 04/14/2017
ms.author: wesmc
ms.openlocfilehash: 4450a8f7f16e3503626c9ee0ca125fc2b1ed9052
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27909848"
---
# <a name="azure-cli-samples-for-azure-redis-cache"></a>Azure CLI örnekleri Azure Redis önbelleği

Aşağıdaki tabloda Azure CLI kullanarak oluşturulan komut dosyalarını bash bağlantılar içerir.

| | |
|---|---|
|**Önbelleği oluşturma**||
| [Bir önbellek oluşturma](./scripts/create-cache.md) | Bir kaynak grubu ve temel bir katman Azure Redis önbelleği oluşturur. |
| [Kümeleme premium önbelleği oluşturma](./scripts/create-premium-cache-cluster.md) | Bir kaynak grubu ve premium katmanı önbelleği kümeleme özelliği etkinleştirilmiş oluşturur.|
| [Önbellek ayrıntıları alma](./scripts/show-cache.md) | Sağlama durumu da dahil olmak üzere bir Azure Redis önbelleği örneğinin ayrıntılarını alır. |
| [Ana bilgisayar adı, bağlantı noktalarını ve anahtarları alma](./scripts/cache-keys-ports.md) | Ana bilgisayar adı, bağlantı noktalarını ve anahtarları için bir Azure Redis önbelleği örneği alır. |
|**Web uygulaması artı önbelleği**||
| [Bir web uygulamasını redis önbelleği için bağlama](./../app-service/scripts/app-service-cli-app-service-redis.md) | Azure web uygulaması ve redis önbelleği oluşturur, ardından redis bağlantı ayrıntıları için uygulama ayarları ekler. |
|**Delete cache**||
| [Bir önbellek Sil](./scripts/delete-cache.md) | Bir Azure Redis önbelleği örneği siler  |
| | |

Azure CLI 2.0 hakkında daha fazla bilgi için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [Azure CLI 2.0 ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).