---
title: Sertifika ortak adı kullanmak için bir Azure Service Fabric kümesi güncelleştirme | Microsoft Docs
description: Sertifika ortak adı kullanarak sertifika parmak izlerini kullanarak bir Service Fabric kümesi geçiş öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi;aljo
ms.openlocfilehash: a2b0d49e9fc81837db5c53bc9e52f3a0649b0b00
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Sertifika parmak izi değişiklik kümeden ortak adı
İki sertifika küme sertifika geçişine veya yönetim zorlaştırır aynı parmak olabilir. Birden çok sertifika, ancak aynı ortak adı veya konu sahip olabilir.  Sertifika ortak adları kullanarak sertifika parmak izlerini kullanarak dağıtılan bir küme geçişi sertifika yönetimini daha kolay hale getirir. Bu makalede, sertifikanın ortak adı yerine sertifika parmak izi kullanmak için çalışan bir Service Fabric kümesi güncelleştirme açıklar.
 
## <a name="get-a-certificate"></a>Sertifika alma
İlk olarak, bir sertifika alın bir [sertifika yetkilisi (CA)](https://wikipedia.org/wiki/Certificate_authority).  Sertifikanın ortak adı, küme ana bilgisayar adı olmalıdır.  Örneğin, "myclustername.southcentralus.cloudapp.azure.com".  

Test amacıyla, boş veya açık bir sertifika yetkilisinden CA imzalı bir sertifika alabilir.

> [!NOTE]
> Azure portalında bir Service Fabric kümesi dağıtırken oluşturulan dahil olmak üzere otomatik olarak imzalanan sertifikalar desteklenmez.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Sertifikayı karşıya yüklemek ve ölçek kümesinde yükleyin
Azure üzerinde bir sanal makine ölçek kümesi üzerinde bir Service Fabric kümesi dağıtılır.  Bir anahtar kasası ve sertifikayı karşıya yüklemek ve küme çalışan sanal makine ölçek kümesini yükleyin.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgropu"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

## <a name="download-and-update-the-template-from-the-portal"></a>Karşıdan yükle ve portal şablonu güncelleştirme
Sertifika temel ölçek kümesinde yüklendi, ancak Ayrıca, sertifika ve ortak adı kullanmak için Service Fabric kümesi güncelleştirmeniz gerekir.  Şimdi, Küme dağıtımı için şablonu indirin.  Oturum [Azure portal](https://portal.azure.com) ve küme barındırma kaynak grubuna gidin.  İçinde **ayarları**seçin **dağıtımları**.  En son dağıtım tıklatıp **şablonu görüntüleme**.

![Görünüm şablonları][image1]

Şablonu ve parametre JSON dosyalarını yerel bilgisayarınıza indirin.

İlk olarak, parametreleri dosyasını bir metin düzenleyicisinde açın ve aşağıdaki parametre değerini ekleyin:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Ardından, şablon dosyasını bir metin düzenleyicisinde açın ve sertifika ortak adı desteklemek için üç güncelleştirmeler yapabilir.

1. İçinde **parametreleri** bölümünde bir *certificateCommonName* parametre:
    ```json
    "certificateCommonName": {
      "type": "string",
      "metadata": {
        "description": "Certificate Commonname"
      }
    },
    ```

    Ayrıca kaldırmayı düşünün *certificateThumbprint*, artık gerekli.

2. İçinde **Microsoft.Compute/virtualMachineScaleSets** kaynak, sertifika parmak izini ayarlarında ortak adı kullanmak için sanal makine uzantısını güncelleştirin.  İçinde **virtualMachineProfile**->**extenstionProfile**->**uzantıları**->**özellikleri** -> **ayarları**->**sertifika**, ekleme `"commonNames": ["[parameters('certificateCommonName')]"],` kaldırıp `"thumbprint": "[parameters('certificateThumbprint')]",`.
    ```json
    "virtualMachineProfile": {
              "extensionProfile": {
                "extensions": [
                  {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": true,
                      "protectedSettings": {
                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                      },
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[variables('vmNodeType0Name')]",
                        "dataPath": "D:\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "enableParallelJobs": true,
                        "nicPrefixOverride": "[variables('subnet0Prefix')]",
                        "certificate": {
                          "commonNames": ["[parameters('certificateCommonName')]"],                          
                          "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
                      "typeHandlerVersion": "1.0"
                    }
                  },
    ```

3.  İçinde **Microsoft.ServiceFabric/clusters** kaynak, güncelleştirme API'sı sürüm "2018-02-01" olarak.  Ayrıca bir **certificateCommonNames** ayarı bir **commonNames** özellik ekleme ve kaldırma **sertifika** (parmak izi özelliğiyle) aşağıdaki gibi ayar Örnek:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
        "addonFeatures": [
            "DnsService",
            "RepairManager"
        ],        
        "certificateCommonNames": {
            "commonNames": [
            {
                "certificateCommonName": "[parameters('certificateCommonName')]",
                "certificateIssuerThumbprint": ""
            }
            ],
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },
        ...
    ```

## <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Güncelleştirilmiş şablonu değişiklikleri yaptıktan sonra yeniden dağıtın.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"

New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* Bilgi edinmek için nasıl [rollover küme sertifika](service-fabric-cluster-rollover-cert-cn.md)
* [Güncelleştirme ve küme sertifikaları yönetme](service-fabric-cluster-security-update-certs-azure.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png