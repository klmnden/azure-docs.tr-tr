---
title: AzCopy kullanarak Azure DevTest Labs kullanarak karşıya VHD dosyası yükleme | Microsoft Docs
description: Laboratuvar depolama hesabına AzCopy kullanarak karşıya VHD dosyası yükleme
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
ms.openlocfilehash: 8cd778762bebf4a9dda3688292ac0a3674e446e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60634985"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>Laboratuvar depolama hesabına AzCopy kullanarak karşıya VHD dosyası yükleme

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de sanal makineler sağlamak için kullanılan özel görüntüler oluşturmak için VHD dosyaları kullanılabilir. Aşağıdaki adımlarda bir laboratuvar depolama hesabına VHD dosyası yüklemek için AzCopy komut satırı yardımcı programını kullanarak aracılığıyla yol. VHD dosyasını yükledikten sonra [Bölümü'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir bazı makaleler listeler. Diskler ve VHD'ler Azure hakkında daha fazla bilgi için bkz: [yönetilen disklere giriş](../virtual-machines/linux/managed-disks-overview.md)

> [!NOTE] 
>  
> AzCopy, yalnızca Windows bir komut satırı yardımcı programıdır.

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlarda Azure DevTest Labs'i kullanarak bir VHD dosyasını karşıya aracılığıyla yol [AzCopy](https://aka.ms/downloadazcopy). 

1. Azure portalını kullanarak Laboratuvar depolama hesabının adını alın:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar dikey penceresinde seçin **yapılandırma**. 

1. Laboratuvar **yapılandırma** dikey penceresinde **özel görüntülerin (VHD)** .

1. Üzerinde **özel görüntüleri** dikey penceresinde seçin **+ Ekle**. 

1. Üzerinde **özel görüntü** dikey penceresinde **VHD**.

1. Üzerinde **VHD** dikey penceresinde **PowerShell kullanarak bir VHD'yi karşıya yükleme**.

    ![PowerShell kullanarak bir VHD'yi karşıya yükleme](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. **PowerShell kullanarak bir görüntüyü karşıya yükleme** dikey bir çağrı görüntüler **Add-AzureVhd** cmdlet'i. İlk parametre (*hedef*) URI blob kapsayıcısı için içeriyor (*yükler*) şu biçimde:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Sonraki adımlarda kullanıldıkça tam URI not edin.

1. AzCopy kullanarak karşıya VHD dosyası yükleme:
 
1. [En güncel AzCopy sürümünü yükleyip](https://aka.ms/downloadazcopy).

1. Bir komut penceresi açın ve AzCopy yükleme dizinine gidin. İsteğe bağlı olarak, sistem yolunuza AzCopy yükleme konumu ekleyebilirsiniz. Varsayılan olarak, AzCopy şu dizine yüklenir:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Depolama hesabı anahtarı ve blob kapsayıcısının URI kullanarak, komut isteminde aşağıdaki komutu çalıştırın. *VhdFileName* değeri tırnak içinde olması gerekir. Bir VHD dosyasını karşıya yükleme işlemi, VHD dosyasının ve bağlantı hızınızı boyutuna bağlı olarak uzun olabilir.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasından bir özel görüntü oluşturma](devtest-lab-create-template.md)
- [Azure DevTest labs'teki PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)