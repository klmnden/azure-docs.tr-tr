---
title: Sertifika ortak adını kullanarak bir Azure Service Fabric kümesi oluşturma | Microsoft Docs
description: Bir şablondan sertifika ortak adını kullanarak bir Service Fabric kümesi oluşturmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: aljo
ms.openlocfilehash: bf28ddf7facbc742a107f67f3d7e81eca5a5c950
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394277"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Parmak izi yerine sertifika ortak adını kullanan bir Service Fabric kümesi dağıtma
İki sertifika küme sertifika geçişi veya yönetim zorlaştırır aynı parmak olabilir. Ancak, aynı ortak adı veya konu birden çok sertifika sahip olabilir.  Sertifika ortak adları kullanarak bir küme, sertifika yönetimi çok daha kolay hale getirir. Bu makalede, sertifika ortak adına sertifika parmak izi yerine kullanılacak bir Service Fabric kümesi dağıtmayı açıklar.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-a-certificate"></a>Sertifika alma
İlk olarak, bir sertifika alın bir [sertifika yetkilisi (CA)](https://wikipedia.org/wiki/Certificate_authority).  Sertifikanın ortak adı için özel etki alanı kendi ve bir etki alanı kayıt şirketi'ndan satın olmalıdır. Örneğin, "azureservicefabricbestpractices.com"; Microsoft çalışanları olmayan kişilerin MS etki alanlarına yönelik sertifikaları sertifikanız için yaygın olarak kullanılan adları olarak LB veya Traffic Manager DNS adlarını kullanamazsınız ve sağlamak için ihtiyacınız olacak şekilde sağlayabilirsiniz değil bir [Azure DNS bölgesi](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns) , kendi özel Azure'da çözülebilir olması için etki alanı. Ayrıca, diğer kümeniz için özel etki alanını yansıtacak şekilde portalı istiyorsanız, kümenin "managementEndpoint" kendi özel etki alanınızı bildirmek isteyeceksiniz.

Test amacıyla, ücretsiz veya açık bir sertifika yetkilisinden bir CA imzalı bir sertifika alabilir.

> [!NOTE]
> Azure portalında bir Service Fabric küme dağıtılırken oluşturulan dahil olmak üzere otomatik olarak imzalanan sertifikalar desteklenmiyor. 

## <a name="upload-the-certificate-to-a-key-vault"></a>Bir anahtar kasasına sertifika yükleme
Azure'da bir sanal makine ölçek kümesi üzerinde bir Service Fabric kümesine dağıtılır.  Sertifikayı bir anahtar Kasası'na yükleyin.  Kümeye dağıttığında, sertifikayı kümesi çalıştıran sanal makine ölçek kümesinde yükler.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
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

3. İçinde **Microsoft.Compute/virtualMachineScaleSets** kaynağın ortak adı yerine parmak izi sertifika ayarlarını kullanmak için sanal makine uzantısını güncelleştirin.  İçinde **virtualMachineProfile**->**extensionprofile öğesine**->**uzantıları**->**özellikleri** -> **ayarları**->**sertifika**, Ekle 
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

4. İçinde **Microsoft.ServiceFabric/clusters** kaynak, "2018-02-01" için güncelleştirme API sürümü.  Ayrıca bir **certificateCommonNames** ayarı bir **commonNames** özelliği ekleme ve kaldırma **sertifika** (parmak izi özelliğiyle) şu şekilde ayarlama Örnek:
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
               "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
           }
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
       },
       ...
   ```
   > [!NOTE]
   > 'CertificateIssuerThumbprint' alanı, özel olarak beklenen verenler sertifikalarının bir belirtilen konu ortak adı ile belirtmeye izin verir. Bu alan, SHA1 parmak izleri virgülle ayrılmış bir numaralandırmasını kabul eder. Bu sertifika doğrulama - veren belirtilmemiş veya boş, sertifika, zincir oluşturulabilir, kimlik doğrulaması ve doğrulayıcı tarafından güvenilen bir kök yukarı biter kabul edilecek söz konusu güçlendirme olduğuna dikkat edin. Veren belirtilirse, sertifika parmak izini doğrudan verenini kök güvenilir olup olmamasına bakılmaksızın bu alana - belirtilen değerlerden herhangi birini eşleşiyorsa kabul edilecektir. Lütfen bir PKI sertifikaları için aynı konu için farklı sertifika yetkilileri kullanabilir ve bu nedenle tüm beklenen verenin parmak izleri belirtmek için belirli bir konu önemli olduğunu unutmayın.
   >
   > Verici belirtme, en iyi uygulama olarak kabul edilir; atlama sırasında çalışmaya devam edecektir - sertifikaları Güvenilen bir köke - zincirleme ayarlamak için bu davranış sınırlamalara sahiptir ve yakın gelecekte aşamalı olacak. Ayrıca kümeleri Azure'da dağıtılır ve X509 ile güvenliği sağlanan Not konu tarafından bildirilen ve özel bir PKI tarafından verilen sertifikaları açamayabilir (için Küme hizmeti iletişimi için), Azure Service Fabric hizmeti tarafından doğrulanmış olarak, PKI'ın sertifika ilkesi bulunabilir, kullanılabilir ve erişilebilir değil. 

## <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Güncelleştirilmiş şablonu, değişiklikleri yaptıktan sonra yeniden dağıtın.

```powershell
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzResourceGroup -Name $groupname -Location $clusterloc

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* Bilgi edinmek için nasıl [geçişi bir küme sertifikası](service-fabric-cluster-rollover-cert-cn.md)
* [Küme sertifikalarını yönetme ve güncelleştirme](service-fabric-cluster-security-update-certs-azure.md)
* Sertifika yönetimi basitleştirmek [değiştirme kümeden sertifika parmak izi için ortak ad](service-fabric-cluster-change-cert-thumbprint-to-cn.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
ic-Cluster-Change-CERT-thumbprint-to-CN.MD))

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
