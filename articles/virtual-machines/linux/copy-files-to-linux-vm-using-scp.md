---
title: Azure Linux Vm'leri ile SCP gelen ve giden dosyaları taşıma | Microsoft Docs
description: Güvenli bir şekilde SCP ve SSH anahtar çifti kullanarak azure'da bir Linux sanal makinesi gelen ve giden dosyaları taşıyın.
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
ms.subservice: disks
ms.openlocfilehash: 7d5b2d2ee7e7320fb8bf91c8a62a0f46c403c977
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328774"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>SCP kullanarak bir Linux VM gelen ve giden dosyaları taşıma

Bu makalede, dosyaları iş istasyonunuzdan Azure Linux VM kadar veya bir Azure güvenli kopya (SCP) kullanarak Linux VM iş istasyonunuzu, aşağı taşımak gösterilmektedir. Dosyaları bir Linux VM iş istasyonunuzu arasında hızlı ve güvenli bir şekilde, taşıma, Azure altyapınızı yönetmek için önemlidir. 

Bu makale için bir Linux VM kullanarak azure'da dağıtılan ihtiyacınız [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ayrıca, yerel bilgisayarınızda bir SCP istemcisi gerekir. SSH üzerinde oluşturulmuş ve çoğu Linux ve Mac bilgisayarlar ve bazı Windows kabuklar varsayılan Bash kabuğunda dahil.

## <a name="quick-commands"></a>Hızlı komutlar

Linux VM'ye kadar bir dosyayı kopyalama

```bash
scp file azureuser@azurehost:directory/targetfile
```

Linux VM'den bir dosya kaydedin

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Örnek olarak, bir Azure yapılandırma dosyası biz Taşı bir Linux VM ve bir günlük dosyası dizini açılır kadar her ikisi de SCP ve SSH anahtarlarını kullanma.   

## <a name="ssh-key-pair-authentication"></a>SSH anahtar çifti kimlik doğrulaması

SCP SSH Aktarım katmanı için kullanır. SSH kimlik doğrulaması hedef konakta işler ve varsayılan olarak, SSH ile sağlanan şifrelenmiş bir tünel dosyayı taşır. SSH kimlik doğrulaması için kullanıcı adları ve parolalar kullanılabilir. Ancak, SSH ortak ve özel anahtar doğrulaması güvenlik açısından en iyisi önerilir. SSH bağlantı doğrulandıktan sonra dosya kopyalamayı SCP sonra başlar. Düzgün bir şekilde yapılandırılmış kullanarak `~/.ssh/config` ve yalnızca bir sunucu adı (veya IP adresi) kullanarak SSH ortak ve özel anahtarlar, SCP bağlantısı kurulabilir. Yalnızca bir SSH anahtarı varsa, SCP içinde arar `~/.ssh/` dizini ve varsayılan olarak sanal Makineye oturum açmak için kullanır.

Yapılandırma hakkında daha fazla bilgi için `~/.ssh/config` ve SSH ortak ve özel anahtarları [SSH anahtarları oluşturma](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-to-a-linux-vm"></a>SCP bir Linux VM için bir dosya

İlk örnek için biz Otomasyon dağıtmak için kullanılan bir Linux VM kadar bir Azure yapılandırma dosyasını kopyalayın. Güvenlik önemlidir, çünkü bu dosya, gizli dizileri içeren Azure API kimlik bilgilerini içerir. SSH tarafından sağlanan şifrelenmiş bir tünel dosyasının içeriğini korur.

Aşağıdaki komut yerel kopyalar *.azure/config* dosya FQDN ile bir Azure VM'ye *myserver.eastus.cloudapp.azure.com*. Azure sanal makinesinde yönetici kullanıcı adı *azureuser*. Dosya için hedeflenen */home/azureuser/* dizin. Bu komutta kendi değerlerinizi yerleştirin.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP bir dizinden bir Linux VM

Bu örnekte, iş istasyonunuzu aşağı Linux VM'den günlük dosyalarının bir dizine kopyalayın. Bir günlük dosyası olabilir veya hassas veya gizli verileri içerebilir. Ancak, SCP'yi kullanarak günlük dosyalarını içeriğini şifrelenmiş sağlar. Dosya aktarımı için SCP'yi kullanarak, ayrıca güvenli olmanın yanı sıra günlük dizini ve iş istasyonunuzu aşağı dosyaları almak için kolay bir yoludur.

Aşağıdaki komut dosyalarında kopyalar */home/azureuser/logs/* Azure VM'deki yerel tmp dizininde dizin:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

`-r` Cli bayrağı, yinelemeli olarak kopyalama komut dosyaları ve dizinleri dizinin noktasından listelenen SCP bildirir.  Ayrıca komut satırı sözdizimi benzer olduğunu fark bir `cp` kopyalama komutu.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar, SSH ve onay yönetmek veya VMAccess uzantısını kullanarak Azure Linux sanal makinelerinde diskleri onarma](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
