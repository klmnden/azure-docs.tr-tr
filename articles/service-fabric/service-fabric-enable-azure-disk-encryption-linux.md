---
title: Disk şifrelemesi service fabric Linux kümeleri için etkinleştirme | Microsoft Docs
description: Bu makale, Service Fabric kümesi ölçeği Azure Resource Manager, Azure anahtar kasası kullanarak Azure'da Ayarla disk şifrelemeyi etkinleştirmek açıklamaktadır.
services: service-fabric
documentationcenter: .net
author: v-viban
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/24/2018
ms.author: v-viban
ms.openlocfilehash: 46f7f88768ab7ae9d84f392f340750865fef3b96
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655824"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Service fabric Linux küme düğümleri için disk şifrelemeyi etkinleştir 
> [!div class="op_single_selector"]
> * [Linux için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Windows için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Service Fabric Linux küme düğümlerinde disk şifrelemeyi etkinleştirmek için aşağıdaki adımları izleyin. Bunlar her düğüm türleri/sanal makine ölçek kümeleri için yapmanız gerekir. Düğümleri şifrelemek için size sanal makine ölçek kümeleri üzerinde Azure Disk Şifrelemesi özelliğini özelliğinden yararlanır.

Kılavuzu, aşağıdaki yordamları içerir:

* Service Fabric Linux kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için devre dışı bilmeniz gereken temel kavramlar ayarlayın.
* Service Fabric Linux kümesi sanal makine ölçek kümesi üzerinde disk şifrelemesi etkinleştirmeden önce izlenmesi için ön koşullar adımları.
* Service Fabric Linux kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için izlenmesi için adımları ayarlayın.


## <a name="prerequisites"></a>Önkoşullar

1. **Kendi kendine kayıt** - kullanabilmeniz için sanal makine ölçek kümesi disk şifreleme Önizleme kendi kendine kayıt gerektirir
2. Aşağıdaki adımları çalıştırarak aboneliğinizi kendi kendine kayıt: 
```Powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
3. 'Kayıtlı' durumu kadar yaklaşık 10 dakika bekleyin. Aşağıdaki komutu çalıştırarak durumunu denetleyebilirsiniz: 
```Powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
4. **Azure anahtar kasası** -sanal makine ölçek ayarlayabilirsiniz ve onun PS cmdlet'ini kullanarak KeyVault üzerinde Erişim İlkesi 'EnabledForDiskEncryption' gibi bir KeyVault aynı abonelikte ve bölgede oluşturun. Ayrıca, Azure portalında KeyVault kullanıcı arabirimini kullanarak ilkeyi ayarlayabilirsiniz: 
```Powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
5. Son yükleme [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) , yeni şifreleme komutlar vardır.
6. En son sürümünü yüklemek [Azure PowerShell'i Azure SDK](https://github.com/Azure/azure-powershell/releases) serbest bırakın. Etkinleştirmek için VMSS ADE cmdlet'leri şunlardır ([ayarlamak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) şifreleme almak ([almak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) şifreleme durum ve kaldırma ([devre dışı](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) şifreleme ölçekte örneği ayarlayın. 

| Komut | Sürüm |  Kaynak  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 veya üstü | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 veya üstü | AzureRM.Compute |
| AzureRmVmssDiskEncryption devre dışı bırak   | 3.4.0 veya üstü | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 veya üstü | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 veya üstü | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 veya üstü | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifrelemesi için desteklenen senaryolar
* Sanal makine ölçek kümesi şifreleme yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmeyen yalnızca ölçek kümeleri için desteklenir.
* Sanal makine ölçek kümesi şifreleme, veri birimi Linux sanal makine ölçek kümesi için desteklenir. Geçerli Önizleme Linux işletim sistemi disk şifrelemesi desteklenmiyor.
* Sanal makine ölçek kümesi VM görüntüsünü ve yükseltme işlemleri geçerli Önizleme'de desteklenmez.


### <a name="create-new-linux-cluster-and-enable-disk-encryption"></a>Yeni Linux kümesi oluşturmak ve disk şifrelemeyi etkinleştir

& Otomatik olarak imzalanan sertifika kümesi oluşturmak ve Azure Resource Manager şablonu kullanarak disk şifrelemeyi etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="log-in-to-azure"></a>Azure'da oturum açma  

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize uygun olarak özel bir şablon Yazar ihtiyacınız varsa, yüksek oranda kullanılabilir şablonlardan birini ile başlamanız önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) Linux kümesi için. 

Zaten bir özel şablon sahip sonra olun emin çift onay, tüm üç sertifika ile ilgili parametreler şablonu ve parametre dosyanın aşağıdaki gibi adlandırılır ve değerler null aşağıdaki gibidir.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

Bu yana Linux sanal makine ölçek kümesi için - Azure Resource Manager şablonu kullanarak veri diski eklemek ihtiyacımız şekilde yalnızca veri disk şifrelemesi desteklenir. Şablonunuz için veri diski sağlama aşağıdaki şekilde güncelleştirin:

```Json
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
```
 

```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Burada, aynı işlemi gerçekleştirmek için eşdeğer CLI komut verilmiştir. Declare deyimlerini değerlerde uygun değerlerle değiştirin. CLI yukarıdaki powershell komutunu destekleyen diğer tüm parametreleri destekler.

```CLI

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

#### <a name="linux-data-disk-mounting"></a>Linux veri diski bağlama
Linux sanal makine ölçek kümesi üzerinde şifrelemeli ilerlemeden önce eklenen veri diski veya doğru şekilde bağlandığından emin olmanız gerekir. Linux küme VM ve çalışma LSBLK komutu oturum açın. Çıkış, eklenen veri diski bağlama noktası sütunu göstermelidir.


#### <a name="deploy-application-to-linux-service-fabric-cluster"></a>Linux Service Fabric kümesi için uygulama dağıtma
Adımlar ve Kılavuzlar izleyin [kümenize uygulama dağıtma](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-quickstart-containers-linux)


#### <a name="enable-disk-encryption-for-service-fabric-linux-cluster-virtual-machine-scale-set-created-above"></a>Yukarıda oluşturduğunuz Service Fabric Linux kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştir
 
```Powershell
$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```CLI

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```

#### <a name="validate-if-disk-encryption-enabled-for-linux-virtual-machine-scale-set"></a>Disk şifreleme için Linux sanal makine ölçek etkin ayarlarsanız doğrulayın.
Tüm sanal makine ölçek kümesi veya herhangi bir örneğine VM ölçek kümesindeki durumunu alın. Aşağıdaki komutları bakın.
Ayrıca kullanıcı Linux küme VM için oturum açın ve LSBLK komutunu çalıştırın. Çıkış, eklenen veri diski için şifreli, eklenen veri diski bağlama noktası sütunu ve türü sütunu göstermelidir.

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```



#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemesi devre dışı bırak 
Devre dışı disk şifrelemesi uygulayan tüm sanal makine ölçek kümesini ve örneği tarafından değil 

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Linux Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemesi etkinleştir/devre dışı bırakılacak nasıl güvenli bir küme var. Ardından, [Disk Windows için şifreleme](service-fabric-enable-azure-disk-encryption-windows.md) 

