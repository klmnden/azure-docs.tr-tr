---
title: Azure Stack'te güvenli şekilde depolanan bir sertifika ile bir sanal makine dağıtma | Microsoft Docs
description: Bir sanal makine dağıtma ve Azure Stack'te bir anahtar kasası kullanarak bir sertifika üzerine gönderme hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 52c66dbf5001b3c49ac959276e59d8e794955193
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57760130"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Sanal makine oluşturma ve Azure Stack anahtar kasasından alınan bir sertifika yükleyin

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack sanal makine'de (VM) oluşturma, bir anahtar kasası sertifikası ile bilgi edinin.

## <a name="overview"></a>Genel Bakış

Active Directory kimlik doğrulaması veya web trafiği şifreleme gibi birçok senaryoda sertifikalar kullanılır. Azure Stack anahtar kasasındaki gizli diziler olarak sertifikaları güvenli bir şekilde depolayabilirsiniz. Azure Stack anahtar kasası kullanmanın avantajları şunlardır:

* Sertifikalar, bir komut dosyası, komut satırı geçmişinde veya şablonda gösterilmez.
* Sertifika yönetimi işlemini kolaylaştırılmış hale getirilir.
* Sertifikalara erişen anahtarları denetiminizde var.

### <a name="process-description"></a>İşlem açıklaması

Aşağıdaki adımlar, sanal makine için bir sertifika göndermek için gerekli işlemi açıklanmaktadır:

1. Bir Key Vault gizli anahtar oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablonu dağıtın.

> [!NOTE]
> VPN birbirine bağlandıysa, dış istemciden veya Azure Stack geliştirme Seti'ni bu adımları kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Key Vault hizmetini içeren bir teklife abone olması gerekir.
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
* [Azure Stack kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

## <a name="create-a-key-vault-secret"></a>Bir Key Vault gizli dizisi oluşturma

Aşağıdaki betiği .pfx biçiminde bir sertifika oluşturur, bir anahtar kasası oluşturulur ve sertifika anahtar kasasında gizli dizi olarak depolar.

> [!IMPORTANT]
> Kullanmalısınız `-EnabledForDeployment` anahtar kasası oluşturma sırasında parametre. Bu parametre, anahtar kasası Azure Resource Manager şablonlarından başvurulabilir sağlar.

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

Önceki betiği çalıştırdığınızda, çıktı gizli anahtar URI'sini içerir. Bu URI not edin. İçinde başvurmak zorunda [Windows Resource Manager şablonu için anında iletme sertifikası](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). İndirme [anında iletme sertifikası windows vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) geliştirme bilgisayarınıza şablon klasörü. Bu klasörde `azuredeploy.json` ve `azuredeploy.parameters.json` dosyaları, sonraki adımlarda gerekecektir.

Değiştirme `azuredeploy.parameters.json` ortam değerlerinize göre dosya. Özel ilgi kasa adını, kasa kaynak grubu ve gizli dizi (önceki betiği tarafından oluşturulan gibi) URI parametrelerdir. Aşağıdaki bölümde, bir parametre dosyası örneği gösterilmektedir.

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy.parameters.json dosyasını güncelleştirme

Güncelleştirme `azuredeploy.parameters.json` ile dosya `vaultName`, gizli URI `VmName`ve diğer değerleri, ortamınızı göre. Şablon parametreleri dosyası örneği aşağıdaki JSON dosyası gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Aşağıdaki PowerShell betiğini kullanarak şablonu dağıtın:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablon başarıyla dağıtıldıktan sonra aşağıdaki çıktı sonuçları:

![Şablon dağıtım sonuçları](media/azure-stack-key-vault-push-secret-into-vm/deployment-output.png)

Azure Stack sertifika dağıtımı sırasında sanal makineye gönderir. Sertifika konumu sanal makinenin işletim sistemine bağlıdır:

* Sertifika eklenir Windows içinde **LocalMachine** sertifika konumu, kullanıcı tarafından sağlanan sertifika deposuna sahip.
* Linux sertifika altına yerleştirilir `/var/lib/waagent directory`, dosya adına sahip &lt;UppercaseThumbprint&gt;.crt X509 için sertifika dosyası ve &lt;UppercaseThumbprint&gt;.prv özel anahtar için.

## <a name="retire-certificates"></a>Sertifikaları devre dışı bırakma

Sertifikaları devre dışı bırakma sertifika yönetimi işleminin parçasıdır. Bir sertifika daha eski sürümünü silinemez, ancak kullanarak devre dışı bırakabilirsiniz `Set-AzureKeyVaultSecretAttribute` cmdlet'i.

Aşağıdaki örnek, bir sertifika devre dışı bırakmak gösterilmektedir. İçin kendi değerlerinizi kullanın **VaultName**, **adı**, ve **sürüm** parametreleri.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault'a erişmesi uygulamaya izin ver](azure-stack-key-vault-sample-app.md)
