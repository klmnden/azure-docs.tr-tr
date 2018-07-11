---
title: Azure’da Linux VM’ler için SSH anahtar çifti oluşturma ve kullanma | Microsoft Docs
description: Nasıl oluşturun ve kimlik doğrulaması işleminin güvenliğini artırmak için azure'da Linux VM'ler için SSH ortak-özel anahtar çifti kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 2e3d86d776f44c47a33bf075cf7f2140a3940e5e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928463"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Hızlı adımlar: oluşturma ve azure'da Linux VM'ler için SSH ortak-özel anahtar çifti kullanma
Güvenli kabuk (SSH) anahtar çiftiyle Azure’da sanal makineler (VM) oluşturabilirsiniz. Bu sayede kimlik doğrulaması için SSH anahtarlarını kullanarak oturum açmak için parolalara duyulan gereksinimi ortadan kaldırırsınız. Bu makalede hızlı bir şekilde oluşturmak ve Linux Vm'leri için SSH ortak-özel anahtar dosyası çifti kullanma gösterilmektedir. Bu adımları Azure Cloud Shell, macOS veya Linux ana bilgisayar, Linux için Windows alt sistemi ve OpenSSH destekleyen diğer araçlar ile tamamlayabilir. 

Daha fazla arka plan ve örnekler için bkz. [SSH oluşturmaya ilişkin ayrıntılı adımlar anahtar çifti](create-ssh-keys-detailed.md).

Oluşturma ve bir Windows bilgisayarda SSH anahtarları kullanma hakkında ek yollar için bkz. [azure'da Windows ile SSH kullanma anahtarları](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Kullanım `ssh-keygen` oluşturulan varsayılan olarak SSH ortak ve özel anahtar dosyaları oluşturmak için komutu `~/.ssh` dizin. Ne zaman farklı bir konum ve bir ek parola (özel anahtar dosyasına erişmek için parola) belirtebilirsiniz istenir. Geçerli konumu bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır.

```bash
ssh-keygen -t rsa -b 2048
```

Kullanırsanız [Azure CLI 2.0](/cli/azure) VM'nizi oluşturmak için isteğe bağlı olarak SSH ortak ve özel anahtar dosyaları çalıştırarak oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--generate-ssh-keys` seçeneği. Anahtarları ~/.ssh dizininde depolanır. Bu konumda zaten varsa bu komut seçeneği anahtarları üzerine yazmaz olduğunu unutmayın.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Bir VM dağıtılırken SSH ortak anahtarı sağlayın
Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için SSH ortak anahtarınız, CLI, Azure portalını kullanarak sanal makine oluştururken belirtin Resource Manager şablonları ya da diğer yöntemleri:

* [Azure portal ile Linux sanal makinesi oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makinesi oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

SSH ortak anahtarı biçimiyle ilgili bilgi sahibi değilseniz, çalıştırarak ortak anahtarınızı görebilirsiniz `cat` şu şekilde değiştirerek `~/.ssh/id_rsa.pub` kendi ortak anahtar dosyası konumunuz ile:

```bash
cat ~/.ssh/id_rsa.pub
```

Azure portalında veya bir Resource Manager şablonunda kullanmak üzere ortak anahtar dosyasının içeriğini kopyalar ve yapıştırırsanız, fazladan boşluk kopyalamadığınızdan emin olun. Örneğin, macOS kullanırsanız, ortak anahtar dosyasını kanal oluşturarak aktarabilirsiniz (varsayılan olarak, `~/.ssh/id_rsa.pub`) için **pbcopy** içeriklerini kopyalamak için (aşağıdaki gibi aynı şeyi yapan diğer Linux programları var. **xclip**).

Azure'da Linux vm'nize yerleştirdiğiniz ortak anahtar, depolanan varsayılan olarak `~/.ssh/id_rsa.pub`anahtarları oluşturduğunuz zaman konumu değiştirmediğiniz sürece. Kullanırsanız [Azure CLI 2.0](/cli/azure) mevcut bir ortak anahtar ile VM oluşturma için değer ya da bu ortak anahtarın konumunu çalıştırarak belirtin [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--ssh-key-value` seçeneği. 

## <a name="ssh-to-your-vm"></a>Sanal makinenize yönelik SSH
Ortak anahtarı, Azure VM üzerinde dağıtılmış ve yerel sisteminizde, IP adresini veya VM'nizi DNS adını kullanarak sanal makinenize yönelik SSH özel anahtarı ile. Değiştirin *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi) ile aşağıdaki komutta:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola sağladıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

SSH anahtarları kullanılarak oluşturulan VM’ler, varsayılan olarak parola kullanımı devre dışı bırakılmış şekilde yapılandırılır; böylece parola tahminiyle gerçekleştirilen saldırı girişimleri önemli ölçüde daha pahalı ve zor hale getirilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, hızlı kullanım için basit bir SSH anahtar çifti oluşturma açıklanmaktadır. 

* SSH anahtar çifti ile çalışmak için daha fazla yardıma ihtiyacınız varsa bkz [anahtar çifti oluşturma ve SSH yönetmek için ayrıntılı adımlar](create-ssh-keys-detailed.md).

* Azure VM'ye SSH bağlantısı ile ilgili sorunlar olup [sorun giderme SSH bağlantıları için bir Azure Linux VM](troubleshoot-ssh-connection.md).


