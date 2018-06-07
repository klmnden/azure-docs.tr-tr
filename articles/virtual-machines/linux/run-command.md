---
title: Azure üzerinde bir Linux VM Kabuk komut dosyalarını çalıştır
description: Bu konu, komut dosyalarını çalıştır komutunu kullanarak bir Azure Linux sanal makine içinde çalıştırmayı açıklar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6f21452ddc6c8a48392d24615e8dbcbba8b996c8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661120"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Kabuk komut dosyalarını çalıştır komutu ile Linux VM ile Çalıştır

Çalıştırma komutu Kabuk komut dosyalarını bir ağ bağlantısı bakılmaksızın Azure Linux VM içinde çalıştırmanıza olanak sağlar. Bu komut dosyalarını genel makine ya da uygulama yönetimi için kullanılan ve hızla tanılamak ve VM erişimi ve ağ sorunları çözmek ve VM iyi bir durumda almak için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize erişmek için kullanılan birden fazla seçeneği vardır. Çalıştır komutunu ağ bağlantısı bağımsız olarak, sanal makinelerde çalıştırabilir ve (yükleme gerekli) varsayılan olarak kullanılabilir. Azure Portalı aracılığıyla çalıştırma komutu kullanılabilir [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Bu özellik, bir komut dosyası etkileşimindeki sanal makineleri çalıştırmak istiyorsanız ve sorun giderme ve hatalı ağa veya yönetici kullanıcı nedeniyle ağa bağlı olmayan bir sanal makine düzeltmek için yalnızca yollarından biri, tüm senaryolarda kullanışlıdır yapılandırma.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutunu kullanırken mevcut olan kısıtlamaları listesi verilmiştir.

* Çıktı son 4096 bayt sınırlıdır
* Yaklaşık 20 saniye bir komut dosyasını çalıştırmak için en düşük saat
* Linux üzerinde yükseltilmiş kullanıcı olarak çalışacak komut dosyaları
* Bir defada tek bir betik çalıştırabilir
* Çalışan bir komut iptal edilemez
* Bir komut dosyası çalıştırabilirsiniz en uzun süre 90 hangi BT sonra zaman aşımına uğrar, dakikadır

## <a name="azure-cli"></a>Azure CLI

Aşağıda bir örnek verilmiştir kullanarak [az vm Çalıştır-command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) bir Azure Linux VM'de bir kabuk betiği çalıştırmak için komutu.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

## <a name="azure-portal"></a>Azure portalına

Bir VM gidin [Azure](https://portal.azure.com) seçip **komutu çalıştırın** altında **OPERATIONS**. VM üzerinde çalıştırmak için kullanılabilir komutların listesini ile sunulur.

![Komut listesini çalıştırın.](./media/run-command/run-command-list.png)

Bir komut çalıştırmak için seçin. Bazı komutlar, isteğe bağlı veya gerekli giriş parametresi olabilir. Bu komutları için parametreleri metin alanları için giriş değerleri sağlamanızı olarak sunulur. Her komut genişleterek çalıştırılmakta olan komut dosyası görüntüleyebilirsiniz **görüntülemek betik**. **RunPowerShellScript** , kendi özel bir komut dosyası sağlamanıza olanak verir gibi diğer komutlarından farklıdır. 

> [!NOTE]
> Yerleşik komutları düzenlenebilir değildir.

Komutu seçildikten sonra tıklayın **çalıştırmak** komut dosyasını çalıştırmak için. Betik çalışır ve tamamlandığında, çıkış ve hataları çıktı penceresinde döndürür. Aşağıdaki ekran görüntüsünde çalışan bir örnek çıkış şunları gösterir **ifconfig** komutu.

![Komut betiği çıktısını çalıştırın](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tablo Linux VM'ler için kullanılabilir komutların listesini gösterir. **RunShellScript** komutu, istediğiniz özel komut dosyaları çalıştırmak için kullanılabilir.

|**Ad**|**Açıklama**|
|---|---|
|**RunShellScript**|Bir Linux Kabuk betiği yürütür.|
|**ifconfig**| Tüm ağ arabirimlerinin yapılandırmasını alın.|

## <a name="limiting-access-to-run-command"></a>Komutu Çalıştır erişimi sınırlandırma

Çalıştırma komutları listeleme veya bir komut ayrıntılarını gösteren gerektiren `Microsoft.Compute/locations/runCommands/read` izni olan yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol ve daha yüksek.

Bir komut çalıştırmak gerektirir `Microsoft.Compute/virtualMachines/runCommand/action` izni, hangi [katkıda bulunan](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve daha yüksek.

Aşağıdakilerden birini kullanabilirsiniz [yerleşik](../../role-based-access-control/built-in-roles.md) rolleri veya oluşturma bir [özel](../../role-based-access-control/custom-roles.md) rol Çalıştır komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bakın, [Linux VM'NİZDE komut dosyalarını Çalıştır](run-scripts-in-vm.md) betikleri ve komutları uzaktan, VM'yi çalıştırmak için diğer yolları hakkında bilgi edinmek için.
