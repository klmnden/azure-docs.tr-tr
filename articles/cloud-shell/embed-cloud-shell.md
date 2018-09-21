---
title: Azure Cloud Shell'i ekleme | Microsoft Docs
description: Azure Cloud Shell'i ekleme hakkında bilgi alın.
services: cloud-shell
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 37ad0adf01172fadc6d9845576045a4542bdae19
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497799"
---
# <a name="embed-azure-cloud-shell"></a>Azure Cloud Shell'i ekleme

Cloud Shell'i ekleme sağlayan doğrudan adanmış bir URL'den Cloud Shell'i açmak, geliştiriciler ve içerik yazarları [shell.azure.com](https://shell.azure.com). Bu araç, Cloud Shell'inizin kimlik doğrulaması'nın tüm gücünden hemen getirir ve kullanıcılarınız için Azure CLI/Azure PowerShell güncel araçlar.

Normal boyutlu düğmesi

[![Normal başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)

Büyük ölçekli düğmesi

[![Büyük başlatma](https://shell.azure.com/images/launchcloudshell@2x.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)

## <a name="how-to"></a>Nasıl yapılır

Cloud Shell'inizin başlatma düğmesi, aşağıdaki kopyalayarak markdown dosyalarına tümleştirin:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Bir açılır Cloud Shell'i ekleme için HTML aşağıda verilmiştir:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Deneyiminizi özelleştirin

Belirli bir kabuk deneyimi URL'nizi geliştirerek ayarlayın.
|Deneyimi   |URL'si   |
|---|---|
|Son kullanılan Kabuğu   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[Shell.Azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[Shell.Azure.com/PowerShell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md)<br>
[PowerShell Cloud Shell hızlı](quickstart-powershell.md)
