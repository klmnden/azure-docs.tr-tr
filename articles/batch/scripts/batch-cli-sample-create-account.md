---
title: "Azure CLI betik örnek - Batch hesabı oluşturun. | Microsoft Docs"
description: "Azure CLI betik örnek - Batch hesabı oluşturma"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: fd2f4682a04c557b69bbfce115f41c54a96d462c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a>Azure CLI ile bir toplu işlem hesabı oluşturun

Bu komut dosyasını bir Azure Batch hesabı oluşturur ve hesabın nasıl çeşitli özellikleri sorgulanan ve güncelleştirilmiş gösterir.

## <a name="prerequisites"></a>Ön koşullar

Sağlanan yönergeleri kullanarak Azure CLI yükleme [Azure CLI Yükleme Kılavuzu'na](https://docs.microsoft.com/cli/azure/install-azure-cli)zaten yapmadıysanız,.

## <a name="batch-account-sample-script"></a>Toplu işlem hesabı örnek komut dosyası

Varsayılan olarak, bir toplu işlem hesabı oluşturduğunuzda, işlem düğümlerini Batch hizmeti tarafından dahili olarak atanır. Hesap ya da paylaşılan anahtar kimlik bilgileri veya bir Azure Active Dirctory belirteci aracılığıyla doğrulanabilir ve ayrılmış işlem düğümleri ayrı çekirdek kotası tabi olacaktır.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Toplu işlem hesabı kullanıcı abonelik örnek komut dosyası kullanma

Toplu işlem düğümlerinden kendi Azure aboneliği oluşturmak için tercih edebilirsiniz.
Allocate hesapları işlem düğümlerin aboneliğinizi içine bir Azure Active Directory token kimlik doğrulaması gerekir ve ayrılan işlem düğümleri abonelik kotanızı sayılacaktır. Bu modda bir hesap oluşturmak için bir anahtar kasası başvuru hesabı oluştururken belirtmeniz gerekir.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Yukarıdaki örnek betikler birini çalıştırdıktan sonra kaynak grubunu kaldırmak için aşağıdaki komutu çalıştırın ve ilişkili tüm kaynaklar (toplu işlem hesabı, Azure depolama hesapları ve Azure anahtar kasalarını dahil).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, toplu işlem hesabı ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az toplu işlem hesabı oluşturun](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_create) | Toplu işlem hesabı oluşturur.  |
| [az toplu işlem hesabı ayarlama](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_set) | Batch hesabı özelliklerini güncelleştirir.  |
| [az toplu işlem hesabı Göster](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_show) | Belirtilen toplu işlem hesabı ayrıntılarını alır.  |
| [az batch hesabı anahtarları listesi](https://docs.microsoft.com/cli/azure/batch/account/keys#az_batch_account_keys_list) | Belirtilen toplu işlem hesabı erişim anahtarları alır.  |
| [az toplu işlem hesabı oturum açma](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Daha fazla CLI etkileşim için belirtilen toplu işlem hesabı karşı doğrular.  |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Bir depolama hesabı oluşturur. |
| [az keyvault oluşturma](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Bir anahtar kasası oluşturur. |
| [az keyvault-ilkesini ayarlama](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Belirtilen anahtar kasası güvenlik ilkesini güncelleştirin. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek toplu CLI kod örnekleri bulunabilir [Azure Batch CLI belgelerine](../batch-cli-samples.md).
