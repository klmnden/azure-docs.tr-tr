---
title: "Bir Azure Linux VM erişimi sıfırlama | Microsoft Docs"
description: "Yönetici kullanıcıları yönetme ve Linux VM'ler VMAccess uzantısını ve Azure CLI 2.0 kullanarak erişimi sıfırlama"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 235a6367ad317945cfeaaa6aae4e060208fb8e8e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a>Yönetici kullanıcılar, SSH ve onay yönetmek veya Azure CLI 2.0 ile VMAccess uzantısını kullanarak Linux VM'ler disklerde onarın
Linux VM diskte hata gösteriliyor. Şekilde Linux VM için kök parola sıfırlama veya yanlışlıkla SSH özel anahtarınızı silinemez. Veri Merkezi gün sonra yeniden oluştuysa, sürücü vardır ve sunucu konsolunda almak için KVM açmak gerekir. Azure VMAccess uzantısını erişim için Linux sıfırlamak veya disk düzeyinde bakım gerçekleştirmek için konsol erişmenize olanak tanır, KVM anahtar düşünün.

Bu makalede Azure VMAccess uzantısını denetleyin veya bir disk onarım, kullanıcı erişimi sıfırlama, yönetici kullanıcı hesaplarını yönetme veya Linux üzerinde SSH yapılandırmasını sıfırlamak için nasıl kullanılacağı gösterilmektedir. Bu adımları [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz.


## <a name="ways-to-use-the-vmaccess-extension"></a>VMAccess uzantısını kullanmanın yolları
Linux Vm'leriniz VMAccess uzantısını kullanmanın iki yolu vardır:

* Azure CLI 2.0 ve gerekli parametreleri kullanır.
* [VMAccess uzantısını işlemek ham JSON dosyaları kullanmak](#use-json-files-and-the-vmaccess-extension) ve ardından hareket.

Aşağıdaki örneklerde [az vm kullanıcı](/cli/azure/vm/user) komutları. Bu adımları gerçekleştirmek için en son gerekir [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

## <a name="reset-ssh-key"></a>SSH anahtarını Sıfırla
Aşağıdaki örnekte kullanıcı için SSH anahtarı sıfırlar `azureuser` adlı VM üzerinde `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Parola sıfırlama
Aşağıdaki örnek kullanıcının parolasını sıfırlar `azureuser` adlı VM üzerinde `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>SSH yeniden başlatma
Aşağıdaki örnek, SSH arka plan programı yeniden başlatır ve SSH yapılandırmasını varsayılan değerlere adlı bir VM'de sıfırlar `myVM`:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Bir yönetici/sudo kullanıcı oluşturun
Aşağıdaki örnek adlı bir kullanıcı oluşturur `myNewUser` ile **sudo** izinleri. Hesap adlı VM kimlik doğrulaması için bir SSH anahtarı kullanıp `myVM`. Bu yöntem, geçerli kimlik bilgileri kaybolur veya unutulursa gerektiğinde, bir VM erişimi yeniden kazanmak yardımcı olmak için tasarlanmıştır. İle en iyi uygulama, hesapları **sudo** izinler sınırlı.

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```



## <a name="delete-a-user"></a>Kullanıcı silme
Aşağıdaki örnek adlı bir kullanıcıyı siler `myNewUser` adlı VM üzerinde `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a>JSON dosyaları ve VMAccess uzantısını kullanır
Aşağıdaki örnekler ham JSON dosyaları kullanın. Kullanım [az vm uzantısı kümesi](/cli/azure/vm/extension#az_vm_extension_set) , JSON dosyalarınızın çağırmak için. Bu JSON dosyaları da Azure şablonlardan çağrılabilir. 

### <a name="reset-user-access"></a>Kullanıcı erişimi sıfırlama
Linux VM üzerinde kök dizinine erişim kaybettiyseniz, bir kullanıcının SSH anahtarı veya parolayı sıfırlamak için VMAccess betik başlatabilirsiniz.

Kullanıcı SSH ortak anahtarını sıfırlamak için adlı bir dosya oluşturun `reset_ssh_key.json` ve şu biçimde ayarları ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `ssh_key` Parametreler:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

Bir kullanıcı parolasını sıfırlamak için adlı bir dosya oluşturun `reset_user_password.json` ve şu biçimde ayarları ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `password` Parametreler:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>SSH yeniden başlatma
Sunucusundaki SSH arka plan yeniden başlatılmasına ve SSH yapılandırmasını varsayılan değerlere sıfırla adlı bir dosya oluşturun `reset_sshd.json`. Aşağıdaki içeriği ekleyin:

```json
{
  "reset_ssh": true
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>Yönetici kullanıcıları yönetme

Bir kullanıcı oluşturmak için **sudo** kimlik doğrulaması için bir SSH anahtarı kullanır, izinler adlı bir dosya oluşturmak `create_new_user.json` ve şu biçimde ayarları ekleyin. İçin kendi değerlerinizi yerleştirin `username` ve `ssh_key` parametreleri. Bu yöntem, geçerli kimlik bilgileri kaybolur veya unutulursa gerektiğinde, bir VM erişimi yeniden kazanmak yardımcı olmak için tasarlanmıştır. İle en iyi uygulama, hesapları **sudo** izinler sınırlı.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Bir kullanıcıyı silmek için adlı bir dosya oluşturun `delete_user.json` ve aşağıdaki içeriği ekleyin. Kendi değeri yerine `remove_user` parametre:

```json
{
  "remove_user":"myNewUser"
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Denetleyin veya disk onarım
VMAccess kullanarak da denetleyin ve Linux VM'ye eklenen bir disk onarın.

Kontrol edin ve ardından diski onarmak için adlı bir dosya oluşturun `disk_check_repair.json` ve şu biçimde ayarları ekleyin. Kendi adı değeri yerine `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Olan VMAccess betiği yürütün:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Sonraki adımlar
Azure VMAccess uzantısını kullanarak Linux güncelleştirme üzerinde çalışan bir Linux VM değişiklik yapmak için bir yöntemdir. Bulut Init ve Azure Resource Manager şablonları gibi araçlar, Linux VM'NİZDE önyüklemede değiştirmek için de kullanabilirsiniz.

[Sanal makine uzantıları ve Linux için özellikleri](extensions-features.md)

[Linux VM uzantıları ile Azure Resource Manager şablonları yazma](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Bir Linux VM oluşturma sırasında özelleştirmek için bulut init kullanma](using-cloud-init.md)

