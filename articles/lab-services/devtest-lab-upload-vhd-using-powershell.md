---
title: PowerShell kullanarak Azure DevTest Labs kullanarak karşıya VHD dosyası yükleme | Microsoft Docs
description: Laboratuvar depolama hesabına PowerShell kullanarak karşıya VHD dosyası yükleme
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 56a66c3eb1dad93fad3ad1572989dc0c0aa14632
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60622788"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>Laboratuvar depolama hesabına PowerShell kullanarak karşıya VHD dosyası yükleme

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de sanal makineler sağlamak için kullanılan özel görüntüler oluşturmak için VHD dosyaları kullanılabilir. Aşağıdaki adımlarda bir laboratuvar depolama hesabına VHD dosyası yüklemek için PowerShell'i kullanma aracılığıyla yol. VHD dosyasını yükledikten sonra [Bölümü'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir bazı makaleler listeler. Diskler ve VHD'ler Azure hakkında daha fazla bilgi için bkz: [yönetilen disklere giriş](../virtual-machines/linux/managed-disks-overview.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlarda PowerShell kullanarak Azure DevTest Labs kullanarak bir VHD dosyasını karşıya yükleme aracılığıyla yol. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar dikey penceresinde seçin **yapılandırma**. 

1. Laboratuvar **yapılandırma** dikey penceresinde **özel görüntülerin (VHD)** .

1. Üzerinde **özel görüntüleri** dikey penceresinde seçin **+ Ekle**. 

1. Üzerinde **özel görüntü** dikey penceresinde **VHD**.

1. Üzerinde **VHD** dikey penceresinde **PowerShell kullanarak bir VHD'yi karşıya yükleme**.

    ![PowerShell kullanarak bir VHD'yi karşıya yükleme](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Üzerinde **PowerShell kullanarak bir görüntüyü karşıya yükleme** dikey penceresinde oluşturulan PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın.

1. Değiştirme **LocalFilePath** parametresinin **Add-AzureVhd** karşıya yüklemek istediğiniz VHD dosyasının konumunu gösterecek şekilde cmdlet'i.

1. Bir PowerShell isteminde çalıştırın **Add-AzureVhd** cmdlet (değiştirilmiş ile **LocalFilePath** parametresi).

> [!WARNING] 
> 
> Bir VHD dosyasını karşıya yükleme işlemi, VHD dosyasının ve bağlantı hızınızı boyutuna bağlı olarak uzun olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasından bir özel görüntü oluşturma](devtest-lab-create-template.md)
- [Azure DevTest labs'teki PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
