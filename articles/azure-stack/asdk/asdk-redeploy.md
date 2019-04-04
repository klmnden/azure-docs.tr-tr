---
title: Azure Stack geliştirme Seti'ni (ASDK) yeniden | Microsoft Docs
description: Bu makalede, ASDK yeniden öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: ''
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: 284e1ce3c3b9a63f3c25e85891b1d2688726183e
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879990"
---
# <a name="redeploy-the-asdk"></a>ASDK yeniden dağıtma
Bu makalede, Azure Stack geliştirme Seti'ni (ASDK) bir üretim dışı ortamda yeniden öğrenin. ASDK yükseltme desteklenmediğinden, tamamen yeni bir sürüme taşımak için yeniden dağıtmanız gerekir. Ayrıca, yalnızca sıfırdan başlamak istiyorsanız dilediğiniz zaman ASDK yeniden dağıtabilirsiniz.

> [!IMPORTANT]
> ASDK yeni bir sürüme yükseltme desteklenmez. Geliştirme Seti ana bilgisayarda ASDK Azure yığını'nın daha yeni bir sürümü değerlendirmek istediğiniz her zaman yeniden dağıtmanız gerekir.

## <a name="remove-azure-registration"></a>Azure kaydı Kaldır 
Azure ile daha önce ASDK yüklemenizi kayıtlıysanız, kayıt kaynağı ASDK yeniden dağıtmadan önce kaldırmanız gerekir. ASDK ASDK yeniden dağıtırken öğeleri Market'te kullanılabilirliğini etkinleştirmek için yeniden kaydedin. ASDK Azure aboneliğinizle daha önce kaydetmediyseniz, bu bölümü atlayabilirsiniz.

Kayıt kaynak kaldırmak için **Remove-AzsRegistration** Azure Stack kaydını kaldırmak için cmdlet. Ardından, **Remove-AzureRMResourceGroup** cmdlet'ini Azure Stack kaynak grubunu Azure aboneliğinizden silebilirsiniz:

1. Ayrıcalıklı uç noktasına erişimi olan bir bilgisayarda bir yönetici olarak bir PowerShell konsolu açın. ASDK için Geliştirme Seti ana bilgisayar olmasıdır.

2. ASDK yüklemenizi kaydını silin ve silmek için aşağıdaki PowerShell komutlarını çalıştırın **azurestack** Azure aboneliğinizde bir kaynak grubu:

   ```Powershell    
   #Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Provide Azure subscription admin credentials
   Add-AzureRmAccount

   # Provide ASDK admin credentials
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

   # Unregister Azure Stack
   Remove-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01

   # Remove the Azure Stack resource group
   Remove-AzureRmResourceGroup -Name azurestack -Force
   ```

3. Komut dosyası çalıştığında, Azure aboneliğinizin ve yerel ASDK yükleme için oturum istenir.
4. Betik tamamlandığında aşağıdaki örneklere benzer bir ileti görmeniz gerekir:

    `De-Activating Azure Stack (this may take up to 10 minutes to complete).`
    `Your environment is now unable to syndicate items and is no longer reporting usage data.`
    `Remove registration resource from Azure...`
    `"Deleting the resource..." on target "/subscriptions/<subscription information>"`
    `********** End Log: Remove-AzsRegistration *********`



Azure Stack artık başarıyla Azure aboneliğinizden kaydı olmalıdır. Ayrıca, Azure ile ASDK kaydedildiğinde oluşturulan azurestack kaynak grubu da silinir.

## <a name="deploy-the-asdk"></a>ASDK dağıtma
Azure Stack yeniden dağıtmak için üzerinde sıfırdan aşağıda açıklandığı gibi başlatmanız gerekir. Adımlar olsun veya olmasın, Azure Stack Yükleyicisi (installer.ps1 asdk) betiği ASDK yüklemek için kullanılan bağlı olarak değişir.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>Yükleyici komut dosyası kullanarak ASDK yeniden dağıtma
1. ASDK bilgisayarda, yükseltilmiş bir PowerShell konsolu açın ve gidin asdk installer.ps1 betik **AzureStack_Installer** dizininde üzerinde bir sistem dışı sürücü. Betiği çalıştırmak ve tıklayın **yeniden**.

   ![Asdk installer.ps1 betiği çalıştırın](media/asdk-redeploy/1.png)

2. Temel işletim sistemi seçin (değil **Azure Stack**) tıklayıp **sonraki**.

   ![Konak işletim sistemine yeniden başlatın](media/asdk-redeploy/2.png)

3. Temel işletim sistemine Geliştirme Seti konak yeniden başlatıldıktan sonra yerel bir yönetici olarak oturum açın. Bulun ve Sil **C:\CloudBuilder.vhdx** önceki dağıtımının bir parçası kullanılan dosya. 

4. İçin ilk gerçekleştirdiğiniz aynı adımları yineleyin [ASDK dağıtma](asdk-install.md).

### <a name="redeploy-the-asdk-without-using-the-installer"></a>ASDK yükleyici kullanmadan yeniden dağıtın.
ASDK yüklemek için asdk installer.ps1 betik kullanmadıysanız ASDK yeniden dağıtmadan önce Geliştirme Seti ana bilgisayarı el ile yapılandırmalısınız.

1. Sistem Yapılandırması yardımcı programını çalıştırarak başlayın **msconfig.exe** ASDK bilgisayarda. Üzerinde **önyükleme** sekmesinde, ana bilgisayar işletim sistemi (Azure Stack değil) seçin, **varsayılan olarak ayarla**ve ardından **Tamam**. Tıklayın **yeniden** istendiğinde.

      ![Önyükleme yapılandırma kümesi](media/asdk-redeploy/4.png)

2. Temel işletim sistemine Geliştirme Seti konak yeniden başlatıldıktan sonra Geliştirme Seti konak bilgisayar için yerel bir yönetici olarak oturum açın. Bulun ve Sil **C:\CloudBuilder.vhdx** önceki dağıtımının bir parçası kullanılan dosya. 

3. İçin ilk gerçekleştirdiğiniz aynı adımları yineleyin [ASDK PowerShell kullanarak dağıtma](asdk-deploy-powershell.md).


## <a name="next-steps"></a>Sonraki adımlar
[Sonrası ASDK dağıtım görevleri](asdk-post-deploy.md)




