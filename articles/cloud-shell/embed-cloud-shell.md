---
title: Azure bulut Kabuk ekleme | Microsoft Docs
description: "Azure bulut Kabuk katıştırmak öğrenin."
services: cloud-shell
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 7c06a51e7f9f402b2ec10e440ca98125a7f2a7cf
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="embed-azure-cloud-shell"></a>Azure bulut Kabuk katıştırma

Bulut Kabuk katıştırma sağlayan geliştiriciler ve içerik yazarları doğrudan adanmış bir URL'den bulut Kabuğu'nu açmak [shell.azure.com](https://shell.azure.com). Bu, bulut kabuğun kimlik doğrulama, araçları ve güncel Azure CLI/Azure PowerShell araçları tam güç kilidini açar.

## <a name="how-to"></a>Nasıl yapılır

Bulut kabuğun Başlat düğmesi aşağıdaki kopyalayarak markdown dosyalarıyla tümleştirmek:

```markdown
[![Launch Cloud Shell](https:shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Açılır bir bulut Kabuk katıştırmak için HTML aşağıdadır:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md)<br>
[PowerShell içinde bulut Kabuk hızlı başlangıç](quickstart-powershell.md)