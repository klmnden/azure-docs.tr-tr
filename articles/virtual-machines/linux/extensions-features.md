---
title: "Sanal makine uzantıları ve Linux için Özellikler | Microsoft Docs"
description: "Hangi uzantıları ne bunlar sağlayın veya geliştirmek tarafından gruplandırılmış Azure sanal makineler için kullanılabilir olduğunu öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 59f718e0e547ed9374152985e706acad4421b35b
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Sanal makine uzantıları ve Linux için özellikleri

Azure sanal makine uzantıları Azure sanal makinelerde dağıtım sonrası yapılandırma ve Otomasyon görevlerini sağlayan küçük uygulamalardır. Örneğin, bir sanal makineye yazılım yükleme, virüsten koruma veya Docker yapılandırma gerektiriyorsa, bu görevleri tamamlamak için bir VM uzantısı kullanılabilir. Azure VM uzantıları, Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portalını kullanarak çalıştırabilirsiniz. Uzantıları ile yeni bir sanal makine dağıtımı paketlenebilir veya varolan bir sistemle bağlantılı çalıştırın.

Bu belge VM uzantıları, Azure VM uzantıları kullanma önkoşulları genel bir bakış sağlar ve algılamak nasıl hakkında yönergeler yönetmek ve VM uzantılarını kaldırın. Birçok VM uzantıları bulunduğundan, bu belgede genelleştirilmiş bilgiler sağlanmaktadır her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantıya özgü ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

## <a name="use-cases-and-samples"></a>Kullanım örnekleri ve örnekler

Birkaç farklı Azure VM uzantıları kullanılabilir, her biri belirli bir kullanım örneği. Bazı örnekler şunlardır:

