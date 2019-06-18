---
title: Bash Azure Cloud Shell hızlı başlangıçta | Microsoft Docs
description: Cloud shell'de Bash için hızlı başlangıç
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: b8f96de7214a46c9e38182c141343a46c0e28139
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199626"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Azure Cloud shell'de Bash için hızlı başlangıç

Azure Cloud Shell'de Bash kullanma ayrıntılı [Azure portalında](https://ms.portal.azure.com/).

> [!NOTE]
> A [Azure Cloud shell'de PowerShell](quickstart-powershell.md) hızlı başlangıç, ayrıca kullanılabilir.

## <a name="start-cloud-shell"></a>Cloud Shell'i Başlat
1. Başlatma **Cloud Shell** Azure portalının üst gezinti bölmesinden. <br>
![](media/quickstart/shell-icon.png)

2. Bir depolama hesabının oluşturulacağı bir abonelik seçin ve Microsoft Azure dosyaları paylaşın.
3. "Depolama oluştur" seçeneğini belirleyin

> [!TIP]
> Azure CLI için her oturumda otomatik olarak doğrulanır.

### <a name="select-the-bash-environment"></a>Bash ortamı seçin
Ortam açılan sol tarafı Kabuk penceresinin bildiren bir onay `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Aboneliğinizi ayarlama
1. Liste abonelikler erişebilirsiniz.
   ```azurecli-interactive
   az account list
   ```

2. Tercih edilen aboneliğinizi ayarlayın: <br>
```azurecli-interactive
az account set --subscription 'my-subscription-name'
```

> [!TIP]
> Aboneliğinizi kullanarak sonraki oturumlar için anımsanır `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
İçinde WestUS "MyRG" adlı yeni bir kaynak grubu oluşturun.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Linux VM oluşturma
Yeni kaynak grubunda Ubuntu sanal makinesi oluşturun. Azure CLI, SSH anahtarları oluşturma ve bunları VM ayarlayın. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Kullanarak `--generate-ssh-keys` oluşturmak ve sanal makinenizin genel ve özel anahtarları ayarlamak için Azure CLI'yı bildirir ve `$Home` dizin. Varsayılan olarak Cloud shell'de anahtarları yerleştirilir `/home/<user>/.ssh/id_rsa` ve `/home/<user>/.ssh/id_rsa.pub`. `.ssh` Klasörü, bağlı dosya paylaşımının 5 GB'lık görüntüde kalıcı hale getirmek için kullanılan kalıcı `$Home`.

Bu VM üzerinde kullanıcı adınızı Cloud Shell içinde kullanılan kullanıcı adı olacaktır ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH Linux vm'nize
1. Azure portal arama çubuğunda sanal Makinenizin adını arayın.
2. VM adı ve genel IP adresini almak için "Bağlan" düğmesine tıklayın. <br>
   ![](media/quickstart/sshcmd-copy.png)

3. SSH ile VM'ye `ssh` cmd'yi
   ```
   ssh username@ipaddress
   ```

SSH bağlantısı kurulduktan sonra Ubuntu Hoş Geldiniz istemini görmeniz gerekir. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
1. Çıkış, ssh oturumu.
   ```azurecli-interactive
   exit
   ```

2. Kaynak grubunuzu ve içerdiği tüm kaynakları silin.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Cloud Shell'deki Bash hizmetinde kalıcı dosyaları hakkında bilgi edinin](persisting-shell-storage.md) <br>
[Azure CLI hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure dosya depolama hakkında bilgi edinin](../storage/files/storage-files-introduction.md) <br>
