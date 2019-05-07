---
title: Azure depolama - blok blob depolama hesabı oluşturma | Microsoft Docs
description: Premium performans özellikleri bir Azure blok blob depolama hesabı oluşturma işlemi gösterilmektedir.
author: tamram
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9d8fb8f5f470dc47088efb30b7f823a0b8c624c8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141011"
---
# <a name="create-a-block-blob-storage-account"></a>Blok blobu depolama hesabı oluşturma

Blok blob depolama hesabı türü, blok blobları ile premium performans özellikleri oluşturmanızı sağlar. Bu depolama hesabı türü, yüksek işlem hızları sahip iş yükleri için optimize edilmiştir veya çok hızlı erişim zamanları gerektirir. Bu makalede, Azure portalı, Azure CLI veya Azure PowerShell kullanarak bir blok blob depolama hesabı oluşturma işlemini gösterir.

Blok blob depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="create-account-in-the-azure-portal"></a>Azure portalında hesabı oluşturma

Azure portalında bir blok blob depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında **tüm hizmetleri** > **depolama** Kategori > **depolama hesapları**.

1. Altında **depolama hesapları**seçin **Ekle**.

1. İçinde **abonelik** alan, depolama hesabının oluşturulacağı aboneliği seçin.

1. İçinde **kaynak grubu** alan, mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur**, yeni kaynak grubu için bir ad girin.

1. İçinde **depolama hesabı adı** hesabı için bir ad girin. Aşağıdakilere dikkat edin:

   - Ad Azure genelinde benzersiz olmalıdır.
   - Ad üç ile 24 arasında olmalıdır. karakter uzunluğunda.
   - Ad yalnızca sayılar ve küçük harfler içerebilir.

1. İçinde **konumu** alan, depolama hesabı için bir konum seçin veya varsayılan konumu kullanın.

1. Ayarların geri kalanı için aşağıdakileri yapılandırın:

   |Alan     |Değer  |
   |---------|---------|
   |**Performans**    |  Seçin **Premium**.   |
   |**Hesap türü**    | Seçin **BlockBlobStorage**.      |
   |**Çoğaltma**    |  Varsayılan ayarını bırakın **yerel olarak yedekli depolama (LRS)**.      |

   ![Portal bir blok blob depolama hesabı oluşturmak için kullanıcı Arabirimi gösterir](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Seçin **gözden + Oluştur** depolama hesabı ayarlarını gözden geçirmek için.

1. **Oluştur**’u seçin.

## <a name="create-account-using-azure-powershell"></a>Azure PowerShell kullanarak hesabı oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. (Yönetici olarak çalıştır) yükseltilmiş Windows PowerShell oturumu açın.

1. En son sürümünü emin olmak için aşağıdaki komutu çalıştırın `Az` PowerShell modülünün yüklü.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Yeni bir PowerShell konsolu ve Azure hesabınızla oturum açın.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. Tırnak işaretleri değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. Blok blob depolama hesabı oluşturun. Tırnak işaretleri değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="create-account-using-azure-cli"></a>Azure CLI kullanarak hesabı oluşturma

Azure CLI kullanarak bir blok blob hesabı oluşturmak için Azure CLI v yüklemeniz gerekir. 2.0.46 veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

1. Azure aboneliğinizde oturum açın.

   ```azurecli
   az login
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. (Köşeli ayraçlar dahil) köşeli ayraçlardaki değerler değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. Blok blob depolama hesabı oluşturun. (Köşeli ayraçlar dahil) köşeli ayraçlardaki değerler değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
