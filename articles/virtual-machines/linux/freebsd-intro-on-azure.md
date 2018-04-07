---
title: Azure üzerinde FreeBSD giriş | Microsoft Docs
description: Azure üzerinde FreeBSD sanal makineleri kullanma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: thomas1206
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: huishao
ms.openlocfilehash: 9c7cf223eab3e989436e12c39b122f2aee7619a0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-freebsd-on-azure"></a>Azure üzerinde FreeBSD giriş
Bu konuda FreeBSD sanal makine Azure'da çalışan bir genel bakış sağlar.

## <a name="overview"></a>Genel Bakış
Microsoft Azure FreeBSD gelişmiş bilgisayar işletim sisteminin güç modern sunuculara, masaüstü bilgisayarları, kullanılan ve embedded platformları ' dir.

Microsoft Corporation'ın yapmadan FreeBSD görüntülerini kullanılabilir ile azure'da [Azure VM Konuk aracısının](https://github.com/Azure/WALinuxAgent/) önceden yapılandırılmış. Şu anda aşağıdaki FreeBSD sürümleri görüntü olarak Microsoft tarafından sunulan:

- FreeBSD 10.3-RELEASE
- FreeBSD 11.0-sürüm
- FreeBSD 11.1-sürüm

Aracı FreeBSD VM ve VM ilk kullanımda (kullanıcı adı, parola veya SSH anahtarı, ana bilgisayar adı, vb.) sağlama ve işlevlerini seçmeli VM uzantıları için etkinleştirmek gibi işlemleri için Azure doku arasındaki iletişim için sorumludur.

FreeBSD gelecek sürümlerinde olduğu gibi stratejisi güncel ve FreeBSD yayın mühendislik ekibi tarafından kısa süre içinde yayımladıktan sonra en son sürümlerde kullanılabilir yapmaktır.

## <a name="deploying-a-freebsd-virtual-machine"></a>FreeBSD sanal makine dağıtma
FreeBSD sanal makine dağıtımı Azure portalından Azure Marketi'nden görüntü kullanarak bir işlemdir:

- [FreeBSD Azure Market'te 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD Azure Market'te 11.0](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)
- [FreeBSD Azure Market'te 11.1](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD111)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Azure CLI 2.0 aracılığıyla bir FreeBSD VM üzerinde FreeBSD oluşturma
Yüklemek gereken ilk [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) FreeBSD makinede komutu aşağıdaki olsa.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

Bash FreeBSD makinenize yüklü değilse, komutu yüklemeden önce aşağıdaki çalıştırın. 

```bash
sudo pkg install bash
```

Python FreeBSD makinenize yüklü değilse, komutları yüklemeden önce aşağıdaki çalıştırın. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Yükleme sırasında istendiğinde `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`. Yanıtı verirseniz `y` ve girin `/etc/rc.conf` olarak `a path to an rc file to update`, sorunu karşılayan `ERROR: [Errno 13] Permission denied`. Bu sorunu gidermek için yazma geçerli kullanıcının dosyayı karşı sağdan vermelisiniz `etc/rc.conf`.

Şimdi Azure'da oturum ve FreeBSD VM oluşturun. Aşağıda bir FreeBSD 11.0 VM oluşturmak için bir örnek verilmiştir. Parametresi de ekleyebilirsiniz `--public-ip-address-dns-name` yeni oluşturulan bir genel IP için genel benzersiz bir DNS adına sahip. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Ardından, sizin FreeBSD VM Dağıtım yukarıdaki çıktıda yazdırılan IP adresi üzerinden oturum açabilir. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>FreeBSD VM uzantıları
Desteklenen VM uzantıları FreeBSD aşağıda verilmiştir.

### <a name="vmaccess"></a>VMAccess
[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) uzantısı kullanabilirsiniz:

* Özgün sudo kullanıcının parolasını sıfırlayın.
* Yeni bir sudo kullanıcı belirtilen parolayla oluşturun.
* Verilen anahtarla genel ana bilgisayar anahtarını ayarlayın.
* Ana makine anahtarı sağlanmazsa, VM sağlama işlemi sırasında sağlanan genel ana bilgisayar anahtarını sıfırlayın.
* SSH bağlantı noktası (22) açın ve reset_ssh ayarlarsanız sshd_config geri true.
* Varolan bir kullanıcı kaldırın.
* Diskleri denetleyin.
* Eklenen bir disk onarın.

### <a name="customscript"></a>CustomScript
[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) uzantısı kullanabilirsiniz:

* Sağlanırsa, özel komut dosyaları Azure Storage veya dış ortak depolama (örneğin, GitHub) indirin.
* Giriş noktası komut dosyasını çalıştırın.
* Satır içi komutları destekler.
* Windows stili yeni satır kabuk ve Python komut dosyaları otomatik olarak dönüştürür.
* Ürün reçetesi kabuk ve Python komut dosyaları otomatik olarak kaldırın.
* CommandToExecute gizli verileri koruyun.

> [!NOTE]
> FreeBSD VM yalnızca destekler CustomScript sürüm 1.x şimdi tarafından.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Kimlik doğrulaması: kullanıcı adları, parolalar ve SSH anahtarları
Azure portalı kullanarak bir FreeBSD sanal makine oluştururken, bir kullanıcı adı, parola veya SSH ortak anahtarı sağlamanız gerekir.
Azure FreeBSD sanal makine dağıtmak için kullanıcı adları system hesaplarının adlarını değil eşleşmesi gerekir (UID < 100) sanal makine ("Kök", örneğin) zaten mevcut.
Şu anda yalnızca RSA SSH anahtarı desteklenir. Çok satırlı SSH anahtarı ile başlamalıdır `---- BEGIN SSH2 PUBLIC KEY ----` ve sonunda `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Süper kullanıcı ayrıcalıkları alma
Azure üzerinde sanal makine örneği dağıtımı sırasında belirtilen kullanıcı hesabı ayrıcalıklı bir hesaptır. Sudo paketi yayımlanmış FreeBSD görüntüde yüklendi.
Bu kullanıcı hesabıyla oturum açtınız sonra komut sözdizimini kullanarak kök olarak komutları çalıştırabilirsiniz.

```
$ sudo <COMMAND>
```

Bir kök Kabuğu'nu kullanarak isteğe bağlı olarak elde edebilirsiniz `sudo -s`.

## <a name="known-issues"></a>Bilinen sorunlar
[Azure VM Konuk aracısının](https://github.com/Azure/WALinuxAgent/) 2.2.2 sahip [bilinen bir sorun] sürüm (https://github.com/Azure/WALinuxAgent/pull/517) , sağlama hatası Azure FreeBSD sanal makine için neden olur. Düzeltme tarafından yakalanan [Azure VM Konuk aracısının](https://github.com/Azure/WALinuxAgent/) sürüm 2.2.3 ve sonraki sürümlerinde. 

## <a name="next-steps"></a>Sonraki adımlar
* Git [Azure Marketi](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) FreeBSD VM oluşturmak için.
