---
title: Azure Backup için VM anlık görüntüsü Windows uzantısı | Microsoft Docs
description: Uygulamayla tutarlı yedekleme sanal makinenin Azure VM anlık görüntü uzantısını kullanarak yedek Al
services: backup, virtual-machines-windows
documentationcenter: ''
author: trinadhk
manager: jeconnoc
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 12/17/2018
ms.author: trinadhk
ms.openlocfilehash: 0392a187bf40e1fe35053b493733c7e89aa6969e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799425"
---
# <a name="vm-snapshot-windows-extension-for-azure-backup"></a>VM anlık görüntüsü Windows Azure Backup uzantısı

Azure Backup, şirket içi iş yüklerinin yedeklenmesi için bulut ve bulut kaynakları kurtarma Hizmetleri kasasına yedeklenmesi için destek sağlar. Azure Backup, VM kapatmaya gerek kalmadan Azure sanal makinesinin bir uygulamayla tutarlı yedekleme gerçekleştirilecek VM anlık görüntü uzantısını kullanır. VM anlık görüntüsü uzantısı yayımlandı ve Azure Backup hizmeti bir parçası olarak Microsoft tarafından desteklenmiyor. Azure yedekleme, yedekleme etkinleştirme ilk zamanlanmış yedekleme tetiklenen post bir parçası olarak uzantıyı yükler. Bu belge, desteklenen platformlar, yapılandırmaları ve VM anlık görüntüsü uzantısı için dağıtım seçenekleri açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi
Desteklenen işletim sistemlerinin listesi için başvurmak [Azure Backup tarafından desteklenen işletim sistemleri](../../backup/backup-azure-arm-vms-prepare.md#before-you-start)

### <a name="internet-connectivity"></a>İnternet bağlantısı

VM anlık görüntüsü uzantısı şu sanal makinenin yedeğini alırken hedef sanal makineyi internet'e bağlı gerektirir.

## <a name="extension-schema"></a>Uzantı şeması

VM anlık görüntü uzantısı için şema aşağıdaki JSON'u göstermektedir. Görev Kimliği uzantısının gerekiyor - bu VM'de anlık görüntü tetiklenen yedekleme işi tanımlar durum blob URI'si - anlık görüntü işlemi durumunu yazıldığı, zamanlanmış başlangıç zamanı anlık görüntünün, günlükleri blob URI'si - karşılık gelen günlükler görev burada anlık görüntü yazılır, VM diskleri ve meta verileri objstr-gösterimi.  Bu ayarlar, hassas veriler olarak değerlendirilip olduğundan, bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi. Bu ayarlar yalnızca yedekleme işinin parçası Azure Backup hizmetinden geçirilecek önerilir unutmayın.

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
      "objectStr": "<blob SAS uri representation of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| taskId | e07354cf-041e-4370-929f-25a319ce8933_1 | string |
| commandStartTimeUTCTicks | 6.36458E + 17 | string |
| yerel ayar | En-us | string |
| objectStr | SAS URI'si dizisi "blobSASUri" kodlama: ["https:\/\/sopattna5365.blob.core.windows.net\/VHD'ler\/vmwin1404ltsc201652903941.vhd? sv 2014-02-14 = & sr = b & sig = TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA % 3D & st = 2017-11-%09T14 %3A23 3A28Z & se = 2017-11-%09T17 %3A38 3A28Z & sp rw = "," https:\/\/sopattna8461.blob.core.windows.net\/VHD'ler\/vmwin1404ltsc 20160629 122418.vhd? sv 2014-02-14 = & sr = b & sig 5S0A6YDWvVwqPAkzWXVy 2BS % 2FqMwzFMbamT5upwx05v8Q % = 3B & st = 2017-11-%09T14 %3A23 3A28Z & se = 2017-11-%09T17 %3A38 3A28Z & sp rw = "," https:\/ \/ sopattna8461.BLOB.Core.Windows.NET\/bootdiagnostics-vmwintu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc 20160629 122541.vhd? sv 2014-02-14 = & sr = b & sig = X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4% 3D & st = 2017-11-%09T14 %3A23 3A28Z & se = 2017-11-%09T17 %3A38 3A28Z & sp rw = "," https:\/\/sopattna5365.blob.core.windows.net\/VHD'ler\/vmwin1404ltsc 20160701 163922.vhd? sv 2014-02-14 = & sr = b & sig oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r 2BC % 2BNIAork % = 3B & st = 2017-11-%09T14 %3A23 3A28Z & se = 2017-11-%09T17 %3A38 3A28Z & sp rw = "," https:\/ \/ sopattna5365.BLOB.Core.Windows.NET\/VHD'ler\/vmwin1404ltsc 20170705 124311.vhd? sv 2014-02-14 = & sr = b & sig ZUM9d28Mvvm 2FfrhJ71TFZh0Ni90m38bBs3zMl % %2FQ9rs0 = 3B & st = 2017-11-%09T14 %3A23 3A28Z & SE = 2017-11-%09T17 %3A38 3A28Z & sp rw = "] | string |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |



## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Ancak, bir sanal makine için bir VM anlık görüntü uzantısı ekleme önerilen sanal makinede yedekleme sağlayarak yoludur. Bu, bir Resource Manager şablonu aracılığıyla gerçekleştirilebilir.  Bir sanal makinede yedekleme örnek bir Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Galerisine](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, bir sanal makinede yedeklemeyi etkinleştirmek için kullanılabilir. Yedeklemeyi etkinleştir POST, ilk zamanlanmış yedekleme işini VM üzerinde Vm anlık görüntü uzantısı yükler.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

Sorun giderme bilgileri bulunabilir [Azure VM yedekleme sorunlarını giderme kılavuzu](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
