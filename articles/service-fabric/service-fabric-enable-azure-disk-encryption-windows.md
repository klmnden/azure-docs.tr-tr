---
title: Service Fabric Windows kümeleri için disk şifrelemesi etkinleştir | Microsoft Docs
description: Bu makalede, Azure Resource Manager ve Azure anahtar kasası kullanarak Azure Service Fabric küme düğümleri için disk şifrelemesi etkinleştirmek açıklar.
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
ms.openlocfilehash: 58f9481fa27f44de30fd80fe52e9b6d06d341b41
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750902"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Service Fabric Windows küme düğümleri için disk şifrelemeyi etkinleştir 
> [!div class="op_single_selector"]
> * [Windows için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Linux için disk şifrelemesi](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Azure Service Fabric Windows Küme düğümlerinde disk şifrelemeyi etkinleştirmek için aşağıdaki adımları kullanın. Bunlar her bir düğüm türleri veya sanal makine ölçek kümeleri için yapmanız gerekir. Düğümleri şifrelemek için sanal makine ölçek kümeleri üzerinde Azure Disk şifrelemesi özelliği kullanacaksınız.

Kılavuzu, aşağıdaki yordamları içerir:

* Sanal makine ölçek kümeleri doku Windows hizmeti kümeler için disk şifrelemesini etkinleştirmek için önemli kavramlar.
* Sanal makinedeki disk şifrelemesi etkinleştirmeden önce izlemeniz gereken adımlar doku Windows hizmeti kümeleri için ayarlar ölçeklendirin.
* Doku Windows hizmeti kümeleri için sanal makine ölçek kümeleri üzerinde şifrelemeyi etkinleştirmek ve devre dışı bırakmak için adımları disk.


## <a name="prerequisites"></a>Önkoşullar
1. Aboneliğinizi şu komutu girerek kendi kendine kaydedin:

   ```PowerShell
   Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
   ```
   
   Durumu kadar yaklaşık 10 dakika bekleyin `Registered`. Aşağıdaki komutları çalıştırarak durumunu denetleyebilirsiniz:

   ```PowerShell
   Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
   ```

2. Bir anahtar kasası ölçek kümesi aynı abonelikte ve bölgede oluşturun. Erişim İlkesi `EnabledForDiskEncryption` PowerShell cmdlet'ini kullanarak anahtar kasası üzerinde. Azure portalında Azure anahtar kasası UI kullanarak İlkesi de ayarlayabilirsiniz.

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
   ```

3. Yükleme [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), en son şifreleme komutlar vardır.

4. En son sürümünü yüklemek [Azure PowerShell'i Azure SDK](https://github.com/Azure/azure-powershell/releases). Etkinleştirmek için aşağıdaki cmdlet'leri kullanın ([ayarlamak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) şifreleme, almak ([almak](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) şifreleme durum ve kaldırma ([devre dışı](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) bir ölçekte şifreleme örneği ayarlayın:

| Komut | Sürüm |  Kaynak  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 veya üzeri | AzureRM.Compute |
| AzureRmVmssDiskEncryption devre dışı bırak   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 veya üzeri | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 veya üzeri | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifrelemesi için desteklenen senaryolar
* Sanal makine ölçek kümesi şifreleme yalnızca yönetilen diskler ile oluşturulan ölçek kümeleri için desteklenir. Yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
* Sanal makine ölçek kümesi şifrelemesi, işletim sistemi ve veri birimleri Windows sanal makine ölçek kümeleri için desteklenir. Etkinleştirin ve şifrelemeyi devre dışı.
* Sanal makine ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez.


## <a name="create-a-windows-cluster"></a>Windows Küme oluşturma

Bir küme oluşturmak ve bir Azure Resource Manager şablonu ve otomatik olarak imzalanan bir sertifika kullanarak disk şifrelemeyi etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="log-in-to-azure"></a>Azure'da oturum açma 

```PowerShell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-a-custom-template"></a>Özel bir şablon kullanın 

Gereksinimlerinize uygun olarak özel bir şablon yazmak gerekiyorsa, biri ile başlamanızı öneririz [Azure Service Fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master).

Özel bir şablon zaten varsa, tüm üç sertifika ile ilgili parametreler şablonu ve parametre dosyası gibi adlı emin olun. Ayrıca değerleri aşağıdaki gibi null olduğundan emin olun.

```JSON
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


```PowerShell


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

### <a name="deploy-an-application-to-the-windows-service-fabric-cluster"></a>Bir uygulamayı Windows Service Fabric kümesi dağıtma
Adımlar ve Kılavuzlar izleyin [kümenize bir uygulamayı dağıtmak](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications).


## <a name="enable-disk-encryption-for-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi için disk şifrelemeyi etkinleştir
Service Fabric Windows Küme için daha önce oluşturduğunuz sanal makine ölçek kümesi için disk şifrelemeyi etkinleştirir.
 
```PowerShell

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


## <a name="validate-that-disk-encryption-is-enabled-for-a-virtual-machine-scale-set"></a>Bu diski doğrula şifreleme için bir sanal makine ölçek kümesi etkin
Tüm sanal makine ölçek kümesi veya herhangi bir örneğine VM ölçek kümesindeki durumunu almak için aşağıdaki komutları kullanın. VM ölçek kümesi ile oturum açın ve sürücülerin şifrelenmesini emin olun.

```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


## <a name="disable-disk-encryption-for-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi için disk şifrelemesi devre dışı bırak 
Disk şifrelemesi için Service Fabric Windows kümesi sanal makine ölçek için devre dışı bırakmanız gerekirse, aşağıdaki komutları kullanın. Disk şifrelemesi devre dışı bırakma, tüm sanal makine ölçek kümesini ve örneği tarafından uygulanır. 


```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, güvenli bir kümeniz olduğuna ve etkinleştirmek ve bir doku Windows hizmeti küme için disk şifrelemesi devre dışı bırakmak nasıl biliyorsunuz. Ardından, öğrenin [Linux disk şifrelemesi](service-fabric-enable-azure-disk-encryption-linux.md). 

