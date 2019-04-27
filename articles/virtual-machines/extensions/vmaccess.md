---
title: Bir Azure Linux VM erişimi sıfırlama | Microsoft Docs
description: Yönetici kullanıcıları yönetme ve VMAccess uzantısı ve Azure CLI kullanarak Linux vm'lerinde erişimi sıfırlama
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2018
ms.author: roiyz
ms.openlocfilehash: 71aecc1748e70e2119b1f54c21a0f705afc5d5d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60800067"
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli"></a>Yönetici kullanıcılar, SSH ve onay yönetmek veya VMAccess uzantısı ile Azure CLI kullanarak Linux vm'lerinde diskler onarın
## <a name="overview"></a>Genel Bakış
Linux VM'nizi diskte hatalarını göstermez. Siz şekilde Linux sanal makinenizin kök parolayı sıfırlama veya SSH özel anahtarınızı yanlışlıkla silinmiş. Veri Merkezi gün içinde geri oluştuysa, sürücü vardır ve sunucu konsolunda almak için KVM açın gerekecektir. Azure VMAccess uzantısı, Linux için erişimi sıfırlama veya disk düzeyinde bakım yapmak için konsoluna erişmek izin veren, KVM anahtarı düşünün.

Bu makalede, denetleyin veya bir diski onarmak için Azure VMAccess uzantısı kullanın, kullanıcı erişimi sıfırlama, yönetim kullanıcısı hesapları yönetme veya Azure Resource Manager sanal makine olarak çalıştırırken Linux'ta SSH yapılandırmasını güncelleştirme işlemini göstermektedir. Klasik sanal makineler - yönetmeniz gerekiyorsa bulunan yönergeleri izleyerek [Klasik VM belgeleri](../linux/classic/reset-access-classic.md). 
 
> [!NOTE]
> AAD oturum açma uzantıyı yükledikten sonra sanal makinenizin parolayı sıfırlamak için VMAccess uzantısını kullanıyorsanız AAD oturum açma, makine için yeniden etkinleştirmek için AAD oturum açma uzantısı yeniden çalıştırmanız gerekecektir.

## <a name="prerequisites"></a>Önkoşullar
### <a name="operating-system"></a>İşletim sistemi

VM erişimi uzantısı bu Linux dağıtımları karşı çalıştırabilirsiniz:

| Dağıtım | Version |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS ve 12.04 LTS |
| Debian | Debian 7,9 +, 8.2 + |
| Red Hat | RHEL 6.7 + 7.1 + |
| Oracle Linux | 6.4+, 7.0+ |
| SuSE | 11 ve 12 |
| OpenSuse | openSUSE 42.2 + artık |
| CentOS | CentOS 6.3+, 7.0+ |
| CoreOS | 494.4.0+ |

## <a name="ways-to-use-the-vmaccess-extension"></a>VMAccess uzantısını kullanmanın yolları
Linux Vm'lerinizi VMAccess uzantısı kullanabileceğiniz iki yolu vardır:

