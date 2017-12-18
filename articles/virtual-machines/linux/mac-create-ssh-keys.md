---
title: "Azure’da Linux VM’ler için SSH anahtar çifti oluşturma ve kullanma | Microsoft Docs"
description: "Kimlik doğrulaması işleminin güvenliğini artırmak için Azure’da Linux VM’leri için SSH ortak ve özel anahtar çifti oluşturma ve kullanma."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: 806d297c40af6ae2834ad529aaa11c51d26826dd
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma
Güvenli kabuk (SSH) anahtar çiftiyle Azure’da sanal makineler (VM) oluşturabilirsiniz. Bu sayede kimlik doğrulaması için SSH anahtarlarını kullanarak oturum açmak için parolalara duyulan gereksinimi ortadan kaldırırsınız. Bu makalede Linux VM’ler için bir SSH protokolü sürüm 2 RSA ortak ve özel anahtar dosyası çiftini hızlı bir şekilde oluşturma ve kullanma adımları anlatılmaktadır. Bu adımları Azure Cloud Shell, macOS veya Linux ana bilgisayarı ya da Linux için Windows Alt Sistemi ile yapabilirsiniz. Daha ayrıntılı adımlar ve başka örnekler için bkz. [SSH anahtar çiftleri ve sertifikaları oluşturmaya yönelik ek adımlar](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
SSH ortak ve özel anahtar dosyaları oluşturmak için `ssh-keygen` komutunu kullanın. Bunlar varsayılan olarak `~/.ssh` dizininde oluşturulur ancak istendiğinde farklı bir konum ve bir ek parola (özel anahtar dosyasına erişmek için parola) belirtebilirsiniz. Aşağıdaki komutu bir Bash kabuğundan çalıştırın ve komut istemlerini kendi bilgilerinizle yanıtlayın.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>SSH anahtar çiftini kullanma
Azure’da Linux VM’nize yerleştirdiğiniz ortak anahtar, oluştururken zaman konumu değiştirmediğiniz sürece varsayılan olarak `~/.ssh/id_rsa.pub` içinde depolanır. VM’nizi oluşturmak için [Azure CLI 2.0](/cli/azure) kullanırsanız, `--ssh-key-path` seçeneği ile [az vm create](/cli/azure/vm#create) kullanırken bu ortak anahtarın konumunu belirtin. Azure portalında veya bir Resource Manager şablonunda kullanmak üzere ortak anahtar dosyasının içeriğini kopyalar ve yapıştırırsanız, fazladan boşluk kopyalamadığınızdan emin olun. Örneğin, OS X kullanıyorsanız, ortak anahtar dosyası (varsayılan olarak, **~/.ssh/id_rsa.pub**) içeriklerini kopyalamak için **pbcopy**’e iletebilirsiniz (bunu yapmak için `xclip` gibi diğer Linux programları da kullanılabilir).

SSH ortak anahtarları hakkında bilgi sahibi değilseniz, aşağıdaki gibi `~/.ssh/id_rsa.pub` öğesini kendi ortak anahtar dosyası konumunuz ile değiştirip `cat` çalıştırarak ortak anahtarınızı görebilirsiniz:

```bash
cat ~/.ssh/id_rsa.pub
```

Azure VM’nizdeki ortak anahtarla, VM’nizin IP adresini veya DNS adını kullanarak SSH’yi VM’inize ekleyin (aşağıda, `azureuser` ve `myvm.westus.cloudapp.azure.com` öğelerini yönetici kullanıcı adı ve tam etki alanı adı veya IP adresi ile değiştirmeyi unutmayın):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola sağladıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

## <a name="next-steps"></a>Sonraki adımlar

SSH anahtarları kullanılarak oluşturulan VM’ler, varsayılan olarak parola kullanımı devre dışı bırakılmış şekilde yapılandırılır; böylece parola tahminiyle gerçekleştirilen saldırı girişimleri önemli ölçüde daha pahalı ve zor hale getirilir. Bu konu başlığında, hızlı kullanım için basit bir SSH anahtar çifti oluşturma açıklanmaktadır. SSH anahtar çiftinizi oluşturma konusunda daha fazla yardıma ihtiyacınız varsa veya ek sertifikalara gereksinim duyuyorsanız bkz. [SSH anahtar çiftleri ve sertifikaları oluşturmaya yönelik ayrıntılı adımlar](create-ssh-keys-detailed.md).

Azure portalı, CLI ve şablonları kullanarak SSH anahtar çiftinizi kullanan VM’ler oluşturabilirsiniz:

* [Azure portalını kullanarak güvenli bir Linux VM oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI 2.0 kullanarak güvenli bir Linux VM oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure şablonu kullanarak güvenli bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
