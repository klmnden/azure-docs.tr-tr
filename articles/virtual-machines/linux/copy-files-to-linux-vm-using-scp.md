---
title: Azure Linux VM'ler SCP ile gelen ve giden dosyaları taşıma | Microsoft Docs
description: Güvenli bir şekilde SCP ve SSH anahtar çifti kullanarak azure'da bir Linux VM gelen ve giden dosyaları taşıyın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 0231e402848e617a46ca70470ba4d3272ace59f7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30904514"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>SCP'yi kullanarak bir Linux VM Taşı dosyaları

Bu makalede, Azure Linux VM'de kadar iş istasyonunuzu veya bir Azure güvenli kopyalama (SCP) kullanarak Linux VM, iş istasyonunuzu aşağıya doğru dosyaları taşıma gösterilmektedir. Hızlı ve güvenli bir şekilde, dosyalar, iş istasyonu ve bir Linux VM arasında taşıma Azure altyapınıza yönetmek için kritik öneme sahiptir. 

Bu makalede, bir Linux VM Azure kullanılarak dağıtılan gereksinim [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ayrıca, yerel bilgisayarınızda bir SCP istemcisi gerekir. SSH üzerinde oluşturulmuş ve çoğu Linux ve Mac bilgisayarların ve bazı Windows Kabukları varsayılan Bash kabuğunda dahil.

## <a name="quick-commands"></a>Hızlı komutlar

Linux VM kadar dosya kopyalama

```bash
scp file azureuser@azurehost:directory/targetfile
```

Linux VM'den aşağı dosya kopyalama

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Örnek olarak, size bir Azure yapılandırma dosyası taşımak bir Linux VM ve bir günlük dosyası dizini açılır kadar her ikisi de SCP ve SSH anahtarları kullanma.   

## <a name="ssh-key-pair-authentication"></a>SSH anahtar çifti kimlik doğrulaması

SCP SSH için Aktarım katmanı kullanır. SSH hedef ana bilgisayarda kimlik doğrulaması gerçekleştirir ve varsayılan olarak SSH ile sağlanan şifrelenmiş tüneli dosyasında taşır. SSH kimlik doğrulaması için kullanıcı adları ve parolalar kullanılabilir. Ancak, SSH ortak ve özel anahtar kimlik doğrulaması güvenlik açısından en iyisi önerilir. SSH bağlantı doğrulamadan sonra SCP sonra dosya kopyalama başlar. Düzgün şekilde yapılandırılmış kullanarak `~/.ssh/config` ve yalnızca bir sunucu adı (veya IP adresi) kullanarak SSH ortak ve özel anahtarlar, SCP bağlantı kurulabileceği. Yalnızca bir SSH anahtarı varsa, SCP içinde arar `~/.ssh/` dizini ve varsayılan olarak VM oturum açmak için kullanır.

Yapılandırma hakkında daha fazla bilgi için `~/.ssh/config` ve SSH ortak ve özel anahtarları bkz [oluşturma SSH anahtarları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-to-a-linux-vm"></a>SCP bir dosyaya bir Linux VM

İlk örnek için Otomasyon dağıtmak için kullanılan bir Linux VM kadar bir Azure yapılandırma dosyasını kopyalayın. Bu dosya gizli kod dizeleri içerir, Azure API kimlik bilgilerini içerdiğinden güvenlik önemlidir. SSH tarafından sağlanan şifrelenmiş tünel dosyasının içeriğini korur.

Aşağıdaki komut yerel kopyalar *.azure/config* FQDN ile bir Azure VM dosyasına *myserver.eastus.cloudapp.azure.com*. Yönetici kullanıcı adı Azure VM'de *azureuser*. Dosya hedeflenen */home/azureuser/* dizin. Bu komutta kendi değerlerinizi yerleştirin.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP bir dizinden bir Linux VM

Bu örnekte, iş istasyonunuzu aşağıya doğru Linux VM'den bir dizin günlük dosyaları kopyalayın. Bir günlük dosyası olabilir veya hassas veya gizli verileri içerebilir. Ancak, SCP'yi kullanarak günlük dosyalarının içerikleri şifrelenir sağlar. Dosya aktarımı için SCP'yi kullanarak, dosya, iş istasyonunuzu aşağı kaydırın ve günlük dizini de güvenli devam ederken almak için kolay bir yoludur.

Aşağıdaki komut dosyalarını kopyalar */home/azureuser/günlükleri/* yerel tmp dizininde Azure VM'ye dizininde:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

`-r` CLI bayrak dosyaları ve dizinleri dizinin noktasından listelenen komutta yinelemeli olarak kopyalanacak SCP söyler.  Ayrıca komut satırı sözdizimi benzer olduğunu fark bir `cp` kopyalama komutu.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar, SSH ve onay yönetmek veya VMAccess uzantısını kullanarak Azure Linux VM'ler disklerde onarın](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)