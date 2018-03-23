---
title: Azure yığın yönetim temel kavramları | Microsoft Docs
description: Azure yığın yönetmek için bilmeniz gerekenleri öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 799651caf937ca2bafc79dc76f99ae43e700673a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-stack-administration-basics"></a>Azure yığın Yönetimi temelleri
Azure yığın yönetim yeni bilmeniz gereken birkaç nokta vardır. Bu kılavuz, rol Azure yığın işleç olarak genel bir bakış ve hızlı bir şekilde üretken olmak için bunları kullanıcılarınıza söylemeniz gerekenler sağlar.

## <a name="understand-the-builds"></a>Derlemeleri anlama

### <a name="integrated-systems"></a>Tümleşik sistemler

Bir Azure tümleşik yığını sistemi kullanıyorsanız, Azure yığın güncelleştirilmiş sürümlerini güncelleştirme paketleri dağıtılır. Bu paketleri içeri aktarmak ve bunları Yönetici portalı'nda güncelleştirmeleri döşeme kullanarak uygulayabilirsiniz.
 
### <a name="development-kit"></a>Geliştirme Seti

Azure yığın Geliştirme Seti kullanıyorsanız, gözden [Azure yığın nedir?](azure-stack-poc.md) makale amacını Geliştirme Seti ve onun kısıtlamaları anladığınızdan emin olun. "Burada Azure yığın değerlendirmek ve geliştirmek ve uygulamalarınızı bir üretim dışı ortamda test etmek bir korumalı alan," olarak Geliştirme Seti kullanmanız gerekir. (Dağıtım bilgileri için bkz: [Azure yığın Geliştirme Seti dağıtım](azure-stack-deploy-overview.md) quickstart.)

Azure gibi biz hızlı bir şekilde yenilik. Biz düzenli olarak yeni yayın derlemeleri. Geliştirme Seti çalıştırıyorsanız ve taşımak en son sürüme için yapmanız gerekenler istediğiniz [Azure yığın dağıtmanız](azure-stack-redeploy.md). Güncelleştirme paketleri uygulanamıyor. Bu işlem zaman alır, ancak en son özellikleri deneyebilirsiniz avantajdır. Bizim Web sitesi Geliştirme Seti belgelerine en son sürüm yapı yansıtır.

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetler hakkında bilgi edinin

Hangi hizmetlerin, kullanıcılarınız için kullanılabilir duruma getirebilirsiniz tanıma gerekir. Azure yığını, Azure hizmetleri kümesini destekler. Desteklenen hizmetlerin listesini gelişmeye devam eder.

**Temel Hizmetleri**

Varsayılan olarak, aşağıdaki "temel Hizmetleri" Azure yığın içerir dağıttığınızda Azure yığını:

- İşlem
- Depolama
- Ağ
- Anahtar Kasası

Bu temel hizmetlerle minimal yapılandırma ile kullanıcılarınıza-olarak-hizmet altyapı (Iaas) sunabilir.

**Ek hizmetler**

Şu anda aşağıdaki ek olarak bir-hizmet Platform (PaaS) Hizmetleri destekler:

- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

Bu hizmetler, bunları kullanılabilir kullanıcılarınıza hale getirmeden önce ek yapılandırma gerektirir. Daha fazla bilgi için "Öğreticileri" ve Azure yığın işleci Belgelerimizi "nasıl yapılır guides\Offer Hizmetleri" bölümlerine bakın.

**Hizmet yol haritası**

Azure yığını, Azure Hizmetleri için destek eklemek devam eder. Tahmini yol haritası için bkz: [Azure yığın: Azure uzantısı](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) teknik incelemesi. Da izleyebilirsiniz [Azure yığın blog gönderileri](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) yeni duyuruları için.

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi Araçlar kullanıyor?
 
Kullanabileceğiniz [Yönetici portalı](azure-stack-manage-portals.md) veya Azure yığın yönetmek için PowerShell. Portalı aracılığıyla temel kavramları öğrenmeniz en kolay yoludur. PowerShell kullanmak istiyorsanız, hazırlık adımları vardır. Gerekir [yüklemek](azure-stack-powershell-install.md) PowerShell [karşıdan](azure-stack-powershell-download.md) ek modüller ve [yapılandırmak](azure-stack-powershell-configure-admin.md) PowerShell.

Azure yığın Azure Resource Manager, temel alınan dağıtım, yönetim ve kuruluş mekanizması olarak kullanır. Azure yığın yönetmek ve kullanıcıların desteklemeye yardımcı olmak için kullanacaksanız, Resource Manager hakkında bilgi edinin. Bkz: [Azure Resource Manager ile Başlarken](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) teknik incelemesi.

## <a name="your-typical-responsibilities"></a>Tipik sizin Sorumluluklarınız

Kullanıcılarınızın hizmetleri kullanmak istiyorsunuz. Kendi açısından, ana hizmetlerin bunları kullanılabilir hale getirmek için rolüdür. Sunmak için hangi hizmetlerin karar ve bu hizmetleri planları, teklifleri ve kotalar oluşturarak kullanılabilmesi gerekir. Daha fazla bilgi için bkz: [Azure yığınında hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md). 

