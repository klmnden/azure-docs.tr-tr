---
title: Azure'da bir Linux VM disklerde şifrelemek | Microsoft Docs
description: Bir Linux VM için Gelişmiş Güvenlik Azure CLI 2.0 kullanan sanal disklerde şifreleme
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/31/2018
ms.author: iainfou
ms.openlocfilehash: 343408366c2970d10a952634ac671721caed74d4
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936879"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makine şifreleme
Geliştirilmiş sanal makine (VM) güvenlik ve uyumluluk için sanal diskler ve VM şifrelenebilir. Sanal makineleri bir Azure anahtar kasası güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarları denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede Azure CLI 2.0 kullanarak bir Linux VM sanal disklerde şifrelemek nasıl ayrıntılarını verir. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.30 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Linux sanal makineleri sanal disklerde rest kullanarak şifrelenir [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Azure sanal diskleri şifreleme ücretsizdir. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bir Azure Active Directory Hizmet sorumlusu VM'ler açma ve kapatma gücü gibi bu şifreleme anahtarları verme için güvenli bir mekanizma sağlar.

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarının bir Azure anahtar kasası oluşturun.
2. Diskleri şifrelemek için kullanılabilmesi için şifreleme anahtarını yapılandırın.
3. Şifreleme anahtarı Azure anahtar Kasası'okumak için bir Azure Active Directory hizmet asıl uygun izinlerle birlikte oluşturun.
4. Kullanılacak Azure Active Directory hizmet asıl ve uygun şifreleme anahtarını belirterek, sanal diskler şifrelemek için komutu yürütün.
5. Azure Active Directory hizmet asıl gereken şifreleme anahtarını Azure anahtar Kasası'ister.
6. Sanal diskler sağlanan şifreleme anahtarı kullanılarak şifrelenir.

## <a name="encryption-process"></a>Şifreleme işlemi
Disk şifrelemesi aşağıdaki ek bileşenlerini dayanır:

* **Azure anahtar kasası** - şifreleme anahtarları ve gizli anahtarları disk şifreleme/şifre çözme işlemi için kullanılan korumak için kullanılan.
  * Varsa, mevcut bir Azure anahtar kasası kullanabilirsiniz. Diskleri şifrelemek için bir anahtar kasası ayrılması gerekmez.
  * Yönetim sınırlarını ve anahtar görünürlük ayırmak için ayrılmış bir anahtar kasası oluşturabilirsiniz.
* **Azure Active Directory** -güvenli değişimi gerekli şifreleme anahtarlarını ve kimlik doğrulaması istenen eylemler için işler.
  * Genellikle, uygulamanızı barındırmak için var olan bir Azure Active Directory örneğini kullanabilirsiniz.
  * Hizmet sorumlusu istemek ve uygun şifreleme anahtarları verilmesi için güvenli bir mekanizma sağlar. Azure Active Directory ile tümleşen gerçek uygulama geliştirme değil.

## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar
Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Aşağıdaki Linux sunucusu SKU'ları - Ubuntu ve CentOS, SUSE ve SUSE Linux Enterprise Server (SLES) ve Red Hat Enterprise Linux.
* Tüm kaynaklar (örneğin, anahtar kasası, depolama hesabı ve VM) aynı Azure bölgesinde ve abonelik olmalıdır.
* Standart A, D, DS, G, GS, vb., seri VM'ler.
* Zaten şifrelenmiş bir Linux VM üzerinde şifreleme anahtarlarını güncelleştiriliyor.

Disk şifrelemesi aşağıdaki senaryolarda şu anda desteklenmiyor:

* Temel katman VM'ler.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* İşletim sistemi disk şifrelemesi Linux sanal makineleri üzerinde devre dışı bırakılıyor.
* Özel Linux görüntüleri kullanın.

Desteklenen senaryolar ve sınırlamalar hakkında daha fazla bilgi için bkz: [Iaas VM'ler için Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md)


## <a name="create-an-azure-key-vault-and-keys"></a>Azure anahtar kasası ve anahtarları oluşturma
Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myKey*, ve *myVM*.

İlk adım, şifreleme anahtarlarını depolamak için bir Azure anahtar kasası oluşturmaktır. Azure anahtar kasası anahtarları, gizli ya da, uygulama ve hizmetlerinize güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için anahtar kasası kullanın.

Azure aboneliğinizle içinde Azure anahtar kasası sağlayıcısını etkinleştir [az sağlayıcı kaydı](/cli/azure/provider#az-provider-register) ve sahip bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az-group-create). Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde `eastus` konumu:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Şifreleme anahtarlarını ve depolama ve VM gibi ilişkili işlem kaynakları içeren Azure anahtar kasası aynı bölgede bulunmalıdır. Bir Azure anahtar kasası ile oluşturma [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyvault_name* gibi:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. HSM kullanmanın premium anahtar kasası gerektirir. Premium yazılım korumalı anahtarlar depolar standart anahtar kasası yerine anahtar kasası oluşturmak için ek bir maliyet yoktur. Premium anahtar kasası oluşturmak için önceki adımda ekleyin `--sku Premium` komutu. Aşağıdaki örnek, standart bir anahtar kasası oluşturulan bu yana yazılım korumalı anahtarlar kullanır.

Her iki koruma modeli için Azure platformu VM sanal diskleri şifresini çözmek için önyüklendiğinde şifreleme anahtarları istemek için erişim verilmesi gerekir. Anahtar kasası ile bir şifreleme anahtarı oluşturmak [az keyvault anahtarı oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create). Aşağıdaki örnek adlı bir anahtar oluşturur *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Azure Active Directory hizmet sorumlusu oluşturma
Sanal diskler şifrelenmiş veya şifresi, kimlik doğrulama ve şifreleme anahtarlarının anahtar Kasası'ndan değişimi işlemek için bir hesap belirtin. Bu hesap, bir Azure Active Directory hizmet asıl adına VM uygun şifreleme anahtarları istemek Azure platformu sağlar. Birçok kuruluş Azure Active Directory dizinleri ayrılmış olsa, aboneliğinizde varsayılan Azure Active Directory örneği kullanılabilir.

Azure Active Directory ile kullanarak bir hizmet sorumlusu oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Aşağıdaki örnek değerler sonraki komutlarını kullanmak için parola ve hizmet sorumlusu için okur:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Parola yalnızca hizmet sorumlusu oluşturma görüntülenir. İsterseniz, görüntüleyin ve parola kaydı (`echo $sp_password`). Hizmet asıl adı ile listeleyebilirsiniz [az ad sp listesi](/cli/azure/ad/sp#az-ad-sp-list) ve belirli bir hizmet asıl ile ilgili ek bilgileri görüntülemek [az ad sp Göster](/cli/azure/ad/sp#az-ad-sp-show).

Başarılı bir şekilde şifrelemek veya sanal diskleri şifresini çözmek için anahtar kasasında depolanan şifreleme anahtarı üzerindeki izinleri anahtarları okumak için Azure Active Directory Hizmet Asıl izin verecek şekilde ayarlanması gerekir. Anahtar kasası ile üzerindeki izinleri ayarla [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). Aşağıdaki örnekte, hizmet asıl kimliği önceki komuttan sağlanır:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Bir VM ile oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) ve 5 Gb veri diski ekleyin. Disk şifrelemesi yalnızca belirli Market görüntülerini destekler. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* kullanarak bir *Ubuntu 16.04 LTS* görüntü:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

VM kullanarak SSH *Publicıpaddress* yukarıdaki komut çıktısında gösterilmektedir. Bir bölüm ve dosya sistemi oluşturun, sonra veri diski bağlayabilir. Daha fazla bilgi için bkz: [yeni disk bağlamak için bir Linux VM Bağlan](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). SSH oturumunuzu kapatın.


## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifrelemek
Sanal diskler şifrelemek için birlikte tüm önceki bileşenleri getirin:

1. Azure Active Directory hizmet asıl ve parola belirtin.
2. Şifrelenmiş diskleriniz için meta verileri depolamak için anahtar kasası belirtin.
3. Gerçek şifreleme ve şifre çözme için kullanılacak şifreleme anahtarları belirtin.
4. İşletim sistemi diski, veri diskleri veya tüm şifrelemek isteyip istemediğinizi belirtin.

VM ile şifrelemek [az vm şifrelemeyi etkinleştirmek](/cli/azure/vm/encryption#az-vm-encryption-enable). Aşağıdaki örnek kullanır *$sp_id* ve *$sp_password* önceki değişkenleri [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) komutu:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Disk şifrelemesi işlemin tamamlanması biraz zaman alır. İşlem durumunu izleme [az vm şifreleme Göster](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

Çıktı aşağıdaki kesilmiş örneğe benzer:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Disk işletim sistemi durumu raporları bekleyin **VMRestartPending**, VM ile yeniden [az vm yeniden başlatma](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

Önyükleme işlemi sırasında disk şifreleme işlemi sonlandırılır, böylece yeniden şifreleme durumunu denetleyerek önce birkaç dakika bekleyin [az vm şifreleme Göster](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

Durum artık işletim sistemi diski ve veri diski olarak bildirmelisiniz **şifreli**.


## <a name="add-additional-data-disks"></a>Ek veri disklerinin ekleme
Veri disklerinizi şifrelenmiş sonra daha sonra ek sanal diskleri VM'nize eklemek ve de şifreleme. Örneğin, ikinci bir sanal disk VM'nize aşağıdaki şekilde ekleyin olanak sağlar:

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --disk myDataDisk \
    --new \
    --size-gb 5
```

Sanal diskler gibi şifrelemek için komutu yeniden çalıştırın:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Sonraki adımlar
* Azure anahtar kasası, şifreleme anahtarlarını ve kasa, silme de dahil olmak üzere yönetme hakkında daha fazla bilgi için bkz: [anahtar kasası CLI kullanarak yönetme](../../key-vault/key-vault-manage-with-cli2.md).
* Azure için karşıya yüklemek için şifrelenmiş bir özel VM hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
