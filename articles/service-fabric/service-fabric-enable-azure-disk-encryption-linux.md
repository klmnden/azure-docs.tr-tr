---
title: Azure Service Fabric Linux kümelerinde için Disk şifrelemeyi etkinleştirme | Microsoft Docs
description: Bu makalede, Azure'da Azure Resource Manager, Azure anahtar Kasası'nı kullanarak Service Fabric küme ölçek için disk şifrelemesini etkinleştirmeyi açıklar.
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
ms.openlocfilehash: f580bf02b222f01a3d5aad1254f208791ea22b38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66161781"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Service fabric Linux küme düğümleri için disk şifrelemeyi etkinleştirme 
> [!div class="op_single_selector"]
> * [Linux için disk şifreleme](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Windows için disk şifreleme](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Service Fabric Linux küme düğümlerinde disk şifrelemeyi etkinleştirmek için aşağıdaki adımları izleyin. Bunlar her düğüm türü/sanal makine ölçek kümeleri için yapmanız gerekir. Düğümleri şifrelemek için sanal makine ölçek kümeleri Azure Disk şifrelemesi yeteneğini yararlanılacaktır.

Kılavuz, aşağıdaki yordamları içerir:

* Service Fabric Linux kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için devre dışı dikkat etmeniz gereken temel kavramlar ayarlayın.
* Service Fabric Linux kümesi sanal makine ölçek kümesinde disk Şifrelemeyi etkinleştirmeden önce izlenmesi için ön koşullar adımları.
* Service Fabric Linux kümesi sanal makine ölçek disk şifrelemesini etkinleştirmek için izlenmesi gereken adımlar ayarlayın.



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

* **Kendi kendine kayıt** - kullanmak için sanal makine ölçek kümesinin disk şifreleme Önizleme kendi kendine kayıt gerektirir
* Aşağıdaki adımları çalıştırarak aboneliğinizi kendi kendine kayıt: 
```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* 'Kayıtlı' durumu kadar yaklaşık 10 dakika bekleyin. Aşağıdaki komutu çalıştırarak durumunu denetleyebilirsiniz: 
```powershell
Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault** -sanal makine ölçek kümesi ve kendi PS cmdlet'ini kullanarak anahtar kasası erişim ilkesini 'EnabledForDiskEncryption' ayarlama gibi bir anahtar kasası aynı abonelik ve aynı bölgede oluşturun. Azure portalında KeyVault kullanıcı arabirimini kullanarak İlkesi de ayarlayabilirsiniz: 
```powershell
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Son yükleme [Azure CLI](/cli/azure/install-azure-cli) , yeni şifreleme komutlar vardır.
* En son sürümünü yükleyin [Azure SDK'sı Azure powershell'den](https://github.com/Azure/azure-powershell/releases) bırakın. Sanal makine ölçek kümesi etkinleştirmek için ADE cmdlet'leri şunlardır ([ayarlamak](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) şifreleme almak ([alma](/powershell/module/az.compute/get-azvmssvmdiskencryption)) şifreleme durum ve remove ([devre dışı](/powershell/module/az.compute/disable-azvmssdiskencryption)) ölçek şifreleme örnek olarak ayarlayın. 

| Komut | Version |  Kaynak  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 veya üzeri | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 veya üzeri | Az.Compute |
| AzVmssDiskEncryption devre dışı bırak   | 1.0.0 veya üzeri | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 veya üzeri | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 veya üzeri | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 veya üzeri | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifrelemesi için desteklenen senaryolar
* Sanal makine ölçek kümesi şifreleme, yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor. yalnızca ölçek kümeleri için desteklenir.
* Sanal makine ölçek kümesi şifreleme, Linux sanal makine ölçek kümesi için veri hacmi için desteklenir. Geçerli Önizleme için Linux işletim sistemi disk şifrelemesi desteklenmiyor.
* Sanal makine ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez.


### <a name="create-new-linux-cluster-and-enable-disk-encryption"></a>Yeni Linux kümesi oluşturma ve disk şifrelemeyi etkinleştirin

Küme oluşturma ve Azure Resource Manager şablonu & otomatik olarak imzalanan sertifika kullanarak disk şifrelemeyi etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="sign-in-to-azure"></a>Oturum açın: Azure  

```powershell

Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Zaten sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) Linux kümesi için. 

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

Bu yana Linux sanal makine ölçek kümesi için - yüzden Azure Resource Manager şablonu kullanarak veri diski eklemek yalnızca veri disk şifreleme desteklenir. Veri diski sağlama aşağıda gösterildiği gibi şablonunuzu güncelleştirin:

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

Aynı işlemi gerçekleştirmek için eşdeğer CLI komutu aşağıda verilmiştir. Declare deyimlerini değerleri uygun değerlerle değiştirin. CLI, yukarıdaki powershell komutunu destekleyen diğer tüm parametreleri destekler.

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

#### <a name="linux-data-disk-mounting"></a>Linux veri disk bağlama
Linux sanal makine ölçek kümesinde şifrelemeli ilerlemeden önce eklenen veri diski veya doğru şekilde bağlandığından emin olmak ihtiyacımız var. Linux küme sanal Makineye oturum açın ve LSBLK komutu çalıştırın. Çıktı, eklenen veri diski takma noktası sütununda göstermelidir.


#### <a name="deploy-application-to-linux-service-fabric-cluster"></a>Linux Service Fabric kümesine uygulama dağıtma
Adımları ve için yönergeleri izleyin [uygulamayı kümenize dağıtma](service-fabric-quickstart-containers-linux.md)


#### <a name="enable-disk-encryption-for-service-fabric-linux-cluster-virtual-machine-scale-set-created-above"></a>Yukarıda oluşturulan Service Fabric Linux kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştirme
 
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

#### <a name="validate-if-disk-encryption-enabled-for-linux-virtual-machine-scale-set"></a>Linux sanal makine ölçek kümesi için disk şifrelemesini ayarlarsanız doğrulayın.
Bir tüm sanal makine ölçek kümesi veya VM ölçek kümesi örnek durumunu alın. Aşağıdaki komutları bakın.
Ayrıca kullanıcı Linux küme sanal Makineye oturum açın ve LSBLK komutu çalıştırın. Çıktı, eklenen veri diski için şifreli, eklenen veri diski takma noktası sütunu ve türü sütunu üzerinde göstermelidir.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi devre dışı bırakma 
Devre dışı disk şifrelemesi uygular ve tüm sanal makine ölçek kümesi örneği tarafından değil 

```powershell
$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Linux Service Fabric kümesi sanal makine ölçek kümesi için disk şifrelemeyi etkinleştirmek/devre dışı nasıl güvenli bir kümeye sahip. Ardından, [Disk şifrelemesi için Windows](service-fabric-enable-azure-disk-encryption-windows.md) 
