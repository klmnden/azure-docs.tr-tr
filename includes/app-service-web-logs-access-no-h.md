---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 0dd6618bdee8e6810d414d4b04b16a1e0a9c90ed
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551225"
---
Kapsayıcı içinde oluşturulan yönlendirilen konsol günlüklerini erişebilirsiniz. İlk olarak, Cloud Shell'de aşağıdaki komutu çalıştırarak kapsayıcı günlük özelliğini açar:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Kapsayıcı günlüğe kaydetme etkinleştirildikten sonra günlük akışını görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Tarayıcı günlük dosyalarını da inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Günlük akışını dilediğiniz zaman durdurmak için `Ctrl` + `C`.
