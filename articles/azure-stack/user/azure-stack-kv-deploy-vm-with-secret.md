---
title: Azure yığında güvenli şekilde depolanan parola ile bir VM'yi dağıtmak | Microsoft Docs
description: Azure yığın anahtar kasasında depolanan bir parola kullanarak bir VM'i dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/07/2018
ms.author: mabrigg
ms.openlocfilehash: 4239eb31afd4abc8b3555f0ee353f5d96716d623
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Azure yığın anahtar kasasında depolanan güvenli bir parola kullanarak bir sanal makine oluşturun

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın anahtar kasasında depolanan bir parola kullanarak bir Windows Server sanal makine dağıtımı aracılığıyla bu makalede adımlar. Bir anahtar kasası parola kullanarak, bir düz metin parola geçirerek değerinden daha güvenlidir.

## <a name="overview"></a>Genel Bakış

Bir parola gibi değerler Azure yığın anahtar kasasına gizli olarak depolayabilirsiniz. Bir gizli anahtar oluşturduktan sonra Azure Resource Manager şablonlarını başvuruda bulunabilir. Gizli Resource Manager ile kullanılması aşağıdaki avantajları sağlar:

* Gizli bir kaynak dağıttığınız her zaman el ile girmeniz gerekmez.
* Belirli kullanıcılar ya da hizmet asıl adı bir gizlilik erişebilirsiniz belirtebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız.
* [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)
* [Azure yığın kullanıcı PowerShell ortamının yapılandırın.](azure-stack-powershell-configure-user.md)

Aşağıdaki adımlar, bir anahtar kasasında depolanan parola alarak bir sanal makine oluşturmak için gereken işlem açıklamaktadır:

1. Gizli bir anahtar kasası oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablon dağıtın.

>[NOT] VPN üzerinden bağlandığı sırada Azure yığın Geliştirme Seti veya bir dış istemcinin aşağıdaki adımları kullanabilirsiniz.

## <a name="create-a-key-vault-secret"></a>Gizli bir anahtar kasası oluşturma

Aşağıdaki komut dosyasını bir anahtar kasası oluşturur ve gizli olarak anahtar kasasında bir parola depolar. Kullanım `-EnabledForDeployment` anahtar kasası oluşturulurken parametre. Bu parametre, anahtar kasası Azure Resource Manager şablonlarını başvurulabilir emin olur.

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

Önceki komut dosyasını çalıştırdığınızda, çıktı gizli URI içerir. Bu URI not edin. İçinde başvurmak zorunda [dağıtımı Windows sanal makine anahtar kasası şablonunda parolayla](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). Karşıdan [101-vm-güvenli-parola](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) geliştirme bilgisayarınıza klasör. Bu klasörde `azuredeploy.json` ve `azuredeploy.parameters.json` sonraki adımlarda gerekir dosyaları.

Değiştirme `azuredeploy.parameters.json` ortamı değerlerinizi göre dosya. Özel ilgi kasa adını, kasa kaynak grubu ve gizli anahtarı (önceki komut dosyası tarafından oluşturulan gibi) URI parametreleridir. Aşağıdaki dosya, bir parametre dosyası örneğidir:

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy.parameters.json dosyasını güncelleştirme

KeyVault URI, secretName, ortamınıza göre sanal makine değerlerin adminUsername ile azuredeploy.parameters.json dosyasını güncelleştirin. Aşağıdaki JSON dosyası şablon parametreleri dosyası örneği gösterilmektedir:

```json
{
    "$schema":  "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Şimdi aşağıdaki PowerShell betiğini kullanarak şablonu dağıtma:

```powershell
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablonu başarıyla dağıtıldığında, şunlara sebep olur:

![Dağıtım çıktı](media/azure-stack-kv-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar kasası ile örnek bir uygulama dağıtma](azure-stack-kv-sample-app.md)

[Anahtar kasası sertifikayla bir VM'yi dağıtmak](azure-stack-kv-push-secret-into-vm.md)
