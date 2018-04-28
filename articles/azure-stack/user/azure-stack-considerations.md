---
title: Azure ve Azure yığın arasındaki temel farklılıklar Hizmetleri ve yapı uygulamaları kullanırken anlama | Microsoft Docs
description: Hizmetleri kullanın veya Azure yığını için uygulamalar oluşturmak için bilmeniz gerekenler.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/16/2018
ms.author: mabrigg
ms.openlocfilehash: eab208175f7eb3b761ec7266483a7cd5268198e8
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="key-considerations-using-services-or-building-apps-for-azure-stack"></a>Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma

Hizmetleri kullanın ya da uygulamalar için Azure yığın oluşturma önce Azure yığını ve Azure arasındaki farkları anlamak gerekir. Karma bulut geliştirme ortamınızı Azure yığın kullandığınızda bu makalede önemli noktalar tanımlar.

## <a name="overview"></a>Genel Bakış

Azure Stack, şirketinizin veya hizmet sağlayıcınızın veri merkezinden Azure hizmetleri sağlama olanağı sunan bir hibrit bulut platformudur. Azure yığında bir uygulama oluşturun ve ardından Azure yığını, Azure veya Azure karma bulut dağıtın.

Azure yığın operatörünüze, hangi Hizmetleri kullanabilmeniz için kullanılabilir ve destek alma bilmeniz olanak tanır. Bunlar, bu hizmetleri aracılığıyla kendi özelleştirilmiş planları ve teklifleri sunar.

Azure teknik içeriği uygulamalar için bir Azure hizmeti Azure yığın yerine geliştirilmektedir varsayar. Derleme ve Azure yığın uygulama dağıtma, bazı temel farklar gibi anlamanız gerekir:

