---
title: Bir Azure sanal makinesi için WinRM erişimi ayarlama | Microsoft Docs
description: WinRM erişimi kullanmak için bir Azure Resource Manager dağıtım modelinde oluşturulan sanal makine ile ayarlayın.
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
ms.openlocfilehash: c4df3d6a55021cafa04bb6bcba643be41dc0e612
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273763"
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Azure Resource Manager'da sanal makineler için WinRM erişimi ayarlama

WinRM bağlantı ile bir VM'yi ayarlamak için atmanız gereken adımlar aşağıda verilmiştir

1. Anahtar kasası oluşturma
2. Otomatik olarak imzalanan sertifika oluşturma
3. Anahtar Kasası'na otomatik olarak imzalanan sertifikanızı karşıya yükleme
4. Anahtar Kasası'nda, otomatik olarak imzalanan sertifika URL'sini alma
5. Bir VM oluşturulurken otomatik olarak imzalanan sertifikalar URL'nizi başvurusu

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="step-1-create-a-key-vault"></a>1\. adım: Anahtar kasası oluşturma
Kullanabileceğiniz aşağıdaki anahtar kasası oluşturma komutu

```
New-AzKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>2\. adım: Otomatik olarak imzalanan sertifika oluşturma
Bu PowerShell Betiği kullanılarak otomatik olarak imzalanan bir sertifika oluşturabilir.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>3\. adım: Anahtar Kasası'na otomatik olarak imzalanan sertifikanızı karşıya yükleme
1\. adımda oluşturulan Key vault'a sertifika yüklemeden önce dönüştürülen Microsoft.Compute kaynak sağlayıcısına anlayabileceği bir biçime gerekir. Aşağıdaki PowerShell Betiği, bunu izin verir

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

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>4\. Adım: Anahtar Kasası'nda, otomatik olarak imzalanan sertifika URL'sini alma
Microsoft.Compute kaynak sağlayıcısına VM sağlarken içinde Key Vault'a gizli dizi için bir URL gerekiyor. Bu, gizli diziyi indirmek ve VM'de eşdeğer sertifika oluşturmak Microsoft.Compute kaynak sağlayıcısına sağlar.

> [!NOTE]
> Gizli dizi URL'sini sürümü de dahil etmek gerekir. URL https gibi görünen bir örnek:\//contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve

#### <a name="templates"></a>Şablonlar
Şablon kullanarak URL bağlantısı alabilirsiniz kodun altına

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Bu URL'yi kullanarak alabileceğiniz aşağıdaki PowerShell komutu

    $secretURL = (Get-AzKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>5\. Adım: Bir VM oluşturulurken otomatik olarak imzalanan sertifikalar URL'nizi başvurusu
#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Şablonlar aracılığıyla bir VM oluştururken, sertifika gizli anahtarları ve winRM bölümleri aşağıda gösterildiği gibi başvurulan:

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

Yukarıdaki için bir örnek şablonu, burada bulunabilir [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Bu şablon için kaynak kodu bulunabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>6\. Adım: Sanal Makineye bağlanma
Emin olmak için gerekir VM bağlanmadan önce makinenizi WinRM uzaktan yönetim için yapılandırılmış. Yönetici olarak PowerShell'i başlatın ve yürütme emin olmak için aşağıdaki komutu, ayarlamış.

    Enable-PSRemoting -Force

> [!NOTE]
> Yukarıdaki işe yaramazsa WinRM hizmetinin çalıştığından emin olmak gerekebilir. Bu kullanarak yapabilirsiniz. `Get-Service WinRM`
> 
> 

Kurulum tamamlandıktan sonra VM kullanarak bağlanabilirsiniz aşağıdaki komutu

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
