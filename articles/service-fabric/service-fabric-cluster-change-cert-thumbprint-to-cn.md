---
title: Sertifika ortak adını kullanmak için Azure Service Fabric kümesi güncelleştirmesi | Microsoft Docs
description: Sertifika ortak adını kullanarak için sertifika parmak izleri kullanarak bir Service Fabric kümesine nasıl değiştireceğinizi öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 378f4609efcc9e9f0652b9ec68f5374b1fe3626a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959657"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Küme sertifikası parmak izini ortak ad olarak değiştirme
İki sertifika küme sertifika geçişi veya yönetim zorlaştırır aynı parmak olabilir. Ancak, aynı ortak adı veya konu birden çok sertifika sahip olabilir.  Sertifika ortak adları kullanarak sertifika parmak izleri kullanarak dağıtılan bir kümenin geçiş sertifika yönetimi çok daha kolay hale getirir. Bu makalede, sertifika ortak adına sertifika parmak izi yerine kullanmak için çalışan bir Service Fabric kümesinin güncelleştirileceğini açıklar.
 
## <a name="get-a-certificate"></a>Sertifika alma
İlk olarak, bir sertifika alın bir [sertifika yetkilisi (CA)](https://wikipedia.org/wiki/Certificate_authority).  Sertifikanın ortak adı, kümenin ana bilgisayar adı olmalıdır.  Örneğin, "myclustername.southcentralus.cloudapp.azure.com".  

Test amacıyla, ücretsiz veya açık bir sertifika yetkilisinden bir CA imzalı bir sertifika alabilir.

> [!NOTE]
> Azure portalında bir Service Fabric küme dağıtılırken oluşturulan dahil olmak üzere otomatik olarak imzalanan sertifikalar desteklenmiyor.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Sertifikayı karşıya yüklemek ve ölçek kümesindeki yükleyin
Azure'da bir sanal makine ölçek kümesi üzerinde bir Service Fabric kümesine dağıtılır.  Bir anahtar kasasına sertifikayı karşıya yüklemek ve kümeyi çalıştıran sanal makine ölçek kümesi yüklemelisiniz.

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
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName `
    -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName `
    -FilePath $certFilename -Password $PasswordSec

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
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault `
    -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Verbose `
    -Name $VmssName -VirtualMachineScaleSet $vmss 
```

## <a name="download-and-update-the-template-from-the-portal"></a>İndirin ve Portalı'ndan şablonunu güncelleştirme
Sertifika temel ölçek kümesi üzerinde yüklü, ancak Ayrıca, sertifika ve ortak adı kullanmak için Service Fabric küme güncelleştirmeniz gerekir.  Şimdi, küme dağıtım için şablonu indirin.  Oturum [Azure portalında](https://portal.azure.com) ve küme barındırma kaynak grubuna gidin.  İçinde **ayarları**seçin **dağıtımları**.  En son dağıtım seçip tıklayın **görünüm şablonu**.

![Görünüm şablonları][image1]

Şablon ve parametreleri JSON dosyaları yerel bilgisayarınıza indirin.

İlk olarak, parametre dosyasını bir metin düzenleyicisinde açın ve aşağıdaki parametreyi ekleyin:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Ardından, şablon dosyasını bir metin düzenleyicisinde açın ve sertifika ortak adına desteklemek için üç güncelleştirmeleri yapın.

1. İçinde **parametreleri** bölümünde, eklemek bir *certificateCommonName* parametresi:
    ```json
    "certificateCommonName": {
        "type": "string",
        "metadata": {
            "description": "Certificate Commonname"
        }
    },
    ```

    Ayrıca kaldırmayı düşünün *certificateThumbprint*, artık gerekli.

2. İçinde **Microsoft.Compute/virtualMachineScaleSets** kaynağın ortak adı yerine parmak izi sertifika ayarlarını kullanmak için sanal makine uzantısını güncelleştirin.  İçinde **virtualMachineProfile**->**extenstionProfile**->**uzantıları**->**özellikleri** -> **ayarları**->**sertifika**, ekleme `"commonNames": ["[parameters('certificateCommonName')]"],` kaldırıp `"thumbprint": "[parameters('certificateThumbprint')]",`.
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
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        "typeHandlerVersion": "1.0"
                    }
                },
    ```

3.  İçinde **Microsoft.ServiceFabric/clusters** kaynak, "2018-02-01" için güncelleştirme API sürümü.  Ayrıca bir **certificateCommonNames** ayarı bir **commonNames** özelliği ekleme ve kaldırma **sertifika** (parmak izi özelliğiyle) şu şekilde ayarlama Örnek:
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
Güncelleştirilmiş şablonu, değişiklikleri yaptıktan sonra yeniden dağıtın.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* Bilgi edinmek için nasıl [geçişi bir küme sertifikası](service-fabric-cluster-rollover-cert-cn.md)
* [Küme sertifikalarını yönetme ve güncelleştirme](service-fabric-cluster-security-update-certs-azure.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
