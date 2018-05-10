---
title: Azure VM uzantıları ve Linux için Özellikler | Microsoft Docs
description: Hangi uzantıları ne bunlar sağlayın veya geliştirmek tarafından gruplandırılmış Azure sanal makineler için kullanılabilir olduğunu öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: 760f832bc12bccbf1cce77db25bf60413ad9a36b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Sanal makine uzantıları ve Linux için özellikleri

Azure sanal makine (VM), dağıtım sonrası yapılandırma ve Otomasyon görevlerini Azure Vm'lerinde sağlayan küçük uygulamalar uzantılarıdır. Örneğin, bir sanal makineye yazılım yükleme, virüsten koruma gerektiriyorsa veya bir komut dosyası, içinde çalıştırmak için bir VM uzantısı kullanılabilir. Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portal ile Azure VM uzantıları çalıştırılabilir. Uzantıları yeni VM Dağıtım ile birlikte veya varolan bir sistemle bağlantılı çalıştırın.

Bu makalede VM uzantıları, Azure VM uzantıları kullanma önkoşulları genel bir bakış sağlar ve algılamak nasıl hakkında yönergeler yönetmek ve VM uzantılarını kaldırın. Birçok VM uzantıları bulunduğundan, bu makalede genelleştirilmiş bilgiler sağlanmaktadır her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantıya özgü ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

## <a name="use-cases-and-samples"></a>Kullanım örnekleri ve örnekler

Birkaç farklı Azure VM uzantıları kullanılabilir, her biri belirli bir kullanım örneği. Bazı örnekler:

- PowerShell istenen durum yapılandırmalar VM DSC uzantısı ile Linux için geçerlidir. Daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Microsoft İzleme Aracısı VM uzantısı olan bir VM izlemeyi yapılandırın. Daha fazla bilgi için bkz: [bir Linux VM izleme](../linux/tutorial-monitoring.md).
- Azure altyapınızın Chef veya Datadog uzantılı izlemeyi yapılandırın. Daha fazla bilgi için bkz: [Chef belgeleri](https://docs.chef.io/azure_portal.html) veya [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).

İşleme özgü uzantılar ek olarak, bir özel betik uzantısı, Windows ve Linux sanal makineleri için kullanılabilir. Linux için özel betik uzantısı, bir VM üzerinde çalıştırılacak Bash komut dosyaları sağlar. Özel komut dosyaları, yerel hangi Azure araçlar sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için faydalıdır. Daha fazla bilgi için bkz: [Linux VM özel betik uzantısı](custom-script-linux.md).

## <a name="prerequisites"></a>Önkoşullar

VM uzantısı işlemek için Azure Linux aracısı yüklü gerekir. Bazı tek tek uzantıların kaynaklarına erişim veya bağımlılıkları gibi önkoşulları vardır.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM Aracısı bir Azure VM ve Azure yapı denetleyicisi arasındaki etkileşimler yönetir. VM Aracısı dağıtma ve yönetme Azure VM'ler, VM Uzantıları'nı çalıştırarak dahil olmak üzere birçok işlevsel görünüşlere için sorumludur. Azure VM Aracısı Azure Marketi görüntülerinde önceden yüklenmiş ve desteklenen işletim sistemlerinde el ile yüklenebilir. Linux için Azure VM Aracısı Linux aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](agent-linux.md).

#### <a name="supported-agent-versions"></a>Desteklenen Aracı sürümleri

Olası en iyi deneyimi sağlamak için aracı en düşük sürümü vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın.

#### <a name="supported-oses"></a>Desteklenen işletim sistemleri

Uzantıları framework bir sınır işletim sistemleri için bu uzantıları sahiptir ancak Linux Aracısı'nı birden çok işletim sistemleri üzerinde çalışır. [Bu makalede] daha fazla bilgi için bkz: (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bazı uzantılar tüm işletim sistemlerinde desteklenmez ve yayabilir *hata kodu 51, 'Desteklenmeyen işletim sistemi'*. Desteklenebilirlik için tek tek uzantısı belgelerine bakın.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure Storage uzantısı deposundan yüklenir ve uzantı durumu yüklemeleri için Azure Storage gönderilen. Kullanırsanız [desteklenen](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) aracıların sürümünü, gereksinim aracı iletişimi aracı iletişimi için Azure yapı denetleyicisi yeniden yönlendirmek için kullanabileceğiniz gibi Azure Storage VM bölgede erişmesine izin vermek. Bir desteklenmeyen Aracı sürümünde varsa, Azure depolama bu bölgede sanal makineden giden erişime izin vermeniz gerekiyor.

