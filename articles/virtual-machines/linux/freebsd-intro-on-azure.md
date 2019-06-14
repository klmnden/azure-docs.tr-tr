---
title: Azure üzerinde Freebsd'ye giriş | Microsoft Docs
description: Azure'da FreeBSD sanal makineleri kullanma hakkında bilgi edinin
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
ms.openlocfilehash: 1f2d3c40352d60d3cc7366aca6f38a8255a7a629
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60627717"
---
# <a name="introduction-to-freebsd-on-azure"></a>Azure üzerinde Freebsd'ye giriş
Bu makalede, FreeBSD sanal makine Azure'da çalışan genel bir bakış sağlar.

## <a name="overview"></a>Genel Bakış
Microsoft Azure için FreeBSD gelişmiş bilgisayar işletim sistemi platformları embedded ve power modern sunuculara, masaüstü, kullanılan ' dir.

Microsoft Corporation çalışmalarımız FreeBSD görüntülerini kullanılabilir Azure ile [Azure VM Konuk Aracısı](https://github.com/Azure/WALinuxAgent/) önceden yapılandırılmış. Şu anda aşağıdaki FreeBSD sürümleri görüntü olarak Microsoft tarafından sunulur:

- FreeBSD 10.3 yayın
- FreeBSD 10.4 yayın
- FreeBSD 11.1 yayın

Aracı, Azure fabric ilk kullanımda (kullanıcı adı, parola veya SSH anahtarı, konak adı, vb.) VM sağlama ve seçmeli VM uzantıları için işlevleri etkinleştirme gibi işlemler için FreeBSD VM arasındaki iletişimi sorumludur.

FreeBSD gelecekteki sürümlerinde olduğu gibi güncel kalın ve FreeBSD yayın mühendislik ekibi tarafından kısa süre içinde yayımladıktan sonra en son sürümlerine hale stratejisidir.

## <a name="deploying-a-freebsd-virtual-machine"></a>FreeBSD sanal makine dağıtma
FreeBSD sanal makine dağıtma Azure portalından Azure Marketi'nden bir görüntü kullanarak bir işlemdir:

- [FreeBSD 10.4 Azure Marketi'nde](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD104)
- [FreeBSD 11.2 Azure Marketi'nde](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD112)

### <a name="create-a-freebsd-vm-through-azure-cli-on-freebsd"></a>Azure CLI aracılığıyla FreeBSD VM üzerinde Freebsd'ye oluşturma
Yüklemek gereken ilk [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) FreeBSD makinede aşağıdaki komut rağmen.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

Bash FreeBSD makinenizde yüklü değilse, aşağıdaki komut yüklemeden önce çalıştırın. 

```bash
sudo pkg install bash
```

Python FreeBSD makinenizde yüklü değilse, komutları yüklemeden önce aşağıdaki çalıştırın. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Yükleme sırasında sizden istenir `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`. Cevabı verirseniz `y` girin `/etc/rc.conf` olarak `a path to an rc file to update`, sorun karşılamak `ERROR: [Errno 13] Permission denied`. Bu sorunu çözmek için yazma geçerli kullanıcının dosyayı karşı sağdan vermelisiniz `etc/rc.conf`.

Artık Azure'da oturum açın ve FreeBSD VM'nizi oluşturun. FreeBSD 11.0 VM oluşturmak için bir örnek aşağıda verilmiştir. Parametre de ekleyebilirsiniz `--public-ip-address-dns-name` yeni oluşturulan bir genel IP için genel olarak benzersiz bir DNS adına sahip. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Ardından, FreeBSD vm'nize dağıtım yukarıdaki çıktıda yazdırılan IP adresi üzerinden oturum açabilir. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>FreeBSD için VM uzantıları
Desteklenen VM uzantıları FreeBSD aşağıda verilmiştir.

### <a name="vmaccess"></a>VMAccess
[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) uzantısı kullanabilirsiniz:

* Sudo kullanıcının parolasını sıfırlayın.
* Belirtilen parolayla yeni bir sudo kullanıcı oluşturun.
* Verilen anahtara sahip genel ana bilgisayar anahtarını ayarlayın.
* Ana bilgisayar anahtarı sağlanmazsa, VM sağlama sırasında sağlanan genel ana bilgisayar anahtarı sıfırlayın.
* SSH bağlantı noktası (22) açın ve reset_ssh ayarlarsanız sshd_config geri true.
* Mevcut kullanıcıyı kaldırın.
* Diskleri denetleyin.
* Eklenen bir diski onarın.

### <a name="customscript"></a>CustomScript
[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) uzantısı kullanabilirsiniz:

* Sağlanırsa, özel komut dosyaları Azure depolama veya genel dış depo (örneğin GitHub) indirin.
* Giriş noktası betiğini çalıştırın.
* Satır içi komutları destekler.
* Windows stili satır içinde kabuk ve Python betiklerini otomatik olarak dönüştürür.
* BOM kabuk ve Python komut dosyaları otomatik olarak kaldırın.
* CommandToExecute hassas verileri koruyun.

> [!NOTE]
> FreeBSD VM CustomScript sürümü yalnızca destekler 1.x şimdi tarafından.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Kimlik doğrulaması: kullanıcı adı, parola ve SSH anahtarları
Azure portalını kullanarak bir FreeBSD sanal makine oluştururken, bir kullanıcı adı, parola veya SSH ortak anahtarı sağlamanız gerekir.
FreeBSD sanal makine azure'da dağıtmak için kullanıcı adları, system hesaplarının adlarını değil eşleşmelidir (UID < 100) ("Kök", örneğin) sanal makine zaten mevcut.
Şu anda yalnızca RSA SSH anahtarı desteklenir. Çok satırlı bir SSH anahtarı ile başlamalıdır `---- BEGIN SSH2 PUBLIC KEY ----` ve ile sona erdi `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Süper kullanıcı ayrıcalıkları edinme
Azure'da sanal makine örneği dağıtım sırasında belirtilen kullanıcı hesabının ayrıcalıklı bir hesapsa. Sudo paketinin yayımlanan FreeBSD görüntüde yüklendi.
Bu kullanıcı hesabıyla oturum açtınız sonra komut söz dizimini kullanarak komutları kök olarak çalıştırabilirsiniz.

```
$ sudo <COMMAND>
```

Kullanarak isteğe bağlı olarak bir kök Kabuk edinebilirsiniz `sudo -s`.

## <a name="known-issues"></a>Bilinen sorunlar
[Azure VM Konuk Aracısı](https://github.com/Azure/WALinuxAgent/) 2.2.2 sürümüne sahip bir [bilinen sorun](https://github.com/Azure/WALinuxAgent/pull/517) azure'da FreeBSD VM için sağlama hatası neden. Düzeltme tarafından yakalanan [Azure VM Konuk Aracısı](https://github.com/Azure/WALinuxAgent/) 2.2.3 Sürüm ve sonraki sürümleri. 

## <a name="next-steps"></a>Sonraki adımlar
* Git [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD112) FreeBSD VM oluşturmak için.
