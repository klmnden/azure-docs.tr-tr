---
title: Sertifika ortak adını kullanarak bir Azure Service Fabric kümesi oluşturma | Microsoft Docs
description: Bir şablondan sertifika ortak adını kullanarak bir Service Fabric kümesi oluşturmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: c4c60cccb890c883e9e57c9f146cc93aae99f224
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060949"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Parmak izi yerine sertifika ortak adını kullanan bir Service Fabric kümesi dağıtma
İki sertifika küme sertifika geçişi veya yönetim zorlaştırır aynı parmak olabilir. Ancak, aynı ortak adı veya konu birden çok sertifika sahip olabilir.  Sertifika ortak adları kullanarak bir küme, sertifika yönetimi çok daha kolay hale getirir. Bu makalede, sertifika ortak adına sertifika parmak izi yerine kullanılacak bir Service Fabric kümesi dağıtmayı açıklar.
 
## <a name="get-a-certificate"></a>Sertifika alma
İlk olarak, bir sertifika alın bir [sertifika yetkilisi (CA)](https://wikipedia.org/wiki/Certificate_authority).  Sertifikanın ortak adı, kümenin ana bilgisayar adı olmalıdır.  Örneğin, "myclustername.southcentralus.cloudapp.azure.com".  

Test amacıyla, ücretsiz veya açık bir sertifika yetkilisinden bir CA imzalı bir sertifika alabilir.

> [!NOTE]
> Azure portalında bir Service Fabric küme dağıtılırken oluşturulan dahil olmak üzere otomatik olarak imzalanan sertifikalar desteklenmiyor.

## <a name="upload-the-certificate-to-a-key-vault"></a>Bir anahtar kasasına sertifika yükleme
Azure'da bir sanal makine ölçek kümesi üzerinde bir Service Fabric kümesine dağıtılır.  Sertifikayı bir anahtar Kasası'na yükleyin.  Kümeye dağıttığında, sertifikayı kümesi çalıştıran sanal makine ölçek kümesinde yükler.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

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
```

## <a name="download-and-update-a-sample-template"></a>İndirin ve bir örnek şablonu güncelleştirin
Bu makalede [güvenli kümesi 5 düğümlü örnek](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) şablon ve şablon parametreleri. İndirme *azuredeploy.json* ve *azuredeploy.parameters.json* bilgisayarınıza dosyaları.

### <a name="update-parameters-file"></a>Parametreler dosyasını güncelleştirme
İlk olarak, açık *azuredeploy.parameters.json* dosyasını bir metin düzenleyicisinde ve aşağıdaki parametreyi ekleyin:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Ardından, ayarlama *certificateCommonName*, *sourceVaultValue*, ve *certificateUrlValue* parametre değerlerini bu önceki komut dosyası tarafından döndürülen:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"sourceVaultValue": {
  "value": "/subscriptions/<subscription>/resourceGroups/testvaultgroup/providers/Microsoft.KeyVault/vaults/testvault"
},
"certificateUrlValue": {
  "value": "https://testvault.vault.azure.net:443/secrets/testcert/5c882b7192224447bbaecd5a46962655"
},
```

### <a name="update-the-template-file"></a>Şablon dosyasını güncelleştirme
Ardından, açık *azuredeploy.json* dosyasını bir metin düzenleyicisinde ve sertifika ortak adına desteklemek için üç güncelleştirmeleri yapın.

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

2. Değerini *sfrpApiVersion* "2018-02-01" değişkenini:
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. İçinde **Microsoft.Compute/virtualMachineScaleSets** kaynağın ortak adı yerine parmak izi sertifika ayarlarını kullanmak için sanal makine uzantısını güncelleştirin.  İçinde **virtualMachineProfile**->**extenstionProfile**->**uzantıları**->**özellikleri** -> **ayarları**->**sertifika**, Ekle 
    ```json
       "commonNames": [
        "[parameters('certificateCommonName')]"
       ],
    ```

    kaldırıp `"thumbprint": "[parameters('certificateThumbprint')]",`.

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

4.  İçinde **Microsoft.ServiceFabric/clusters** kaynak, "2018-02-01" için güncelleştirme API sürümü.  Ayrıca bir **certificateCommonNames** ayarı bir **commonNames** özelliği ekleme ve kaldırma **sertifika** (parmak izi özelliğiyle) şu şekilde ayarlama Örnek:
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
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* Bilgi edinmek için nasıl [geçişi bir küme sertifikası](service-fabric-cluster-rollover-cert-cn.md)
* [Küme sertifikalarını yönetme ve güncelleştirme](service-fabric-cluster-security-update-certs-azure.md)
* Sertifika yönetimi basitleştirmek [değiştirme kümeden sertifika parmak izi için ortak ad](service-fabric-cluster-change-cert-thumbprint-to-cn.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
