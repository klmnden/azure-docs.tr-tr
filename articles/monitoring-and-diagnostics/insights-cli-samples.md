---
title: Azure CLI 2.0 İzleyici hızlı başlangıç örnekleri. | Microsoft Docs
description: Azure İzleyicisi özelliklerine ilişkin örnek CLI 2.0 komutlar. Azure İzleyicisi uyarı bildirimleri gönderebilir, yapılandırılmış telemetri verilerini ve otomatik ölçeklendirme bulut Hizmetleri, sanal makinelerin ve Web uygulamaları değerlerine göre web URL'leri çağrı olanak sağlayan bir Microsoft Azure hizmetidir.
author: kamathashwin
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: ashwink
ms.openlocfilehash: 475ee794d2b67639b447a1ca66b12d3d589425cb
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-monitor-cli-20-quick-start-samples"></a>Azure CLI 2.0 İzleyici hızlı başlangıç örnekleri
Bu makalede, örnek Azure İzleyicisi özelliklerine erişmenize yardımcı olması için komut satırı arabirimi (CLI) komutları gösterir. Azure İzleyici otomatik ölçeklendirme bulut Hizmetleri, sanal makineleri ve Web uygulamaları ve uyarı bildirimleri gönderebilir veya web URL'leri yapılandırılmış telemetri verilerini değerlerine göre arama için sağlar.

## <a name="prerequisites"></a>Önkoşullar

Azure CLI henüz yüklemediyseniz, yönergeleri izleyin [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Aynı zamanda [Azure bulut Kabuk](/azure/cloud-shell) CLI tarayıcınızda etkileşimli bir deneyim olarak çalıştırmak için. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
İlk adım, Azure hesabınızda oturum açın.

```azurecli
az login
```

Bu komutu çalıştırdıktan sonra ekrandaki yönergeleri aracılığıyla oturum açmaya sahip. Tüm komutlar, varsayılan abonelik bağlamında çalışır.

Geçerli aboneliğiniz ayrıntılarını listelemek için aşağıdaki komutu kullanın.

```azurecli
az account show
```

Farklı bir aboneliğe çalışma bağlamını değiştirmek için aşağıdaki komutu kullanın.

```azurecli
az account set -s <Subscription ID or name>
```

Tüm desteklenen Azure İzleyici komutlarının listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Bir abonelik için etkinlik günlüğü görüntüle

Etkinlik günlüğü olaylarını listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor activity-log list
```

Tüm kullanılabilir seçenekleri görüntülemek için aşağıdakileri deneyin.

```azurecli
az monitor activity-log list -h
```

Bir kaynak grubu tarafından listesi günlüklerine örneği

```azurecli
az monitor activity-log list --resource-group <group name>
```

Liste günlükleri çağıran tarafından örneği

```azurecli
az monitor activity-log list --caller myname@company.com
```

Çağıran bir tarih aralığındaki bir kaynak türünde listesi günlüklerde örneği

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Uyarılarla çalışma

Uyarılarla çalışma için bölümündeki bilgileri kullanın.

### <a name="get-alert-rules-in-a-resource-group"></a>Uyarı kuralları bir kaynak grubunda Al

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-rule"></a>Ölçüm bir uyarı kuralı oluştur

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-rule"></a>Bir uyarı kuralını Sil

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Günlük profilleri

Günlük profilleri ile çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-log-profile"></a>Günlük profilini Al

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Bekletme günlüğü profiliyle ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Bekletme ve EventHub günlük profiliyle ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Tanılama

Tanılama ayarları ile çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-diagnostic-setting"></a>Tanılama bir ayar edinme

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-log-setting"></a>Bir tanılama günlük ayar oluşturun 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

### <a name="delete-a-diagnostic-setting"></a>Tanılama ayar silme

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Otomatik Ölçeklendirme

Otomatik ölçeklendirme ayarları ile çalışmak için bu bölümdeki bilgileri kullanın. Bu örnekler değiştirmeniz gerekir.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Bir kaynak grubu için otomatik ölçeklendirme ayarlarını al

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Otomatik ölçeklendirme ayarlarını adıyla bir kaynak grubunda Al

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını belirleme

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```