> [!IMPORTANT]
> Erişimi engellemişse *168.63.129.1* Konuk Güvenlik Duvarı'nı kullanarak, ardından uzantıları yukarıdaki yedeklemiş başarısız.

Aracıları yalnızca uzantısı paketleri ve raporlama durumu indirmek için kullanılabilir. Örneğin, bir uzantı yükleme komut dosyası (özel komut dosyası) Github'dan karşıdan yüklemek gereken veya Azure Storage (Azure yedekleme) sonra erişim ek gerekiyorsa Güvenlik Duvarı/güvenlik ağ grubu bağlantı noktalarının açılması gerekir. Uygulamaları kendi sağ olduğundan farklı uzantıları farklı gereksinimleri vardır. Azure depolamaya erişim gerektiren uzantılar, Azure NSG hizmet etiketleri kullanarak erişim izni verebilirsiniz [depolama](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

Aracı trafiği isteklerini yeniden yönlendirmek için proxy sunucu desteği Linux Aracısı'nı vardır. Ancak, bu proxy sunucu desteği uzantıları geçerli değildir. Bir proxy ile çalışmak için tek tek her uzantı yapılandırmanız gerekir.

## <a name="discover-vm-extensions"></a>VM uzantıları Bul

Birçok farklı VM uzantıları, Azure sanal makineler ile kullanmak için kullanılabilir. Tam listesini görmek için [az vm uzantısı görüntü listesi](/cli/azure/vm/extension/image#az-vm-extension-image-list). Aşağıdaki örnekte tüm kullanılabilir uzantıları listeler *westus* konumu:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>VM uzantıları çalıştırın

Azure VM uzantıları, yapılandırma değişikliklerini yapın veya zaten dağıtılmış bir VM'de bağlantısı kurtarmak gerektiğinde faydalı olan mevcut Vm'lerinde çalıştırın. VM uzantıları, Azure Resource Manager şablonu dağıtımlarında da gönderilebilir. Resource Manager şablonları ile uzantılarını kullanarak Azure VM'ler dağıtılabilir ve dağıtım sonrası müdahalesi olmadan yapılandırılmış.

Aşağıdaki yöntemlerden bir uzantı karşı mevcut bir VM'yi çalıştırmak için kullanılabilir.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Azure VM uzantıları, mevcut bir VM'yi ile karşı çalıştırılabilir [az vm uzantısı kümesi](/cli/azure/vm/extension#az-vm-extension-set) komutu. Aşağıdaki örnek özel betik uzantısı adlı bir VM'den karşı çalışan *myVM* bir kaynak grubunda adlı *myResourceGroup*:

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Uzantı doğru çalıştığı zaman çıktı aşağıdaki örneğe benzer:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure portalına

VM uzantıları, mevcut bir VM'yi Azure Portalı aracılığıyla uygulanabilir. Portalda VM seçin, **uzantıları**seçeneğini belirleyip **Ekle**. Kullanılabilir uzantılar listeden istediğiniz ve Sihirbazı'ndaki yönergeleri izleyin uzantı seçin.

Aşağıdaki resim Azure portalından Linux özel betik uzantısı yüklemesini gösterir:

![Özel betik uzantısı yükleyin](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları, bir Azure Resource Manager şablonu eklenir ve şablon dağıtımı ile yürütüldü. Bir şablon uzantılı dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturamazsınız. Örneğin, aşağıdaki JSON yük dengeli sanal makineleri ve Azure SQL veritabanını kümesi dağıtır ve ardından her VM .NET Core uygulamayı yükleyen bir Resource Manager şablonu alınır. VM uzantısı yazılım yüklemesi mvc'deki.

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

Resource Manager şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>VM uzantısı verileri güvenli

VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarlarını gibi hassas bilgileri içerecek şekilde gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef VM içinde şifresini çözer korumalı bir yapılandırmayı içerir. Belirli korumalı yapılandırma şeması her uzantısına sahip ve her uzantıya özgü belgelerinde ayrıntılı olarak gösterilmiştir.

Aşağıdaki örnek, Linux için özel betik uzantısı örneğini gösterir. Komutun yürütülmesi için kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifreli değil:

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

Taşıma **yürütülecek komut** özelliğine **korumalı** yapılandırma aşağıdaki örnekte gösterildiği gibi yürütme dize güvenliğini sağlar:

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

### <a name="how-do-agents-and-extensions-get-updated"></a>Aracılar ve uzantıları güncelleştirilme?

Uzantıları ve aracıları aynı güncelleştirme mekanizması paylaşır. Bazı güncelleştirmeler, ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanıma hazır, bir değişiklik uzantılarını ve diğer VM Model değişiklikleri gibi olduğunda yalnızca VM yüklenir:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama kapsayıcısı
- Konuk işletim sistemi gizli
- VM boyutu
- Ağ profili

Olası farklı sürümleri üzerinde farklı bölgelerdeki VM'ye sahip olacak şekilde yayımcılar güncelleştirmeleri farklı zamanlarda bölgelere kullanıma sunma.

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Linux VM Aracısı'nı içeren *sağlama aracısı kodu* ve *uzantısı işleme kod* bir pakette hangi olamaz ayrılmış. Devre dışı bırakabilirsiniz *sağlama Aracısı* bulut init kullanarak azure'da sağlamak istediğinizde. Bunu yapmak için bkz: [bulut init kullanarak](../linux/using-cloud-init.md).

Otomatik Güncelleştirmeler aracıların desteklenen sürümlerini kullanabilirsiniz. Güncelleştirilebilir yalnızca kodu *uzantısı işleme kod*, sağlama kod değil. *Sağlama aracısı kodu* bir kez çalıştır kodudur.

*Uzantısı işleme kod* Azure doku ile iletişim kurmasını ve VM uzantıları işlemleri gibi işleme, durum bildirimi, tek tek uzantıların güncelleştirme ve bunları kaldırma yüklemeler için sorumludur. Güncelleştirmeleri, güvenlik düzeltmeleri, hata düzeltmeleri ve geliştirmeler içerir *uzantısı işleme kod*.

Üst arka plan programı, aracıyı yüklediğinizde oluşturulur. Bu üst daha sonra uzantılarını işlemek için kullanılan bir alt işlem olarak çoğaltılır. Aracı için bir güncelleştirme varsa, karşıdan yüklenir, üst alt işlemi durdurur, onu yükseltir, sonra yeniden başlatır. Güncelleştirme ile ilgili bir sorun olması, üst işlemi önceki alt sürüme geri alınır.

Üst işlem güncelleştirilmiş otomatik olamaz. Üst yalnızca bir distro paket güncelleştirmesi tarafından güncelleştirilebilir.

Çalıştırdığınız hangi sürümünü denetlemek için denetleme `waagent` gibi:

```bash
waagent --version
```

Çıktı aşağıdaki örneğe benzer:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

Yukarıdaki örnek çıktıda üst 'paket sürüm dağıtılan' mi *WALinuxAgent 2.2.17*

'Hedef Durumu Aracı' otomatik güncelleştirme sürümüdür.

Otomatik Güncelleştirme Aracısı için her zaman sahip önerilen [AutoUpdate.Enabled=y](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent). Değil el ile aracı güncelleştirme tutmanız gerekir yani etkin olması ve hata ve güvenlik düzeltmeleri almak değil.

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirme kullanılabilir olduğunda, Linux aracısını yükler ve uzantısını yükseltir. Otomatik uzantısı güncelleştirmelerin ya da *küçük* veya *düzeltme*. Kabul ya da uzantıları dışında opt *küçük* uzantısı sağladığınızda güncelleştirir. Aşağıdaki örnek Resource Manager şablonu ile ikincil sürümlerinde otomatik olarak Yükselt gösterilmektedir *autoUpgradeMinorVersion ": true,'*:

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

En son alt sürüm hata düzeltmeleri almak için otomatik güncelleştirme her zaman uzantısı dağıtımlarınızı seçin kullanmamanız önerilir. Güvenlik veya anahtar hata düzeltmeleri taşımak düzeltme güncelleştirmelerini geri çevrildi olamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri belirleme

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Uzantı üzerinde bir VM ile aynı autoUpgradeMinorVersion ayarlarsanız tanımlama

Uzantı 'autoUpgradeMinorVersion' ile sağlanan durumunda VM modelden görebilirsiniz. Denetlemek için kullanın [az vm Göster](/cli/azure/vm#az-vm-show) ve VM ve kaynak grubu adı aşağıdaki gibi belirtin:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

Aşağıdaki örnek çıkış gösterir *autoUpgradeMinorVersion* ayarlanır *true*:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Bir autoUpgradeMinorVersion oluştuğunda tanımlama

Aracısı'nı gözden geçirin, uzantı için bir güncelleştirme gerçekleştiği bakın oturum açtığında VM */var/log/waagent.log*.

Aşağıdaki örnekte, VM vardı *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9025* yüklü. Bir düzeltme için kullanılabilir *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9027*:

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

## <a name="agent-permissions"></a>Aracı izinleri

Görevleri gerçekleştirmek için aracı olarak çalıştırmak gerek duyduğu *kök*.

## <a name="troubleshoot-vm-extensions"></a>VM uzantıları sorun giderme

Her VM uzantısı, sorun giderme adımları belirli uzantısına sahip olabilir. Örneğin, özel betik uzantısı kullandığınızda, komut dosyası yürütme ayrıntılarını yerel olarak uzantısı çalıştırdığı VM üzerinde bulunabilir. Uzantı özel sorun giderme işlemleri uzantıya özgü belgelerinde açıklanmıştır.

Tüm VM uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

1. Linux Aracısı günlük denetlemek için uzantınızı içinde hazırlandığında faaliyeti Ara */var/log/waagent.log*

2. Daha ayrıntılı bilgi için gerçek uzantı günlükleri denetleyin   */var/oturum/azure /<extensionName>*

3. Hata kodları, bilinen sorunlar vb. için bölümleri sorun giderme uzantıya özgü belgelere bakın.

3. Sistem günlüklerine bakın. Özel Paket Yöneticisi erişim gerektiren başka bir uygulamanın uzun süren bir yükleme gibi uzantıya sahip uğratmıştır diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hataları yaygın nedenler

1. Uzantılara sahip çalıştırmak için 20 dakika (özel durumlar: CustomScript uzantıları, Chef ve 90 dakika sahip DSC). Bu süre, dağıtımınızı aşarsa, bir zaman aşımı olarak işaretlenir. Bunun nedeni düşük kaynak VM'ler, diğer VM yapılandırmaları/başlangıç uzantısı sağlamak okunurken yüksek miktarda kaynak kullanan görevler nedeniyle olabilir.

2. Minimum Önkoşullar karşılanmadı. Bazı uzantılar HPC görüntüleri gibi VM SKU'larında bağımlılıkları vardır. Uzantılar, Azure Storage veya kamu hizmetleri için iletişim gibi belirli ağ erişim gereksinimleri gerektirebilir. Diğer örnekler paket depoları, disk alanı veya güvenlik kısıtlamaları dışında çalışan erişimi olabilir.

3. Özel Paket Yöneticisi erişim. Bazı durumlarda, bir uzun süre çalışan VM yapılandırma ve genişletme yüklemesinin çakışan, burada her ikisi de özel Paket Yöneticisi erişmesi karşılaşabilirsiniz.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüle

VM uzantısı bir VM'ye karşı çalıştırdıktan sonra kullanmak [az vm get-örnek-görünümü](/cli/azure/vm#az-vm-get-instance-view) uzantı durumunu aşağıdaki gibi döndürmek için:

```azurecli
az vm get-instance-view \
    --resource-group rgName \
    --name myVM \
    --query "instanceView.extensions"
```

Çıktı aşağıdaki örnek çıkış benzer:

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

Uzantı yürütme durumu de Azure portalında bulunabilir. Uzantı durumunu görüntülemek için VM seçin, **uzantıları**, istenen uzantıyı seçin.

### <a name="rerun-a-vm-extension"></a>VM uzantısı yeniden çalıştırın

VM uzantısı yeniden çalıştırılması gereken durumlar olabilir. Uzantı, kaldırarak ve tercih ettiğiniz yürütme yöntemiyle uzantısı yeniden çalıştırma çalıştırabilirsiniz. Bir uzantıyı kaldırmak için kullanın [az vm uzantısı delete](/cli/azure/vm/extension#az-vm-extension-delete) gibi:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

Ayrıca bir uzantı Azure portalında şu şekilde kaldırabilirsiniz:

1. Bir VM'yi seçin.
2. Seçin **uzantıları**.
3. İstenen uzantıyı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extension-reference"></a>Ortak VM uzantısı başvurusu

| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Linux için özel betik uzantısı |Bir Azure sanal makinesi karşı komut dosyalarını çalıştır |[Linux için özel betik uzantısı](custom-script-linux.md) |
| VM Erişimi uzantısı |Bir Azure sanal makinesi erişimi yeniden kazanmak |[VM Erişimi uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure tanılama uzantısını |Azure tanılama yönetme |[Azure tanılama uzantısını](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişim uzantısı |Kullanıcılar ve kimlik bilgilerini yönetme |[Linux VM erişim uzantısı](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantıları ve özellikleri genel bakış](overview.md).
