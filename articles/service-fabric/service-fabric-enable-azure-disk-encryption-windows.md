---
title: Azure Service Fabric Windows kümeleri için Disk şifrelemeyi etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Resource Manager, Azure anahtar Kasası'nı kullanarak azure'daki Service Fabric küme düğümleri için disk şifrelemeyi etkinleştirmek açıklar.
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
ms.openlocfilehash: a620563be9ffe18ae0f7fa4a78df83ea5b35a5d2
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488417"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Service fabric Windows kümesi düğümleri için disk şifrelemeyi etkinleştirme 
> [!div class="op_single_selector"]
> * [Windows için disk şifreleme](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Linux için disk şifreleme](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Service Fabric Windows kümesi düğümleri disk şifrelemesini etkinleştirmek için aşağıdaki adımları izleyin. Bunlar her düğüm türü/sanal makine ölçek kümeleri için yapmanız gerekir. Düğümleri şifrelemek için sanal makine ölçek kümeleri Azure Disk şifrelemesi yeteneğini yararlanılacaktır.

Kılavuz, aşağıdaki yordamları içerir:

* Service Fabric Windows kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için devre dışı dikkat etmeniz gereken temel kavramlar ayarlayın.
* Service Fabric Windows kümesi sanal makine ölçek kümesinde disk Şifrelemeyi etkinleştirmeden önce izlenmesi için ön koşullar adımları.
* Service Fabric Windows kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için izlenmesi gereken adımlar ayarlayın.


## <a name="prerequisites"></a>Önkoşullar
* **Kendi kendine kayıt** - kullanmak için sanal makine ölçek kümesinin disk şifreleme Önizleme kendi kendine kayıt gerektirir
* Aşağıdaki adımları çalıştırarak aboneliğinizi kendi kendine kayıt: 
```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* 'Kayıtlı' durumu kadar yaklaşık 10 dakika bekleyin. Aşağıdaki komutu çalıştırarak durumunu denetleyebilirsiniz: 
```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault** -ölçeği ayarlayabilirsiniz ve kendi PS cmdlet'ini kullanarak anahtar kasası Erişim İlkesi 'EnabledForDiskEncryption' gibi aynı abonelik ve aynı bölgede bir anahtar kasası oluşturun. Azure portalında KeyVault kullanıcı arabirimini kullanarak İlkesi de ayarlayabilirsiniz: 
```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Son yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) , yeni şifreleme komutlar vardır.
* En son sürümünü yükleyin [Azure SDK'sı Azure powershell'den](https://github.com/Azure/azure-powershell/releases) bırakın. Şunlardır sanal makine ölçek kümesi etkinleştirmek için ADE cmdlet'leri ([ayarlayın](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) şifreleme almak ([alma](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) şifreleme durum ve remove ([devre dışı](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) ölçek kümesinde şifreleme örneği.

| Komut | Sürüm |  Kaynak  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 veya üzeri | AzureRM.Compute |
| Disable-AzureRmVmssDiskEncryption   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 veya üzeri | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 veya üzeri | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 veya üzeri | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifrelemesi için desteklenen senaryolar
* Sanal makine ölçek kümesi şifreleme, yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor. yalnızca ölçek kümeleri için desteklenir.
* Sanal makine ölçek kümesi şifreleme, Windows sanal makine ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir. Devre dışı şifreleme, Windows ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir.
* Sanal makine ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Yeni küme oluşturma ve disk şifrelemesini etkinleştirme

Küme oluşturma ve Azure Resource Manager şablonu & otomatik olarak imzalanan sertifika kullanarak disk şifrelemeyi etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

```powershell
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Zaten sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Yönergeler ve açıklamaları için izleme [küme şablonunuzu özelleştirme] [ customize-your-cluster-template] bölümüne bakın.

Zaten sahip özel bir şablon sonra olun emin çifte denetim, tüm üç sertifika ile ilgili parametreler şablonu ve parametre dosyası şu şekilde adlandırılır ve değerleri null gibidir.

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


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

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

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Windows Service Fabric kümesine uygulama dağıtma
Adımları ve için yönergeleri izleyin [uygulamayı kümenize dağıtma](service-fabric-deploy-remove-applications.md)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Yukarıda oluşturulan Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştirme
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Disk şifrelemesi için Windows sanal makine ölçek etkin ayarlarsanız doğrulayın.
Ölçek kümesindeki tüm sanal makine ölçek kümesi bir ya da herhangi bir örneğine durumunu alın. Aşağıdaki komutları bakın.
Ayrıca kullanıcı VM ölçek kümesi ' oturum açın ve sürücüleri şifrelenmiş emin olun

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi devre dışı bırakma 
Devre dışı disk şifrelemesi uygular ve tüm sanal makine ölçek kümesi örneği tarafından değil 

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştirmek/devre dışı nasıl güvenli bir kümeye sahip. Ardından, [Disk şifrelemesi için Linux](service-fabric-enable-azure-disk-encryption-linux.md) 

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template