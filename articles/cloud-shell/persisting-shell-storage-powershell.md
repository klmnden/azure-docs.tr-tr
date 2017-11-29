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
ms.openlocfilehash: d0bc16bc951fce17235d8070012de44ebab89888
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>PowerShell Azure bulut Kabuğu (Önizleme) içinde nasıl çalışır?
PowerShell bulut Kabuğu (Önizleme) dosyaları aşağıdaki yöntemle devam eder: 
* Belirtilen Azure dosya paylaşımı olarak takma `clouddrive` içinde `$Home` doğrudan dosya paylaşımı etkileşim için dizin.

## <a name="list-cloud-drive-azure-file-shares"></a>Bulut sürücü Azure dosya paylaşımları
`Get-CloudDrive` Komutu bulut Kabuğu'nda bulut sürücü tarafından şu anda takılı Azure dosya paylaşımı bilgileri alır. <br>
![Get-CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Bulut sürücü çıkarın
Bulut Kabuk herhangi bir anda takılı bir Azure dosya paylaşımı çıkarın. Azure dosya paylaşımı kaldırdıysanız oluşturmak ve bir sonraki oturumda yeni bir Azure dosya paylaşımı bağlamak için istenir.

`Dismount-CloudDrive` Komutu, geçerli depolama hesabından bir Azure dosya paylaşımı çıkarır. Bulut sürücü çıkarma geçerli oturumu sona erer. Kullanıcı oluşturmak ve bir sonraki oturumu sırasında yeni bir Azure dosya paylaşımı bağlamak için istenir.
![Çıkarma CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell için hızlı başlangıç](quickstart-powershell.md) <br>
[Azure dosyaları hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Depolama etiketler hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>