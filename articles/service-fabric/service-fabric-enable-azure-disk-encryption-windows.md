---
title: Azure Service Fabric Windows kümeleri için Disk şifrelemeyi etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Resource Manager'da Azure Key Vault'u kullanarak Azure Service Fabric küme düğümleri için disk şifrelemeyi etkinleştirmek açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/22/2019
ms.author: aljo
ms.openlocfilehash: c31fc43729bcb58c755959db0c8bc5185b8197f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66471403"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-windows"></a>Windows Azure Service Fabric küme düğümleri için disk şifrelemeyi etkinleştirme 
> [!div class="op_single_selector"]
> * [Windows için disk şifreleme](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Linux için disk şifreleme](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Bu öğreticide, Service Fabric küme düğümlerinde Windows disk şifrelemeyi etkinleştirmek öğreneceksiniz. Her düğüm türleri ve sanal makine ölçek kümeleri için şu adımları izlemesi gerekir. Düğümleri şifrelemek için Azure Disk şifreleme özelliği sanal makine ölçek kümeleri üzerinde kullanacağız.

Kılavuzu, aşağıdaki konuları içerir:

* Zaman içinde Windows disk şifrelemesini Service Fabric kümesi sanal makine ölçek kümesi dikkat edilecek temel kavramları.
* Service Fabric üzerinde disk şifrelemesi etkinleştirmeden önce izlenmesi gereken adımlar Windows düğümleri kümesi.
* Service Fabric küme düğümlerinde Windows disk şifrelemeyi etkinleştirmek için izlenmesi gereken adımlar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

**Kendi kendine kayıt** 

Sanal makine ölçek kümesi için disk şifreleme Önizleme kendi kendine kayıt gerektirir. Aşağıdaki adımları kullanın: 

1. İlk olarak, aşağıdaki komutu çalıştırın:
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Durum olana kadar yaklaşık 10 dakika bekleyin *kayıtlı*. Aşağıdaki komutu çalıştırarak durumunu denetleyebilirsiniz: 
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Anahtar Kasası.** 

1. Bir anahtar kasası aynı abonelikte ve bölgededir ölçek kümesi oluşturun ve ardından **EnabledForDiskEncryption** kendi PowerShell cmdlet'ini kullanarak ilke anahtar kasasındaki erişim. Ayrıca, aşağıdaki komutla Azure portalında, anahtar kasası kullanıcı arabirimini kullanarak İlkesi de ayarlayabilirsiniz:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. En son sürümünü yükleyin [Azure CLI](/cli/azure/install-azure-cli), yeni şifreleme komutlar vardır.
3. En son sürümünü yükleyin [Azure SDK'sı Azure powershell'den](https://github.com/Azure/azure-powershell/releases) bırakın. Aşağıda sanal makine ölçek kümesi etkinleştirmek için Azure Disk şifrelemesi cmdlet'leri ([ayarlayın](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) şifreleme, almak ([alma](/powershell/module/az.compute/get-azvmssvmdiskencryption)) şifreleme durum ve remove ([devre dışı](/powershell/module/az.compute/disable-azvmssdiskencryption)) Şifreleme ölçek kümesi örneği.

| Komut | Version |  source  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 veya sonraki bir sürümü | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 veya sonraki bir sürümü | Az.Compute |
| AzVmssDiskEncryption devre dışı bırak   | 1.0.0 veya sonraki bir sürümü | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 veya sonraki bir sürümü | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 veya sonraki bir sürümü | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 veya sonraki bir sürümü | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifrelemesi için desteklenen senaryolar
* Sanal makine ölçek kümeleri için şifreleme, yalnızca yönetilen diskler ile oluşturulan ölçek kümeleri için desteklenir. Yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmez.
* İşletim Sistemleri için desteklenen şifreleme ve veri hacimleri sanal makine ölçek Windows ayarlar. Devre dışı şifreleme işletim sistemi için de desteklenir ve veri birimleri için sanal makine ölçek Windows ayarlar.
* Sanal makine ölçek kümeleri için sanal makine yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme sürümünde desteklenmez.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Yeni küme oluşturma ve disk şifrelemeyi etkinleştir

Küme oluşturma ve bir Azure Resource Manager şablonu ve otomatik olarak imzalanan bir sertifika kullanarak disk şifrelemeyi etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 
Aşağıdaki komutları bilgilerinizle oturum açın:
```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Zaten sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat öneririz [Azure Service Fabric kümesi oluşturma şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) sayfası. İçin [küme şablonunuzu özelleştirme] [ customize-your-cluster-template] bölümünde, aşağıdaki kılavuza bakın.

Özel bir şablon zaten varsa, şablonu ve parametre dosyası tüm üç sertifika ile ilgili parametreler şu şekilde adlandırılır ve değerleri gibi null denetleyin:

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


```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```azurecli

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

### <a name="deploy-an-application-to-a-service-fabric-cluster-in-windows"></a>Windows Service Fabric kümesinde uygulamayı dağıtma
Bir uygulamayı kümenize dağıtmak için adımları ve adresindeki yönergeleri izleyin [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](service-fabric-deploy-remove-applications.md).


### <a name="enable-disk-encryption-for-the-virtual-machine-scale-sets-created-previously"></a>Daha önce oluşturduğunuz sanal makine ölçek kümeleri için disk şifrelemeyi etkinleştirme

Sanal makine ölçek disk şifrelemeyi etkinleştirmek için aşağıdaki komutları çalıştırın önceki adımlarında oluşturduğunuz ayarlar:
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-windows"></a>Sanal makine ölçek kümesi içinde Windows için disk şifreleme etkinse doğrula
Bir ölçek kümesindeki aşağıdaki komutları çalıştırarak bir tüm sanal makine ölçek kümesi veya herhangi bir örneğine durumunu alın.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


Ayrıca, sanal makine ölçek kümesi için oturum açın ve sürücüleri şifrelendiğinden emin olun.

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>İçin sanal makine ölçek kümesi bir Service Fabric kümesinde disk şifrelemeyi devre dışı bırakma 
Disk şifrelemesi için sanal makine ölçek kümesi aşağıdaki komutları çalıştırarak devre dışı bırakın. Disk şifreleme devre dışı bırakıldığında tüm sanal makine ölçek kümesi ve tek bir örneği için geçerli olduğunu unutmayın.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, güvenli bir kümeye sahip ve nasıl etkinleştirileceği ve Service Fabric küme düğümleri ve sanal makine ölçek kümeleri için disk şifreleme devre dışı bilmeniz gerekir. Linux'ta Service Fabric küme düğümlerine benzer yönergeler için bkz. [Disk şifrelemesi için Linux](service-fabric-enable-azure-disk-encryption-linux.md).

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template
