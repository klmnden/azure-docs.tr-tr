---
title: "Azure CLI komut dosyası örneği ile toplu işi - | Microsoft Docs"
description: "Azure CLI komut dosyası örneği ile toplu işi-"
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
ms.openlocfilehash: 73d93622d418359be421e043d0af4e4befc6f4b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Azure CLI ile Azure batch işleri çalıştırma

Bu komut dosyası toplu iş oluşturur ve bir dizi görev projeye ekler. Ayrıca, bir işi ve görevleri izleme gösterir. Son olarak, Batch hizmeti iş görevler hakkında bilgi için verimli bir şekilde sorgulama gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan yönergeleri kullanarak Azure CLI yükleme [Azure CLI Yükleme Kılavuzu'na](https://docs.microsoft.com/cli/azure/install-azure-cli)zaten yapmadıysanız,.
- Zaten yoksa, bir toplu işlem hesabı oluşturun. Bkz: [Azure CLI ile bir toplu işlem hesabı oluşturun](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) bir hesap oluşturan bir örnek komut dosyası için.
- Henüz yapmadıysanız bir başlangıç görevi çalıştırmak için bir uygulamayı yapılandırın. Bkz: [Azure CLI ile Azure Batch uygulamalarına ekleme](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) bir uygulama oluşturur ve Azure'a bir uygulama paketi yükler bir örnek komut dosyası.
- İşin çalışacağı havuzunu yapılandırın. Bkz: [Azure CLI ile yönetme Azure Batch havuzları](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) bir bulut hizmet yapılandırması veya bir sanal makine yapılandırması ile bir havuz oluşturur bir örnek komut dosyası için.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>İş temizleme

Yukarıdaki örnek komut dosyasını çalıştırdıktan sonra iş ve tüm görevleri kaldırmak için aşağıdaki komutu çalıştırın. Havuz ayrı olarak silinmesi gerektiğini unutmayın. Bkz: [Azure CLI ile yönetme Azure Batch havuzları](./batch-cli-sample-manage-pool.md) oluşturma ve havuzların silinmesi hakkında daha fazla bilgi.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları bir toplu işi ve görevleri oluşturmak üzere kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az toplu işlem hesabı oturum açma](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Toplu işlem hesabı karşı kimlik doğrulaması.  |
| [az toplu işlem işi oluşturma](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_create) | Bir toplu işi oluşturur.  |
| [az toplu işi ayarlama](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_set) | Toplu iş özelliklerini güncelleştirir.  |
| [az toplu işi Göster](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_show) | Belirtilen toplu iş ayrıntılarını alır.  |
| [az toplu görev oluşturma](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_create) | Belirtilen toplu iş bir görev ekler.  |
| [az toplu görev Göster](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_show) | Bir görev ayrıntılarını ve belirtilen toplu işinden alır.  |
| [az toplu görev listesi](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_list) | Belirtilen işlemle ilişkili görevleri listeler.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek toplu CLI kod örnekleri bulunabilir [Azure Batch CLI belgelerine](../batch-cli-samples.md).
