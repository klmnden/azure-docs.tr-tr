---
title: Oluşturma ve Azure CLI ile bir Windows VM şifreleme
description: Bu hızlı başlangıçta, Azure CLI oluşturmak ve bir Windows sanal makinesi şifreleme için kullanmayı öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 3cacb2e179cb7abe99d62d91d396a56fdeaf1ca2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081465"
---
# <a name="quickstart-create-and-encrypt-a-windows-vm-with-the-azure-cli"></a>Hızlı Başlangıç: Oluşturma ve Azure CLI ile bir Windows VM şifreleme

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç oluşturmak ve bir Windows Server 2016 sanal makine (VM) şifrelemek için Azure CLI'yı kullanmayı gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm?view=azure-cli-latest#az-vm-create) ile bir VM oluşturun. Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur. Bu örnekte yönetici kullanıcı için *azureuser* kullanıcı adı ve *myPassword12* parolası kullanılır. 

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnekte VM oluşturma işleminin başarılı olduğu gösterilmektedir.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Şifreleme anahtarları için yapılandırılmış bir Key Vault oluşturma

Azure disk şifrelemesini bir Azure anahtar Kasası'nda şifreleme anahtarını depolar. Bir Key Vault oluşturma [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create). Şifreleme anahtarları depolamak için Key Vault etkinleştirmek için kullanılır etkin-için-disk şifreleme parametresi.
> [!Important]
> Her Key Vault benzersiz bir ad olmalıdır. Aşağıdaki örnekte adlı bir anahtar kasası oluşturulmaktadır *myKV*, ancak sizinki farklı bir şey adlandırmalısınız.

```azurecli
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>Sanal makinesi şifreleme

Şifreleme ile sanal makinenize [az vm şifreleme](/cli/azure/vm/encryption?view=azure-cli-latest), kendi benzersiz Key Vault sağlama disk şifreleme keyvault parametre adı.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

Bu şifreleme ile sanal Makinenizin üzerinde etkin doğrulayabilirsiniz [az vm show](/cli/azure/vm/encryption#az-vm-encryption-show)

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Döndürülen çıkış aşağıdaki görürsünüz:

```azurecli-interactive
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil](/cli/azure/group) kaynak grubunu, VM'yi ve anahtar Kasası'nı kaldırmak için komutu. 

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sanal makine etkinleştirmek için şifreleme anahtarlarını ve VM şifreli bir Key Vault oluşturdunuz, oluşturuldu.  IaaS VM'leri için Azure Disk Şifrelemesi önkoşulları hakkında daha fazla bilgi edinmek üzere bir sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md)

