---
title: "Azure yığında güvenli şekilde depolanan bir sertifika ile bir sanal makine dağıtma | Microsoft Docs"
description: "Bir sanal makine dağıtma ve Azure yığınında bir anahtar kasası kullanarak anında iletme sertifikası üzerine hakkında bilgi edinin"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/03/2017
ms.author: mabrigg
ms.openlocfilehash: e319f5c6d27d3a223764b0a5593480f02864ddbe
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-virtual-machine-and-include-certificate-retrieved-from-a-key-vault"></a>Bir sanal makine oluşturun ve bir anahtar Kasası'nı alınan sertifika içerir

Bu makalede, Azure yığını ve anında iletme sertifikaların bir sanal makine oluşturmak için yardımcı olur. 

## <a name="prerequisites"></a>Ön koşullar

* Anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız. 
* [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)  
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)

Bir anahtar kasası Azure yığınında sertifikaları depolamak için kullanılır. Sertifikalar birçok farklı senaryolarda yardımcı olur. Örneğin, bir sanal makine bir sertifika gerektiren bir uygulama çalıştıran Azure yığına sahip olduğu bir senaryo düşünün. Bu sertifika için şifreleme, Active Directory ile kimlik doğrulaması için ya da SSL için bir Web sitesinde kullanılabilir. Bir anahtar kasası yardımcı sertifikada emin olun sahip olan güvenlidir.

Bu makalede, sizi, bir Windows sanal makine sertifikası Azure yığınında göndermek için gereken adımlarda size yol. VPN üzerinden bağlandığı sırada Azure yığın Geliştirme Setinden veya Windows tabanlı bir dış istemci bu adımları kullanabilirsiniz.

Aşağıdaki adımlarda, bir sanal makine sertifikası göndermek için gereken işlemi açıklanmaktadır:

1. Gizli bir anahtar kasası oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablonu dağıtma

## <a name="create-a-key-vault-secret"></a>Gizli bir anahtar kasası oluşturma

Aşağıdaki komut dosyası .pfx biçiminde bir sertifika oluşturur, bir anahtar kasası oluşturur ve sertifika anahtar kasası gizli olarak depolar. Kullanmalısınız `-EnabledForDeployment` anahtar kasası oluşturulurken parametre. Bu parametre, anahtar kasası Azure Resource Manager şablonlarını başvurulabilir emin olur.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Önceki komut dosyasını çalıştırdığınızda, çıktı gizli URI içerir. Bu URI not edin. İçinde başvurmak zorunda [Windows Resource Manager şablonu itme sertifikaya](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Karşıdan [itme sertifika windows vm şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) geliştirme bilgisayarınıza klasör. Bu klasörde `azuredeploy.json` ve `azuredeploy.parameters.json` sonraki adımlarda gerekir dosyaları.

Değiştirme `azuredeploy.parameters.json` ortamı değerlerinizi göre dosya. Özel ilgi kasa adını, kasa kaynak grubu ve gizli anahtarı (önceki komut dosyası tarafından oluşturulan gibi) URI parametreleridir. Aşağıdaki dosya, bir parametre dosyası örneğidir:

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy.parameters.json dosyasını güncelleştirme

VaultName, gizli URI, VmName ve diğer değerleri ortamınıza uygun şekilde azuredeploy.parameters.json dosyasını güncelleştirin. Aşağıdaki JSON dosyası şablon parametreleri dosyası örneği gösterilmektedir: 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şimdi aşağıdaki PowerShell betiğini kullanarak şablonu dağıtma:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablonu başarıyla dağıtıldığında, şunlara sebep olur:

![Dağıtım çıktı](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Bu sanal makine dağıtılırken sanal makine sertifikası Azure yığın iter. Windows, LocalMachine sertifika konumla kullanıcı tarafından sağlanan sertifika deposuna sertifika eklenir. Linux sertifika dosya adı ile /var/lib/waagent dizinde altına yerleştirilmiş &lt;UppercaseThumbprint&gt;X509 .crt Sertifika dosyası ve &lt;UppercaseThumbprint&gt;.prv özel anahtar için .

## <a name="retire-certificates"></a>Sertifikaları devre dışı bırakma

Önceki bölümde, biz, bir sanal makine üzerine yeni bir sertifika göndermek nasıl oluşturulacağını gösterir. Eski sertifikanızı hala sanal makinede olması ve kaldırılamaz. Ancak, eski sürüm gizli kullanarak devre dışı bırakabilirsiniz `Set-AzureKeyVaultSecretAttribute` cmdlet'i. Bu cmdlet kullanım örneği verilmiştir. Kasa adı, gizli anahtar adı ve sürüm değerlerini ortamınıza göre değiştirdiğinizden emin olun:

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)
* [Anahtar kasası erişmek bir uygulama izin verme](azure-stack-kv-sample-app.md)


