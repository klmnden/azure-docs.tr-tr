---
title: "Azure bulut Kabuk hızlı başlangıcı bash | Microsoft Docs"
description: "Bulut Kabuk Bash'te için hızlı başlangıç"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: juluk
ms.openlocfilehash: e48c54216c5c4ae8e53d4802aafce8883ee97c11
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Azure bulut Kabuk Bash'te için hızlı başlangıç

Bu belgede Azure bulut Kabuğu'nda Bash kullanmak nasıl ayrıntıları [Azure portal](https://ms.portal.azure.com/).

> [!NOTE]
> A [Azure bulut Kabuk PowerShell'de](quickstart-powershell.md) hızlı başlangıç kullanılabilir de.

## <a name="start-cloud-shell"></a>Bulut Kabuğu'nu başlatın
1. Başlatma **bulut Kabuk** üst gezinti Azure Portalı'ndan. <br>
![](media/quickstart/shell-icon.png)

2. Bir depolama hesabı oluşturmak için bir abonelik seçin ve Microsoft Azure dosyaları paylaşın.
3. "Depolama oluştur" seçeneğini belirleyin

> [!TIP]
> Her oturum için Azure CLI 2.0 otomatik olarak doğrulanır.

### <a name="select-the-bash-environment"></a>Bash ortamını seçin
Ortamdan açılan taraftaki Kabuk penceresinin yazan onay `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Aboneliğinizi
1. Liste abonelikler erişebilirsiniz.
```azurecli-interactive
az account list
```

2. Tercih edilen aboneliğinizi ayarlayın: <br>
```azurecli-interactive
az account set --subscription my-subscription-name`
```

> [!TIP]
> Aboneliğinizi kullanarak sonraki oturumlar için hatırlanır `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
İçinde WestUS "MyRG" adlı yeni bir kaynak grubu oluşturun.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Linux VM oluşturma
Bir Ubuntu VM, yeni kaynak grubu oluşturun. Azure CLI 2.0 SSH anahtarları oluşturma ve bunlarla VM ayarlayın. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Kullanarak `--generate-ssh-keys` oluşturmak ve ortak ve özel anahtarlar, VM'deki ayarlamak için Azure CLI 2.0 bildirir ve `$Home` dizin. Varsayılan olarak bulut kabuğunda anahtarları yerleştirilir `/home/<user>/.ssh/id_rsa` ve `/home/<user>/.ssh/id_rsa.pub`. `.ssh` Klasörü, ekli dosya paylaşımının 5 GB görüntüde sürdürmek için kullanılan kalıcı `$Home`.

Bulut Kabuğu'nda kullanılan kullanıcı adı, kullanıcı adı bu VM'de olacaktır ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH, Linux VM'NİZDE içine
1. Azure portal arama çubuğunda, VM adını arayın.
2. Sanal makine adını ve genel IP adresi almak için "Bağlan" düğmesine tıklayın. <br>
![](media/quickstart/sshcmd-copy.png)

3. SSH VM ile içine `ssh` cmd
```
ssh username@ipaddress
```

SSH bağlantı kurulduktan sonra Ubuntu Hoş Geldiniz istemi görmeniz gerekir. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
1. Çıkış, ssh oturumu.
```azurecli-interactive
exit
```

2. Kaynak grubu ve içerdiği tüm kaynaklar silin.
```azurecli-interactive
Run `az group delete -n MyRG`
```

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk Bash'te için kalıcı dosyaları hakkında bilgi edinin](persisting-shell-storage.md) <br>
[Azure CLI 2.0 hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure dosyaları depolama hakkında bilgi edinin](../storage/files/storage-files-introduction.md) <br>
