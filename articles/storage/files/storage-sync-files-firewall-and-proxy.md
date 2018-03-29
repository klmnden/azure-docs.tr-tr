---
title: Azure dosya eşitleme şirket içi güvenlik duvarı ve proxy ayarları | Microsoft Docs
description: Azure dosya eşitleme içi ağ yapılandırması
services: storage
documentationcenter: ''
author: fauhse
manager: klaasl
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 81425c6ac4e463bd4242328206bd43ce78a1105a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Azure dosya eşitleme proxy ve güvenlik duvarı ayarları
Azure dosya eşitleme Azure dosyalara çok siteli eşitleme ve bulut özelliklerini katmanlama etkinleştirilmesi, şirket içi sunucularınızı bağlanır. Bu nedenle, bir şirket içi sunucu internet'e bağlı olmalıdır. Bir BT yöneticisi, Azure bulut hizmetlerine erişmek sunucu için en iyi yolu karar vermeniz gerekir.

Bu makalede belirli gereksinimleri ve başarıyla ve güvenli bir şekilde Azure dosya eşitleme sunucunuza bağlanmak kullanılabilir seçenekler hakkında bilgi sağlar.

## <a name="overview"></a>Genel Bakış
Azure dosya eşitleme, Windows Server, Azure dosya paylaşımı ve diğer Azure hizmetleriyle birkaç eşitleme grubunuzdaki açıklandığı gibi veri eşitlemesine izin arasında orchestration hizmeti olarak görev yapar. Azure dosya düzgün çalışması eşitleme için sunucularınızı aşağıdaki Azure Hizmetleri ile iletişim kurmak için yapılandırmanız gerekir:

- Azure Storage
- Azure Dosya Eşitleme
- Azure Resource Manager
- Kimlik doğrulama hizmetleri

> [!Note]  
> Windows Server'da Azure dosya eşitleme Aracısı bulut giden trafiği bir güvenlik duvarı açısından dikkate alınması gereken yalnızca elde etmeyle sonuçları Hizmetleri için tüm istekleri başlatır. <br /> Hiçbir Azure hizmeti Azure dosya eşitleme aracısı için bir bağlantı başlatır.


## <a name="ports"></a>Bağlantı Noktaları
Azure dosya eşitleme dosya verileri ve meta verileri yalnızca HTTPS üzerinden taşır ve olması açmak için giden bağlantı noktası 443 gerektirir.
Sonuç olarak tüm trafiği şifrelenir.

## <a name="networks-and-special-connections-to-azure"></a>Ağlar ve Azure özel bağlantıları
Azure dosya eşitleme Aracısı ilgili özel kanalları gibi bir gereksinimi yoktur [ExpressRoute](../../expressroute/expressroute-introduction.md), Azure vs.

Azure dosya eşitleme otomatik olarak bant genişliği, gecikme süresi gibi çeşitli ağ özellikleri uyarlama yanı sıra ince ayar yapmak için yönetici denetim sunan Azure içine ulaşma izin herhangi araçlarla çalışır. Tüm özellikleri şu anda kullanılabilir. Aracılığıyla belirli bir davranışı yapılandırmak istiyorsanız,'ın bize bildirin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Ara sunucu
Azure dosya eşitleme, şu anda makine genelinde proxy ayarlarını destekler. Bu proxy ayarını Azure dosya eşitleme Aracısı saydam aynıdır sunucusunun tüm trafik, bu proxy üzerinden yönlendirilir.

Uygulamaya özgü proxy ayarları şu anda geliştirilme aşamasındadır ve Azure dosya eşitleme Aracısı'nın gelecekteki bir sürümde desteklenmez. Bu, özellikle Azure dosya eşitleme trafiği için bir proxy yapılandırılmasına izin verir.

## <a name="firewall"></a>Güvenlik duvarı
Önceki bölümde belirtildiği gibi bağlantı noktası 443 gereksinimlerini olmasını giden açın. Datacenter, şube veya bölgenizde ilkelerine bağlı olarak, daha fazla trafik belirli etki alanları için bu bağlantı noktası üzerinden kısıtlama istenen gerekli veya olabilir.

Aşağıdaki tabloda, iletişim için gereken etki alanları açıklanmaktadır:

