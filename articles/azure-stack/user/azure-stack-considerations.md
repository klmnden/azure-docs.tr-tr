---
title: Azure Stack ile Azure arasındaki temel farklılıklar Hizmetleri ve uygulamaları oluşturma kullanırken anlama | Microsoft Docs
description: Hizmetleri kullanın veya Azure Stack için uygulamalar oluşturmak için bilmeniz gerekenler.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/08/2019
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 6e7d3ca2ac2d833c0a82a15aafaa3ee3e93097a2
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282602"
---
# <a name="key-considerations-using-services-or-building-apps-for-azure-stack"></a>Önemli noktalar yer: Hizmetleri kullanarak veya Azure Stack için uygulamalar oluşturma

Hizmetleri kullanın veya Azure Stack için uygulamalar oluşturmak önce Azure Stack ve Azure arasındaki farkları anlamak önemlidir. Bu makalede, Azure Stack, hibrit bulut geliştirme ortamı olarak kullandığınızda önemli noktalar yer tanımlar.

## <a name="overview"></a>Genel Bakış

Azure Stack, şirketinizin veya hizmet sağlayıcısı veri merkezinizi Azure hizmetlerinden kullanmanıza olanak sağlayan bir karma bulut platformudur. Azure Stack'te uygulama oluşturma ve Azure Stack, Azure veya Azure hibrit bulut dağıtın.

Azure Stack operatörünüze hangi Hizmetleri kullanabilmeniz için kullanılabilir ve destek alma size sağlayacaktır. Bunlar, kendi özelleştirilmiş planlar ve teklifler aracılığıyla bu hizmetleri sağlar.

Azure teknik içerik, uygulamaları Azure Stack yerine bir Azure hizmeti için Geliştiriliyor varsayar. Derleme ve Azure Stack'e uygulamaları dağıtma, bazı temel farklar gibi anlamanız gerekir:

