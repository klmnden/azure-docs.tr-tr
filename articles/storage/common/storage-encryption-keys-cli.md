---
title: Müşteri tarafından yönetilen anahtarlar Azure clı'dan Azure depolama şifrelemesi için yapılandırma
description: Azure CLI, Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarlar yapılandırmak için kullanmayı öğrenin. Müşteri tarafından yönetilen anahtarlar oluşturun, döndürme, devre dışı bırakın ve erişim denetimleri iptal olanak sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/24/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 925b69e064e260a78a102a068f052ad7d396c380
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357069"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-azure-cli"></a>Müşteri tarafından yönetilen anahtarlar Azure clı'dan Azure depolama şifrelemesi için yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, Azure CLI kullanarak müşteri tarafından yönetilen anahtarlarla anahtar kasası yapılandırma gösterilmektedir.

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabı için bir kimlik atama

Müşteri tarafından yönetilen anahtarları depolama hesabınız için etkinleştirmek için önce sistem tarafından atanan bir yönetilen kimlik depolama hesabına atayın. Bu yönetilen kimlik, anahtar kasasına erişmek için depolama hesabı izinleri vermek için kullanın.

Azure CLI kullanarak yönetilen bir kimlik atanacak çağrı [az depolama hesabını güncelleştirme](/cli/azure/storage/account#az-storage-account-update). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Azure CLI ile yönetilen kimlikleri sistem tarafından atanan yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma kimliklerini Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Yeni key vault oluşturma

Azure depolama şifrelemesi etkin, iki anahtar koruma ayarı bulunmalıdır, müşteri tarafından yönetilen anahtarları depolamak için kullandığınız bir anahtar kasası **geçici silme** ve **yapmak değil Temizleme**. Bu ayarlar etkinken, PowerShell veya Azure CLI kullanarak yeni bir anahtar kasası oluşturmak için aşağıdaki komutları yürütün. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

Azure CLI kullanarak yeni bir anahtar kasası oluşturmak için arama [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

## <a name="configure-the-key-vault-access-policy"></a>Anahtar kasası erişim ilkesini yapılandırma

Ardından, depolama hesabına erişmek için izinlere sahip olacak şekilde anahtar kasası için erişim ilkesi yapılandırın. Bu adımda, önceden atanmış depolama hesabına yönetilen bir kimlik kullanmanız gerekir.

Anahtar kasası erişim ilkesini ayarlamak için çağrı [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)
az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group>
    --object-id $storage_account_principal \
    --key-permissions get recover unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Yeni anahtar oluştur

Ardından, anahtar kasasında bir anahtarı oluşturun. Bir anahtar oluşturmak için arama [az keyvault key oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault key create
    --name <key> \
    --vault-name <key-vault>
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifreleme yapılandırma

Varsayılan olarak, Microsoft tarafından yönetilen anahtarlar Azure depolama şifrelemesi kullanır. Azure depolama hesabınız için müşteri tarafından yönetilen anahtarlar yapılandırma ve depolama hesabıyla ilişkilendirmek için anahtarı belirtin.

Depolama hesabının şifreleme ayarlarını güncelleştirmek için çağrı [az depolama hesabını güncelleştirme](/cli/azure/storage/account#az-storage-account-update). Bu örnek ayrıca sorgular anahtar kasası URI'si ve anahtar sürümü için hangi değerlerin her ikisini de anahtar depolama hesabıyla ilişkilendirmek için gereklidir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
key_version=$(az keyvault key list-versions \
    --name <key> \
    --vault-name <key-vault> \
    --query [].kid \
    --output tsv | cut -d '/' -f 6)
az storage account update 
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-version $key_version \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

## <a name="update-the-key-version"></a>Anahtar sürümü güncelleştir

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabının güncelleştirilmesi gerekir. İlk olarak, anahtar kasası için URI çağırarak sorgu [az keyvault show](/cli/azure/keyvault#az-keyvault-show), çağırarak anahtar sürümü [az keyvault anahtar listesi sürümleri](/cli/azure/keyvault/key#az-keyvault-key-list-versions). Ardından çağırın [az depolama hesabını güncelleştirme](/cli/azure/storage/account#az-storage-account-update) önceki bölümde gösterildiği gibi anahtarın yeni sürümü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure depolama şifrelemesi](storage-service-encryption.md) 
- [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
