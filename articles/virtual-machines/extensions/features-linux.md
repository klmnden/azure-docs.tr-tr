---
title: Azure VM uzantıları ve özellikleri için Linux | Microsoft Docs
description: Hangi uzantıların hangi kullanıcılar sağlar veya geliştirmek göre gruplanır, Azure sanal makineler için kullanılabilir olduğunu öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.openlocfilehash: bf6eca33eb1448eb84065fb7fe184d01e77feb61
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387285"
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Sanal makine uzantıları ve Linux için özellikleri

Azure sanal makinesi (VM), Azure Vm'leri üzerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sunan küçük uygulamalar uzantılarıdır. VM uzantısı, örneğin, bir sanal makineye yazılım yükleme, virüsten koruma gerektiriyorsa veya içindeki bir betik çalıştırmak için kullanılabilir. Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portalı ile Azure VM uzantıları çalıştırılabilir. Uzantıları ile yeni bir VM dağıtımını toplanmış veya mevcut bir sistemle bağlantılı çalıştırın.

Bu makalede VM uzantıları, Azure VM uzantıları kullanma önkoşulları genel bir bakış sağlar ve algılamak hakkında yönergeler yönetmek ve VM uzantılarını kaldırın. Birçok VM uzantıları bulunduğundan, bu makale, genelleştirilmiş bilgi sağlar. Her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantı özel ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

## <a name="use-cases-and-samples"></a>Kullanım ve örnekleri

Birkaç farklı Azure VM uzantıları kullanılabilir, her biri belirli bir kullanım örneği. Bazı örnekler:

