---
title: Azure Stack yönetim temel bilgileri | Microsoft Docs
description: Azure Stack yönetmek için bilmeniz gerekenleri öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.openlocfilehash: 37b8eff2d4ed89c90f1fa6f128673ed5bacaaa90
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339959"
---
# <a name="azure-stack-administration-basics"></a>Azure Stack yönetim temel bilgileri
Azure Stack Yönetim için yeni bilmeniz gereken birkaç şey vardır. Bu kılavuz, Azure Stack operatörü olarak sizin rolünüze genel bir bakış ve hızlı bir şekilde üretken olmak için bunları kullanıcılarınıza söylemeniz gerekenler sağlar.

## <a name="understand-the-builds"></a>Yapıları anlama

### <a name="integrated-systems"></a>Tümleşik sistemler

Bir Azure Stack tümleşik sistemi kullanıyorsanız, Azure Stack güncelleştirilmiş sürümlerini güncelleştirme paketleri dağıtılır. Bu paketleri içeri aktarmak ve güncelleştirmeleri kutucuk Yönetici portalı'nda kullanarak uygulayabilirsiniz.
 
### <a name="development-kit"></a>Geliştirme Seti

Azure Stack geliştirme Seti'ni kullanıyorsanız, gözden [Azure Stack nedir?](.\asdk\asdk-what-is.md) makalenin amacı, Geliştirme Seti ve kısıtlamalarını anladığınızdan emin olun. ", Azure Stack'i değerlendirin ve geliştirme ve üretim dışı ortamda uygulamalarınızı test etmek bir korumalı alan," olarak Geliştirme Seti kullanmanız gerekir. (Dağıtım bilgileri için bkz. [Azure Stack geliştirme Seti'ni dağıtım](.\asdk\asdk-install.md) makale.)

Azure'da olduğu gibi biz yenilikleri hızlıca sunun. Biz yeni derlemeler düzenli olarak kullanıma sunarız. Geliştirme Seti çalıştırıyorsanız ve taşımak en son sürüme gerekir istediğiniz [Azure Stack'i yeniden dağıtma](.\asdk\asdk-redeploy.md). Güncelleştirme paketleri uygulanamıyor. Bu işlem zaman alır, ancak en son özellikleri deneyin avantajdır. Web Geliştirme Seti belgelerine en son sürüm yapısı yansıtır.

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetleri hakkında bilgi edinin

Hangi Hizmetleri, kullanıcılarınıza sunabileceğiniz bir farkındalık gerekir. Azure Stack, Azure hizmetlerin bir alt kümesini destekler. Desteklenen hizmet listesini gelişmeye devam eder.

**Temel Hizmetleri**

Varsayılan olarak, Azure Stack şu "temel" Hizmetleri Azure Stack dağıtırken:

- İşlem
- Depolama
- Ağ
- Key Vault

Bu temel hizmetlerle minimal yapılandırma ile kullanıcılarınıza-olarak-hizmet altyapı (Iaas) sunabilir.

**Ek hizmetler**

Şu anda aşağıdaki ek olarak bir-hizmet Platform (PaaS) Hizmetleri destekler:

- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

Bu hizmetler, bunları kullanıcılarınıza sunabileceğiniz önce ek yapılandırma gerektirir. Daha fazla bilgi için "Öğreticiler" ve müşterilerimizin Azure Stack operatör belgeleri "nasıl yapılır guides\Offer Hizmetleri" bölümlerine bakın.

**Hizmet yol haritası**

Azure Stack, Azure Hizmetleri için destek eklemeye devam edeceğiz. Gelecekteki yol haritası için bkz. [Azure Stack: bir Azure uzantısı](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) teknik incelemesi. Ayrıca izleyebilirsiniz [Azure Stack blog gönderilerini](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) yeni duyuruları.

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure Stack yönetirken bilmeniz gereken birkaç hesabında dikkate alınacak noktalar vardır. Dağıtımlarda özellikle Windows Server Active Directory Federasyon Hizmetleri (ADFS) yerine Azure Active Directory (Azure AD) kimlik sağlayıcısı olarak kullanma. Azure Stack tümleşik sistemleri ve ASDK dağıtımları için aşağıdaki hesabı maddeler geçerlidir:


|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (. \Administrator)|ASDK konak yönetici|ASDK konak yönetici|
|AzureStack\AzureStackAdmin|ASDK konak yönetici<br><br>Azure Stack yönetim portalında oturum açmak için kullanılabilir<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim|ASDK konak yönetici<br><br>Azure Stack yönetim portalına erişim yok<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim<br><br>Artık sahibi varsayılan sağlayıcı aboneliği (DPS)|
|AzureStack\CloudAdmin|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın<br><br>ASDK ana bilgisayara oturum yok<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|
|Azure AD genel Yöneticisi|Yükleme sırasında kullanılan<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|Uygulanamaz|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi araçları kullanabilir?
 
Kullanabileceğiniz [Yönetici portalı](azure-stack-manage-portals.md) veya Azure Stack yönetmek için PowerShell'i. Temel kavramları öğrenmenin en kolay yolu portalı kullanmaktır. PowerShell kullanmak istiyorsanız, hazırlık adımları vardır. Gerekir [yükleme](azure-stack-powershell-install.md) PowerShell [indirme](azure-stack-powershell-download.md) ek modüller ve [yapılandırma](azure-stack-powershell-configure-admin.md) PowerShell.

Azure Stack, Azure Resource Manager, temel alınan dağıtımı, yönetimi ve kuruluş mekanizması olarak kullanır. Azure Stack yönetmek ve kullanıcıların desteklemeye yardımcı olmak için kullanacaksanız, Resource Manager hakkında bilgi edinin. Bkz: [Azure Resource Manager ile Başlarken](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) teknik incelemesi.

## <a name="your-typical-responsibilities"></a>Tipik sizin Sorumluluklarınız