* Azure Stack Hizmetleri ve Azure'da kullanılabilir olan özelliklerin bir alt kümesini sunar.
* Şirket veya hizmet sağlayıcınızın sunmak istediğiniz hizmetleri seçebilirsiniz. Kullanılabilir seçenekler, özelleştirilmiş hizmet veya uygulamalar içerebilir. Bunlar, kendi özelleştirilmiş belgeleri sunabilir.
* Doğru kullanmanız gerekir (örneğin, bir portal adresi ve Azure Resource Manager uç nokta URL'leri) Azure Stack özel uç noktaları.
* Azure yığını tarafından desteklenen PowerShell ve API sürümleri kullanmanız gerekir. Desteklenen sürümler kullanarak uygulamalarınızı Azure Stack ve Azure iş sağlar.

## <a name="cheat-sheet-high-level-differences"></a>Kopya kağıdı: Üst düzey farklılıklar

Aşağıdaki tabloda, Azure Stack ve Azure arasında üst düzey farklılıklar açıklanmaktadır. Azure Stack için geliştirme veya Azure Stack hizmetlerini kullanırken bu farklılıkları göz önünde bulundurun.

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

| Alan | Azure (Genel) | Azure Stack |
| -------- | ------------- | ----------|
| Kimin çalışır? | Microsoft | Kuruluş veya hizmet sağlayıcısı.|
| Kimin için desteğe başvurun? | Microsoft | Tümleşik bir sistem için destek, Azure Stack operatörü (en, kuruluş veya hizmet sağlayıcınızın) başvurun.<br><br>Azure Stack geliştirme Seti'ni desteği ziyaret [Microsoft forumları](https://social.msdn.microsoft.com/Forums/home?forum=azurestack). Geliştirme Seti değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla sunulan resmi desteği yoktur.
| Kullanılabilir hizmetler | Listesine bakın [Azure ürünleri](https://azure.microsoft.com/services/?b=17.04b). Kullanılabilir hizmetler Azure bölgeye göre farklılık gösterir. | Azure Stack, Azure hizmetlerin bir alt kümesini destekler. Gerçek Hizmetleri, kuruluş veya hizmet sağlayıcınızın sunmak seçtiği göre değişir.
| Azure Resource Manager uç noktası * | https://management.azure.com | Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlayan uç noktayı kullanın.<br><br>Geliştirme Seti için kullanın: https://management.local.azurestack.external.
| Portal URL'si * | [https://portal.azure.com](https://portal.azure.com) | Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlayan URL'yi kullanın.<br><br>Geliştirme Seti için kullanın: https://portal.local.azurestack.external.
| Bölge | Dağıtmak istediğiniz bölgeyi seçebilirsiniz. | Sisteminizde kullanılabilir bölge için bir Azure Stack tümleşik sistemi kullanın.<br><br>Geliştirme Seti için her zaman bölgesi olacak **yerel**.
| Kaynak grupları | Bir kaynak grubu bölgeleri yayılabilir. | Tümleşik sistemler ve Geliştirme Seti için yalnızca bir bölgesi yoktur.
|Desteklenen ad alanlarında, kaynak türleri ve API sürümleri | En son (veya değil henüz kullanım dışı önceki sürümler). | Azure Stack belirli sürümlerini destekler. Bkz: [sürüm gereksinimleri](#version-requirements) bu makalenin.
| | |

* Azure Stack operatörü varsa, bkz. [Yönetici portalını kullanarak](../azure-stack-manage-portals.md) ve [yönetim temel bilgileri](../azure-stack-manage-basics.md) daha fazla bilgi için.

## <a name="helpful-tools-and-best-practices"></a>Faydalı araca ve en iyi uygulamalar

Microsoft Azure Stack için geliştirme, Araçlar ve yardımcı olan rehberlik sağlar.

| Öneri | Başvurular |
| -------- | ------------- |
| Doğru Araçlar, geliştirici iş istasyonunuzda yükleyin. | - [PowerShell'i yükleme](azure-stack-powershell-install.md)<br>- [Araçları indirin](azure-stack-powershell-download.md)<br>- [PowerShell yapılandırma](azure-stack-powershell-configure-user.md)<br>- [Visual Studio'yu yükleyin](azure-stack-install-visual-studio.md) 
| Aşağıdaki öğeler hakkındaki bilgileri gözden geçirin:<br>-Azure Resource Manager şablonu konuları<br>-Hızlı Başlangıç şablonları nasıl bulabileceğiniz<br>-Azure, Azure Stack için geliştirme için kullanmanıza yardımcı olması için bir ilke modülü kullanma | [Azure Stack için geliştirme](azure-stack-developer.md) | 
| Gözden geçirin ve şablonlar için en iyi uygulamaları izleyin. | [Resource Manager hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#best-practices)
| | |

## <a name="version-requirements"></a>Sürüm gereksinimleri

Azure Stack, Azure PowerShell ve Azure hizmet API'lerini belirli sürümlerini destekler. Desteklenen sürümler her iki Azure stack ve azure'a uygulamanızın dağıtabilirsiniz emin olmak için kullanın.

Azure PowerShell doğru sürümünü kullandığınızdan emin olmak için [API sürümü profillerini](azure-stack-version-profiles.md). Kullanabileceğiniz en son API Sürüm profili belirlemek için kullanmakta olduğunuz Azure Stack yapımı bulun. Azure Stack yöneticinizden bu bilgi edinebilirsiniz.

> [!NOTE]
> Azure Stack geliştirme Seti'ni kullanıyorsanız ve yönetici erişiminiz varsa bkz [geçerli sürümünü](../azure-stack-updates.md#determine-the-current-version) Azure Stack derleme belirlemek için bölüm.

Diğer API'ler için ad alanları, kaynak türleri ve Azure Stack aboneliğinizde desteklenen API sürümlerini çıktısını almak için aşağıdaki PowerShell komutunu çalıştırın. Not hala bir özellik düzeyinde farklar olabilir. Bu komutun çalışması için önceden olmalıdır [yüklü](azure-stack-powershell-install.md) ve [yapılandırılmış](azure-stack-powershell-configure-user.md) PowerShell için Azure Stack ortamı. Ayrıca Azure Stack'te teklif aboneliği olması gerekir.

```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

Örnek çıktı: (kesilmiş) ![Get-AzureRmResourceProvider komutunun örnek çıkışı](media/azure-stack-considerations/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir hizmet düzeyinde farklılıklar hakkında daha ayrıntılı bilgi için bkz:

* [Azure Stack'te sanal makineler için dikkat edilmesi gerekenler](azure-stack-vm-considerations.md)
* [Azure stack'teki depolama için dikkat edilmesi gerekenler](azure-stack-acs-differences.md)
* [Azure Stack ağ iletişimi için dikkat edilmesi gerekenler](azure-stack-network-differences.md)
