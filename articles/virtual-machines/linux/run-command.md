---
title: Azure üzerinde bir Linux VM Kabuk komut dosyalarını çalıştır
description: Bu konu, komut dosyalarını çalıştır komutunu kullanarak bir Azure Linux sanal makine içinde çalıştırmayı açıklar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0e87243b4b6e8362cb840a6510c175d2712b8a1a
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285765"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Kabuk komut dosyalarını çalıştır komutu ile Linux VM ile Çalıştır

Bir Azure Linux VM dahilinde Kabuk komut dosyalarını çalıştırmak için VM Aracısı farklı çalıştır komutunu kullanır. Bu komut dosyalarını genel makine ya da uygulama yönetimi için kullanılan ve hızla tanılamak ve VM erişimi ve ağ sorunları çözmek ve VM iyi bir durumda almak için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize erişmek için kullanılan birden fazla seçeneği vardır. Çalıştır komutunu kullanarak uzaktan VM Aracısı, sanal makinelerde çalıştırabilir. Azure Portalı aracılığıyla çalıştırma komutu kullanılabilir [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Bu yetenek burada betik etkileşimindeki sanal makineleri çalıştırmak istediğiniz ve sorun giderme ve RDP sahip olmayan bir sanal makine düzeltmek için yalnızca yollarından biri, veya SSH bağlantı noktası açın hatalı ağ veya yönetici kullanıcı nedeniyle tüm senaryolarda kullanışlıdır yapılandırma.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutunu kullanırken mevcut olan kısıtlamaları listesi verilmiştir.

* Çıktı son 4096 bayt sınırlıdır
* Yaklaşık 20 saniye bir komut dosyasını çalıştırmak için en düşük saat
* Komut dosyaları varsayılan yükseltilmiş kullanıcı Linux üzerinde çalışır
* Bir defada tek bir betik çalıştırabilir
* Bilgi (etkileşimli mod) isteminde betikleri desteklenmez.
* Çalışan bir komut iptal edilemez
* Bir komut dosyası çalıştırabilirsiniz en uzun süre 90 hangi BT sonra zaman aşımına uğrar, dakikadır

## <a name="azure-cli"></a>Azure CLI

Aşağıda bir örnek verilmiştir kullanarak [az vm Çalıştır-command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) bir Azure Linux VM'de bir kabuk betiği çalıştırmak için komutu.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Farklı bir kullanıcı olarak komutları çalıştırmak için kullanabileceğiniz `sudo -u` kullanmak için bir kullanıcı hesabı belirtmek için.

## <a name="azure-portal"></a>Azure portalına

Bir VM gidin [Azure](https://portal.azure.com) seçip **komutu çalıştırın** altında **OPERATIONS**. VM üzerinde çalıştırmak için kullanılabilir komutların listesini ile sunulur.

![Komut listesini çalıştırın.](./media/run-command/run-command-list.png)

Bir komut çalıştırmak için seçin. Bazı komutlar, isteğe bağlı veya gerekli giriş parametresi olabilir. Bu komutları için parametreleri metin alanları için giriş değerleri sağlamanızı olarak sunulur. Her komut genişleterek çalıştırılmakta olan komut dosyası görüntüleyebilirsiniz **görüntülemek betik**. **RunShellScript** , kendi özel bir komut dosyası sağlamanıza olanak verir gibi diğer komutlarından farklıdır. 

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
