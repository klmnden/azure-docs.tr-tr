---
title: Azure yığınında Azure CLI kullanarak bir Linux sanal makine oluşturma | Microsoft Docs
description: Azure yığınında CLI ile Linux sanal makine oluşturun.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 21F7D599-1FEC-4827-A5C3-06495C5F53A4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 90b36183ba32e75e06d434098d26cb10f3736373
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure yığınında Azure CLI kullanarak Linux sunucusu sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure CLI kullanarak bir Ubuntu Server 16.04 LTS sanal makine oluşturabilirsiniz. Bir sanal makine oluşturmak için bu makaledeki adımları izleyin. Bu makalede ayrıca adımlarını sağlar:

* Uzak bir istemci ile sanal makineye bağlanabilirsiniz.
* NGINX web sunucusu yüklemek ve varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizlemek.

## <a name="prerequisites"></a>Önkoşullar

* **Azure yığın Market Linux görüntüde**

   Azure yığın Market Linux görüntü varsayılan olarak içermiyor. Sağlamak üzere Azure yığın işleci alma **Ubuntu Server 16.04 LTS** ihtiyacınız görüntü. İşleç açıklanan adımları kullanabilirsiniz [karşıdan Market öğesi Azure'dan Azure yığınına](../azure-stack-download-azure-marketplace-item.md) makalesi.

* Azure yığın oluşturmak ve kaynakları yönetmek için Azure CLI belirli bir sürümünü gerektirir. Azure yığını için yapılandırılmış Azure CLI yoksa, oturum [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) ve adımlarını izleyin [ Azure CLI'yi yükleme ve yapılandırma](azure-stack-version-profiles-azurecli2.md).

* Windows kullanıcı profiliniz .ssh dizinine adı id_rsa.pub ile SSH ortak anahtarı. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [oluşturma SSH anahtarları Windows'da](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu, dağıtmak ve Azure yığın kaynaklarını yönetmek mantıksal bir kapsayıcısıdır. Geliştirme Seti veya Azure yığınından tümleşik çalıştırılan sistemi [az grubu oluşturma](/cli/azure/group#az_group_create) bir kaynak grubu oluşturmak için komutu.

>[!NOTE]
 Kod örnekleri tüm değişkenleri için değerleri atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

Aşağıdaki örnek, yerel bir konum myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir sanal makine oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. Aşağıdaki örnek, myVM adlı VM oluşturur. Bu örnek için bir yönetici kullanıcı adı Demouser kullanır ve Demouser@123 kullanıcı parolası. Ortamınız için uygun bir şey için bu değerleri değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Genel IP adresi döndürülür **Publicıpaddress** parametresi. Sanal makine erişimini gerektiğinden bu adresi yazın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

IIS web sunucusu çalıştırmak için bu sanal makine olacağından Internet trafiği 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm#open-port) komutunu kullanın.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Sanal makineye bağlanmak için SSH kullanın

SSH yüklü olan bir istemci bilgisayardan sanal makineye bağlanın. Windows istemcisi üzerinde çalışıyorsanız kullanmak [Putty](http://www.putty.org/) bağlantı oluşturmak için. Sanal makineye bağlanmak için aşağıdaki komutu kullanın:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynakları güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki betiği çalıştırın:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

Yüklü NGINX ve bağlantı noktası 80, sanal makinenizde açık ile sanal makinenin ortak IP adresini kullanarak web sunucusuna erişebilir. Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![NGINX web server Karşılama sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmeyen kaynakları temizlemek. Kullanabileceğiniz [az grubu Sil](/cli/azure/group#az_group_delete) bu kaynakları kaldırmak için komutu. Kaynak grubu ve tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç web sunucusu ile temel bir Linux sunucusu sanal makine dağıtılabilir. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
