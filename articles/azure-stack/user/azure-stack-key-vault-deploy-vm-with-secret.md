---
title: Azure Stack'te güvenli şekilde depolanan parola ile VM dağıtma | Microsoft Docs
description: Azure Stack Key Vault'ta depolanan bir parola kullanarak bir VM dağıtma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 3318e52b29723eaa08d8c3a4fba18e278e6cfe9c
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487762"
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Azure Stack Key Vault'ta depolanan bir güvenli parola kullanarak bir sanal makine oluşturun

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Key Vault'ta depolanan bir parola kullanarak bir Windows Server sanal makine dağıtımı aracılığıyla bu makalede adımları. Bir anahtar kasası parolası kullanarak, bir düz metin parola geçirerek değerinden daha güvenlidir.

## <a name="overview"></a>Genel Bakış

Azure Stack anahtar kasasındaki gizli dizi olarak parola gibi değerleri depolayabilir. Gizli dizi oluşturduktan sonra Azure Resource Manager şablonlarında başvurabilirsiniz. Gizli dizileri ile Kaynak Yöneticisi'ni kullanarak aşağıdaki avantajları sağlar:

* Gizli dizi olarak kaynak dağıtma her zaman el ile girmeniz gerekmez.
* Belirli kullanıcılar ya da hizmet sorumluları bir gizli dizi erişip belirtebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Key Vault hizmetini içeren bir teklife abone olması gerekir.
* [Azure Stack için PowerShell yükleyin.](azure-stack-powershell-install.md)
* [PowerShell ortamınızı yapılandırın.](azure-stack-powershell-configure-user.md)

Aşağıdaki adımlar, bir anahtar Kasası'nda depolanan parola alarak sanal makine oluşturmak için gereken işlemi açıklanmaktadır:

1. Bir Key Vault gizli anahtar oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablonu dağıtın.

> [!NOTE]  
> VPN birbirine bağlandıysa, dış istemciden veya Azure Stack geliştirme Seti'ni bu adımları kullanabilirsiniz.

## <a name="create-a-key-vault-secret"></a>Bir Key Vault gizli dizisi oluşturma

Aşağıdaki betik, bir anahtar kasası oluşturulur ve bir parola anahtar kasasında gizli dizi olarak depolar. Kullanım `-EnabledForDeployment` anahtar kasası oluşturulurken parametre. Bu parametre, anahtar kasası Azure Resource Manager şablonlarından başvurulabilir emin olur.

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Önceki betiği çalıştırdığınızda, çıktı gizli anahtar URI'sini içerir. Bu URI not edin. İçinde başvurmak zorunda [dağıtma Windows sanal makine şablonunda anahtar kasası parolası ile](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). İndirme [101-vm-güvenli-parola](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) geliştirme bilgisayarınıza klasör. Bu klasörde `azuredeploy.json` ve `azuredeploy.parameters.json` dosyaları, sonraki adımlarda gerekecektir.

Değiştirme `azuredeploy.parameters.json` ortam değerlerinize göre dosya. Özel ilgi kasa adını, kasa kaynak grubu ve gizli dizi (önceki betiği tarafından oluşturulan gibi) URI parametrelerdir. Aşağıdaki dosya, bir parametre dosyası örneğidir:

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy.parameters.json dosyasını güncelleştirme

KeyVault URI'si secretName, ortamınıza göre sanal makine değerlerinin adminUsername olan azuredeploy.parameters.json dosyasını güncelleştirin. Şablon parametreleri dosyası örneği aşağıdaki JSON dosyası gösterir:

```json
{
    "$schema":  "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Şablon dağıtımı

Şimdi aşağıdaki PowerShell betiğini kullanarak şablonu dağıtın:

```powershell  
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablon başarıyla dağıtıldıktan sonra aşağıdaki çıktı sonuçları:

![Dağıtım çıkışı](media/azure-stack-key-vault-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault örnek bir uygulamayla dağıtma](azure-stack-key-vault-sample-app.md)
* [Anahtar kasası sertifikası ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
