---
title: "Azure yığında güvenli şekilde depolanan parola ile bir VM'yi dağıtmak | Microsoft Docs"
description: "Azure yığın anahtar kasasında depolanan bir parola kullanarak bir VM'i dağıtmayı öğrenin"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2017
ms.author: sngun
ms.openlocfilehash: 3292a2dfefc17e5034c66122a3eab24d6c03e694
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-by-retrieving-the-password-stored-in-a-key-vault"></a>Bir anahtar kasasında depolanan parola alarak bir sanal makine oluşturun

Dağıtım sırasında bir parola gibi güvenli bir değerle geçirmek gerektiğinde, bir Azure yığın anahtar kasasına gizli olarak bu değer depolayın ve Azure Resource Manager şablonları başvurusu. Bunu gizli el ile girmek için kaynakları, dağıttığınız her zaman Ayrıca hangi kullanıcıların belirtebilirsiniz değil veya hizmet asıl adı gizli erişebilir. 

Bu makalede, sizi, anahtar kasasında depolanan parola alarak Azure yığınında Windows sanal makine dağıtmak için gereken adımlarda size yol. Bu nedenle parola düz metin şablonu parametre dosyası olarak hiçbir zaman yerleştirilir. VPN üzerinden bağlandığı sırada Azure yığın Geliştirme Setinden veya bir dış istemcinin bu adımları kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
 
* Anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız gerekir.  
* [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)  
* [Azure yığın kullanıcı PowerShell ortamının yapılandırın.](azure-stack-powershell-configure-user.md)

Aşağıdaki adımlar, bir anahtar kasasında depolanan parola alarak bir sanal makine oluşturmak için gereken işlem açıklamaktadır:

1. Gizli bir anahtar kasası oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablon dağıtın.

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

