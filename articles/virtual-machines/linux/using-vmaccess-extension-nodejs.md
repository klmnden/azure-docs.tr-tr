---
title: VMAccess uzantısını kullanarak Azure Linux VM'ler erişimi sıfırlama | Microsoft Docs
description: VMAccess uzantısını kullanarak Azure Linux VM'ler erişimi sıfırlayın.
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 5fb130fc2e448f3cbc648991ea6bebd5795bc78b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915067"
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-10"></a>Kullanıcılar, SSH ve onay yönetmek veya Azure CLI 1.0 ile VMAccess uzantısını kullanarak Azure Linux VM'ler disklerde onarın
Bu makalede Azure VMAcesss uzantısı denetleyin veya bir disk onarım, kullanıcı erişimi sıfırlama, kullanıcı hesaplarını yönetme veya Linux'ta SSHD yapılandırmasını sıfırlamak için nasıl kullanılacağı gösterilmektedir. Bu makale için şunlar gereklidir:

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/)).
* `azure login` ile oturum açılmış [Azure CLI'si](../../cli-install-nodejs.md).
* Azure CLI'si, `azure config mode arm` Azure Resource Manager modunda *olmalıdır*.


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands)– bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="quick-commands"></a>Hızlı komutlar
Linux Vm'leriniz VMAccess kullanmanın iki yolu vardır:

* Azure CLI 1.0 ve gerekli parametreleri kullanma.
* VMAccess işleyen ve ardından hareket ham JSON dosyaları kullanıyor.

Hızlı komut bölümü için size Azure CLI 1.0 kullanacaksanız `azure vm reset-access` yöntemi. Aşağıdaki komut örneklerinde içeren kendi ortamınızdaki değerlerle "örnek" değerler değiştirin.

## <a name="create-a-resource-group-and-linux-vm"></a>Bir kaynak grubu ve Linux VM oluşturun
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Debian bir VM oluşturma
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a>Kök parola sıfırlama
Kök parolasını sıfırlamak için:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH anahtarı sıfırlama
Kök olmayan kullanıcı SSH anahtarı sıfırlamak için:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Kullanıcı oluşturma
Bir kullanıcı oluşturmak için:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Kullanıcıyı kaldırma
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD Sıfırla
SSHD yapılandırmasını sıfırlamak için:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz
### <a name="vmaccess-defined"></a>Tanımlanan VMAccess:
Linux VM diskte hata gösteriliyor. Şekilde Linux VM için kök parola sıfırlama veya yanlışlıkla SSH özel anahtarınızı silinemez. Veri Merkezi gün sonra yeniden oluştuysa, sürücü vardır ve sunucu konsolunda almak için KVM açmak gerekir. Azure VMAccess uzantısını erişim için Linux sıfırlamak veya disk düzeyinde bakım gerçekleştirmek için konsol erişmenize olanak tanır, KVM anahtar düşünün.

Ayrıntılı bilgi için biz ham JSON dosyaları kullanan VMAccess uzun formu kullanacaksanız.  Bu VMAccess JSON dosyaları da Azure şablonlardan çağrılabilir.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Denetleyin veya bir Linux VM diski onarmak için VMAccess kullanma
VMAccess kullanarak bir fsck yapabilirsiniz, Linux VM'NİZDE altında diskteki çalıştırın.  Ayrıca, bir disk denetimi ve bir VMAccess kullanarak bir disk onarım yapabilirsiniz.

Disk denetleyin ve sonra onarmak için bu VMAccess komut dosyasını kullanın:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Linux için kullanıcı erişimini sıfırlamak için VMAccess kullanma
Linux VM üzerinde kök dizinine erişim kaybettiyseniz, kök parolasını sıfırlamak için VMAccess betik başlatabilirsiniz.

Kök parolasını sıfırlamak için bu VMAccess komut dosyasını kullanın:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

Kök olmayan kullanıcı SSH anahtarı sıfırlamak için bu VMAccess komut dosyasını kullanın:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Linux kullanıcı hesaplarını yönetmek için VMAccess kullanma
VMAccess oturum açmayı ve sudo veya kök hesabı kullanarak Linux VM kullanıcıları yönetmek için kullanılan bir Python komut dosyasıdır.

Bir kullanıcı oluşturmak için bu VMAccess komut dosyasını kullanın:

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

Bir kullanıcıyı silmek için bu VMAccess komut dosyasını kullanın:

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>SSHD yapılandırmasını sıfırlamak için VMAccess kullanma
Linux sanal makineleri SSHD yapılandırma değişiklikleri yapmak ve değişiklikleri doğrulama önce SSH bağlantısını kapatırsanız, SSH'ing engellenebilir geri dönün.  SSH üzerinden oturum açılmış olmadan geri bir bilinen iyi yapılandırmaya SSHD yapılandırmasını sıfırlamak için VMAccess kullanılabilir.

SSHD yapılandırmasını sıfırlamak için bu VMAccess komut dosyasını kullanın:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Olan VMAccess betiği yürütün:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Sonraki adımlar
Linux güncelleştirme Azure VMAccess uzantılarını kullanarak çalışan bir Linux VM üzerinde değişiklik yapmak için bir yöntemdir.  Bulut Init ve Azure şablonlarını gibi araçlar, Linux VM'NİZDE önyüklemede değiştirmek için de kullanabilirsiniz.

[Sanal makine uzantıları ve özellikleri hakkında](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Linux VM uzantıları ile Azure Resource Manager şablonları yazma](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Bir Linux VM oluşturma sırasında özelleştirmek için bulut init kullanma](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