* Azure yığın bir alt kümesini Azure'da kullanılabilen özellikleri ve hizmetleri sunar.
* Şirket veya hizmet sağlayıcınız sunmak için istedikleri hangi hizmetlerin seçebilirsiniz. Kullanılabilir seçenekler, özelleştirilmiş Hizmetleri veya uygulamaları içerebilir. Bunlar, kendi özelleştirilmiş belgeleri sunabilir.
* Doğru kullanmanız gerekir (örneğin, portal adresi ve Azure Resource Manager uç nokta URL'leri) Azure yığın özel uç noktaları.
* Azure yığını tarafından desteklenen PowerShell ve API sürümleri kullanmanız gerekir. Desteklenen sürümlerini kullanarak uygulamalarınızı Azure yığın hem Azure çalışacağını sağlar.

## <a name="cheat-sheet-high-level-differences"></a>Kopya sayfası: üst düzey farklılıklar

Aşağıdaki tabloda Azure yığını ve Azure arasında üst düzey farklılıklar açıklanmaktadır. Geliştirmek için Azure yığın veya Azure yığın Hizmetleri kullandığınızda bu farklılıklar aklınızda bulundurun.
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

| Alan | Azure (Genel) | Azure Stack |
| -------- | ------------- | ----------|
| Kimin çalışır? | Microsoft | Kuruluş ya da hizmet sağlayıcısı.|
| Kimin için destek iletişim? | Microsoft | Tümleşik bir sistem için Azure yığın operatörünüze (Sağlayıcınızdaki kuruluş veya hizmet) desteği ile iletişime geçin.<br><br>Azure yığın Geliştirme Seti desteği ziyaret [Microsoft forumları](https://social.msdn.microsoft.com/Forums/home?forum=azurestack). Geliştirme Seti bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri'ne (CSS) aracılığıyla sunulan resmi desteği yoktur.
| Kullanılabilir hizmetler | Listesine bakın [Azure ürünleri](https://azure.microsoft.com/services/?b=17.04b). Kullanılabilir hizmetler Azure bölgeye göre değişir. | Azure yığını, Azure hizmetleri kümesini destekler. Gerçek Hizmetleri, kuruluş veya hizmet sağlayıcınızın sunmak seçtiği göre değişir.
| Azure Resource Manager uç nokta * | https://management.azure.com | Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan uç noktası kullan.<br><br>Geliştirme Seti için kullanın: https://management.local.azurestack.external
| Portal URL * | [https://portal.azure.com](https://portal.azure.com) | Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan URL'sine gidin.<br><br>Geliştirme Seti için kullanın: https://portal.local.azurestack.external
| Bölge | Dağıtmak istediğiniz hangi bölgeyi seçebilirsiniz. | Bir Azure tümleşik yığını sistemi için sisteminizde kullanılabilir bir bölge kullanın.<br><br>Geliştirme Seti için bölge her zaman olacaktır **yerel**.
| Kaynak grupları | Bir kaynak grubu bölgeler yayılabilir. | Tümleşik sistemleri ve Geliştirme Seti için yalnızca bir bölgesi yoktur.
|Desteklenen ad alanları, kaynak türleri ve API sürümleri | En son (veya değil henüz kullanım dışı bırakılmıştır önceki sürümler). | Azure yığın belirli sürümlerini destekler. Bu makalenin "Sürüm gereksinimleri" bölümüne bakın.
| | |

* Bir Azure yığın işleç olup olmadığını görmek [Yönetici portalı'nı kullanarak](../azure-stack-manage-portals.md) ve [Yönetimi Temelleri](../azure-stack-manage-basics.md) daha fazla bilgi için.

## <a name="helpful-tools-and-best-practices"></a>Yararlı Araçlar ve en iyi yöntemler
 
 Microsoft Azure yığın için çeşitli araçlar ve yardımcı olacak yönergeler geliştirmek sağlar.

| Öneri | Başvurular | 
| -------- | ------------- | 
| Doğru Araçlar Geliştirici iş istasyonunuza yükleyin. | - [PowerShell yükle](azure-stack-powershell-install.md)<br>- [Araçları yükleyin](azure-stack-powershell-download.md)<br>- [PowerShell yapılandırma](azure-stack-powershell-configure-user.md)<br>- [Visual Studio yükleme](azure-stack-install-visual-studio.md) 
| Aşağıdaki öğeler hakkındaki bilgileri gözden geçirin:<br>-Azure Resource Manager şablonu konuları<br>-Hızlı Başlangıç şablonlarını bulma hakkında<br>-Azure yığını geliştirmek için Azure kullanmanıza yardımcı olması için bir ilke modülü'ı kullanın | [Azure Stack için geliştirme](azure-stack-developer.md) | 
| Gözden geçirin ve şablonları için en iyi uygulamaları izleyin. | [Resource Manager hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#best-practices)
| | |

## <a name="version-requirements"></a>Sürüm gereksinimleri

Azure yığını, Azure PowerShell ve Azure hizmeti API'ler belirli sürümlerini destekler. Desteklenen sürümlerin hem Azure yığınına ve Azure uygulamanızı dağıtabilirsiniz emin olmak için kullanın.

Azure PowerShell doğru sürümünü kullandığınızdan emin olmak için [API sürümü profilleri](azure-stack-version-profiles.md). Kullanabileceğiniz en son API sürümü profili belirlemek için kullanmakta olduğunuz Azure yığının yapımı bulun. Bu bilgiler Azure yığın yöneticinizden alabilirsiniz.

>[!NOTE]
 Azure yığın Geliştirme Seti kullanıyorsanız ve yönetim erişimi varsa, "geçerli sürümünü belirleme" bölümüne bakın [yönetmek güncelleştirmeleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#determine-the-current-version) Azure yığın yapı belirlemek için.

Diğer API'lara için ad alanları, kaynak türleri ve Azure yığın aboneliğinizde desteklenen API sürümleri çıktısını almak için aşağıdaki PowerShell komutunu çalıştırın. Not hala özelliği düzeyinde farklılıklar olabilir. (Bu komutun çalışması için önceden olmalıdır [yüklü](azure-stack-powershell-install.md) ve [yapılandırılmış](azure-stack-powershell-configure-user.md) PowerShell Azure yığın ortamı için. Ayrıca Azure yığın teklif aboneliğine sahip olmalıdır.)

 ```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

(Kesilmiş) örnek çıktı: ![örnek çıktı Get-AzureRmResourceProvider komutu](media/azure-stack-considerations/image1.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Bir hizmet düzeyinde farklar hakkında daha ayrıntılı bilgi için bkz:

* [Sanal makineler Azure yığınında dikkate alınacak noktalar](azure-stack-vm-considerations.md)
* [Azure yığın depolama için dikkat edilecek noktalar](azure-stack-acs-differences.md)
* [Azure yığın ağ için ilgili önemli noktalar](azure-stack-network-differences.md)
