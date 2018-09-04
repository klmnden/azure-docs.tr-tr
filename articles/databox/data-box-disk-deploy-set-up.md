---
title: Microsoft Azure Data Box Disk'i ayarlama | Microsoft Docs
description: Azure Data Box Disk'inizi nasıl ayarlayabileceğinizi öğrenmek için bu öğreticiyi kullanın
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 6fcc7823a7e2f2f1e280622a1fa05d4417a71546
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143491"
---
# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Öğretici: Azure Data Box Disk'i paketinden çıkarma, bağlama ve kilidini açma

Bu öğreticide, Azure Data Box Disk'inizi paketinden çıkarma, bağlama ve kilidini açma işlemleri açıklanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Data Box Disk'inizi paketinden çıkarma
> * Data Box Disk'i bağlama ve kilidini açma.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğretici: Azure Data Box Disk sipariş etme](data-box-disk-deploy-ordered.md) konusunu tamamladınız.
2. Disklerinizi aldınız ve portaldaki iş durumu **Teslim Edildi** olarak güncelleştirildi.
3. Data Box Disk kilidini açma aracını yükleyebileceğiniz bir konak bilgisayarınız var. Konak bilgisayarınızda:
    - [Desteklenen bir işletim sistemi](data-box-disk-system-requirements.md) çalıştırılmalıdır.
    - [Windows PowerShell 4 yüklü](https://www.microsoft.com/download/details.aspx?id=40855) olmalıdır.
    - [.NET Framework 4.5.1 yüklü](https://www.microsoft.com/download/details.aspx?id=30653) olmalıdır.
    - [BitLocker etkin](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server) olmalıdır.
    - [Windows Management Framework 4 yüklü](https://www.microsoft.com/en-us/download/details.aspx?id=40855) olmalıdır. 

## <a name="unpack-your-disks"></a>Disklerinizi paketinden çıkarma

 Disklerinizin paketinden çıkarmak için aşağıdaki adımları gerçekleştirin.

1. Data Box Disk'ler küçük bir sevkiyat kutusunda postalanır. Kutuyu açın ve içindekileri çıkarın. Paketi kontrol edip içinde 1 ile 5 arası katı hal sürücüsü (SSD) ve disk başına bir USB bağlantı kablosu bulunduğundan emin olun. Üzerinde oynanıp oynanmadığını veya başka herhangi bir hasarı olup olmadığını anlamak için kutuyu inceleyin. 

    ![Data Box Disk sevkiyat paketi](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Sevkiyat kutusuyla oynanmışsa veya üzerinde ciddi bir hasar varsa, kutuyu açmayın. Disklerin düzgün çalışır durumda olup olmadığını ve bunların yerine başkalarının gönderilmesinin gerekip gerekmediğini değerlendirmenize yardımcı olmaları için Microsoft Desteği'ne başvurun.
3. Kutuda, iade gönderimi için sevkiyat etiketini (geçerli etiketin altında) içeren bir şeffaf kılıf bulunduğunu doğrulayın. Bu etiket kayıpsa veya hasar görmüşse, istediğiniz zaman Azure portaldan yenisini indirebilir ve yazdırabilirsiniz. 

    ![Data Box Disk sevkiyat etiketi](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Disklerin iade gönderimi için kutuyu ve ambalaj köpüklerini saklayın.

## <a name="connect-and-unlock-your-disks"></a>Disklerinizi bağlama ve kilitlerini açma

Disklerinizi bağlamak ve kilitlerini açmak için aşağıdaki adımları gerçekleştirin.

1. Diski önkoşullarda belirtildiği gibi desteklenen bir işletim sisteminin çalıştırıldığı Windows bilgisayarına bağlamak için, verilen kabloyu kullanın. 

    ![Data Box Disk bağlantısı](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. Azure portalda **Genel > Cihaz ayrıntıları**'na gidin. 
3. **Data Box Disk kilidi açma aracını indirin**'e tıklayın. 

    ![Data Box Disk kilidi açma aracını indirme](media/data-box-disk-deploy-set-up/data-box-disk-unload1.png)     

4. Aracı, verileri kopyalamak için kullanacağınız bilgisayarda ayıklayın.
5. Aynı bilgisayarda bir Komut İstemi penceresi açın veya yönetici olarak Windows PowerShell'i çalıştırın.
6. (İsteğe bağlı) Diskin kilidini açarken kullandığınız bilgisayarın işletim sistemi gereksinimlerini karşıladığını doğrulamak için, system check komutunu çalıştırın. Örnek çıktı aşağıda gösterilmiştir. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

7. Azure portalda **Genel > Cihaz ayrıntıları**'na gidin. Destek anahtarını kopyalamak için kopyala simgesini kullanın.
8. `DataBoxDiskUnlock.exe` öğesini çalıştırın ve destek anahtarını sağlayın. Diske atanan sürücü harfi görüntülenir. Örnek çıktı aşağıda gösterilmiştir.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkeys (format: passkey1;passkey2;passkey3):
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

9. Gelecekte yeniden disk eklerken 6-8 arası adımları yineleyin. Data Box Disk kilidini açma aracıyla ilgili yardıma ihtiyacınız olursa, help komutunu kullanın.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKeys:<passkey_list_separated_by_semicolon>
    
    Example: DataBoxUnlock /PassKeys:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKeys:       Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
10. Diskin kilidi açıldıktan sonra, disk içeriğini görüntüleyebilirsiniz.    

    ![Data Box Disk içeriği](media/data-box-disk-deploy-set-up/data-box-disk-content.png) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk'inizi paketinden çıkarma
> * Data Box Disk'lerini bağlama ve kilidini açma


Data Box Disk'inize verileri kopyalama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Disk'inize verileri kopyalama](./data-box-disk-deploy-copy-data.md)