Kullanıcıların hizmetleri kullanmak istiyorsunuz. Kendi bakış açısına ana rolünüz hizmetlerin kullanılabilir olmasını sağlamaktır. Hangi Hizmetleri sunmaya karar verin ve planlar, teklifler ve kotalar oluşturarak bu hizmetleri kullanılabilir yapın. Daha fazla bilgi için [Azure Stack'te hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md). 

Öğe ekleme gerekecektir [Market](azure-stack-marketplace.md), sanal makine görüntüleri gibi. En kolay yolu [Azure Stack için Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> Planlar, teklifler ve Hizmetleri test etmek isterseniz, kullanması gereken [kullanıcı portalı](azure-stack-manage-portals.md); olmayan Yönetici portalı.

Hizmetleri sağlamaya ek olarak, bir işlecin tüm normal görevlerini Azure Stack'te çalışır tutmak için gerçekleştirmeniz gerekir. Bu görevler aşağıdakileri içerir:

- Kullanıcı hesaplarını ekleyin (için [Azure Active Directory](azure-stack-add-new-user-aad.md) dağıtım veya [Active Directory Federasyon Hizmetleri](azure-stack-add-users-adfs.md) dağıtım)
- [Rol tabanlı erişim denetimi (RBAC) Rolleri Ata](azure-stack-manage-permissions.md) (Bu yöneticilerin sınırlı değildir.)
- [Altyapı sistem durumunu izleme](azure-stack-monitor-health.md)
- Yönetme [ağ](azure-stack-viewing-public-ip-address-consumption.md) ve [depolama](azure-stack-manage-storage-accounts.md) kaynakları
- Hatalı donanım değiştirin; örneğin [hatalı bir diski değiştirme](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Kullanıcılarınıza söylemeniz gerekenler

Azure Stack'te hizmetler ile nasıl çalışılacağını ve ortamına bağlanmak nasıl tekliflere abone olma, kullanıcılarınıza gerekecektir. Özel belgeler yanı sıra kullanıcılara sağlamak istediğiniz kullanıcıları Azure Stack kullanıcı belgeleri sitesine yönlendirebilir.

**Azure Stack'te hizmetler ile nasıl çalışılacağını anlama**

Hizmetleri kullanın ve Azure Stack'te uygulamalar oluşturmak için önce kullanıcılarınızın anlamalısınız bilgisi yoktur. Örneğin, belirli PowerShell ve API sürüm gereksinimleri vardır. Ayrıca, bir Azure hizmeti ve Azure stack'teki eşdeğer hizmeti arasında bazı özellik farkları vardır. Kullanıcılarınızın aşağıdaki makaleleri gözden emin olun:

- [Anahtar dikkat edilmesi gerekenler: hizmetlerini kullanarak veya Azure Stack için uygulamalar oluşturma](user/azure-stack-considerations.md)
- [Azure Stack'te sanal makineler için dikkat edilmesi gerekenler](user/azure-stack-vm-considerations.md)
- [Depolama: farklılıklar ve dikkat edilmesi gerekenler](user/azure-stack-acs-differences.md)

Bilgiler Bu makaleler, bir Azure hizmeti ve Azure Stack arasındaki farklar özetlenmektedir. Bu, genel Azure belgeleri bir Azure hizmeti için kullanılabilir olan bilgileri sağlar.

**Bir kullanıcı olarak Azure Stack'e bağlanma**

Bir kullanıcının Geliştirme Seti barındırmak için Uzak Masaüstü erişimi yoksa, Azure Stack erişebilmeniz için önce bir geliştirme seti ortamında, sanal özel ağ (VPN) bağlantısı yapılandırmanız gerekir. Bkz: [Azure Stack'e bağlanma](azure-stack-connect-azure-stack.md). 

Kullanıcılarınızın kattığını bilmek isteyecektir nasıl [kullanıcı portalı ](user/azure-stack-use-portal.md) veya PowerShell üzerinden bağlanma. Tümleşik sistemleri ortamında dağıtım Kullanıcı Portalı adresi değişir. Kullanıcılarınızın doğru URL ile sağlamanız gerekir.

PowerShell kullanarak, kullanıcıların Hizmetleri kullanabilmeniz için önce kaynak sağlayıcılarını kaydetme gerekebilir. (Bir kaynak sağlayıcısı, bir hizmet yönetir. For example, ağ kaynak sağlayıcısı sanal ağlar, ağ arabirimleri ve yük Dengeleyiciler gibi kaynakları yönetir.) Bunlar gerekir [yükleme](user/azure-stack-powershell-install.md) PowerShell [indirme](user/azure-stack-powershell-download.md) ek modüller ve [yapılandırma](user/azure-stack-powershell-configure-user.md) (kaynak Sağlayıcısı kaydı içeren) PowerShell.

**Bir teklife abone olma**

Bir kullanıcı Hizmetleri erişebilmeniz için önce bunlar gerekir [teklife abone olma](azure-stack-subscribe-plan-provision-vm.md) operatör oluşturduğunuz.

## <a name="where-to-get-support"></a>Nereden destek

### <a name="integrated-systems"></a>Tümleşik sistemler

Tümleşik bir sistem için Eşgüdümlü yükseltme ve özgün ekipman üreticisi (OEM) donanım iş ortaklarımız ile Microsoft arasındaki çözümleme işlemi yoktur.

Bulut Hizmetleri sorun varsa, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla destek sunulur. Yönetici portalının sağ üst köşedeki Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından **yeni destek isteği**, bu site burada doğrudan açabileceğiniz bir destek isteği açar.

Dağıtım ile ilgili bir sorun varsa, düzeltme eki ve güncelleştirme, donanım (alan değiştirebilen birim dahil) ve donanım markalı yazılımları, donanım yaşam döngüsü konak üzerinde çalışan yazılımı gibi ilk OEM donanım satıcınıza başvurun.

Diğer her şey için Microsoft CSS ile iletişime geçin.

### <a name="development-kit"></a>Geliştirme Seti

Geliştirme Seti için destek ile ilgili sorular sorabilirsiniz [Microsoft forumları](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Yönetici portalının sağ üst köşedeki Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından **yeni destek isteği**, doğrudan bu forum sitesini açar. Bu Forum düzenli olarak izlenir. Geliştirme Seti değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)


