---
title: "Bir Azure Active Directory DS RedHat Linux VM katılma | Microsoft Docs"
description: "Bir Azure Active Directory etki alanı hizmeti için mevcut bir RedHat Enterprise Linux 7 VM'yi katılmaya nasıl."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: 2e46a0f3c9bdbe267d121b4bf62e25d5d411fcc2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="join-a-redhat-linux-vm-to-an-azure-active-directory-domain-service"></a>Bir Azure Active Directory etki alanı hizmeti RedHat Linux VM katılma

Bu makalede bir Red Hat Enterprise Linux (RHEL) 7 sanal makine bir Azure Active Directory etki alanı Hizmetleri (AADDS) yönetilen etki alanına katılma kullanmayı gösterir.  Gereksinimler şunlardır:

- [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)

- [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md)

- [DC bir Azure Active Directory etki alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Hızlı Komutlar

_Tüm örnekleri kendi ayarlarınızla değiştirin._

### <a name="switch-the-azure-cli-to-classic-deployment-mode"></a>Azure CLI Klasik dağıtım moduna geç

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>RHEL sürüm ve görüntü arayın

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Redhat Linux VM oluşturma

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-to-the-vm"></a>Sanal Makineye SSH uygulama

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a>Güncelleştirme YUM paketleri

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Gerekli paketleri yüklemek

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Gerekli paketleri Linux sanal makinede yüklü olan, sonraki görev sanal makineyi yönetilen etki alanına belirlemektir.

### <a name="discover-the-aad-domain-services-managed-domain"></a>AAD etki alanı Hizmetleri yönetilen etki Bul

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Kerberos başlatma

'AAD DC Yöneticiler' grubuna ait bir kullanıcı belirttiğinizden emin olun. Yalnızca bu kullanıcıların bilgisayarları yönetilen etki alanına katılmasını sağlayabilir.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-the-machine-to-the-domain"></a>Makine etki alanına ekleme

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-the-machine-is-joined-to-the-domain"></a>Makine etki alanına katılan doğrulayın

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Sonraki Adımlar

* [Red Hat güncelleştirme altyapısı (RHUI) isteğe bağlı Red Hat Enterprise Linux VM'ler için Azure'da için](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Sanal makineler Azure Kaynak Yöneticisi'nde için anahtar kasasını oluşturup](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Dağıtma ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
