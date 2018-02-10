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
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 69431979769a03b62a7f9fd7760e6eb614e37cd6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Azure bulut Kabuk Bash'te için hızlı başlangıç

Bu belgede Azure bulut Kabuğu'nda Bash kullanmak nasıl ayrıntıları [Azure portal](https://ms.portal.azure.com/).

> [!NOTE]
> A [Azure bulut Kabuk PowerShell'de](quickstart-powershell.md) hızlı başlangıç kullanılabilir de.

## <a name="start-cloud-shell"></a>Bulut Kabuğu'nu başlatın
1. Başlatma **bulut Kabuk** Azure portal'ın üst gezinti gelen <br>
![](media/quickstart/shell-icon.png)
2. Bir depolama hesabı oluşturmak için bir abonelik seçin ve Microsoft Azure dosyaları paylaşma
3. "Depolama oluştur" seçeneğini belirleyin

> [!TIP]
> Azure CLI 2.0 için her oturumu otomatik olarak doğrulanır.

### <a name="select-the-bash-environment"></a>Bash ortamını seçin
1. Gelen ortam açılan seçin Kabuk penceresinin sol tarafındaki <br>
![](media/quickstart/env-selector.png)
2. Bash seçin

### <a name="set-your-subscription"></a>Aboneliğinizi
1. Liste abonelikler erişebilirsiniz: <br>
`az account list`
2. Tercih edilen aboneliğinizi ayarlayın: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Aboneliğinizi kullanarak sonraki oturumlar için hatırlanır `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
İçinde WestUS "MyRG" adlı yeni bir kaynak grubu oluşturun: <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Linux VM oluşturma
Bir Ubuntu VM, yeni kaynak grubu oluşturun. Azure CLI 2.0 SSH anahtarları oluşturma ve bunlarla VM ayarlayın. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> VM kimliğini doğrulamak için kullanılan ortak ve özel anahtarlar yerleştirilir `/home/<user>/.ssh/id_rsa` ve `/home/<user>/.ssh/id_rsa.pub` varsayılan olarak Azure CLI 2.0 tarafından. .Ssh klasörü, bağlı Azure dosya paylaşımının 5 GB görüntüde kalıcıdır.

Bulut Kabuğu'nda kullanılan kullanıcı adı, kullanıcı adı bu VM'de olacaktır ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH, Linux VM'NİZDE içine
1. Azure portal arama çubuğunda, VM adı arayın
2. "Bağlan"'ı tıklatın ve çalıştırın:`ssh username@ipaddress`

![](media/quickstart/sshcmd-copy.png)

SSH bağlantı kurulduktan sonra Ubuntu Hoş Geldiniz istemi görmeniz gerekir. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
Kaynak grubu ve içerdiği tüm kaynaklar Sil: <br>
`az group delete -n MyRG` öğesini çalıştırın

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk Bash'te için kalıcı dosyaları hakkında bilgi edinin](persisting-shell-storage.md) <br>
[Azure CLI 2.0 hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure dosyaları depolama hakkında bilgi edinin](../storage/files/storage-files-introduction.md) <br>
