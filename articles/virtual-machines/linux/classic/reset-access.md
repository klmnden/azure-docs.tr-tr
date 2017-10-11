---
title: "Linux VM parolası ve clı'dan SSH anahtarı sıfırlama | Microsoft Docs"
description: "Bir Linux VM parola veya SSH anahtarı sıfırlama, SSH yapılandırmasını düzeltmek ve disk tutarlılık denetimi için VMAccess uzantısını gelen Azure komut satırı arabirimi (CLI) kullanma"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
ms.openlocfilehash: 74765877e7836d6878284b350a25d8355dc83d7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Bir Linux VM parola veya SSH anahtarı sıfırlama, SSH yapılandırmasını düzeltmek ve VMAccess uzantısını kullanarak disk tutarlılık denetimi hakkında
Unutulmuş parola nedeniyle, yanlış bir güvenli Kabuk (SSH) anahtarını Azure Linux sanal makineye bağlanılamıyor veya SSH yapılandırması ile ilgili bir sorun VMAccessForLinux uzantısını Azure CLI ile parola veya SSH anahtarını sıfırlamak için kullanıyorsanız, SSH Düzelt Yapılandırma ve onay disk tutarlılık. 

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) öğrenin.

Azure CLI ile kullandığınız **azure vm uzantısı kümesi** komutlara erişmek için komut satırı arabiriminden (Bash, Terminal, komut istemi) komutu. Çalıştırma **azure Yardım vm uzantısı kümesi** ayrıntılı uzantısını kullanım için.

Azure CLI ile bunu aşağıdaki görevleri gerçekleştirebilirsiniz:

