---
title: Azure’da Linux VM’ler için SSH anahtar çifti oluşturma ve kullanma | Microsoft Docs
description: Nasıl oluşturun ve kimlik doğrulama işlemi güvenliğini artırmak için Azure Linux VM'ler için bir SSH genel-özel anahtar çifti kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: iainfou
ms.openlocfilehash: 137fb13ff286e5284b8e8910834913ec9f1d48a9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Hızlı adımlar: oluşturma ve Azure Linux VM'ler için bir SSH genel-özel anahtar çiftini kullanma
Güvenli kabuk (SSH) anahtar çiftiyle Azure’da sanal makineler (VM) oluşturabilirsiniz. Bu sayede kimlik doğrulaması için SSH anahtarlarını kullanarak oturum açmak için parolalara duyulan gereksinimi ortadan kaldırırsınız. Bu makalede hızlı bir şekilde oluşturmak ve SSH ortak özel anahtar dosyası çifti Linux VM'ler için nasıl kullanılacağını gösterir. Azure bulut Kabuk, bir macOS veya Linux ana bilgisayar, Linux Windows alt ve OpenSSH destekleyen diğer araçları ile adımları tamamlayabilirsiniz. 

Daha fazla arka plan ve örnekler için bkz: [SSH oluşturmak için ayrıntılı adımlar anahtar çiftleri](create-ssh-keys-detailed.md).

Oluşturma ve bir Windows bilgisayarda SSH anahtarları kullanmak üzere ek yöntemler için bkz: [SSH kullanma anahtarları azure'da Windows ile](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Kullanım `ssh-keygen` oluşturulan varsayılan olarak SSH ortak ve özel anahtar dosyaları oluşturmak için komutu `~/.ssh` dizin. Farklı bir konum ve ek bir parola (özel anahtar dosyası erişim için parola) ne zaman belirtebilirsiniz istenir. SSH anahtar çifti geçerli konumdan birinde varsa, bu dosyaların üzerine yazılır.

```bash
ssh-keygen -t rsa -b 2048
```

Kullanırsanız [Azure CLI 2.0](/cli/azure) VM oluşturmak için isteğe bağlı olarak SSH ortak ve özel anahtar dosyaları çalıştırarak oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--generate-ssh-keys` seçeneği. Anahtarlar ~/.ssh dizininde depolanır. Bu konumda zaten varsa bu komut seçeneği anahtarları üzerine yazmaz olduğunu unutmayın.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>SSH ortak anahtarı bir VM dağıtırken sağlayın
Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için SSH ortak anahtarınızı CLI, Azure portalını kullanarak VM oluşturulurken belirtin Resource Manager şablonları ya da diğer yöntemleri:

* [Azure portalıyla Linux sanal makine oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makine oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bir Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Bir SSH ortak anahtarı biçimiyle bilmiyorsanız, çalıştırarak ortak anahtarınız görebilirsiniz `cat` aşağıdaki gibi değiştirme `~/.ssh/id_rsa.pub` kendi ortak anahtar dosyası konumu:

```bash
cat ~/.ssh/id_rsa.pub
```

Azure portalında veya bir Resource Manager şablonunda kullanmak üzere ortak anahtar dosyasının içeriğini kopyalar ve yapıştırırsanız, fazladan boşluk kopyalamadığınızdan emin olun. MacOS kullanırsanız, örneğin, ortak anahtar dosyasını iletebildiğiniz (varsayılan olarak, `~/.ssh/id_rsa.pub`) için **pbcopy** içeriği kopyalamak için (aynı gibi şeyler da diğer Linux programlar **xclip**).

Depolanan varsayılan olarak, Azure Linux VM'de Yerleştir ortak anahtarı olan `~/.ssh/id_rsa.pub`tuşları oluşturduğunuz zaman konumu değiştirilip sürece. Kullanırsanız [Azure CLI 2.0](/cli/azure) var olan bir ortak anahtar ile VM oluşturmak için değer konumunu veya bu ortak anahtar çalıştırarak belirtmek [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--ssh-key-value` seçeneği. 

## <a name="ssh-to-your-vm"></a>SSH, VM
Azure VM'nizi dağıtılan ortak anahtar ve özel anahtarı yerel sisteminizde, IP adresi veya VM DNS adını kullanarak, VM SSH ile. Değiştir *azureuser* ve *myvm.westus.cloudapp.azure.com* aşağıdaki komut yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi ile):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola sağladıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

SSH anahtarları kullanılarak oluşturulan VM’ler, varsayılan olarak parola kullanımı devre dışı bırakılmış şekilde yapılandırılır; böylece parola tahminiyle gerçekleştirilen saldırı girişimleri önemli ölçüde daha pahalı ve zor hale getirilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Hızlı kullanım için basit bir SSH anahtar çifti oluşturma açıklanmaktadır. 

* SSH anahtar çifti ile çalışmak için daha fazla yardıma gereksinim duyarsanız, bkz: [oluşturmak ve SSH yönetmek için ayrıntılı adımlar anahtar çiftleri](create-ssh-keys-detailed.md).

* Bir Azure VM SSH bağlantı sorunlarınız varsa bkz [sorun giderme SSH bağlantı Azure Linux VM'de](troubleshoot-ssh-connection.md).


