---
title: Azure CLI ile Azure ölçek ayarlar için diskleri şifrelemek | Microsoft Docs
description: VM örnekleri ve Linux sanal makine ölçek kümesindeki bağlı disklerde şifrelemek için Azure CLI 2.0 kullanmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: iainfou
ms.openlocfilehash: 22d3c763317def137b4e0beb155f28585d7c6ae1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776423"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli-20-preview"></a>İşletim sistemi ve Azure CLI 2.0 (Önizleme) ayarlama bir sanal makine ölçek eklenen veri disklerini şifrele

Korumak ve rest endüstri standart şifreleme teknolojisi kullanarak verileri korumak için Azure Disk şifrelemesi (ADE) sanal makine ölçek kümesini destekler. Linux ve Windows sanal makine için şifreleme etkinleştirilebilir ölçek kümeleri. Daha fazla bilgi için bkz: [Linux ve Windows Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Sanal makine ölçek kümeleri için Azure disk şifrelemesi genel Önizleme, tüm Azure ortak bölgelerde kullanılabilir şu anda kullanılıyor.

Azure disk şifrelemesi desteklenir:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- işletim sistemi ve veri birimleri için Windows ölçek kümeleri. Devre dışı şifrelemesi, işletim sistemi ve veri birimleri Windows ölçek kümeleri için desteklenir.
- Linux ölçek kümeleri veri birimleri için. İşletim sistemi disk şifrelemesi Linux ölçek kümeleri için geçerli Önizleme'de desteklenmez.

Ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez. Sanal makine ölçek kümeleri Önizleme için Azure disk şifrelemesi yalnızca sınama ortamlarında önerilir. Önizleme'de, disk şifreleme yere bir şifrelenmiş ölçek kümesindeki bir işletim sistemi görüntüsüne yükseltmeniz gerekebilir üretim ortamlarında etkinleştirmeyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.31 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="register-for-disk-encryption-preview"></a>Disk şifrelemesi önizlemesi için kaydolun

Sanal makine ölçek Önizleme ayarlar için Azure disk şifrelemesi aboneliğinizle kendi kendine kaydettirmek gerektirir [az özelliği kayıt](/cli/azure/feature#az_feature_register). Yalnızca ilk kez disk şifreleme önizleme özelliğini kullandığınızda aşağıdaki adımları gerçekleştirmeniz gerekir:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Yaymak kayıt isteği 10 dakika kadar sürebilir. Kayıt durumu ile denetleyebilirsiniz [az özelliği Göster](/cli/azure/feature#az_feature_show). Zaman `State` raporları *kayıtlı*, yeniden kaydettirin *Mirosoft.Compute* sağlayıcısıyla [az sağlayıcı kaydı](/cli/azure/provider#az_provider_register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

Ölçek kümesi oluşturabilmek için [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bu adımda [az vmss create](/cli/azure/vmss#az_vmss_create) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, değişiklikler uygulandıkça otomatik güncelleştirilecek şekilde ayarlanan *myScaleSet* adlı bir ölçek kümesi oluşturur ve *~/.ssh/id_rsa* içinde yoksa SSH anahtarları oluşturur. Bir 32Gb veri diski her VM örneği ve Azure bağlı [özel betik uzantısının](../virtual-machines/linux/extensions-customscript.md) ile veri diskleri hazırlamak için kullanılan [az vmss uzantı kümesi](/cli/azure/vmss/extension#az_vmss_extension_set):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure anahtar kasası oluşturma

Azure anahtar kasası anahtarları, gizli ya da, uygulama ve hizmetlerinize güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz.

Kendi benzersiz tanımlama *keyvault_name*. KeyVault ile oluşturursunuz [az keyvault oluşturma](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) aynı abonelik ve bölge ve ayarlayın ölçek olarak *---için-disk-şifreleme etkin* erişim ilkesi.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Var olan bir anahtar kasası kullanın

Bu adım yalnızca olan disk şifrelemesi ile kullanmak istediğiniz bir anahtar kasası varsa gerekli. Bir anahtar kasası önceki bölümde oluşturduysanız, bu adımı atlayın.

Kendi benzersiz tanımlama *keyvault_name*. Ardından, KeyVault ile güncelleştirilmiş [az keyvault güncelleştirme](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) ve *---için-disk-şifreleme etkin* erişim ilkesi.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştir

Ölçek kümesindeki VM örnekleri şifrelemek için önce anahtar kasası kaynak kimliği ile ilgili bazı bilgi edinmek [az keyvault Göster](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Bu değişkenleri ile şifreleme işlemini başlatmak için kullanılan [az vmss şifrelemeyi etkinleştirmek](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Bir veya iki başlatmak şifreleme işlemi için dakika sürebilir.

Bir önceki adımda ölçek kümesi içinde oluşturulan ayarlamak ölçekte yükseltme ilkesi olarak ayarlandığında *otomatik*, VM örnekleri otomatik olarak şifreleme işlemini başlatın. Burada yükseltme İlkesi, el ile ölçek kümeleri üzerinde şifreleme ilkesi ile VM örneklerinde Başlat [az vmss güncelleştirme-örnekleri](/cli/azure/vmss#az-vmss-update-instances).

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme

Disk şifrelemesi durumunu denetlemek için kullanın [az vmss şifreleme Göster](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

VM örnekleri şifreli olduğunda, durum kodu raporları *EncryptionState ve şifrelenmiş*aşağıdaki örnek çıktıda görüldüğü gibi:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Şifreleme devre dışı bırak

Artık şifrelenmiş VM örnekleri diskleri kullanmak istiyorsanız, şifreleme ile devre dışı bırakabilirsiniz [az vmss şifrelemeyi devre dışı](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) gibi:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure CLI 2.0 bir sanal makine ölçek kümesi şifrelemek için kullanılır. Aynı zamanda [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) için ya da şablon [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

Uçtan uca toplu iş dosyası örneği Linux ölçek kümesi veri disk şifrelemesi için bulunabilir [burada](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). Bu örnek, bir kaynak grubu, Linux ölçek kümesi oluşturur, 5 GB veri diski bağlar ve sanal makine ölçek kümesi şifreler.