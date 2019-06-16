---
title: Azure İzleyici CLI hızlı başlangıç örnekleri
description: Azure İzleyici'özellikleri için örnek CLI komutları. Azure İzleyici uyarı bildirimleri gönderecek, yapılandırılmış telemetri verileri ve otomatik ölçeklendirme bulut Hizmetleri, sanal makineler ve Web Apps değerlerine göre web URL'leri çağrı olanak tanıyan bir Microsoft Azure hizmetidir.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: robb
ms.subservice: ''
ms.openlocfilehash: fa3293346fee6f6666db01dab5587dd760df84b2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60740892"
---
# <a name="azure-monitor-cli-quick-start-samples"></a>Azure İzleyici CLI hızlı başlangıç örnekleri
Bu makalede, örnek Azure İzleyicisi özelliklerine erişmenize yardımcı olması için komut satırı arabirimi (CLI) komutlarını gösterilmektedir. Azure İzleyici otomatik ölçeklendirme bulut Hizmetleri, sanal makineler ve Web uygulamaları ve uyarı bildirimleri gönderecek veya web URL'leri yapılandırılmış telemetri verilerinin değerlerine göre arama için sağlar.

## <a name="prerequisites"></a>Önkoşullar

Azure CLI'yi henüz yüklemediyseniz, yönergeleri izleyin [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Ayrıca [Azure Cloud Shell](/azure/cloud-shell) CLI'yi tarayıcınızda etkileşimli deneyim olarak çalıştırılacak. Tüm kullanılabilir komutların tam bir işinize yarayacak [Azure İzleyici CLI başvuru](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
İlk adımı Azure hesabınızda oturum açın.

```azurecli
az login
```

Bu komutu çalıştırdıktan sonra ekrandaki yönergeleri aracılığıyla imzalamanız gerekir. Tüm komutlar, varsayılan aboneliğinizde bağlamında çalışır.

Geçerli aboneliğiniz ile ilgili ayrıntıları listelemek için aşağıdaki komutu kullanın.

```azurecli
az account show
```

Farklı bir aboneliğe çalışma bağlamı değiştirmek için aşağıdaki komutu kullanın.

```azurecli
az account set -s <Subscription ID or name>
```

Desteklenen Azure İzleyici komutların bir listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Bir abonelik için etkinlik günlüğü görüntüle

Etkinlik günlüğü olayları bir listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor activity-log list
```

Tüm kullanılabilir seçenekleri görmek için aşağıdakileri deneyin.

```azurecli
az monitor activity-log list -h
```

Bir kaynak grubu tarafından listesi günlükleri için bir örnek aşağıda verilmiştir

```azurecli
az monitor activity-log list --resource-group <group name>
```

Çağıran tarafından listesi günlükleri örneği

```azurecli
az monitor activity-log list --caller myname@company.com
```

Örnek bir tarih aralığındaki bir kaynak türü üzerinde çağıran tarafından listesi günlüklerine

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Uyarılarla çalışma 
> [!NOTE]
> Yalnızca (Klasik) uyarıları, CLI'de bu zaman desteklenir. 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>Bir kaynak grubunda uyarı (Klasik) kurallarını Al

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>Ölçüm uyarı (Klasik) kuralı oluşturma

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>Bir uyarı (Klasik) kuralını Sil

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Günlük profilleri

Günlük profilleriyle çalışma, bu bölümdeki bilgileri kullanın.

### <a name="get-a-log-profile"></a>Günlük profilini Al

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Bir günlük profili ile bekletme Ekle

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Elde tutma ve EventHub ile bir günlük profili Ekle

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

Tanılama ayarları ile çalışma için bu bölümdeki bilgileri kullanın.

### <a name="get-a-diagnostic-setting"></a>Tanılama ayarını Al

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-log-setting"></a>Tanılama Günlüğü ayarı oluşturma 

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

### <a name="delete-a-diagnostic-setting"></a>Tanılama ayarını Sil

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Otomatik Ölçeklendirme

Otomatik ölçeklendirme ayarları ile çalışma için bu bölümdeki bilgileri kullanın. Bu örnekler değiştirmeniz gerekir.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Bir kaynak grubu için otomatik ölçeklendirme ayarlarını alma

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Otomatik ölçeklendirme ayarları, bir kaynak grubunda ada göre alma

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını belirleme

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```

