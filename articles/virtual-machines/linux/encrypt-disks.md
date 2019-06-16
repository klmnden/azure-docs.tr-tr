---
title: Azure'daki bir Linux sanal diskleri şifreleme | Microsoft Docs
description: Gelişmiş güvenlik için Azure CLI kullanarak Linux sanal makinesinde sanal diskleri şifreleme
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 15bd3cf2ab6ea5285662610c2c0a850bb180e2f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60731626"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makinesi şifreleme

Geliştirilmiş sanal makine (VM) güvenlik ve uyumluluk için sanal diskleri ve VM şifrelenebilir. Vm'leri bir Azure Key Vault'ta güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarlarını denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede, Azure CLI kullanarak bir Linux sanal makinesinde sanal diskleri şifreleme işlemi açıklanmaktadır. 

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.30 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Rest kullanarak Linux sanal makineleri sanal disklerde şifrelenir [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Azure'da sanal diskler şifrelemek için ücret alınmaz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. 

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarı Azure anahtar Kasası'nda oluşturun.
1. Disk şifreleme için kullanılabilir olması için şifreleme anahtarını yapılandırın.
1. Sanal diskler için disk şifrelemeyi etkinleştirir.
1. Gerekli şifreleme anahtarları Azure Key Vault'tan istenir.
1. Sanal diskler belirtilen şifreleme anahtarı kullanılarak şifrelenir.


## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar
Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Aşağıdaki Linux sunucusu SKU'ları - Ubuntu, CentOS, Red Hat Enterprise Linux, SUSE ve SUSE Linux Enterprise Server (SLES).
* Tüm kaynakları (örneğin, Key Vault, depolama hesabı ve sanal makine), aynı Azure bölgesindeki ve abonelikte olmalıdır.
* Standart A, D, DS, G, GS, vb., serisi VM'ler.
* Şifreleme anahtarları zaten şifrelenmiş bir Linux sanal makinesinde güncelleştiriliyor.

Disk şifrelemesi, şu anda aşağıdaki senaryolar desteklenmez:

* Temel katmanı Vm'lerini.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* Linux vm'lerde işletim sistemi disk şifreleme devre dışı bırakılıyor.
* Özel Linux görüntüleri kullanın.

Desteklenen senaryolar ve sınırlamalar hakkında daha fazla bilgi için bkz. [Iaas Vm'leri için Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md)


## <a name="create-an-azure-key-vault-and-keys"></a>Bir Azure Key Vault ve anahtarlar oluşturma
Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myKey*, ve *myVM*.

İlk adım, bir Azure Key Vault, şifreleme anahtarlarını depolamak için oluşturmaktır. Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için Key Vault'u kullanın.

Azure Key Vault sağlayıcısının ile Azure aboneliğinizde etkinleştirme [az provider register](/cli/azure/provider#az-provider-register) ve bir kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group#az-group-create). Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde `eastus` konumu:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
resourcegroup="myResourceGroup"
az group create --name $resourcegroup --location eastus
```

Azure Key Vault şifreleme anahtarlarını ve depolama alanı ve VM gibi ilişkili işlem kaynakları içeren aynı bölgede bulunmalıdır. Bir Azure Key Vault ile oluşturma [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyvault_name* gibi:

```azurecli-interactive
keyvault_name=myvaultname$RANDOM
az keyvault create \
    --name $keyvault_name \
    --resource-group $resourcegroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. Bir HSM kullanıldığında, anahtar kasası premium gerektirir. Bir premium yazılım korumalı anahtarlar depolayan standart Key Vault yerine Key Vault oluşturma için ek bir maliyet yoktur. Anahtar kasası premium oluşturmak için önceki adımda ekleme `--sku Premium` komutu. Aşağıdaki örnek, standart bir anahtar kasası oluşturduktan sonra yazılım korumalı anahtarlar kullanır.

Her iki koruma modeli için Azure platformu sanal disklerin şifresini çözmek için VM önyükleme yaptığında, şifreleme anahtarlarını istemek için erişim verilmesi gerekir. Bir şifreleme anahtarı ile anahtar Kasası'nda oluşturma [az keyvault key oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create). Aşağıdaki örnekte adlı bir anahtar oluşturur *myKey*:

```azurecli-interactive
az keyvault key create \
    --vault-name $keyvault_name \
    --name myKey \
    --protection software
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
İle bir VM [az vm oluşturma](/cli/azure/vm#az-vm-create) ve 5 Gb veri diski ekleme. Disk şifrelemesi yalnızca belirli Market görüntüleri destekler. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kullanarak bir *Ubuntu 16.04 LTS* görüntü:

```azurecli-interactive
az vm create \
    --resource-group $resourcegroup \
    --name myVM \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH kullanarak VM'ye *Publicıpaddress* önceki komutun çıktısında gösterilen. Bir bölüm ve dosya sistemi oluşturun ve ardından veri diski bağlayın. Daha fazla bilgi için [bir Linux VM için yeni diski takın Bağlan](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). SSH oturumunuzu kapatın.


## <a name="encrypt-the-virtual-machine"></a>Sanal makinesi şifreleme


Şifreleme ile sanal makinenize [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable):

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Disk şifreleme işleminin tamamlanması biraz zaman alabilir. İle işlem durumunu izleme [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group $resourcegroup --name myVM --query 'status'
```

Tamamlandığında, çıkış aşağıdaki örneğe benzer görünecektir:

```json
[
  {
    "code": "ProvisioningState/succeeded",
    "displayStatus": "Provisioning succeeded",
    "level": "Info",
    "message": "Encryption succeeded for all volumes",
    "time": null
  }
]
```


## <a name="add-additional-data-disks"></a>Ek veri diskleri ekleme
Şifrelenmiş veri disklerinizi sonra ek sanal diskler, VM ekleme ve şifreleme. 

Veri diski sanal Makineye eklendikten sonra sanal diskler gibi şifrelemek için komutu yeniden çalıştırın:

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type data
```


## <a name="next-steps"></a>Sonraki adımlar
* Azure Key Vault, şifreleme anahtarlarını ve Kasaları silme de dahil olmak üzere yönetme hakkında daha fazla bilgi için bkz. [yönetme CLI ile anahtar kasası](../../key-vault/key-vault-manage-with-cli2.md).
* Şifrelenmiş özel bir VM'yi Azure'a yüklemek için hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
