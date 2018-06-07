---
title: Service fabric Windows kümeler için disk şifrelemesi etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Resource Manager, Azure anahtar kasası kullanarak Azure Service Fabric küme düğümleri için disk şifrelemeyi etkinleştirmek açıklar.
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
ms.date: 05/23/2018
ms.author: v-viban
ms.openlocfilehash: 0b84d270cc50888822b8463df2b95aedaa34ee9a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655823"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Service fabric Windows küme düğümleri için disk şifrelemeyi etkinleştir 
> [!div class="op_single_selector"]
> * [Windows için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Linux için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Service Fabric Windows Küme düğümlerinde disk şifrelemeyi etkinleştirmek için aşağıdaki adımları izleyin. Bunlar her düğüm türleri/sanal makine ölçek kümeleri için yapmanız gerekir. Düğümleri şifrelemek için size sanal makine ölçek kümeleri üzerinde Azure Disk Şifrelemesi özelliğini özelliğinden yararlanır.

Kılavuzu, aşağıdaki yordamları içerir:

* Service Fabric Windows kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için devre dışı bilmeniz gereken temel kavramlar ayarlayın.
* Service Fabric Windows kümesi sanal makine ölçek kümesi üzerinde disk şifrelemesi etkinleştirmeden önce izlenmesi için ön koşullar adımları.
* Service Fabric Windows kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için izlenmesi için adımları ayarlayın.


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
4. **Azure anahtar kasası** -ölçeği ayarlayabilirsiniz ve onun PS cmdlet'ini kullanarak KeyVault üzerinde Erişim İlkesi 'EnabledForDiskEncryption' gibi bir KeyVault aynı abonelikte ve bölgede oluşturun. Ayrıca, Azure portalında KeyVault kullanıcı arabirimini kullanarak ilkeyi ayarlayabilirsiniz: 
```Powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
5. Son yükleme [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) , yeni şifreleme komutlar vardır.
6. En son sürümünü yüklemek [Azure PowerShell'i Azure SDK](https://github.com/Azure/azure-powershell/releases) serbest bırakın. Şunlardır'ın sanal makine ölçek kümesi etkinleştirmek için ADE cmdlet'leri ([ayarlamak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) şifreleme almak ([almak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) şifreleme durum ve kaldırma ([devre dışı](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) ölçek kümesini şifreleme örneği.

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
* Sanal makine ölçek kümesi şifreleme Windows VMSS için işletim sistemi ve veri birimleri için desteklenir. Devre dışı şifreleme Windows ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir.
* Sanal makine ölçek kümesi VM görüntüsünü ve yükseltme işlemleri geçerli Önizleme'de desteklenmez.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Yeni küme oluşturma ve disk şifrelemesi etkinleştirme

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

Gereksinimlerinize uygun olarak özel bir şablon Yazar ihtiyacınız varsa, yüksek oranda kullanılabilir şablonlardan birini ile başlamanız önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Kılavuzu ve açıklamalar [küme şablonunuzu özelleştirmek için] izleyin [Özelleştirme Sihirbazı-küme-şablon] bölümüne bakın.

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

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Windows Service Fabric kümesi için uygulama dağıtma
Adımlar ve Kılavuzlar izleyin [kümenize uygulama dağıtma](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Yukarıda oluşturduğunuz Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştir
 
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


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Windows sanal makine ölçek için etkinleştirilmiş disk şifrelemesi ayarlarsanız doğrulayın.
Ölçek kümesindeki tüm sanal makine ölçek kümesi veya herhangi bir örneğine durumunu alın. Aşağıdaki komutları bakın.
Ayrıca kullanıcı VM ölçek kümesindeki oturum açın ve sürücüleri şifrelenmiş emin olun

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
Bu noktada, Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemesi etkinleştir/devre dışı bırakılacak nasıl güvenli bir küme var. Ardından, [Disk Linux için şifreleme](service-fabric-enable-azure-disk-encryption-linux.md) 

