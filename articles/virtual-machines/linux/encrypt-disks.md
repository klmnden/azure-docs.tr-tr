---
title: Azure'daki bir Linux sanal diskleri şifreleme | Microsoft Docs
description: Gelişmiş Güvenlik Azure CLI 2.0 kullanarak bir Linux sanal makinesinde sanal diskleri şifreleme
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
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 75ec087536d6f833a9a2106b1fdf4ed1fd73ef8e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932420"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makinesi şifreleme
Geliştirilmiş sanal makine (VM) güvenlik ve uyumluluk için sanal diskleri ve VM şifrelenebilir. Vm'leri bir Azure Key Vault'ta güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarlarını denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede, Azure CLI 2.0 kullanarak bir Linux VM üzerinde sanal diskleri şifreleme işlemi açıklanmaktadır. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.30 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Rest kullanarak Linux sanal makineleri sanal disklerde şifrelenir [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Azure'da sanal diskler şifrelemek için ücret alınmaz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bir Azure Active Directory Hizmet sorumlusu, Vm'leri açıp desteklenir gibi veren bu şifreleme anahtarları için güvenli bir mekanizma sağlar.

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarı Azure anahtar Kasası'nda oluşturun.
2. Disk şifreleme için kullanılabilir olması için şifreleme anahtarını yapılandırın.
3. Şifreleme anahtarı Azure Key Vault'tan okumak için bir Azure Active Directory Hizmet sorumlusu uygun izinlerle oluşturma.
4. Azure Active Directory Hizmet sorumlusu ve uygun şifreleme anahtarı belirterek, sanal diskleri şifreleme için komutu Yürüt.
5. Azure Active Directory Hizmet sorumlusu, gerekli bir şifreleme anahtarı Azure Key Vault'tan ister.
6. Sanal diskler belirtilen şifreleme anahtarı kullanılarak şifrelenir.

## <a name="encryption-process"></a>Şifreleme işlemi
Disk şifrelemesi üzerinde aşağıdaki ek bileşenleri kullanır:

* **Azure Key Vault** - şifreleme anahtarlarını ve gizli disk şifreleme/şifre çözme işlemi için kullanılan korumak için kullanılır.
  * Varsa, var olan bir Azure anahtar Kasası'nı kullanabilirsiniz. Disk şifreleme için bir Key Vault ayırmanız gerekmez.
  * Yönetim sınırları ve anahtar görünürlük ayırmak için ayrılmış bir anahtar kasası oluşturabilirsiniz.
* **Azure Active Directory** -güvenli değişimi gereken şifreleme anahtarlarını ve kimlik doğrulaması için istenen eylemleri işler.
  * Genellikle, uygulamanızı barındırmak için var olan bir Azure Active Directory örneğini kullanabilirsiniz.
  * Hizmet sorumlusu, istek ve uygun şifreleme anahtarları verilmesi güvenli bir mekanizma sağlar. Azure Active Directory ile tümleştirilen gerçek bir uygulama geliştiriyorsunuz değil.

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
az group create --name myResourceGroup --location eastus
```

Azure Key Vault şifreleme anahtarlarını ve depolama alanı ve VM gibi ilişkili işlem kaynakları içeren aynı bölgede bulunmalıdır. Bir Azure Key Vault ile oluşturma [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyvault_name* gibi:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. Bir HSM kullanıldığında, anahtar kasası premium gerektirir. Bir premium yazılım korumalı anahtarlar depolayan standart Key Vault yerine Key Vault oluşturma için ek bir maliyet yoktur. Anahtar kasası premium oluşturmak için önceki adımda ekleme `--sku Premium` komutu. Aşağıdaki örnek, standart bir anahtar kasası oluşturduktan sonra yazılım korumalı anahtarlar kullanır.

Her iki koruma modeli için Azure platformu sanal disklerin şifresini çözmek için VM önyükleme yaptığında, şifreleme anahtarlarını istemek için erişim verilmesi gerekir. Bir şifreleme anahtarı ile anahtar Kasası'nda oluşturma [az keyvault key oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create). Aşağıdaki örnekte adlı bir anahtar oluşturur *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Azure Active Directory hizmet sorumlusu oluşturma
Sanal diskler şifrelenmiş veya şifresi, kimlik doğrulaması ve şifreleme anahtarları Key vault'tan değişimi işlemek için bir hesap belirtin. Bu hesap bir Azure Active Directory Hizmet sorumlusu, uygun şifreleme anahtarları VM adına istemek üzere Azure platformunun sağlar. Birçok kuruluşun, Azure Active Directory dizin ayrılmış ancak varsayılan Azure Active Directory örneğine aboneliğinizde, kullanılabilir.

Azure Active Directory ile kullanarak bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Aşağıdaki örnek, hizmet sorumlusu ve daha sonraki komutlarda kullanılmak üzere parola değerleri okur:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Parola yalnızca hizmet sorumlusu oluşturma görüntülenir. İsterseniz, görüntüleyin ve parolayı kaydedin (`echo $sp_password`). Hizmet sorumluları ile listeleyebilirsiniz [az ad sp listesi](/cli/azure/ad/sp#az-ad-sp-list) ve belirli bir hizmet sorumlusu ile ilgili ek bilgileri görüntüleyebilir [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show).

Başarılı bir şekilde şifrelemek veya sanal disklerin şifresini çözmek için anahtarları okumak için Azure Active Directory Hizmet sorumlusu izin vermek için Key Vault'ta depolanan şifreleme anahtarı izinlerini ayarlamanız gerekir. İzinleri anahtar kasanıza Ayarla [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). Aşağıdaki örnekte, önceki komutta hizmet sorumlusu kimliği sağlanır:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
İle bir VM [az vm oluşturma](/cli/azure/vm#az-vm-create) ve 5 Gb veri diski ekleme. Disk şifrelemesi yalnızca belirli Market görüntüleri destekler. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kullanarak bir *Ubuntu 16.04 LTS* görüntü:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH kullanarak VM'ye *Publicıpaddress* önceki komutun çıktısında gösterilen. Bir bölüm ve dosya sistemi oluşturun ve ardından veri diski bağlayın. Daha fazla bilgi için [bir Linux VM için yeni diski takın Bağlan](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). SSH oturumunuzu kapatın.


## <a name="encrypt-the-virtual-machine"></a>Sanal makinesi şifreleme
Sanal disklerini şifrelemek için tüm önceki bileşenleri bir araya:

1. Azure Active Directory Hizmet sorumlusu ve parolayı belirtin.
2. Şifrelenmiş diskleriniz için meta verileri depolamak için Key Vault belirtin.
3. Şifreleme anahtarları gerçek şifrelemeyi ve şifre çözme için kullanılacağını belirtin.
4. İşletim sistemi diski, veri disklerini veya tüm şifrelemek isteyip istemediğinizi belirtin.

Şifreleme ile sanal makinenize [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable). Aşağıdaki örnekte *$sp_id* ve *$sp_password* önceki değişkenleri [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) komutu:

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

Disk şifreleme işleminin tamamlanması biraz zaman alabilir. İle işlem durumunu izleme [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

Kesilmiş aşağıdaki örneğe benzer bir çıkış:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Disk için işletim sistemi durumu raporları bekleyin **VMRestartPending**, VM'nizi yeniden [az vm restart](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

Disk şifreleme işlemi önyükleme işlemi sırasında sonlandırılır, bu nedenle yeniden şifreleme durumunu iade etmeden önce birkaç dakika bekleyin [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

Durum artık işletim sistemi diski ve veri diski olarak raporlamalıdır **şifreli**.


## <a name="add-additional-data-disks"></a>Ek veri diskleri ekleme
Veri disklerinizi Şifrelemiş sonra daha sonra ek sanal diskler sanal makinenizde ekleyebilir ve de şifreleme. Örneğin, sanal makinenizde aşağıdaki şekilde ikinci bir sanal disk ekleyin olanak sağlar:

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --disk myDataDisk \
    --new \
    --size-gb 5
```

Sanal diskleri gibi şifrelemek için komutu yeniden çalıştırın:

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
* Azure Key Vault, şifreleme anahtarlarını ve Kasaları silme de dahil olmak üzere yönetme hakkında daha fazla bilgi için bkz. [yönetme CLI ile anahtar kasası](../../key-vault/key-vault-manage-with-cli2.md).
* Şifrelenmiş özel bir VM'yi Azure'a yüklemek için hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