* Azure CLI ve gerekli parametreleri kullanın.
* [VMAccess uzantısı işlem ham JSON dosyalarını kullanmak](#use-json-files-and-the-vmaccess-extension) ve bunlar üzerinde işlem yapma.

Aşağıdaki örneklerde [az vm kullanıcı](/cli/azure/vm/user) komutları. Bu adımları gerçekleştirmek için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

## <a name="update-ssh-key"></a>SSH anahtarını güncelleştir
Aşağıdaki örnek, kullanıcı için SSH anahtarı güncelleştirmeleri `azureuser` adlı VM'de `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

> **NOT:** `az vm user update` Komut ekler için yeni ortak anahtar metnini `~/.ssh/authorized_keys` VM'de yönetici kullanıcı için dosya. Bunun yerine veya mevcut bir SSH anahtarınız kaldırın. Bu, dağıtım süresini ya da sonraki güncelleştirmeler VMAccess uzantısı aracılığıyla ayarlanan önceki anahtarlar kaldırmaz.

## <a name="reset-password"></a>Parola sıfırlama
Aşağıdaki örnek, kullanıcının parolasını sıfırlar `azureuser` adlı VM'de `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>SSH yeniden başlatma
Aşağıdaki örnek, SSH arka plan programı yeniden başlatır ve SSH Yapılandırması adlı bir VM varsayılan değerlerine sıfırlar `myVM`:

```azurecli-interactive
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Yönetim/sudo kullanıcı oluşturma
Aşağıdaki örnekte adlı bir kullanıcı oluşturur `myNewUser` ile **sudo** izinleri. Hesap adlı VM üzerinde kimlik doğrulaması için bir SSH anahtarı kullanıp `myVM`. Bu yöntem, geçerli kimlik bilgilerini kaybolur veya unutulursa olay, bir VM erişebilmek yardımcı olmak için tasarlanmıştır. İle en iyi uygulama, hesapları **sudo** izinleri sınırlı olmalıdır.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Kullanıcı silme
Aşağıdaki örnekte adlı bir kullanıcıyı siler `myNewUser` adlı VM'de `myVM`:

```azurecli-interactive
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```

## <a name="use-json-files-and-the-vmaccess-extension"></a>JSON dosyaları ve VMAccess uzantısını kullanma
Aşağıdaki örnekler, ham JSON dosyalarını kullanın. Kullanım [az vm uzantısı kümesi](/cli/azure/vm/extension) JSON dosyalarınızı ardından çağırmak için. Bu JSON dosyaları Azure şablonlarını da çağrılabilir. 

### <a name="reset-user-access"></a>Kullanıcı erişimi sıfırlama
Linux sanal makinenizde kök dizinine erişim kaybettiyseniz, bir kullanıcının SSH anahtarını veya parolasını güncelleştirmek için VMAccess betik başlatabilirsiniz.

Bir kullanıcı SSH ortak anahtarını güncelleştirmek için adlı bir dosya oluşturun. `update_ssh_key.json` ve ayarları aşağıda gösterilen biçimde ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `ssh_key` parametreleri:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings update_ssh_key.json
```

Bir kullanıcının parolasını sıfırlamak için adlı bir dosya oluşturun. `reset_user_password.json` ve ayarları aşağıda gösterilen biçimde ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `password` parametreleri:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>SSH yeniden başlatma
SSH arka plan programını yeniden başlatın ve SSH yapılandırmasını varsayılan değerlerine sıfırlamak için adlı bir dosya oluşturun. `reset_sshd.json`. Aşağıdaki içeriği ekleyin:

```json
{
  "reset_ssh": true
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>Yönetici kullanıcıları yönetme

Bir kullanıcı oluşturmak için **sudo** bir SSH anahtarı kimlik doğrulaması için kullandığı izinler adlı bir dosya oluşturmak `create_new_user.json` ve ayarları aşağıda gösterilen biçimde ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `ssh_key` parametreleri. Bu yöntem, geçerli kimlik bilgilerini kaybolur veya unutulursa olay, bir VM erişebilmek yardımcı olmak için tasarlanmıştır. İle en iyi uygulama, hesapları **sudo** izinleri sınırlı olmalıdır.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Bir kullanıcıyı silmek için adlı bir dosya oluşturun. `delete_user.json` ve aşağıdaki içeriği ekleyin. Kendi değeri `remove_user` parametresi:

```json
{
  "remove_user":"myNewUser"
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Denetleyin veya disk onarım
VMAccess kullanarak da kontrol edin ve Linux VM'ye eklenen bir disk onarın.

Kontrol edin ve ardından diski onarmak için adlı bir dosya oluşturun. `disk_check_repair.json` ve ayarları aşağıda gösterilen biçimde ekleyin. Kendi değeri adını `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

VMAccess komut dosyasını yürütün:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```
## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
