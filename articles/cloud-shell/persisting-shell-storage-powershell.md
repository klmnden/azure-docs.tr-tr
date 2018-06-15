---
title: Azure bulut Kabuğu (Önizleme) PowerShell dosyalarında kalıcı | Microsoft Docs
description: Azure bulut Kabuk dosyaları nasıl devam ederse gözden geçirme.
services: azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: b87c4a408393e4ae341898e8cfa23e9acbcb4fc2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32154407"
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>PowerShell Azure bulut Kabuğu (Önizleme) içinde nasıl çalışır?
PowerShell bulut Kabuğu (Önizleme) dosyaları aşağıdaki yöntemle devam eder: 
* Belirtilen Azure dosya paylaşımı olarak takma `clouddrive` içinde `$Home` doğrudan dosya paylaşımı etkileşim için dizin.

## <a name="list-clouddrive-azure-file-shares"></a>Liste `clouddrive` Azure dosya paylaşımları
`Get-CloudDrive` Komutu tarafından o anda bağlı Azure dosya paylaşımı bilgileri alır `clouddrive` bulut Kabuğu'nda. <br>
![Get-CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-clouddrive"></a>Çıkarın `clouddrive`
Bulut Kabuk herhangi bir anda takılı bir Azure dosya paylaşımı çıkarın. Azure dosya paylaşımı kaldırdıysanız oluşturmak ve bir sonraki oturumda yeni bir Azure dosya paylaşımı bağlamak için istenir.

`Dismount-CloudDrive` Komutu, geçerli depolama hesabından bir Azure dosya paylaşımı çıkarır. Çıkarma `clouddrive` geçerli oturumu sona erer. Kullanıcı oluşturmak ve bir sonraki oturumu sırasında yeni bir Azure dosya paylaşımı bağlamak için istenir.
![Çıkarma CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell için hızlı başlangıç](quickstart-powershell.md) <br>
[Azure dosyaları hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Depolama etiketler hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>