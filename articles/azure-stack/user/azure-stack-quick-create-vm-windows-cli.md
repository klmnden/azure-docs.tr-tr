---
title: "Azure CLI kullanarak Azure yığında bir Windows sanal makine oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak Azure yığında Windows VM oluşturma hakkında bilgi edinin"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: ea972db9ce3488d9a46a7d059714c8bbe820d47d
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-a-windows-virtual-machine-on-azure-stack-using-azure-cli"></a>Azure CLI kullanarak Azure yığında bir Windows sanal makine oluşturma

Azure CLI oluşturmak ve komut satırından Azure yığın kaynakları yönetmek için kullanılır. Azure CLI kullanarak Azure yığınında bir Windows Server 2016 sanal makine oluşturmak için bu kılavuzu ayrıntıları. Sanal makine oluşturulduktan sonra Uzak Masaüstü'nü, IIS yüklemek, bağlanmak sonra varsayılan Web sitesini görüntülemek. 

## <a name="prerequisites"></a>Önkoşullar 

* Azure yığın operatörünüze Azure yığın Market "Windows Server 2016" görüntü ekledi emin olun.  

* Azure yığını, Azure CLI oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. Azure CLI Azure yığını için yapılandırılmış yoksa adımlarını izleyin [Azure CLI'yi yükleme ve yapılandırma](azure-stack-connect-cli.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu hangi Azure yığına kaynakları dağıtılan yönetilen ve mantıksal bir kapsayıcısıdır. Geliştirme Seti veya Azure yığınından tümleşik çalıştırılan sistemi [az grubu oluşturma](/cli/azure/group#az_group_create) bir kaynak grubu oluşturmak için komutu. Bu belgedeki tüm değişkenleri için değerleri atamış olduğunuz, bunları ya da farklı bir değer atamak gibi kullanabilirsiniz. Aşağıdaki örnek, yerel bir konum myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. Aşağıdaki örnek, myVM adlı VM oluşturur. Bu örnek için bir yönetici kullanıcı adı Demouser kullanır ve Demouser@123 ve parola olarak. Bu değerleri ortamınız için uygun olan bir değerle güncelleştirin. Bu değerler, sanal makineye bağlanırken gereklidir.

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

VM oluşturulduğunda, Not *Publicıpaddress* VM erişmek için kullanacağı çıkışı, parametre.
 
## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Varsayılan olarak, Azure yığınında dağıtılmış bir Windows sanal makine için yalnızca RDP bağlantılara izin verilir. Bu VM bir web sunucusu olacaksa, İnternet’ten 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm#open-port) komutunu kullanın.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin genel IP adresi ile değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

Azure VM’de oturum açtıktan sonra tek bir PowerShell satırı kullanarak IIS yükleyebilir ve web trafiğine izin vermek üzere yerel güvenlik duvarı kuralını etkinleştirebilirsiniz. Bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Sanal makinenizde İnternet’ten IIS yüklenmiş ve bağlantı noktası 80 açık olduğunda, varsayılan IIS karşılama sayfasını görüntülemek için seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için yukarıda belgelediğiniz genel IP adresini kullandığınızdan emin olun. 

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç basit Windows sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
