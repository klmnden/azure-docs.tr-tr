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
ms.openlocfilehash: 3ceddb94336fc2703e6f916f05ab1ec3676cb50d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="embed-azure-cloud-shell"></a>Azure bulut Kabuk katıştırma

Bulut Kabuk katıştırma sağlayan geliştiriciler ve içerik yazarları doğrudan adanmış bir URL'den bulut Kabuğu'nu açmak [shell.azure.com](https://shell.azure.com). Bu bulut kabuğun kimlik doğrulama, araç, tam güç hemen getirir ve kullanıcılarınız için güncel Azure CLI/Azure PowerShell araçları.

Normal boyutlu düğmesi

[![](https://shell.azure.com/images/launchcloudshell.png "Azure bulut Kabuğu'nu başlatın")](https://shell.azure.com)

Büyük ölçekli düğmesi

[![](https://shell.azure.com/images/launchcloudshell@2x.png "Azure bulut Kabuğu'nu başlatın")](https://shell.azure.com)

## <a name="how-to"></a>Nasıl yapılır

Bulut kabuğun Başlat düğmesi aşağıdaki kopyalayarak markdown dosyalarıyla tümleştirmek:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Açılır bir bulut Kabuk katıştırmak için HTML aşağıdadır:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Deneyimini özelleştirme

Belirli Kabuğu deneyiminin URL'nizi program.cs'ye olarak ayarlayın.
|Deneyimi   |URL   |
|---|---|
|Son kullanılan Kabuğu   |shell.azure.com           |
|Bash                       |shell.azure.com/bash       |
|PowerShell                 |shell.azure.com/powershell |

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md)<br>
[PowerShell içinde bulut Kabuk hızlı başlangıç](quickstart-powershell.md)
