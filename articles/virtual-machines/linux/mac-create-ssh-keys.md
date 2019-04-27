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
ms.date: 09/11/2018
ms.author: cynthn
ms.openlocfilehash: d442d09c8c8ded3aa50faf74e28c8d95ded24a5e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614412"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Hızlı adımlar: Oluşturma ve azure'da Linux VM'ler için SSH ortak-özel anahtar çifti kullanma

Güvenli Kabuk (SSH) anahtar çiftiyle azure'da oturum açmak parola gereksinimini ortadan kimlik doğrulaması için SSH anahtarları kullanan sanal makineler (VM) oluşturabilirsiniz. Bu makalede hızlı bir şekilde oluşturmak ve Linux Vm'leri için SSH ortak-özel anahtar dosyası çifti kullanma gösterilmektedir. Bu adımları Azure Cloud Shell, macOS veya Linux ana bilgisayar, Linux için Windows alt sistemi ve OpenSSH destekleyen diğer araçlar ile tamamlayabilir. 

> [!NOTE]
> SSH anahtarları kullanılarak oluşturulan VM'ler, deneme yanılma saldırılarına saldırıları zorluk önemli ölçüde artıran devre dışı, parolaları ile yapılandırılmış varsayılan ' dir. 

Daha fazla arka plan ve örnekler için bkz. [SSH oluşturmaya ilişkin ayrıntılı adımlar anahtar çifti](create-ssh-keys-detailed.md).