- PowerShell istenen durum yapılandırmaları DSC uzantısı ile sanal makineye Linux için geçerlidir. Daha fazla bilgi için [Azure Desired State configuration uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Microsoft İzleme Aracısı VM uzantısı ile sanal makine izlemeyi yapılandırın. Daha fazla bilgi için [bir Linux VM izleme](../linux/tutorial-monitoring.md).
- Azure altyapınızı Chef veya Datadog uzantısı ile izlemeyi yapılandırma. Daha fazla bilgi için [Chef belgeleri](https://docs.chef.io/azure_portal.html) veya [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).

İşleme özel uzantılar ek olarak, bir özel betik uzantısı, hem Windows hem de Linux sanal makineler için kullanılabilir. Linux için özel betik uzantısı, bir sanal makine üzerinde çalıştırılacak herhangi bir Bash betiğinin sağlar. Özel komut dosyaları, yerel hangi Azure Araçları sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için kullanışlıdır. Daha fazla bilgi için [Linux VM özel betik uzantısı](custom-script-linux.md).

## <a name="prerequisites"></a>Önkoşullar

Sanal makine uzantısını işlemek için Azure Linux Aracısı gerekir. Bazı uzantıları ayrı ayrı kaynaklar veya bağımlılıkları erişim gibi önkoşulları sahip.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM Aracısı, Azure VM ve Azure yapı denetleyicisi arasındaki etkileşimler yönetir. VM Aracısı dağıtma ve yönetme Azure Vm'leri, VM uzantılarını çalıştırmak dahil çok sayıda işlevsel görünüşlere için sorumludur. Azure VM Aracısı, Azure Marketi görüntülerinde önceden yüklenmiş olarak ve el ile desteklenen işletim sistemlerine yüklenebilir. Linux için Azure VM Aracısı, Linux aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](agent-linux.md).

#### <a name="supported-agent-versions"></a>Desteklenen Aracı sürümleri

Mümkün olan en iyi deneyimi sağlamak için en düşük aracı sürümü vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın.

#### <a name="supported-oses"></a>Desteklenen işletim sistemleri

Uzantıları framework limiti işletim sistemleri için bu uzantılar vardır ancak Linux Aracısı birden çok Oses'te çalıştırır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems
) bakın.

Bazı uzantılar tüm işletim sistemlerinde desteklenmez ve yayabilir *hata kodu 51, 'Desteklenmeyen işletim sistemi'*. Desteklenebilirlik için ayrı bir uzantı belgelerine bakın.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure depolama uzantısı deposundan yüklenir ve uzantı durumu karşıya Azure Depolama'ya gönderilen değerler. Kullanırsanız [desteklenen](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) aracıların sürümünü, gereksinim gibi aracı iletişimi Azure yapı denetleyicisi için aracı iletişimlerini yeniden yönlendirmek için kullanabilirsiniz VM bölgesindeki Azure Depolama'da erişime izin vermek. Bir desteklenmeyen Aracı sürümünde varsa, Azure depolama bu bölgede VM'den giden erişime izin gerekir.

> [!IMPORTANT]
> Erişim engellenirse *168.63.129.16* Konuk Güvenlik Duvarı'nı kullanarak, daha sonra uzantıları yukarıdaki bağımsız olarak başarısız.

Aracıları yalnızca uzantı paketleri ve raporlama durumu indirmek için kullanılabilir. Örneğin, bir uzantı yükleme (özel betik) Github'dan bir betik indirmeniz gerekiyor veya Azure depolama (Azure Backup) sonra erişim ek gerekirse Güvenlik Duvarı/güvenlik ağ Grup bağlantı noktaları açılması gerekir. Uygulamaları kendi sağ olduğundan farklı uzantılarına farklı gereksinimlere sahiptir. Azure depolama erişimi gerektiren uzantılar için Azure NSG hizmet etiketleri kullanarak erişim izni verebilirsiniz [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Aracı trafiğini isteklerini yeniden yönlendirmek için Linux Aracısı, proxy sunucu desteği vardır. Ancak, bu proxy sunucu desteği uzantıları geçerli değildir. Ara sunucu ile çalışmak için tek tek her uzantı yapılandırmanız gerekir.

## <a name="discover-vm-extensions"></a>VM uzantıları bulma

Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Tam listesini görmek için [az vm uzantısı görüntü listesi](/cli/azure/vm/extension/image#az-vm-extension-image-list). Aşağıdaki örnekte tüm kullanılabilir uzantıları listeler *westus* konumu:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>VM Uzantıları'nı çalıştırın

Azure VM uzantıları mevcut Vm'lerde çalıştıracağınız başka yapılandırma değişiklikleri yapmak veya zaten dağıtılmış bir sanal makine bağlantısı kurtarmak gerektiğinde bu faydalıdır. VM uzantıları Azure Resource Manager şablon dağıtımları ile toplanmış. Resource Manager şablonları ile uzantıları kullanarak, Azure Vm'leri dağıtılabilir ve dağıtım sonrası müdahalesi olmadan yapılandırılmış.

Aşağıdaki yöntemlerden bir uzantı mevcut bir VM'ye karşı çalıştırmak için kullanılabilir.

### <a name="azure-cli"></a>Azure CLI

Azure VM uzantıları ile mevcut bir VM'ye karşı çalıştırılabilir [az vm uzantısı kümesi](/cli/azure/vm/extension#az-vm-extension-set) komutu. Aşağıdaki örnek özel betik uzantısı adlı bir VM karşı çalışan *myVM* adlı bir kaynak grubu içinde *myResourceGroup*:

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Uzantı düzgün çalıştığında, çıktı aşağıdaki örneğe benzer olacaktır:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure portal

VM uzantıları, var olan bir sanal makineye Azure Portalı aracılığıyla uygulanabilir. Portalda VM seçin, **uzantıları**, ardından **Ekle**. Sihirbazdaki yönergeleri izleyin ve kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin.

Aşağıdaki resimde, Azure portalında Linux özel betik uzantısı'nın yüklemesini göstermektedir:

![Özel betik uzantısı yükleme](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları bir Azure Resource Manager şablonuna eklenebilir ve şablon dağıtımı ile yürütüldü. Uzantı bir şablon ile dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturamazsınız. Örneğin, aşağıdaki JSON yük dengeli VM'ler ile Azure SQL veritabanı kümesi dağıtır ve ardından her VM'de bir .NET Core uygulamasını yükleyen bir Resource Manager şablonu alınır. VM uzantısı yazılım yüklemesi üstlenir.

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

Resource Manager şablonları oluşturmaya daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Güvenli VM uzantısı verileri

VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarları gibi hassas bilgileri içerecek şekilde gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef sanal makine içinde şifresini çözer ve korumalı bir yapılandırma içerir. Belirli bir korumalı yapılandırma şeması her uzantısına sahiptir ve her uzantı özgü belgelerinde ayrıntılı.

Aşağıdaki örnek, Linux için özel betik uzantısı'nın bir örneğini gösterir. Yürütülecek komut, kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifreli değil:

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

Taşıma **yürütmek için komut** özelliğini **korumalı** yapılandırma aşağıdaki örnekte gösterildiği gibi yürütme dize güvenliğini sağlar:

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

### <a name="how-do-agents-and-extensions-get-updated"></a>Nasıl aracısı ve uzantıları güncelleştirilmesi?

Uzantıları ve aracıları aynı güncelleştirme mekanizmasını paylaşın. Bazı güncelleştirmeler, ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanılabilir olduğunda, bir değişikliği uzantılarını ve diğer VM Model değişiklikleri gibi olduğunda yalnızca sanal makinede yüklü:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama kapsayıcı
- Konuk işletim sistemi gizli dizileri
- VM boyutu
- Ağ profili

Yayımcılar olası farklı sürümleri üzerinde farklı bölgelerde Vm'niz olabilir, bu nedenle güncelleştirmeler farklı zamanlarda bölgeler kullanılabilir hale getirmek.

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Linux VM aracısını içeren *sağlama aracı kodu* ve *uzantısı işleme kodu* bir pakette hangi kullanılamaz ayrılmış. Devre dışı bırakabilirsiniz *aracı sağlama* cloud-init kullanarak azure'da sağlamak istediğinizde. Bunu yapmak için bkz: [cloud-init kullanarak](../linux/using-cloud-init.md).

Otomatik Güncelleştirmeler aracıların desteklenen sürümlerini kullanabilirsiniz. Güncelleştirilebilir yalnızca kodu *kod uzantısı işleme*, sağlama kodu değil. *Sağlama aracı kodu* kez çalıştırılan kodudur.

*Uzantısı işleme kodu* Azure yapısı ile iletişim kurmasını ve sanal makine uzantıları işlem gibi işleme, bunları kaldırma durumu raporlama ve uzantıları ayrı ayrı güncelleştirme yüklemeler için sorumludur. Güncelleştirmeleri, güvenlik düzeltmeleri, hata düzeltmeleri ve geliştirmeleri içerir *uzantısı işleme kodu*.

Üst arka plan programı, aracıyı yüklediğinizde oluşturulur. Bu üst daha sonra uzantıları işlemek için kullanılan bir alt işlemi olarak çoğaltılır. Aracı için bir güncelleştirme varsa, karşıdan yüklenir, üst, alt işlemi durdurur, yükseltilen sonra yeniden başlatır. Güncelleştirme ile ilgili bir sorun olması, üst işlemdeki önceki alt sürüme geri alır.

Üst işleme otomatik güncelleştirilmiş olamaz. Üst yalnızca distro paket güncelleştirmesi tarafından güncelleştirilebilir.

Çalıştırdığınız hangi sürümünü denetlemek için kontrol `waagent` gibi:

```bash
waagent --version
```

Çıktı aşağıdaki örneğe benzer:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

Yukarıdaki örnek çıktıda üst veya 'Paket sürümü dağıtılan' olduğu *WALinuxAgent 2.2.17*

'Hedef aracı durumu' otomatik güncelleştirme sürümüdür.

Her zaman aracısı için otomatik güncelleştirme sahip önerilen [AutoUpdate.Enabled=y](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent). Değil aracıyı el ile güncelleştirme tutmanız gerekir yani etkinleştirilmiş olması ve hata ve güvenlik düzeltmeleri almak değil.

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirme kullanılabilir olduğunda, Linux aracısını yükler ve uzantısını yükseltir. Uzantı otomatik güncelleştirmelerin ya da *küçük* veya *düzeltme*. Kabul et veya uzantıları dışında iyileştirilmiş *küçük* uzantı sağladığınızda güncelleştirir. Aşağıdaki örnek bir Resource Manager şablonu ile küçük sürümlerde otomatik olarak yükseltme gösterir *autoUpgradeMinorVersion ": true,'*:

```json
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
```

En son alt sürüm hata düzeltmeleri almak için otomatik güncelleştirme her zaman uzantısı dağıtımlarınızda seçin önerilir. Güvenlik veya anahtar hata düzeltmeleri taşıyan düzeltme güncelleştirmelerini vazgeçti olamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri belirleme

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Uzantı bir VM ile aynı autoUpgradeMinorVersion ayarlarsanız tanımlama

Uzantı 'ile aynı autoUpgradeMinorVersion' sağladıysanız VM modelden görebilirsiniz. Denetlemek için kullanmak [az vm show](/cli/azure/vm#az-vm-show) ve VM ve kaynak grubu adı şu şekilde sağlayın:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

Aşağıdaki örnek çıktı gösterilmektedir *autoUpgradeMinorVersion* ayarlanır *true*:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Bir autoUpgradeMinorVersion oluştuğunda tanımlama

Uzantısı için bir güncelleştirme gerçekleştiği görmek için gözden geçirme aracı VM açtığında */var/log/waagent.log*.

Aşağıdaki örnekte, sanal makine vardı *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9025* yüklü. Bir düzeltme kullanılabilir *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9027*:

```bash
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Expected handler state: enabled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Decide which version to use
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Use version: 2.3.9027
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Current handler state is: NotInstalled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Download extension package
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Unpack extension package
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Download, message=Download succeeded
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Initialize extension directory
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update settings file: 0.settings
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Disable extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Launch command:diagnostic.py -disable
...
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Disable, message=Launch command succeeded: diagnostic.py -disable
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Launch command:diagnostic.py -update
2017/08/14 20:21:57 LinuxAzureDiagnostic started to handle.
```

## <a name="agent-permissions"></a>Aracısı izinleri

Görevleri gerçekleştirmek için aracının Çalıştır gerekir *kök*.

## <a name="troubleshoot-vm-extensions"></a>VM uzantı sorunlarını giderme

Her VM uzantısı, sorun giderme adımları belirli uzantısına sahip olabilir. Örneğin, özel betik uzantısı kullandığınızda, betik yürütme ayrıntıları yerel olarak VM uzantısı çalıştırdığı bulunabilir. Uzantı özel sorun giderme işlemleri uzantısı özgü belgelerinde açıklanmıştır.

Tüm VM uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

1. Linux Aracısı günlük denetlemek için uzantınız içinde sağlanırken faaliyeti Ara */var/log/waagent.log*

2. Daha fazla bilgi için gerçek uzantı günlükleri denetleyin   */var/oturum/azure /<extensionName>*

3. Hata kodları, bilinen sorunlar vb. için bölümlere gidermek uzantısı özgü belgelere bakın.

3. Sistem günlüklerine bakın. Uzantılı bir uzun süre çalışan özel Paket Yöneticisi erişim gerektiren başka bir uygulama yüklemesini gibi zorlayıcı nedenleriniz diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hatalarının sık karşılaşılan nedenleri

1. Uzantılı çalıştırmak için 20 dakika (özel durumlar: CustomScript uzantıları, Chef ve 90 dakika olan DSC). Dağıtımınız bu defa aşıyorsa, bir zaman aşımı işaretlenir. Bunun nedeni düşük kaynak VM, diğer sanal makine yapılandırmaları/başlangıç uzantı için sağlama okunurken yüksek miktarda kaynak tüketen görevlerini nedeniyle olabilir.

2. En düşük Önkoşullar karşılanmadı. Bazı uzantılar, HPC görüntüleri gibi VM SKU'ları, bağımlılıkları vardır. Uzantılar, Azure depolama veya genel hizmetlerle iletişim kurma gibi belirli ağ erişim gereksinimleri gerektirebilir. Diğer örnekler paket depolarına, disk alanı veya güvenlik kısıtlamaları dışında çalışan erişimi olabilir.

3. Özel Paket Yöneticisi erişim. Bazı durumlarda, uzun süre çalışan bir VM yapılandırması ve uzantı yüklemesi çakışan, burada her ikisi de Paket Yöneticisi özel erişmesi gereken karşılaşabilirsiniz.

### <a name="view-extension-status"></a>Uzantı durumu görüntüle

Bir VM'ye karşı VM uzantısı çalıştırıldıktan sonra kullanmak [az vm get-instance-view](/cli/azure/vm#az-vm-get-instance-view) uzantı durumu şu şekilde geri dönmek için:

```azurecli
az vm get-instance-view \
    --resource-group rgName \
    --name myVM \
    --query "instanceView.extensions"
```

Aşağıdaki örnek çıktıya benzer bir çıkış:

```bash
  {
    "name": "customScript",
    "statuses": [
      {
        "code": "ProvisioningState/failed/0",
        "displayStatus": "Provisioning failed",
        "level": "Error",
        "message": "Enable failed: failed to execute command: command terminated with exit status=127\n[stdout]\n\n[stderr]\n/bin/sh: 1: ech: not found\n",
        "time": null
      }
    ],
    "substatuses": null,
    "type": "Microsoft.Azure.Extensions.customScript",
    "typeHandlerVersion": "2.0.6"
  }
```

Uzantı yürütme durumu, ayrıca Azure portalında bulunabilir. Bir uzantı durumunu görüntülemek için VM seçin, **uzantıları**, ardından istediğiniz uzantıyı seçin.

### <a name="rerun-a-vm-extension"></a>VM uzantısı yeniden çalıştırın

VM uzantısı çalıştırılması gereken durumlar olabilir. Uzantı, kaldırma ve uzantı ile kendi tercih ettiğiniz bir yürütme yöntemi daha sonra yeniden çalıştırabilirsiniz. Bir uzantıyı kaldırmak için [az vm uzantısı silme](/cli/azure/vm/extension#az-vm-extension-delete) gibi:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

Ayrıca uzantı Azure portalında şu şekilde kaldırabilirsiniz:

1. Bir VM'yi seçin.
2. Seçin **uzantıları**.
3. İstediğiniz uzantıyı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extension-reference"></a>Yaygın VM uzantısı başvurusu

| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Linux için özel betik uzantısı |Bir Azure sanal makinesi karşı betikleri çalıştırma |[Linux için özel betik uzantısı](custom-script-linux.md) |
| VM Erişimi uzantısı |Bir Azure sanal makinesi yeniden erişebilmek |[VM Erişimi uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure Tanılama uzantısı |Azure Tanılama'yı yönetme |[Azure tanılama uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişimi uzantısı |Kullanıcı ve kimlik bilgilerini yönetme |[Linux için VM erişimi uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantılarına ve özelliklerine genel bakış](overview.md).
