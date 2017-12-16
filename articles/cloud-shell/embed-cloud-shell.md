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
ms.openlocfilehash: 78b539136971aa282e5447d7882ecb02f73f346b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="embed-azure-cloud-shell"></a>Azure bulut Kabuk katıştırma

Bulut Kabuk katıştırma sağlayan geliştiriciler ve içerik yazarları doğrudan adanmış bir URL'den bulut Kabuğu'nu açmak [shell.azure.com](https://shell.azure.com). Bu bulut kabuğun kimlik doğrulama, araç, tam güç hemen getirir ve kullanıcılarınız için güncel Azure CLI/Azure PowerShell araçları.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure bulut Kabuğu'nu başlatın")](https://shell.azure.com)

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
|Son kullanılan Kabuğu   |Shell.Azure.com           |
|Bash                       |Shell.Azure.com/bash       |
|PowerShell                 |Shell.Azure.com/PowerShell |

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md)<br>
[PowerShell içinde bulut Kabuk hızlı başlangıç](quickstart-powershell.md)
