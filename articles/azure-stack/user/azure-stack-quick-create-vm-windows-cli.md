---
title: Azure CLI kullanarak Azure Stack üzerinde bir Windows sanal makinesi oluşturun | Microsoft Docs
description: Azure CLI kullanarak Azure Stack üzerinde bir Windows VM oluşturmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/14/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 67e0ccfa883e79d66eb9ca38a6cf15f00154c487
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59265721"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack'te Azure CLI kullanarak bir Windows Server sanal makinesi oluşturma

‎*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure CLI kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturup, bu makaledeki adımları izleyin. Bu makalede ayrıca aşağıdakileri sağlar:

* Sanal Makine Uzak istemcisi ile bağlanın.
* IIS web sunucusunu yükleme ve varsayılan giriş sayfasını görüntüleyin.
* Kaynaklarınızı temizleme.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack operatörü eklediğiniz emin **Windows Server 2016** Azure Stack marketini görüntüye.

* Azure Stack kaynakları oluşturup yönetmek için Azure CLI'yı belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış Azure CLI yoksa, adımları [Azure CLI'yi yükleme ve yapılandırma](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu, dağıtma ve Azure Stack kaynaklarını yönetme mantıksal bir kapsayıcıdır. Azure Stack ortamınızdan çalıştırma [az grubu oluşturma](/cli/azure/group#az-group-create) bir kaynak grubu oluşturmak için komutu.

> [!NOTE]
>  Kod örnekleri tüm değişkenler için değerler atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

Aşağıdaki örnek, yerel konuma myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir sanal makine (VM) oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) komutu. Aşağıdaki örnek, myVM adlı bir VM oluşturur. Bu örnekte yönetici kullanıcı adı için Demouser ve Demouser@123 kullanıcı parolası. Bu değerleri ortamınız için uygun bir şey değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

VM oluşturulduğunda **Publicıpaddress** çıktı parametresi sanal makinenin genel IP adresini içerir. Bu adres sanal makineye erişmek için buna ihtiyacınız olduğundan yazma.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

IIS web sunucusu çalıştırmak için bu VM olacağından Internet trafiği için 80 numaralı bağlantı noktasını açmanız gerekir.

Kullanım [az vm open-port](/cli/azure/vm) 80 numaralı bağlantı noktasını açmak için komutu.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makinenize Uzak Masaüstü bağlantısı oluşturmak için sonraki komutu kullanın. "Genel IP adresi" sanal makinenizin IP adresiyle değiştirin. İstendiğinde kullanıcı adı ve sanal makine için parolayı girin.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

Sanal makineye oturum açtığınız, IIS yüklemek için PowerShell kullanabilirsiniz. Sanal makinede PowerShell'i başlatın ve aşağıdaki komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Varsayılan IIS Karşılama sayfasını görüntülemek için seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için önceki bölümde belirtildiği genel IP adresini kullanın.

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. Kullanım [az grubu Sil](/cli/azure/group#az-group-delete) komutunu sanal makinenin kaynak grubunu ve tüm ilgili kaynakları.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir temel Windows Server sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [sanal Makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
