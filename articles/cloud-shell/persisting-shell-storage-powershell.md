---
title: "Azure bulut Kabuğu (Önizleme) PowerShell dosyalarında kalıcı | Microsoft Docs"
description: "Azure bulut Kabuk dosyaları nasıl devam ederse gözden geçirme."
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: 66d0e20d670e49cdfe64614d1fc6f5739fde6155
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>PowerShell Azure bulut Kabuğu (Önizleme) içinde nasıl çalışır?
PowerShell bulut Kabuğu (Önizleme) dosyaları aşağıdaki yöntemle devam eder: 
* Belirtilen dosya paylaşımı olarak takma `clouddrive` içinde `$Home` doğrudan dosya paylaşımı etkileşim için dizin.

## <a name="list-cloud-drive-file-shares"></a>Bulut sürücü dosya paylaşımları
`Get-CloudDrive` Komutu bulut Kabuğu'nda bulut sürücü tarafından şu anda bağlı dosya paylaşımı bilgileri alır. <br>
![Get-CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Bulut sürücü çıkarın
Bulut Kabuk herhangi bir anda takılı bir dosya paylaşımı çıkarın. Dosya Paylaşımı kaldırdıysanız oluşturmak ve bir sonraki oturumda yeni bir dosya paylaşımı bağlamak için istenir.

`Dismount-CloudDrive` Komutu, geçerli depolama hesabından bir dosya paylaşımı çıkarır. Bulut sürücü çıkarma geçerli oturumu sona erer. Kullanıcı oluşturmak ve bir sonraki oturumu sırasında yeni bir dosya paylaşımı bağlamak için istenir.
![Çıkarma CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell için hızlı başlangıç](quickstart-powershell.md) <br>
[Azure File storage hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Depolama etiketler hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>