* [Parola sıfırlama](#pwresetcli)
* [SSH anahtarını Sıfırla](#sshkeyresetcli)
* [Parola ve SSH anahtarı sıfırlama](#resetbothcli)
* [Yeni bir sudo kullanıcı hesabı oluşturma](#createnewsudocli)
* [SSH yapılandırmasını sıfırlayın](#sshconfigresetcli)
* [Kullanıcı silme](#deletecli)
* [VMAccess uzantısını durumunu görüntüleyin](#statuscli)
* [Eklenen diskler tutarlılık denetimi](#checkdisk)
* [Linux VM eklenen disklerde onarın](#repairdisk)

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakileri yapmanız gerekir:

* Etmeniz [Azure CLI yükleme](../../../cli-install-nodejs.md) ve [aboneliğinize bağlanma](../../../xplat-cli-connect.md) hesabınızla ilişkili Azure kaynaklarını kullanmak için.
* Komut isteminde aşağıdakini yazarak Klasik dağıtım modeli için doğru moda ayarlayın:
    ``` 
        azure config mode asm
    ```
* Bunlardan birini sıfırlamak istiyorsanız bir yeni bir parola veya SSH anahtarları kümesi vardır. SSH yapılandırmasını sıfırlamak istiyorsanız, bu gerekmez.

## <a name="pwresetcli"></a>Parola sıfırlama
1. Bu satırlar ile PrivateConf.json adlı yerel bilgisayarınızdaki bir dosya oluşturun. Değiştir **KullanıcıAdım** ve  **myP@ssW0rd**  kendi kullanıcı adı ve parola ile ve kendi sona erme tarihini ayarlayın.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Sanal makine için adını değiştirerek bu komutu çalıştırmak **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>SSH anahtarını Sıfırla
1. Bu içerikle PrivateConf.json adlı bir dosya oluşturun. Değiştir **KullanıcıAdım** ve **mySSHKey** kendi bilgilerinizi değerlerle.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Sanal makine için adını değiştirerek bu komutu çalıştırmak **myVM**.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Parola ve SSH anahtarı sıfırlama
1. Bu içerikle PrivateConf.json adlı bir dosya oluşturun. Değiştir **KullanıcıAdım**, **mySSHKey** ve  **myP@ssW0rd**  kendi bilgilerinizi değerlerle.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Sanal makine için adını değiştirerek bu komutu çalıştırmak **myVM**.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Yeni bir sudo kullanıcı hesabı oluşturma

Kullanıcı adınızı unutursanız, Vmaccess'in sudo yetkisine sahip yeni bir tane oluşturmak için kullanabilirsiniz. Bu durumda, var olan kullanıcı adı ve parola değiştirilmeyecek.

Parola erişimi ile yeni bir sudo kullanıcı oluşturmak için komut dosyasındaki kullanmak [parola sıfırlama](#pwresetcli) ve yeni bir kullanıcı adı belirtin.

SSH anahtar erişimi ile yeni bir sudo kullanıcı oluşturmak için komut dosyasındaki kullanın [SSH anahtarı sıfırlama](#sshkeyresetcli) ve yeni bir kullanıcı adı belirtin.

Aynı zamanda [parola ve SSH anahtarı sıfırlama](#resetbothcli) hem parola hem de SSH anahtar erişimi ile yeni bir kullanıcı oluşturmak için.

## <a name="sshconfigresetcli"></a>SSH yapılandırmasını sıfırlayın
SSH yapılandırması istenmeyen bir durumda ise, erişim VM'ye kaybedebilirsiniz. Varsayılan durumuna getirmek yapılandırmasını sıfırlamak için VMAccess uzantısını kullanabilirsiniz. Bunu yapmak için yalnızca "True" "reset_ssh" anahtarını ayarlamak yeterlidir. Uzantı SSH sunucuyu yeniden başlatın, VM'yi SSH bağlantı noktası açın ve SSH yapılandırmasını varsayılan değerlere sıfırla. Kullanıcı hesabı (adı, parola veya SSH anahtarları) değiştirilmez.

> [!NOTE]
> Sıfırlama SSH yapılandırma dosyası /etc/ssh/sshd_config bulunur.
> 
> 

1. Bu içerikle PrivateConf.json adlı bir dosya oluşturun.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Sanal makine için adını değiştirerek bu komutu çalıştırmak **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Kullanıcı silme
Oturum açmayı VM doğrudan olmadan bir kullanıcı hesabı silmek istiyorsanız, bu komut dosyasını kullanabilirsiniz.

1. İçin kaldırmak için kullanıcı adını değiştirerek bu içerikle PrivateConf.json adlı bir dosya oluşturun **removeUserName**. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Sanal makine için adını değiştirerek bu komutu çalıştırmak **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>VMAccess uzantısını durumunu görüntüleyin
VMAccess uzantısını durumunu görüntülemek için bu komutu çalıştırın.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Eklenen diskler tutarlılık denetimi
Linux sanal makinedeki tüm disklerde fsck çalıştırmak için aşağıdakileri yapmanız gerekir:

1. Bu içerikle PublicConf.json adlı bir dosya oluşturun. Onay Disk veya sanal makinenize bağlı diskler denetlenip denetlenmeyeceğini için bir Boole değeri alır. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Sanal makine için adını değiştirerek yürütmek için bu komutu çalıştırmak **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Onarım diskleri
Değil bağlanması veya takma yapılandırma hataları olan diskleri onarmak için Linux sanal makinenizde bağlama yapılandırmasını sıfırlamak için VMAccess uzantısını kullanın. İçin disk adınızı değiştirerek **myDisk**.

1. Bu içerikle PublicConf.json adlı bir dosya oluşturun. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Sanal makine için adını değiştirerek yürütmek için bu komutu çalıştırmak **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Sonraki adımlar
* Parola veya SSH anahtarını sıfırlamak için Azure PowerShell cmdlet'lerini veya Azure Resource Manager şablonları kullanmak istiyorsanız, SSH yapılandırması ve onay disk tutarlılık, bkz: düzeltme [VMAccess uzantısını belgelerine github'da](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* Aynı zamanda [Azure portal](https://portal.azure.com) parola veya SSH anahtarı bir Linux VM sıfırlamak için Klasik dağıtım modelinde dağıtılmış. Şu anda portal do kullanamazsınız Resource Manager dağıtım modelinde dağıtılan bu bir Linux VM için.
* Bkz: [sanal makine uzantıları ve özellikleri hakkında](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Azure sanal makineler için VM uzantıları kullanma hakkında daha fazla bilgi için.

