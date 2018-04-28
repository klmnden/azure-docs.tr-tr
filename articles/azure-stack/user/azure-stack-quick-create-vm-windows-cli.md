---
title: Azure CLI kullanarak Azure yığında bir Windows sanal makine oluşturma | Microsoft Docs
description: Azure CLI kullanarak Azure yığında Windows VM oluşturma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 381c1c37b0675d97adc058979a5d9b5c4fd2cc8b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure yığınında Azure CLI kullanarak bir Windows Server sanal makine oluşturma

‎*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure CLI kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturmak için bu makaledeki adımları izleyin. Bu makalede ayrıca, aşağıdaki adımları sunar:

* Uzak bir istemci ile sanal makineye bağlanabilirsiniz.
* IIS web sunucusu yüklemek ve varsayılan giriş sayfasını görüntüleyin.
* Kaynakları temizlemek.

## <a name="prerequisites"></a>Önkoşullar

* Azure yığın operatörünüze eklediğinizden emin olun **Windows Server 2016** Azure yığın Market görüntüsünü.

* Azure yığını, Azure CLI oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. Azure CLI Azure yığını için yapılandırılmış yoksa adımlarını izleyin [Azure CLI'yi yükleme ve yapılandırma](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu, dağıtmak ve Azure yığın kaynaklarını yönetmek mantıksal bir kapsayıcısıdır. Azure yığın ortamınızdan çalıştırmak [az grubu oluşturma](/cli/azure/group#az_group_create) bir kaynak grubu oluşturmak için komutu.

>[!NOTE]
 Kod örnekleri tüm değişkenleri için değerleri atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

Aşağıdaki örnek, yerel bir konum myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir sanal makine (VM) oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. Aşağıdaki örnek, myVM adlı VM oluşturur. Bu örnek için bir yönetici kullanıcı adı Demouser kullanır ve Demouser@123 kullanıcı parolası. Ortamınız için uygun bir şey için bu değerleri değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

VM oluşturulduğunda, **Publicıpaddress** çıkış parametresi sanal makine için genel IP adresini içerir. Sanal makine erişimini gerektiğinden bu adresi yazın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

IIS web sunucusu çalıştırmak için bu VM olacağından Internet trafiği 80 numaralı bağlantı noktasını açmanız gerekir.

Kullanım [az vm Aç-port](/cli/azure/vm#open-port) 80 numaralı bağlantı noktasını açmak için komutu.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine için Uzak Masaüstü bağlantısı oluşturmak için İleri komutunu kullanın. "Genel IP adresi" sanal makineniz IP adresiyle değiştirin. İstendiğinde, kullanıcı adı ve sanal makine için kullanılan parolayı girin.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

Sanal makineye oturum açtığınız, IIS yüklemek için PowerShell'i kullanabilirsiniz. Sanal makinede PowerShell başlatın ve aşağıdaki komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Varsayılan IIS Karşılama sayfasını görüntülemek için tercih ettiğiniz bir web tarayıcısı kullanabilirsiniz. Varsayılan sayfasını ziyaret etmek için önceki bölümde belgelenen ortak IP adresi kullanın.

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmeyen kaynakları temizlemek. Kullanım [az grubu Sil](/cli/azure/group#az_group_delete) tüm kaynakları ilgili ve kaynak grubu, sanal makineyi kaldırmak için komutu.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç temel bir Windows Server sanal makine dağıtılabilir. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
