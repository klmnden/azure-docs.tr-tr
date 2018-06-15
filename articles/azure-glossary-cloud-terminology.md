---
title: Azure sözlüğünü - Azure sözlüğünü | Microsoft Docs
description: Azure platformunda bulut terminolojisi anlamak için Azure sözlüğünü kullanın. Bu kısa Azure sözlük tanımları için Azure ortak bulut koşulları sağlar.
keywords: Azure sözlüğünü, bulut terminolojisi, Azure sözlüğünü, terim tanımları, bulut koşulları
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: ''
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: 84766ba4cf9e844184752bc44d2e0a471b97db27
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32159137"
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure sözlüğü: Azure platformunda bulut terimler sözlüğü

Microsoft Azure sözlüğü Azure platformu bulut terminolojisi kısa Sözlüğü ' dir. Ayrıca bkz:

* [Microsoft Azure ve Amazon Web Hizmetleri](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -tanımları, Azure Hizmetleri ve AWS'dekiler.<!-- I propose to link to https://azure.microsoft.com/services/ instead of this -->
* [Bulut bilgi işlem koşullarını](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -genel endüstri bulut koşulları.

## <a name="account"></a>account
Erişim ve bir Azure aboneliğini yönetmek için kullanılan bir hesap. Bir hesap aşağıdakilerden herhangi birini olsa da, genellikle bir Azure hesabı adlandırılır: olan bir iş, okul, veya kişisel Microsoft hesabı veya bir Office 365 kullanıcı adı ve parola. İçin oturum açtığınızda bir Azure aboneliğini yönetmek için bir hesap oluşturabilirsiniz [ücretsiz deneme sürümü](https://azure.microsoft.com).  
Bkz: [Office 365 hesabınıza bir Azure aboneliği için kaydolun](billing/billing-use-existing-office-365-account-azure-subscription.md) ve [oturum açmak için kullanabileceğiniz hesaplar](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API uygulaması
Başka bir ad [App Service uygulaması](#app-service-app).

## <a name="app-service-app"></a>App Service uygulaması
İşlem kaynakları, [Azure App Service](app-service/app-service-web-overview.md) bir Web sitesi veya web uygulamasını web API barındırmak için veya [mobil uygulama arka ucu](app-service-mobile/app-service-mobile-value-prop.md). App Service uygulamalarının de denir *uygulama hizmetleri*, *web uygulamaları*, *API uygulamaları*, ve *mobil uygulamaları*.

## <a name="availability-set"></a>Kullanılabilirlik kümesi
Uygulama artıklık ve güvenilirlik sağlamak üzere birlikte yönetilen sanal makineler koleksiyonu. Bir kullanılabilirlik kümesi kullanımını ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Azure Klasik dağıtım modeli
İki birini [dağıtım modellerini](resource-manager-deployment-model.md) (yeni modeldir Azure Resource Manager) Azure kaynaklarında dağıtmak için kullanılır. Bazı Azure Hizmetleri yalnızca Resource Manager dağıtım modelini destekleyen, bazı yalnızca klasik dağıtım modelini destekler ve bazı her ikisi de destekler. Her Azure hizmeti belgelerine destekledikleri hangi modellere belirtir.

## <a name="cli"></a>Azure komut satırı arabirimi (CLI)
Windows, macOS ve Linux Azure hizmetleri yönetmek için kullanılan bir komut satırı arabirimi.  Bazı hizmet veya hizmet özellikleri yalnızca PowerShell veya CLI yönetilebilir. Bkz: [Azure CLI 2.0](/cli/azure)

## <a name="powershell"></a>Azure PowerShell
Windows bilgisayarlarından komut satırı aracılığıyla Azure hizmetleri yönetmek için bir komut satırı arabirimi. Bazı hizmet veya hizmet özellikleri yalnızca PowerShell veya CLI yönetilebilir.
Bkz: [nasıl Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview)

## <a name="arm-model"></a>Azure Resource Manager dağıtım modeli
İki birini [dağıtım modellerini](resource-manager-deployment-model.md) (diğeri, Klasik dağıtım modeli) Microsoft Azure kaynaklarında dağıtmak için kullanılır. Bazı Azure Hizmetleri yalnızca Resource Manager dağıtım modelini destekleyen, bazı yalnızca klasik dağıtım modelini destekler ve bazı her ikisi de destekler. Her Azure hizmeti belgelerine destekledikleri hangi modellere belirtir.

## <a name="fault-domain"></a>hata etki alanı
Sanal makineler aynı anda büyük olasılıkla başarısız olabilir bir kullanılabilirlik kümesinde koleksiyonu. Bir örnek, bir dolapta ortak bir güç kaynağı ve ağ anahtarı paylaşmak makineler grubudur. Azure üzerinde bir kullanılabilirlik kümesi'nde sanal makinelerin birden çok hata etki alanları arasında otomatik olarak ayrılır.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>coğrafi
Genellikle iki veya daha fazla bölge içeren veri residency için tanımlanmış bir sınır. Sınırlar içinde veya Ulusal sınırların dışında olabilir ve vergi düzenlemesi tarafından etkilenir. Her coğrafi en az bir bölge vardır. Asya Pasifik ve Japonya geos örnekleridir. Olarak da bilinir *Coğrafya*.  
Bkz: [Azure bölgeleri](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>coğrafi çoğaltma
BLOB'lar, tablolar ve Kuyruklar bölgesel çifti içinde gibi içeriği otomatik olarak çoğaltılması işlemidir.  
Bkz: [Azure SQL veritabanı için etkin coğrafi çoğaltma](sql-database/sql-database-geo-replication-overview.md)
<!-- The meaning of "geo" in this term seems to be different than the meaning provided in the "geo" entry -->

## <a name="image"></a>görüntü
İşletim sistemi ve herhangi bir sayıda sanal makineler oluşturmak için kullanılan uygulama yapılandırması içeren bir dosya. Azure'da görüntülerinin iki tür vardır: VM görüntüsü ve işletim sistemi görüntüsü. Bir VM görüntüsü, işletim sistemi ve görüntü oluşturulduğunda, bir sanal makineye bağlı tüm diskleri içerir. Bir işletim sistemi görüntüsü yalnızca bir genelleştirilmiş işletim sistemiyle hiçbir veri disk yapılandırmaları içerir.  
Bkz: [erişin ve PowerShell veya CLI ile azure'da Windows sanal makine görüntülerini seçin](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Sınırları
Oluşturulabilir kaynakları veya elde edilebilir performans Kıyaslama sayısı. Sınırlar genellikle abonelikler, hizmetleri ve teklifleri ile ilişkilendirilir.  
Bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>Yük Dengeleyici
Gelen trafiği bir ağdaki bilgisayarlar arasında dağıtır bir kaynaktır. Azure üzerinde bir yük dengeleyici sanal makinelere yük dengeleyici kümesinde tanımlanan trafiği dağıtır. A [yük dengeleyici](load-balancer/load-balancer-overview.md) iç olabilir veya internet'e yönelik olabilir.  

## <a name="mobile-app"></a>mobil uygulama
Başka bir ad [uygulama hizmeti](#app-service-app).

## <a name="offer"></a>teklif
Fiyatlandırma, kredi ve ilgili koşulların bir Azure aboneliğine uygulanabilir.  
Bkz: [Azure Teklifi Ayrıntıları sayfası](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
Dağıtma ve Azure hizmetleri yönetmek için kullanılan güvenli web portalı.

## <a name="region"></a>bölge
Çapraz Ulusal sınırların vermez ve bir veya daha fazla veri merkezleri içeren bir coğrafi içindeki bir alanı. Fiyatlandırma, bölgesel Hizmetleri ve teklif türleri bölge düzeyinde sunulur. Bir bölge genellikle birkaç yüz mil kadar uzakta olabilir başka bir bölge ile eşleştirilmiş. Bölgesel çiftleri, olağanüstü durum kurtarma ve yüksek kullanılabilirlik senaryoları için bir mekanizma olarak kullanılabilir. Olarak da adlandırılan *konumu*.  
Bkz: [Azure bölgeleri](best-practices-availability-paired-regions.md)

## <a name="resource"></a>kaynak
Azure çözümünüzü parçası olan bir öğe. Her Azure hizmet, farklı türdeki veritabanları ya da sanal makineleri gibi kaynakların dağıtmanıza olanak tanır.   
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>kaynak grubu
Kaynak bir uygulama için ilgili kaynakları tutan Yöneticisi'nde bir kapsayıcı. Kaynak grubunun tüm uygulama için kaynakları veya yalnızca mantıksal olarak birlikte gruplandırılmış kaynaklar dahil olabilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınızı belirleyebilirsiniz.  
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager şablonu
Bir veya daha fazla Azure kaynaklarını bildirimli olarak tanımlayan ve dağıtılan kaynakları arasındaki bağımlılıkların tanımlayan bir JSON dosyası. Şablon, kaynakları tutarlı ve sürekli olarak dağıtmak için kullanılabilir.  
Bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>kaynak sağlayıcıs
Kaynakları sağlayan bir hizmet dağıtmak ve Resource Manager aracılığıyla yönetebilirsiniz. Her kaynak sağlayıcısı dağıtılan kaynaklarla çalışmaya yönelik işlemler sunar. Kaynak sağlayıcıları, Azure portalı, Azure PowerShell ve çeşitli programlama SDK erişilebilir.  
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>rol
Kullanıcılar, gruplar ve hizmetlere atanmış erişimi denetlemek için bir anlamına gelir. Roller gibi oluştururken, yönetmek ve Azure kaynakları okuma Eylemler gerçekleştirebilecek.  
Bkz: [RBAC: yerleşik roller](role-based-access-control/built-in-roles.md)

## <a name="sla"></a>Hizmet düzeyi sözleşmesi (SLA)
Açık kalma süresi ve bağlantı için Microsoft'un taahhüt açıklar anlaşma. Her Azure hizmetin belirli bir SLA'sı vardır.  
Bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>Paylaşılan erişim imzası (SAS)
Hesap anahtarınızı sokmadan sınırlı bir kaynağa erişim vermek sağlar imzası. Örneğin, [Azure depolama kullanan SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) BLOB'ları gibi nesneleri istemci erişim vermek için. [IOT hub'ı kullanan SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) aygıtları telemetri göndermesine izin vermek için.

## <a name="storage-account"></a>depolama hesabı
Size bir hesap erişim Azure Storage Azure Blob, kuyruk, tablo ve dosya hizmetlere. Depolama hesabı adı Azure Storage veri nesneleriniz için benzersiz ad alanını tanımlar.  
Bkz: [Azure storage hesapları hakkında](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>aboneliği
Azure Hizmetleri elde etmek bunları sağlayan Microsoft ile bir müşterinin anlaşma. Abonelik fiyatlandırma ve ilgili koşulların abonelik için seçilen teklif tarafından yönetilir.
Bkz: [Microsoft çevrimiçi Abonelik Sözleşmesi](https://azure.microsoft.com/support/legal/subscription-agreement/) ve [Azure aboneliklerinin Azure Active Directory ile ilişkili](active-directory/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>etiket
Kaynakları yönetme ve fatura, gereksinimlerinize göre sınıflandırmak sağlar dizin oluşturma bir terim. Kaynaklar karmaşık koleksiyonu varsa, bu varlıkları en anlamlı şekilde görselleştirmek için etiketleri kullanabilirsiniz. Örneğin, kuruluşunuzda benzer görevleri üstlenen veya aynı departmana ait olan kaynakları etiketleyebilirsiniz.  
Bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md)

## <a name="update-domain"></a>etki alanı güncelleştirme
Sanal makineleri bir kullanılabilirlik kümesinde aynı anda güncelleştirilmiş koleksiyonu. Sanal makineler aynı güncelleştirme etki alanında planlı bakım sırasında birlikte yeniden başlatılır. Azure birden çok güncelleştirme etki alanı aynı anda yeniden başlatmaz. Bir yükseltme etki alanı da bilinir.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>Sanal makine
Bir işletim sistemi çalıştıran fiziksel bir bilgisayar yazılım uygulamasıdır. Birden çok sanal makineler, aynı donanımda çalıştırabilirsiniz. Azure'da sanal makineler çeşitli boyutlarda kullanılabilir.  
Bkz: [Virtual Machines belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>sanal makine uzantısı
Davranışları veya ya da iş veya çalışan bir bilgisayara ile etkileşim kurmak olanağı sağlamak diğer programları yardımcı olan özellikler uygulayan bir kaynaktır. Örneğin, sıfırlamak veya bir Azure sanal makinesi üzerinde uzaktan erişim değerlerini değiştirmek için VM erişim uzantısı kullanabilirsiniz.
<!-- This definition seems obscure to me; maybe a list of examples would work better than a conceptual definition? -->
Bkz: [sanal makine uzantıları ve özellikleri (Windows) hakkında](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [sanal makine uzantıları ve özellikleri (Linux) hakkında](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>Sanal ağ
Bir ağ, diğer tüm Azure kiracılardan yalıtılmış Azure kaynaklarınızı arasında bağlantı sağlar. Bir [Azure VPN ağ geçidi](vpn-gateway/vpn-gateway-about-vpngateways.md) sanal ağ arasında bağlantılar oluşturmanızı sağlar ve [bir sanal ağ ve bir şirket içi ağ arasındaki](vpn-gateway/vpn-gateway-plan-design.md). IP adres blokları, DNS ayarları, güvenlik ilkeleri ve bir sanal ağ içinde yönlendirme tabloları tam olarak denetleyebilirsiniz.  
Bkz: [Virtual Network'e genel bakış](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Web uygulaması
Başka bir ad [uygulama hizmeti](#app-service-app).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure’ı kullanmaya başlama](https://azure.microsoft.com/get-started/)
* [Bulut kaynağı merkezi](https://azure.microsoft.com/resources/)  
* [İş uygulamanız için Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure, veri merkezinizdeki](https://azure.microsoft.com/overview/business-apps-on-azure/)