| Hizmet | Etki alanı | Kullanım |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Tüm kullanıcı çağrısıyla (PowerShell) ilk sunucu kayıt çağrısı dahil olmak üzere bu URL için/üzerinden gider. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager çağrıları kimliği doğrulanmış bir kullanıcı tarafından yapılması gerekir. Başarılı olması için bu URL kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.windows.net/ | Azure dosya eşitleme dağıtma bir parçası olarak bir hizmet sorumlusu aboneliğinin Azure Active Directory'de oluşturulur. Bu URL için kullanılır. Bu asıl hakları Azure dosya eşitleme hizmeti için en az bir dizi için temsilci seçme için kullanılır. Azure dosya eşitleme ilk kurulumu yapan kullanıcının kimliği doğrulanmış bir kullanıcı abonelik sahibi ayrıcalıklarına sahip olması gerekir. |
| **Azure Depolama** | &ast;.core.windows.net | Sunucunun bir dosya yüklediğinde, ardından sunucu daha fazla bu veri taşıma verimli depolama hesabındaki Azure dosya paylaşımı için doğrudan konuşurken gerçekleştirir. Sunucu yalnızca hedeflenen dosya paylaşımı erişimi veren bir SAS anahtarı sahiptir. |
| **Azure dosya eşitleme** | &ast;.one.microsoft.com | İlk sunucu kayıttan sonra sunucu bu bölgede Azure dosya eşitleme hizmet örneği için bölgesel bir URL alır. Sunucunun URL'sini doğrudan ve verimli bir şekilde kendi eşitleme işleme örneğiyle iletişim kurmak için kullanabilirsiniz. |

> [!Important]
> Trafiğine izin verirken &ast;. one.microsoft.com, daha fazlasını eşitleme hizmeti trafiğini sunucudan mümkün. Alt etki alanları altında pek çok daha fazla Microsoft hizmetleri vardır.

Varsa &ast;. one.microsoft.com çok geniş, yalnızca Azure dosyaları eşitleme hizmetinin açık bölgesel örnekleri sağlayarak sunucu iletişimi sınırlayabilirsiniz. Seçmek için hangi örnekler depolama eşitleme için dağıttıktan ve sunucu kaydedildi hizmeti bölgeyi temel bağlıdır. Bu sunucu için izin vermeniz bölgedir. Yakında yeni iş sürekliliği özellikleri etkinleştirmek için daha fazla URL olacaktır. 

| Bölge | Azure dosya eşitleme bölgesel uç nokta URL'si |
|--------|---------------------------------------|
| Avustralya Doğu | https://kailani-aue.one.microsoft.com |
| Orta Kanada | https://kailani-cac.one.microsoft.com |
| Doğu ABD | https://kailani1.one.microsoft.com |
| Güneydoğu Asya | https://kailani10.one.microsoft.com |
| Birleşik Krallık Güney | https://kailani-uks.one.microsoft.com |
| Batı Avrupa | https://kailani6.one.microsoft.com |
| Batı ABD | https://kailani.one.microsoft.com |

> [!Important]
> Bu ayrıntılı güvenlik duvarı kurallarını tanımlarsanız, bu belgede sık sık kontrol edin ve güvenlik duvarı ayarlarınızı eski ya da eksik URL listelerinde nedeniyle hizmet kesintilerine uğramaması için güvenlik duvarı kurallarını güncelleştirin.

## <a name="summary-and-risk-limitation"></a>Özet ve risk sınırlama
Bu belgedeki daha önceki listeleri kurduğu şu anda Azure dosya eşitleme URL'leri içerir. Güvenlik duvarları onlardan yanıtları yanı sıra bu etki alanları için giden trafiğe izin verecek şekilde olması gerekir. Güncelleştirilmiş bu listesini tutmak Microsoft içindedir.

Güvenlik duvarı kuralları kısıtlama etki alanının ayarlanmasında güvenliğini artırmak için bir ölçü olabilir. Bu güvenlik duvarı yapılandırmaları kullandıysanız, bir URL'leri eklenebilir ve zaman içinde değişmesi gerektiğini göz önünde bulundurmanız gerekir. Bu nedenle bir değişiklik Yönetimi işlemini bir Azure dosya eşitleme Aracısı sürümünden en son Aracısı dağıtımını test üzerinde başka bir parçası olarak bu belgedeki tabloları denetleyin akıllıca ölçüsüdür. Bu şekilde güvenlik duvarınızın en son aracı etki alanları trafiğine izin verecek şekilde yapılandırıldığından emin olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md)
