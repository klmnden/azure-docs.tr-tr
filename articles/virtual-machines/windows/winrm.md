---
title: Bir Azure VM için WinRM erişimini Ayarla | Microsoft Docs
description: Resource Manager dağıtım modelinde oluşturulmuş bir Azure sanal makinesi ile kullanım için WinRM erişim ayarlayın.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: kasing
ms.openlocfilehash: 5fa82dd4a85ff2e62848df0fdc6006922005a84b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30914554"
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Sanal makineler Azure Kaynak Yöneticisi'nde için WinRM erişimi ayarlama
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>Azure Resource Manager Azure Hizmet Yönetimi vs'de WinRM

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* Bir Azure Kaynak Yöneticisi'nin için lütfen bu bkz [makale](../../azure-resource-manager/resource-group-overview.md)
* Azure Hizmet Yönetimi ve Azure Resource Manager arasındaki farklar için lütfen bu bakın [makale](../../resource-manager-deployment-model.md)

İki yığınları arasında WinRM yapılandırması ayarlanması anahtar sertifika VM nasıl yüklendiğini farktır. Sertifikalar, anahtar kasası kaynak sağlayıcısı tarafından yönetilen kaynaklar olarak Azure Kaynak Yöneticisi yığınında modellenir. Bu nedenle, kullanıcı kendi sertifikayı sağlayın ve bu bir anahtar Kasası'na VM ile kullanmadan önce karşıya yüklemesi gerekiyor.

WinRM bağlantısına sahip bir VM'yi ayarlamak için atmanız gereken adımlar şunlardır

1. Anahtar kasası oluşturma
2. Otomatik olarak imzalanan sertifika oluşturma
3. Anahtar Kasası'na otomatik olarak imzalanan sertifikanızın karşıya yükle
4. Otomatik olarak imzalanan sertifikanızın anahtar kasasında URL'sini alma
5. Bir VM oluşturulurken otomatik olarak imzalanan sertifikalar URL'nizi başvurusu

## <a name="step-1-create-a-key-vault"></a>1. adım: bir anahtar kasası oluşturma
Kullanabileceğiniz anahtar kasası oluşturmak için komutu aşağıdaki

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>2. adım: otomatik olarak imzalanan sertifika oluşturma
Bu PowerShell Betiği kullanılarak otomatik olarak imzalanan bir sertifika oluşturabilir.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>3. adım: anahtar Kasası'na otomatik olarak imzalanan sertifikanızın karşıya yükle
1. adımda oluşturduğunuz anahtar kasası için sertifika karşıya önce onu dönüştürülen Microsoft.Compute kaynak sağlayıcısındaki anlayabileceği bir biçime gerekir. Aşağıdaki PowerShell komut dosyası, bunu izin verir

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>4. adım: otomatik olarak imzalanan sertifikanızın anahtar kasasında URL'sini alma
Microsoft.Compute kaynak sağlayıcısındaki VM sağlarken gizli anahtar kasası içinde bir URL gerekir. Bu, gizli anahtarı indirin ve eşdeğer sertifika VM oluşturmak Microsoft.Compute kaynak sağlayıcısındaki sağlar.

> [!NOTE]
> Gizli URL'sini sürümü de içermesi gerekir. Aşağıdaki gibi bir örnek URL'si görünüyor https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve
> 
> 

#### <a name="templates"></a>Şablonlar
Şablon kullanarak URL bağlantısını alma kodu aşağıda

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Bu URL'yi kullanarak elde edebilirsiniz PowerShell komutunu aşağıda

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>5. adım: bir VM oluşturulurken otomatik olarak imzalanan sertifikalar URL'nizi başvuru
#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
VM şablonları aracılığıyla oluşturulurken, sertifika gizli bölümü ve aşağıdaki şekilde winRM bölümünde başvurulan:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Yukarıdaki için bir örnek şablon burada bulunabilir [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Bu şablon için kaynak kodunu bulunabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>6. adım: VM'ye bağlanma
Emin olmak için gerekir VM bağlanmadan önce makinenizde WinRM uzaktan yönetim için yapılandırılmış. Yönetici olarak PowerShell'i başlatın ve yürütme emin olmak için komut, ayarlamış.

    Enable-PSRemoting -Force

> [!NOTE]
> Yukarıdaki işe yaramazsa WinRM hizmetinin çalıştığından emin olmak gerekebilir. Bu kullanarak yapabilirsiniz `Get-Service WinRM`
> 
> 

Kurulum tamamlandığında, VM kullanmaya bağlanabilir komutu aşağıda

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