Oluşturma ve bir Windows bilgisayarda SSH anahtarları kullanma hakkında ek yollar için bkz. [azure'da Windows ile SSH kullanma anahtarları](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Kullanım `ssh-keygen` SSH ortak ve özel anahtar dosyaları oluşturmak için komutu. Varsayılan olarak, bu dosyaları ~/.ssh dizininde oluşturulur. Farklı bir konum ve isteğe bağlı bir parola belirtin (*parola*) özel anahtar dosyasına erişmek için. Belirtilen konumda aynı ada sahip bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır.

Aşağıdaki komut, RSA şifreleme ve 2048 bit uzunluğu kullanarak SSH anahtar çifti oluşturur:

```bash
ssh-keygen -t rsa -b 2048
```

Kullanırsanız [Azure CLI](/cli/azure) VM'nizi oluşturmak için [az vm oluşturma](/cli/azure/vm#az-vm-create) komutunu kullanarak SSH ortak ve özel anahtar dosyaları oluşturun isteğe bağlı olarak `--generate-ssh-keys` seçeneği. Anahtar dosyaları ile aksi belirtilmediği sürece ~/.ssh dizinde depolanır `--ssh-dest-key-path` seçeneği. `--generate-ssh-keys` Seçeneği yerine bir hata döndürüyor, mevcut anahtar dosyaları değil üzerine yazılacak. Aşağıdaki komutta *VMname* ve *RGname* kendi değerlerinizle:

```azurecli
az vm create --name VMname --resource-group RGname --generate-ssh-keys 
```

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>Bir VM dağıtılırken bir SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için Azure portalı, Azure CLI, Azure Resource Manager şablonları ya da diğer yöntemleri kullanarak VM oluşturma sırasında SSH ortak anahtarınızı belirtin:

* [Azure portal ile Linux sanal makinesi oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makinesi oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

SSH ortak anahtarı biçimiyle ilgili bilgi sahibi değilseniz, aşağıdaki ortak anahtarınızı görüntüleyebilirsiniz `cat` komutuyla değiştirerek `~/.ssh/id_rsa.pub` yolunu ve gerekirse kendi ortak anahtar dosyasının dosya adı:

```bash
cat ~/.ssh/id_rsa.pub
```

Tipik bir ortak anahtar değeri şu örnekteki gibi görünür:

```
ssh-rsa AAAAB3NzaC1yc2EAABADAQABAAACAQC1/KanayNr+Q7ogR5mKnGpKWRBQU7F3Jjhn7utdf7Z2iUFykaYx+MInSnT3XdnBRS8KhC0IP8ptbngIaNOWd6zM8hB6UrcRTlTpwk/SuGMw1Vb40xlEFphBkVEUgBolOoANIEXriAMvlDMZsgvnMFiQ12tD/u14cxy1WNEMAftey/vX3Fgp2vEq4zHXEliY/sFZLJUJzcRUI0MOfHXAuCjg/qyqqbIuTDFyfg8k0JTtyGFEMQhbXKcuP2yGx1uw0ice62LRzr8w0mszftXyMik1PnshRXbmE2xgINYg5xo/ra3mq2imwtOKJpfdtFoMiKhJmSNHBSkK7vFTeYgg0v2cQ2+vL38lcIFX4Oh+QCzvNF/AXoDVlQtVtSqfQxRVG79Zqio5p12gHFktlfV7reCBvVIhyxc2LlYUkrq4DHzkxNY5c9OGSHXSle9YsO3F1J5ip18f6gPq4xFmo6dVoJodZm9N0YMKCkZ4k1qJDESsJBk2ujDPmQQeMjJX3FnDXYYB182ZCGQzXfzlPDC29cWVgDZEXNHuYrOLmJTmYtLZ4WkdUhLLlt5XsdoKWqlWpbegyYtGZgeZNRtOOdN6ybOPJqmYFd2qRtb4sYPniGJDOGhx4VodXAjT09omhQJpE6wlZbRWDvKC55R2d/CSPHJscEiuudb+1SG2uA/oik/WQ== username@domainname
```

Azure portalı veya Resource Manager şablonunda kullanmak için ortak anahtar dosyasının içeriğini kopyalayıp, tüm sondaki boşluk kopyalamadığınızdan emin olun. Bir ortak anahtar macOS kopyalamak için ortak anahtar dosyasını kanal oluşturarak aktarabilirsiniz **pbcopy**. Benzer şekilde, Linux, ortak anahtar dosyasını programları gibi kanal oluşturarak aktarabilirsiniz **xclip**.

Azure'da Linux vm'nize yerleştirdiğiniz ortak anahtar, depolanan varsayılan olarak ~ /.ssh/id_rsa.pub, anahtar çifti oluştururken farklı bir konum belirtilmediği sürece. Kullanılacak [Azure CLI 2.0](/cli/azure) mevcut bir ortak anahtar ile VM oluşturma için değer ve isteğe bağlı olarak bu ortak anahtarınızı kullanarak konumu belirtin [az vm oluşturma](/cli/azure/vm#az-vm-create) komutunu `--ssh-key-value` seçeneği. Aşağıdaki komutta *VMname*, *RGname*, ve *keyFile* kendi değerlerinizle:

```azurecli
az vm create --name VMname --resource-group RGname --ssh-key-value @keyFile
```

## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama

Ortak anahtarı, Azure VM üzerinde dağıtılmış ve özel anahtarı yerel sisteminizde, IP adresini veya VM'nizi DNS adını kullanarak VM'NİZDE SSH gerçekleştirin. Aşağıdaki komutta *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi) ile:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola belirtilmişse oturum açma işlemi sırasında istendiğinde bu parolayı girin. VM ~/.ssh/known_hosts dosyanıza eklenir ve Azure VM değişikliklerinizi ya da ortak anahtar kadar yeniden bağlanmak için sorulmaz veya sunucu adı ~/.ssh/known_hosts kaldırılır.

VM tam zamanında erişim ilkesi kullanıyorsa, sanal Makineye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında İlkesi hakkında daha fazla bilgi için bkz: [yalnızca kullanarak sanal makine erişimini yönetme zamanında İlkesi](../../security-center/security-center-just-in-time.md).

## <a name="next-steps"></a>Sonraki adımlar

* SSH anahtar çiftleri ile çalışma hakkında daha fazla bilgi için bkz. [anahtar çifti oluşturma ve SSH yönetmek için ayrıntılı adımlar](create-ssh-keys-detailed.md).

* Azure Vm'leri için SSH bağlantıları zorluklarla karşılaşırsanız, bkz. [sorun giderme SSH bağlantıları için bir Azure Linux VM](troubleshoot-ssh-connection.md).