- Linux için DSC uzantısı kullanarak bir sanal makine için PowerShell istenen durum yapılandırması uygulayın. Daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Microsoft İzleme Aracısı VM uzantısı olan bir sanal makinenin izlemeyi yapılandırın. Daha fazla bilgi için bkz: [bir Linux VM izleme](tutorial-monitoring.md).
- Azure altyapınızın Datadog uzantılı izlemeyi yapılandırın. Daha fazla bilgi için bkz: [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Docker ana Docker VM uzantısı kullanılarak Azure sanal makinesi üzerinde yapılandırın. Daha fazla bilgi için bkz: [Docker VM uzantısı](dockerextension.md).

İşleme özgü uzantılar ek olarak, bir özel betik uzantısı, Windows ve Linux sanal makineleri için kullanılabilir. Linux için özel betik uzantısı, bir sanal makine üzerinde çalıştırılacak Bash komut dosyaları sağlar. Özel komut dosyaları, yerel hangi Azure araçlar sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için faydalıdır. Daha fazla bilgi için bkz: [Linux VM özel betik uzantısı](extensions-customscript.md).


## <a name="prerequisites"></a>Ön koşullar

Her sanal makine uzantısı önkoşulları kendi kümesi olabilir. Örneğin, Docker VM uzantısı desteklenen Linux dağıtım önkoşul vardır. Tek tek uzantıların gereksinimlerini uzantıya özgü belgelerinde açıklanmıştır.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM Aracısı bir Azure sanal makinesi ve Azure yapı denetleyicisi arasındaki etkileşimler yönetir. VM Aracısı dağıtma ve yönetme Azure sanal makineler, çalışan VM uzantıları dahil olmak üzere birçok işlevsel görünüşlere için sorumludur. Azure VM Aracısı Azure Marketi görüntülerinde önceden yüklenmiş ve desteklenen işletim sistemlerinde el ile yüklenebilir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>VM uzantıları Bul

Birçok farklı VM uzantıları, Azure sanal makineler ile kullanmak için kullanılabilir. Tam listesini görmek için örnek konum tercih ettiğiniz konumu ile değiştirerek Azure CLI ile aşağıdaki komutu çalıştırın.

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a>VM uzantıları çalıştırın

Yapılandırma değişikliklerini yapın veya zaten dağıtılmış bir VM'de bağlantısı kurtarmak gerektiğinde, yararlı olan ve mevcut sanal makineler, Azure sanal makine uzantıları çalıştırılabilir. VM uzantıları, Azure Resource Manager şablonu dağıtımlarında da gönderilebilir. Resource Manager şablonları ile uzantıları kullanarak, Azure sanal makineleri dağıtılabilir ve dağıtım sonrası müdahalesi olmadan yapılandırılmış.

Aşağıdaki yöntemlerden bir uzantısı olan bir sanal makineyi karşı çalıştırmak için kullanılabilir.

### <a name="azure-cli"></a>Azure CLI

Azure sanal makine uzantıları çalıştırılabilir karşı var olan bir sanal makine kullanarak `az vm extension set` komutu. Bu örnek özel betik uzantısı bir sanal makine çalışır.

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Komut dosyası aşağıdakine benzer bir çıktı üretir:

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure portalına

VM uzantıları olan bir sanal makineyi Azure Portalı aracılığıyla uygulanabilir. Bunu yapmak için sanal makineyi seçin, **uzantıları**, tıklatıp **Ekle**. Kullanılabilir uzantılar listeden istediğiniz ve Sihirbazı'ndaki yönergeleri izleyin uzantıyı seçin.

Aşağıdaki resim Azure portalından Linux özel betik uzantısı yüklemesini gösterir.

![Özel betik uzantısı yükleyin](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları, bir Azure Resource Manager şablonu eklenir ve şablon dağıtımı ile yürütüldü. Bir şablon uzantılı dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturamazsınız. Örneğin, aşağıdaki JSON Resource Manager şablonu alınır. Şablonu bir dizi yük dengeli sanal makineler ve Azure SQL Veritabanını dağıtır ve ardından bir .NET Core uygulaması her VM yükler. VM uzantısı yazılım yüklemesi mvc'deki.

Daha fazla bilgi için bkz: tam [Resource Manager şablonu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).

## <a name="secure-vm-extension-data"></a>VM uzantısı verileri güvenli

VM uzantısı çalıştırırken kimlik bilgilerini, depolama hesabı adları ve depolama hesabı erişim anahtarlarını gibi hassas bilgiler dahil etmek gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef sanal makine içinde şifresini çözer korumalı bir yapılandırmayı içerir. Belirli korumalı yapılandırma şeması her uzantısına sahip ve her uzantıya özgü belgelerinde ayrıntılı olarak gösterilmiştir.

Aşağıdaki örnek, Linux için özel betik uzantısı örneğini gösterir. Komutun yürütülmesi için kimlik bilgileri kümesini içerdiğine dikkat edin. Bu örnekte, yürütülecek komut şifrelenmez.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Taşıma **yürütülecek komut** özelliğine **korumalı** yapılandırma yürütme dize güvenliğini sağlar.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>VM uzantıları sorun giderme

Her VM uzantısı, sorun giderme adımları belirli uzantısına sahip olabilir. Örneğin, özel betik uzantısı kullanırken, komut dosyası yürütme ayrıntılarını yerel olarak sanal makinede uzantı çalıştırıldığı bulunabilir. Uzantı özel sorun giderme işlemleri uzantıya özgü belgelerinde açıklanmıştır.

Tüm sanal makine uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüle

Bir sanal makine sonra bir sanal makine uzantısı çalıştırılmış, uzantısı durumuna döndürmek için aşağıdaki Azure CLI komutunu kullanın. Örnek parametre adları kendi değerlerinizle değiştirin.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Çıktı aşağıdaki metni gibi görünür:

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

Uzantı yürütme durumu de Azure portalında bulunabilir. Uzantı durumunu görüntülemek için sanal makineyi seçin, **uzantıları**ve istenen uzantı seçin.

### <a name="rerun-a-vm-extension"></a>VM uzantısı yeniden çalıştırın

Bir sanal makine uzantısı yeniden çalıştırılması gereken durumlar olabilir. Uzantı, kaldırarak ve tercih ettiğiniz yürütme yöntemiyle uzantısı yeniden çalıştırma çalıştırabilirsiniz. Bir uzantıyı kaldırmak için Azure CLI ile aşağıdaki komutu çalıştırın. Örnek parametre adları kendi değerlerinizle değiştirin.

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

Azure portalında aşağıdaki adımları kullanarak bir uzantı kaldırabilirsiniz:

1. Bir sanal makineyi seçin.
2. Seçin **uzantıları**.
3. İstenen uzantıyı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extension-reference"></a>Ortak VM uzantısı başvurusu
| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Linux için özel betik uzantısı |Bir Azure sanal makinesi karşı komut dosyalarını çalıştır |[Linux için özel betik uzantısı](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker uzantısı |Uzak Docker komutları desteklemek için Docker arka plan programı yükleyin. |[Docker VM uzantısı](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| VM Erişimi uzantısı |Bir Azure sanal makinesi erişimi yeniden kazanmak |[VM Erişimi uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure tanılama uzantısını |Azure tanılama yönetme |[Azure tanılama uzantısını](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişim uzantısı |Kullanıcılar ve kimlik bilgilerini yönetme |[Linux VM erişim uzantısı](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