Öğeleri Ekle gerekecektir [Market](azure-stack-marketplace.md), sanal makine görüntüleri gibi. En kolay yolu [Market öğesi Azure'dan Azure yığınına karşıdan](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> Test planları, teklifleri ve Hizmetleri istiyorsanız, kullanması gereken [kullanıcı portalı](azure-stack-manage-portals.md); değil Yönetici portalı.

Hizmetleri sağlamaya ek olarak, bir işleç tüm normal görevleri Azure yığın hazır ve çalışır tutmak için gerçekleştirmeniz gerekir. Bu görevleri şunlardır:

- Kullanıcı hesaplarını ekleyin (için [Azure Active Directory](azure-stack-add-new-user-aad.md) dağıtım veya [Active Directory Federasyon Hizmetleri](azure-stack-add-users-adfs.md) dağıtım)
- [Rol tabanlı erişim denetimi (RBAC) Rolleri Ata](azure-stack-manage-permissions.md) (Bu yöneticilere sınırlı değildir.)
- [İzleyici altyapı durumu](azure-stack-monitor-health.md)
- Yönetme [ağ](azure-stack-viewing-public-ip-address-consumption.md) ve [depolama](azure-stack-manage-storage-accounts.md) kaynakları
- Hatalı donanım, örneğin yerine [hatalı bir diski değiştirmek](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Kullanıcılarınıza söylemeniz gerekenler

Kullanıcılarınıza Azure yığınında Hizmetleri ile nasıl çalışılacağını, ortama bağlanma ve tekliflerini abone olma bilmeniz gerekir. Tüm özel belge yanı sıra, kullanıcılarınızın sağlamak isteyebilirsiniz, kullanıcıları Azure yığın kullanıcıların belgeleri sitesine yönlendirebilirsiniz.

**Azure yığınında Hizmetleri ile nasıl çalışılacağını anlama**

Hizmetleri kullanın ve Azure yığınında uygulamalar oluşturmak için önce kullanıcılarınızın anlamalısınız bilgisi yoktur. Örneğin, belirli PowerShell ve API sürümü gereksinimleri vardır. Ayrıca, bir hizmet olarak Azure yığınında eşdeğer hizmeti arasındaki bazı özellik farkları vardır. Kullanıcılarınızın aşağıdaki makaleleri gözden emin olun:

- [Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](user/azure-stack-considerations.md)
- [Sanal makineler Azure yığınında dikkate alınacak noktalar](user/azure-stack-vm-considerations.md)
- [Depolama: farklar ve ilgili önemli noktalar](user/azure-stack-acs-differences.md)

Bu makaleler bilgileri Azure hizmetinde ve Azure yığını arasındaki farklar özetlenmektedir. Bir Azure hizmeti genel Azure belgelerinde kullanılabilir bilgi tamamlar.

**Azure yığınına kullanıcı olarak bağlanma**

Bir kullanıcının Geliştirme Seti barındırmak için Uzak Masaüstü erişimi yoksa, Azure yığın erişebilmeniz için önce bir geliştirme seti ortamında, bunlar bir sanal özel ağ (VPN) bağlantısı yapılandırmanız gerekir. Bkz: [Azure yığınına bağlanmak](azure-stack-connect-azure-stack.md). 

Kullanıcılarınızın bilmek isteyeceksiniz nasıl [Kullanıcı Portalı erişim ](user/azure-stack-use-portal.md) veya PowerShell aracılığıyla bağlanma. Tümleşik sistemleri ortamında dağıtım başına kullanıcı portalı adresi değişir. Kullanıcılarınızın doğru URL sağlamak gerekir.

PowerShell kullanarak, kullanıcılar Hizmetleri kullanabilmeniz için önce kaynak sağlayıcıları kaydetmek zorunda kalabilirsiniz. (Bir hizmet bir kaynak sağlayıcısı yönetir. For example, ağ kaynak sağlayıcısı sanal ağlar, ağ arabirimleri ve yük Dengeleyiciler gibi kaynakları yönetir.) Bunlar gerekir [yüklemek](user/azure-stack-powershell-install.md) PowerShell [karşıdan](user/azure-stack-powershell-download.md) ek modüller ve [yapılandırma](user/azure-stack-powershell-configure-user.md) (kaynak Sağlayıcısı kaydı içerir) PowerShell.

**Bir teklife abone olma**

Bir kullanıcı Hizmetleri erişmeden önce bunlar gerekir [teklife abone](azure-stack-subscribe-plan-provision-vm.md) bir operatör olarak oluşturduğunuz.

## <a name="where-to-get-support"></a>Destek almak nereye

### <a name="integrated-systems"></a>Tümleşik sistemler

Tümleşik bir sistem için Eşgüdümlü sorun giderme ve çözümleme işlemi Microsoft ve özgün donanım üreticisi (OEM) donanım ortaklarımızın arasındaki yoktur.

Bulut Hizmetleri sorun varsa, destek Microsoft Müşteri Destek Hizmetleri'ne (CSS) aracılığıyla sunulur. Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından, **yeni destek isteği**, burada doğrudan açabilirsiniz bir destek isteği bir site açılır.

Dağıtım ile ilgili bir sorun varsa, düzeltme ve güncelleştirme, (alan değiştirebilen birim dahil) donanım ve donanım markalı yazılımları, donanım yaşam döngüsü konakta çalışan yazılımı gibi ilk OEM donanım satıcınıza başvurun.

Başka bir şey için Microsoft CSS başvurun.

### <a name="development-kit"></a>Geliştirme Seti

Geliştirme Seti için destek ilgili sorular sorabilirsiniz [Microsoft forumları](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından, **yeni destek isteği**, forumlar sitenin doğrudan açılır. Bu forumları düzenli olarak izlenir. Geliştirme Seti bir değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında bölge Yönetimi](azure-stack-region-management.md)


