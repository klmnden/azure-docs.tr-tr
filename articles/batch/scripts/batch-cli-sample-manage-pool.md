---
title: "Azure CLI komut dosyası örneği - Batch havuzlarında yönetme | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - Batch havuzlarında yönetme"
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
ms.openlocfilehash: ae7eab97c1da1113b0248b74a9dd67de8ce49e36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Azure Batch havuzları Azure CLI ile yönetme

Bu komut dosyası oluşturmak ve Azure Batch hizmeti işlem düğümleri havuzlarının yönetmek için Azure CLI kullanılabilen araçların bazıları gösterir.

> [!NOTE]
> Bu örnek komutlarda Azure sanal makine oluşturun. Sanal makineleri çalıştıran hesabınıza ücretler tahakkuk. Bu ücretleri en aza indirmek için işiniz bittiğinde VM'ler silme örneği çalışıyor. Bkz: [havuzları temiz](#clean-up-pools).

Batch havuzları, bulut Hizmetleri Yapılandırması (yalnızca Windows) veya bir sanal makine yapılandırma (Windows ve Linux) iki şekilde yapılandırılabilir. Aşağıdaki örnek komut havuzları hem yapılandırmalarının ile nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan yönergeleri kullanarak Azure CLI yükleme [Azure CLI Yükleme Kılavuzu'na](https://docs.microsoft.com/cli/azure/install-azure-cli)zaten yapmadıysanız,.
- Zaten yoksa, bir toplu işlem hesabı oluşturun. Bkz: [Azure CLI ile bir toplu işlem hesabı oluşturun](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) bir hesap oluşturan bir örnek komut dosyası için.
- Henüz yapmadıysanız bir başlangıç görevi çalıştırmak için bir uygulamayı yapılandırın. Bkz: [Azure CLI ile Azure Batch uygulamalarına ekleme](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) bir uygulama oluşturur ve Azure'a bir uygulama paketi yükler bir örnek komut dosyası.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Bulut hizmeti yapılandırma örnek komut dosyası havuzuyla

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Sanal makine yapılandırma örnek betiği havuzuyla

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Havuzları Temizle

Yukarıdaki örnek komut dosyasını çalıştırdıktan sonra havuzu silmek için aşağıdaki komutu çalıştırın.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, Batch havuzları oluşturma ve aşağıdaki komutları kullanır.
Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az toplu işlem hesabı oturum açma](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Toplu işlem hesabı karşı kimlik doğrulaması.  |
| [az toplu uygulama Özet listesi](https://docs.microsoft.com/cli/azure/batch/application/summary#az_batch_application_summary_list) | Batch hesabında kullanılabilir uygulamaları listeleyin.  |
| [az batch havuzu oluşturma](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Sanal makineleri havuzu oluşturun.  |
| [az batch havuzu ayarlama](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Bir havuz özelliklerini güncelleştirir.  |
| [az batch havuzu düğüm Aracısı SKU listesi](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Liste kullanılabilir düğüm Aracısı SKU'ları ve görüntü bilgileri.  |
| [az batch havuzu yeniden boyutlandırma](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | Sanal makineleri belirtilen havuzunda çalışan sayısı yeniden boyutlandırın.  |
| [az batch havuzu Göster](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Bir havuz özelliklerini görüntüler.  |
| [az batch havuzu silme](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_delete) | Belirtilen havuzu silin.  |
| [az batch havuzu otomatik ölçeklendirme etkinleştir](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Bir havuz üzerinde otomatik ölçeklendirmeyi etkinleştirmek ve bir formül uygulayın.  |
| [az batch havuzu otomatik ölçeklendirme devre dışı bırak](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Bir havuz üzerinde otomatik ölçeklendirme devre dışı bırakın.  |
| [az toplu işlem düğüm listesi](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Belirtilen havuzu tüm işlem düğümünde listeleyin.  |
| [az toplu düğümü yeniden başlatma](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Belirtilen işlem düğümü yeniden başlatın.  |
| [az toplu düğümü Sil](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Listelenen düğümleri belirtilen havuzdan silin.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek toplu CLI kod örnekleri bulunabilir [Azure Batch CLI belgelerine](../batch-cli-samples.md).

