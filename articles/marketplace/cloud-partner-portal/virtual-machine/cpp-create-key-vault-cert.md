---
title: Bir Azure Key Vault sertifikası oluşturun | Microsoft Docs
description: Azure ile dağıtılan bir VHD'den VM kaydedileceği açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: a25418f30225184424011527def468d0d3909563
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045705"
---
# <a name="create-certificates-for-azure-key-vault"></a>Azure anahtar kasası için sertifikaları oluşturma

Bu makalede, Azure'da barındırılan sanal makine'de (VM) bir Windows Uzaktan Yönetim (WinRM) bağlantı kurmak için gereken otomatik olarak imzalanan sertifikalar açıklanmaktadır. Bu işlemi üç adımdan oluşur:

1.  Güvenlik sertifikası oluşturun. 
2.  Bu sertifika deposu için Azure Key Vault oluşturacaksınız. 
3.  Bu anahtar kasasına sertifikaları Store. 

Bu iş için yeni veya mevcut bir Azure kaynak grubunu kullanabilirsiniz.  Eski yaklaşım, aşağıdaki açıklamada kullanılır.



[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="create-the-certificate"></a>Sertifika oluşturma

Düzenleme ve yerel bir klasörde sertifika dosyasını (.pfx) oluşturmak için aşağıdaki Azure Powershell betiği çalıştırın.  Aşağıdaki parametreler için değerleri değiştirmeniz gerekir:

|  **Parametre**        |   **Açıklama**                                                               |
|  -------------        |   ---------------                                                               |
| `$certroopath` | .Pfx dosyasına kaydetmek için yerel klasör  |
| `$location`    | Azure standart coğrafi konumlardan birine  |
| `$vmName`      | Planlı Azure sanal makine adı   |
| `$certname`    | Sertifika adı; Planlanan bir VM tam etki alanı adı ile eşleşmelidir  |
| `$certpassword` | Sertifikalar için parola planlanan bir VM için kullanılan parola aynı olmalıdır  |
|  |  |

```powershell
    # Certification creation script 
    
    # pfx certification stored path
    $certroopath = "C:\certLocation"
    
    #location of the resource group
    $location = "westus"
    
    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"
    
    # Certification name - should match with FQDN of Windows Azure creating VM 
    $certname = "$vmName.$location.cloudapp.azure.com"
    
    # Certification password - should be match with password of Windows Azure creating VM 
    $certpassword = "SecretPassword@123"
    
    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb 

```
> [!TIP]
> Böylece, çeşitli parametrelerin değerleri korunur aynı PowerShell konsol oturumuna adımları sırasında etkin tutabilirsiniz.

> [!WARNING]
> Bu komut dosyasını kaydederseniz, güvenlik bilgilerini (parola) içerdiği için yalnızca güvenli bir konumda depolayın.


## <a name="create-the-key-vault"></a>Anahtar kasası oluşturma

İçeriğini kopyalayın [anahtar kasası dağıtım şablonu](./cpp-key-vault-deploy-template.md) yerel makinenizde bir dosyaya. (Aşağıdaki örnek komut dosyasında bu kaynaktır `C:\certLocation\keyvault.json`.)  Düzenleyin ve bir Azure Key Vault örneği ile ilişkili kaynak grubu oluşturmak için aşağıdaki Azure Powershell betiğini çalıştırın.  Aşağıdaki parametreler için değerleri değiştirmeniz gerekir:

|  **Parametre**        |   **Açıklama**                                                               |
|  -------------        |   ---------------                                                               |
| `$postfix`            | Dağıtım tanımlayıcıları için isteğe bağlı bir sayısal dize                     |
| `$rgName`             | Azure kaynak grubu (RG) adı oluşturmak için                                        |
|  `$location`          | Azure standart coğrafi konumlardan birine                                  |
| `$kvTemplateJson`     | Anahtar kasası Resource Manager şablonu içeren dosyanın (keyvault.json) yolu |
| `$kvname`             | Yeni key vault adı                                                       |
|  |  |

```powershell
    # Creating Key vault in resource group
    
    # "Random" number for deployment identifiers
    $postfix = "0101048"
    
    # Resource group name
    $rgName = "TestRG$postfix"
    
    # Location of Resource Group
    $location = "westus"
    
    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"
    
    # Key vault name
    $kvname = "isvkv$postfix"
    
    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzureAccount
        $accountNum = 0
        $accounts.Id | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host 
        
        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }
        
        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Id
                              
        Write-Host "User $id Selected"
        $myobjectId=(Get-AzADUser -Mail $id)[0].Id
      }
      catch
      {
      Write-Host $_.Exception.Message    
      Break
      } 

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzureSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host
    
        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].SubscriptionId
        $mysubName=$subslist[$selectedsub-1].SubscriptionName
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message    
        Break
        }
    
    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     Create-ResourceGroup -rgName $rgName -location $location
     Write-Host "-----------------------------------" 
    
    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId
    
    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys all -PermissionsToSecrets all 
        
```

## <a name="store-the-certificate"></a>Sertifika Store

Artık aşağıdaki betiği çalıştırarak, yeni anahtar kasasına .pfx dosyasında yer alan sertifikaları depolayabilirsiniz. 

```powershell
    #push certificate to key vault secret

     $fileName =$certroopath+"\$certname"+".pfx" 
      
     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    
            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
    "@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
            $objAzureKeyVaultSecret=Set-AzureKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id 
    
```


## <a name="next-steps"></a>Sonraki adımlar

Ardından şunları yapacaksınız [kullanıcı VM görüntüsünden VM dağıtma](./cpp-deploy-vm-user-image.md).
