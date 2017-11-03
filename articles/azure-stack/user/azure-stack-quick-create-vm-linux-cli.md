---
title: "Azure yığınında Azure CLI kullanarak bir Linux sanal makine oluşturma | Microsoft Docs"
description: "Azure yığınında CLI ile Linux sanal makine oluşturun."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: de2ff697c083493b43ab0d1b5bcde532c28684e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-linux-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Azure yığınında Azure CLI kullanarak bir Linux sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure CLI oluşturmak ve komut satırından Azure yığın kaynakları yönetmek için kullanılır. Azure yığınında Linux sanal makine oluşturmak için Azure CLI kullanarak bu hızlı başlangıç ayrıntıları.  VM oluşturulduktan sonra bir web sunucusu yüklü ve bağlantı noktası 80 web trafiğine izin vermek üzere açılır.

## <a name="prerequisites"></a>Ön koşullar 

* Azure yığın operatörünüze Azure yığın Market "Ubuntu Server 16.04 LTS" görüntü ekledi emin olun. 

* Azure yığını, Azure CLI oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. Azure CLI Azure yığını için yapılandırılmış yoksa, oturum [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) ve adımlarını izleyin [yükleyin ve Azure CLI yapılandırma](azure-stack-connect-cli.md).

* Genel SSH anahtarı adı id_rsa.pub ile Windows kullanıcı profiliniz .ssh dizininin oluşturulması gerekir. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [oluşturma SSH anahtarları Windows'da](../../virtual-machines/linux/ssh-from-windows.md). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu hangi Azure yığına kaynakları dağıtılan yönetilen ve mantıksal bir kapsayıcısıdır. Geliştirme Seti veya Azure yığınından tümleşik çalıştırılan sistemi [az grubu oluşturma](/cli/azure/group#create) bir kaynak grubu oluşturmak için komutu. Bu belgedeki tüm değişkenleri için değerleri atamış olduğunuz, bunları ya da farklı bir değer atamak gibi kullanabilirsiniz. Aşağıdaki örnek, yerel bir konum myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm#create) komutu. Aşağıdaki örnek, myVM adlı VM oluşturur. Bu örnek için bir yönetici kullanıcı adı Demouser kullanır ve Demouser@123 ve parola olarak. Bu değerleri ortamınız için uygun olan bir değerle güncelleştirin. Bu değerler, sanal makineye bağlanırken gereklidir.

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

Tamamlandıktan sonra komut parametreleri sanal makineniz için çıktısı.  Not *Publicıpaddress*, bu yana bağlanmak ve sanal makineniz yönetmek için bunu kullanın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Varsayılan olarak, Azure’a dağıtılmış Linux sanal makinelerinde yalnızca SSH bağlantılarına izin verilir. Bu VM bir web sunucusu olacaksa, İnternet’ten 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm#open-port) komutunu kullanın.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama

SSH yüklü bir sistemden sanal makineye bağlanmak için aşağıdaki komutu kullanın. Windows üzerinde çalışıyorsanız, bağlantı oluşturmak için [Putty](http://www.putty.org/) kullanılabilir. Doğru ortak IP adresi, sanal makine ile değiştirdiğinizden emin olun. Bizim örneğimizde yukarıdaki IP adresini 192.168.102.36 oluştu.

```bash
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>NGINX yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki bash betiğini kullanın. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için yukarıda belgelediğiniz *publicIpAddress* değerini kullandığınızdan emin olun. 

![Varsayılan NGINX sitesi](./media/azure-stack-quick-create-vm-linux-cli/nginx.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, basit Linux sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).

