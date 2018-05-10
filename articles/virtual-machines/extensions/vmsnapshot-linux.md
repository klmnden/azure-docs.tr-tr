---
title: Azure Backup için VM anlık görüntü Linux uzantısı | Microsoft Docs
description: Uygulama tutarlı sanal makine Azure VM anlık görüntü uzantısını kullanarak Yedekleme'den yedekleyin
services: backup, virtual-machines-linux
documentationcenter: ''
author: trinadhk
manager: jeconnoc
editor: ''
ms.assetid: 57759670-0baa-44db-ae14-8cdc00d3a906
ms.service: backup, virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: trinadhk
ms.openlocfilehash: bed5716b6d4ea6d81214a95d0f2360f359048893
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="vm-snapshot-linux-extension-for-azure-backup"></a>Azure Backup için VM anlık görüntü Linux uzantısı

## <a name="overview"></a>Genel Bakış

Azure Backup, şirket içi iş yüklerini yedeklemeye Bulut ve bulut kaynakları kurtarma Hizmetleri kasasına yedekleme için destek sağlar. Azure yedekleme VM anlık görüntü uzantısı bir uygulama tutarlı yedekleme gerek kalmadan Azure sanal makinesinin VM kapatma almak için kullanır. VM anlık görüntü Linux uzantısı yayımlanan ve Azure Backup hizmeti bir parçası olarak Microsoft tarafından desteklenmiyor. Azure Backup uzantısı yedekleme etkinleştirme ilk zamanlanmış yedekleme tetiklenen post bir parçası olarak yükler. Bu belge, desteklenen platformlar, yapılandırmaları ve VM anlık görüntü uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi
Desteklenen işletim sistemlerinin listesi için lütfen bakın [Azure yedekleme tarafından desteklenen işletim sistemleri](../../backup/backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup)

### <a name="internet-connectivity"></a>İnternet bağlantısı

VM anlık görüntü uzantısı biz sanal makinenin yedeğini zaman hedef sanal makine internet'e bağlı gerektirir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON VM anlık görüntü uzantısı için şemayı gösterir. Görev Kimliği uzantısı gerektirir - bu VM, anlık görüntü tetiklemiş yedekleme işi tanımlar durum blob URI'si anlık görüntü işlemi durumunu yazıldığı, - zamanlanmış anlık görüntü başlangıç saati, günlükleri blob URI'si - karşılık gelen günlükleri görev burada anlık görüntü yazılır, objstr-VM diskleri ve meta veri gösterimi.  Bu ayarları hassas veriler olarak değerlendirilmesi için bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi. Bu ayarlar yalnızca yedekleme işinin bir parçası Azure yedekleme Hizmeti'nden geçirilecek önerilir unutmayın.

```json
{
  "type": "extensions",
  "name": "VMSnapshot",
  "location":"<myLocation>",
  "properties": {
    "publisher": "Microsoft.RecoveryServices",
    "type": "VMSnapshot",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "locale":"<location>",
      "taskId":"<taskId used by Azure Backup service to communicate with extension>",
      "commandToExecute": "snapshot",
      "commandStartTimeUTCTicks": "<scheduled start time of the snapshot task>",
      "vmType": "microsoft.compute/virtualmachines"
    },
    "protectedSettings": {
      "objectStr": "<blob SAS uri represenattion of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Taskıd | e07354cf-041e-4370-929f-25a319ce8933_1 | dize |
| commandStartTimeUTCTicks | 6.36458E + 17 | dize |
| Yerel ayar | en-us | dize |
| objectStr | SAS URI'si dizisi "blobSASUri" kodlaması: ["https:\/\/sopattna5365.blob.core.windows.net\/VHD'ler\/vmubuntu1404ltsc201652903941.vhd? sv 2014-02-14 = & sr = b & SIG = TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA % 3B & st = 2017 11 %09T14 %3A23 3A28Z & se 2017 11 %09T17 %3A38 3A28Z = & sp rw = "," https:\/\/sopattna8461.blob.core.windows.net\/VHD'ler\/vmubuntu1404ltsc 20160629 122418.vhd? sv 2014-02-14 = & sr = b & SIG = 5S0A6YDWvVwqPAkzWXVy % 2BS % 2FqMwzFMbamT5upwx05v8Q % 3B & st = 2017 11 %09T14 %3A23 3A28Z & se 2017 11 %09T17 %3A38 3A28Z = & sp rw = "," https:\/ \/ sopattna8461.BLOB.Core.Windows.NET\/bootdiagnostics-vmubuntu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc 20160629 122541.vhd? sv 2014-02-14 = & sr = b & SIG = X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4% 3B & st = 2017 11 %09T14 %3A23 3A28Z & se 2017 11 %09T17 %3A38 3A28Z = & sp rw = "," https:\/\/sopattna5365.blob.core.windows.net\/VHD'ler\/vmubuntu1404ltsc 20160701 163922.vhd? sv 2014-02-14 = & sr = b & SIG = oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r % 2BC % 2BNIAork % 3B & st = 2017 11 %09T14 %3A23 3A28Z & se 2017 11 %09T17 %3A38 3A28Z = & sp rw = "," https:\/ \/ sopattna5365.BLOB.Core.Windows.NET\/VHD'ler\/vmubuntu1404ltsc 20170705 124311.vhd? sv 2014-02-14 = & sr = b & SIG = ZUM9d28Mvvm % 2FfrhJ71TFZh0Ni90m38bBs3zMl % %2FQ9rs0 3B & st 2017 11 %09T14 %3A23 3A28Z = & se 2017 11 %09T17 %3A38 3A28Z = & sp rw = "] | dize |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | dize |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | dize |



## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Ancak, bir sanal makine için bir VM anlık görüntü uzantısı eklemek önerilen sanal makinedeki yedekleme etkinleştirerek yoludur. Bu bir Resource Manager şablonu aracılığıyla gerçekleştirilebilir.  Bir sanal makinede yedekleme sağlayan örnek Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, bir sanal makinede yedeklemeyi etkinleştirmek için kullanılabilir. Yedeklemeyi etkinleştir POST, ilk zamanlanmış yedekleme işi VM'de Vm anlık görüntü uzantısı yükler.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure CLI kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure CLI kullanarak şu komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısını aşağıdaki dosyasına kaydedilir:

```
/var/log/waagent.log
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

Sorun giderme bilgileri bulunur [sorun giderme kılavuzu Azure VM backup](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
