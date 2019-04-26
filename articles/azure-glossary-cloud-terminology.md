---
title: Azure sözlüğünü - Azure sözlüğünü | Microsoft Docs
description: Azure platformunda bulut terimleri için Azure sözlüğünü kullanın. Bu kısa Azure sözlüğünü tanımları için Azure genel bulut koşulları sağlar.
keywords: Azure sözlüğü, bulut terimleri, Azure sözlüğünü, terim tanımları, bulut koşulları
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
ms.openlocfilehash: 9a93786759941def4cf8677509b1b2565cac5090
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325434"
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure sözlüğünü: Azure platformunda bulut terimleri sözlüğü

Microsoft Azure sözlüğünü kısa bir Azure platformu bulut terimleri Sözlüğü ' dir. Ayrıca bkz:

* [Microsoft Azure ve Amazon Web Hizmetleri](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -tanımları Azure hizmetlerini ve AWS karşılıkları.<!-- I propose to link to https://azure.microsoft.com/services/ instead of this -->
* [Bulut bilgi işlem terimleri](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -genel sektör bulut terimleri.

## <a name="account"></a>account
Erişim ve bir Azure aboneliğini yönetmek için kullanılan bir hesap. Aşağıdakilerden herhangi birinin bir hesap olmasına karşın, genellikle bir Azure hesabı adlandırılır: var olan bir iş, okul veya kişisel Microsoft hesabı veya bir Office 365 kullanıcı adı ve parola. İçin kaydolduğunuzda, bir Azure aboneliğini yönetmek için bir hesap oluşturabilirsiniz [ücretsiz deneme sürümü](https://azure.microsoft.com).  
Bkz: [Office 365 hesabınızı kullanarak bir Azure aboneliği için kaydolun](billing/billing-use-existing-office-365-account-azure-subscription.md) ve [oturum açmak için kullanabileceğiniz hesaplar](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API uygulaması
Başka bir ad [App Service uygulaması](#app-service-app).

## <a name="app-service-app"></a>App Service uygulaması
İşlem kaynakları, [Azure App Service](app-service/overview.md) bir Web sitesi veya web uygulamasını web API barındırmak için veya [mobil uygulama arka ucu](app-service-mobile/app-service-mobile-value-prop.md). App Service uygulamalarını da denir *uygulama hizmetleri*, *web uygulamaları*, *API apps*, ve *mobil uygulamalar*.

## <a name="availability-set"></a>kullanılabilirlik kümesi
Uygulama yedeklilik ve güvenilirliği sağlamak için birlikte yönetilen sanal makineler koleksiyonudur. Bir kullanılabilirlik kümesi kullanımı ya da planlı veya Plansız bakım olayı sırasında en az bir sanal makine kullanılabilir olmasını sağlar.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Azure Klasik dağıtım modeli
İki birini [dağıtım modelleri](resource-manager-deployment-model.md) (Azure Resource Manager yeni modeli olduğundan) Azure kaynakları dağıtmak için kullanılır. Bazı Azure Hizmetleri yalnızca Resource Manager dağıtım modelini destekleyen, bazı yalnızca klasik dağıtım modelini destekler ve bazı her ikisini de destekler. Her bir Azure hizmeti için belgelere destekledikleri hangi modellere belirtir.

## <a name="cli"></a>Azure komut satırı arabirimi (CLI)
Windows, macOS ve Linux'tan Azure hizmetlerini yönetmek için kullanılan bir komut satırı arabirimi.  Bazı hizmetler ve hizmet özellikleri yalnızca PowerShell veya CLI aracılığıyla yönetilebilir. Bkz: [Azure CLI](/cli/azure)

## <a name="powershell"></a>Azure PowerShell
Windows bilgisayarlarından Azure hizmetlerini bir komut satırı üzerinden yönetmek için bir komut satırı arabirimi. Bazı hizmetler ve hizmet özellikleri yalnızca PowerShell veya CLI aracılığıyla yönetilebilir.
Bkz: [nasıl Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview)

## <a name="arm-model"></a>Azure Resource Manager dağıtım modeli
İki birini [dağıtım modelleri](resource-manager-deployment-model.md) (diğeri ise Klasik dağıtım modeli) Microsoft Azure kaynakları dağıtmak için kullanılır. Bazı Azure Hizmetleri yalnızca Resource Manager dağıtım modelini destekleyen, bazı yalnızca klasik dağıtım modelini destekler ve bazı her ikisini de destekler. Her bir Azure hizmeti için belgelere destekledikleri hangi modellere belirtir.

## <a name="fault-domain"></a>Hata etki alanı
Aynı anda büyük olasılıkla başarısız olabilir bir kullanılabilirlik kümesindeki sanal makinelerin koleksiyonu. Bir örnek, ortak bir güç kaynağı ve ağ anahtarını paylaşan bir raf üstü makinelerinizde grubudur. Azure'da sanal makineler bir kullanılabilirlik kümesindeki birden çok hata etki alanlarına otomatik olarak ayrılır.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>Coğrafi
Genellikle iki veya daha fazla bölge içeren veri yerleşikliği için tanımlı bir sınırı. Sınırları içinde veya Ulusal sınırların ötesinde olabilir ve vergi düzenlemeleriyle etkilenir. Her coğrafi en az bir bölge vardır. Asya Pasifik ve Japonya coğrafyalar örnekleridir. Olarak da adlandırılan *Coğrafya*.  
Bkz: [Azure bölgeleri](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>Coğrafi çoğaltma
Bloblar, tablolar ve Kuyruklar bölgesel çift içinde gibi içerik otomatik olarak çoğaltma işlemi.  
Bkz: [Azure SQL veritabanı için etkin coğrafi çoğaltma](sql-database/sql-database-geo-replication-overview.md)
<!-- The meaning of "geo" in this term seems to be different than the meaning provided in the "geo" entry -->

## <a name="image"></a>image
Herhangi bir sayıda sanal makineler oluşturmak için kullanılan uygulama yapılandırması ve işletim sistemi içeren bir dosya. Azure'da görüntülerinin iki tür vardır: Sanal makine görüntüsünü ve işletim sistemi görüntüsü. Bir VM görüntüsü, işletim sistemi ve görüntü oluşturulduğunda bir sanal makineye bağlı tüm diskleri içerir. Yalnızca genelleştirilmiş işletim sistemi hiçbir veri disk yapılandırmaları olan bir işletim sistemi görüntüsü içerir.  
Bkz: [seçin PowerShell veya CLI ile azure'da Windows sanal makine görüntülerine erişin ve](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>sınırları
Oluşturulabilecek kaynakları ya da elde edilebilir performans Kıyaslama sayısı. Sınırlar genellikle abonelikler, hizmetler ve teklifleri ile ilişkilidir.  
Bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>yük dengeleyici
Gelen trafiği bir ağdaki bilgisayarlar arasında dağıtan bir kaynaktır. Azure'da bir yük dengeleyici sanal makineleri bir yük dengeleyici kümesinde tanımlanan trafiği dağıtır. A [yük dengeleyici](load-balancer/load-balancer-overview.md) internet'e yönelik olabilir ya da iç olabilir.  

## <a name="mobile-app"></a>mobil uygulama
Başka bir ad [App Service uygulaması](#app-service-app).

## <a name="offer"></a>Teklif
Fiyatlandırma, krediler ve bir Azure aboneliği için geçerli olan ilgili koşulları.  
Bkz: [Azure Teklif Ayrıntıları sayfası](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
Dağıtma ve Azure hizmetlerini yönetmek için kullanılan güvenli web portalı.

## <a name="region"></a>bölge
Çapraz Ulusal Kenarlıklar ve bir veya daha fazla veri içeren bir coğrafi içindeki alan. Fiyatlandırma, bölgesel hizmetler ve teklif türleri bölge düzeyinde kullanıma sunulur. Bir bölge, genellikle birkaç yüz mil kadar uzaklıkta olabilecek başka bir bölgeyle eşleştirilir. Bölgesel çiftler, olağanüstü durum kurtarma ve yüksek kullanılabilirlik senaryoları için bir mekanizma kullanılabilir. Olarak da adlandırılan *konumu*.  
Bkz: [Azure bölgeleri](best-practices-availability-paired-regions.md)

## <a name="resource"></a>kaynak
Azure çözümünüzü parçası olan bir öğe. Her bir Azure hizmeti veritabanları veya sanal makineler gibi kaynakların farklı türlerini dağıtmanıza olanak sağlar.   
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>kaynak grubu
Kaynak bir uygulama için ilgili kaynakları tutan Yöneticisi'nde bir kapsayıcı. Kaynak grubunun tüm kaynaklar için bir uygulama veya mantıksal olarak birlikte gruplandırılmış kaynaklar dahil edebilirsiniz. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınızı belirleyebilirsiniz.  
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager şablonu
Bir veya daha fazla Azure kaynakları bildirimli olarak tanımlayan ve dağıtılan kaynaklar arasındaki bağımlılıkları tanımlayan bir JSON dosyası. Şablon, kaynakları tutarlı ve sürekli olarak dağıtmak için kullanılabilir.  
Bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>Kaynak sağlayıcısı
Kaynakları sağlayan bir hizmet dağıtma ve Resource Manager ile yönetin. Her kaynak sağlayıcısı dağıtılan kaynaklarla çalışmaya yönelik işlemler sunar. Kaynak sağlayıcıları, Azure portalı, Azure PowerShell ve çeşitli programlama SDK'ları erişilebilir.  
Bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>rol
Kullanıcılar, gruplar ve hizmetlere atanabilen erişimi denetlemek için bir anlamına gelir. Rolleri gibi oluşturmak, yönetmek ve Azure kaynaklarında okuma eylemleri gerçekleştiremezsiniz.  
Bkz: [RBAC: Yerleşik roller](role-based-access-control/built-in-roles.md)

## <a name="sla"></a>Hizmet düzeyi sözleşmesi (SLA)
Çalışma süresi ve bağlantı için Microsoft'un taahhütleri açıklar sözleşme. Her bir Azure hizmeti, belirli bir SLA yoktur.  
Bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>paylaşılan erişim imzası (SAS)
Hesap anahtarınız açığa çıkarmadan bir kaynağa sınırlı erişim vermenizi sağlayan bir imza. Örneğin, [Azure depolama kullanan SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) blobları gibi nesneleri istemci erişim vermek için. [IOT Hub kullanan SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) cihazları telemetri göndermesine izin vermek için.

## <a name="storage-account"></a>depolama hesabı
Azure Storage Azure Blob, kuyruk, tablo ve dosya hizmetleri sunan bir hesap erişim. Depolama hesabı adı, Azure Storage veri nesneleriniz için benzersiz ad alanı tanımlar.  
Bkz: [Azure depolama hesapları hakkında](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>aboneliği
Bir müşteri sözleşme ile Microsoft'un Azure Hizmetleri elde etmelerini sağlayan. Abonelik fiyatlandırması ve ilgili koşulların, seçilen abonelik için teklif tarafından yönetilir.
Bkz: [Microsoft çevrimiçi Abonelik Sözleşmesi](https://azure.microsoft.com/support/legal/subscription-agreement/) ve [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>etiket
Bir dizin oluşturma terim yönetme ve fatura gereksinimlerinize göre kaynakları kategorilere ayırmanızı sağlar. Karmaşık bir kaynak koleksiyonu olduğunda, bu varlıkları en anlamlı bir şekilde görselleştirmeniz etiketleri kullanabilirsiniz. Örneğin, kuruluşunuzda benzer görevleri üstlenen veya aynı departmana ait olan kaynakları etiketleyebilirsiniz.  
Bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md)

## <a name="update-domain"></a>Güncelleme etki alanı
Sanal makineler bir kullanılabilirlik kümesinde aynı anda güncelleştirilir koleksiyonu. Sanal makineler aynı güncelleştirme etki alanında, planlanan bakım sırasında birlikte yeniden başlatılır. Azure, aynı anda birden çok güncelleştirme etki alanı hiçbir zaman yeniden başlatılır. Bir yükseltme etki alanı da bilinir.  
Bkz: [Windows sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerin kullanılabilirliğini yönetme](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>Sanal makine
Bir işletim sistemini çalıştıran bir fiziksel bilgisayar yazılım uygulamasıdır. Birden çok sanal makine aynı donanımda aynı anda çalıştırabilirsiniz. Azure'da sanal makineler bir çeşitli boyutlarda kullanılabilir.  
Bkz: [sanal makineler belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>sanal makine uzantısı
Bir kaynak, davranış veya diğer programları iş ya da etkileşime olanağı ile çalışan bir bilgisayara sağlamak ya da yardımcı olan özellikler uygular. Örneğin, VM erişimi uzantısı sıfırlama veya bir Azure sanal makinesinde uzaktan erişim değerleri değiştirmek için kullanabilirsiniz.
<!-- This definition seems obscure to me; maybe a list of examples would work better than a conceptual definition? -->
Bkz: [sanal makine uzantıları ve özellikleri (Windows) hakkında](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [ilgili sanal makine uzantıları ve özellikleri (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>Sanal ağ
Bir ağ, diğer tüm Azure kiracılardan yalıtılmış Azure kaynaklarınızı arasında bağlantı sağlar. Bir [Azure VPN ağ geçidi](vpn-gateway/vpn-gateway-about-vpngateways.md) bir sanal ağ ve şirket içi ağ arasında sanal ağlar arasında bağlantı sağlar. Ayrıca, IP adres bloklarını, DNS ayarları, güvenlik ilkeleri ve rota tabloları sanal ağ içinde tam olarak denetleyebilirsiniz.  
Bkz: [sanal ağa genel bakış](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Web uygulaması
Başka bir ad [App Service uygulaması](#app-service-app).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure’ı kullanmaya başlama](https://azure.microsoft.com/get-started/)
* [Bulut Kaynak Merkezi](https://azure.microsoft.com/resources/)  
* [İş uygulamanız için Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Veri merkezinizde Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)

