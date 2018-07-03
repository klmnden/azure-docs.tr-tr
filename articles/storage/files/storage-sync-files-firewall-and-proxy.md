---
title: Azure dosya eşitleme şirket içi güvenlik duvarı ve proxy ayarları | Microsoft Docs
description: Azure dosya eşitleme şirket ağ yapılandırması
services: storage
documentationcenter: ''
author: fauhse
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 7d86082abb6412072af44a6b2d794bcf536fa18d
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342735"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Azure Dosya Eşitleme proxy’si ve güvenli duvarı ayarları
Azure dosya eşitleme, şirket içi sunucularınızı Azure çok siteli eşitleme ve bulut katmanlaması özellikleri etkinleştirme dosyaları'na bağlanır. Bu nedenle, bir şirket içi sunucu internet'e bağlanması gerekir. Bir BT yöneticisi Azure bulut hizmetlerine erişmek sunucu için en iyi yolu karar vermeniz gerekir.

Bu makalede belirli gereksinimleri ve başarıyla ve güvenli bir şekilde Azure dosya eşitleme için sunucunuza bağlanmak kullanılabilir seçenekler hakkında Öngörüler sağlar.

> [!Important]
> Azure dosya eşitleme henüz güvenlik duvarları ve sanal ağlar için bir depolama hesabı desteklemez. 

## <a name="overview"></a>Genel Bakış
Azure dosya eşitleme, Windows Server, Azure dosya paylaşımınızı ve birden fazla Azure hizmetini eşitleme grubunuz içinde anlatıldığı gibi veri eşitlemesine izin arasında bir düzenleme hizmeti işlevi görür. Azure dosya düzgün çalışması eşitleme için sunucularınızı aşağıdaki Azure Hizmetleri ile iletişim kurmak için yapılandırmanız gerekir:

- Azure Storage
- Azure Dosya Eşitleme
- Azure Resource Manager
- Kimlik doğrulama hizmetleri

> [!Note]  
> Azure dosya eşitleme aracısını Windows Server, bulut hizmetlerine giden trafik bir güvenlik duvarı açısından dikkate alınması gereken yalnızca etmeyle sonucunda, tüm istekleri başlatır. <br /> Bir Azure hizmeti, Azure dosya eşitleme aracısının bağlantısı başlatır.


## <a name="ports"></a>Bağlantı Noktaları
Azure dosya eşitleme dosya verileri ve meta verileri yalnızca HTTPS üzerinden geçer ve olması açmak için giden bağlantı noktası 443 gerektirir.
Sonuç olarak tüm trafik de şifrelenir.

## <a name="networks-and-special-connections-to-azure"></a>Ağ ve Azure ile özel bağlantılar
Azure dosya eşitleme aracısının ilgili özel kanallar gibi bir gereksinimi yoktur [ExpressRoute](../../expressroute/expressroute-introduction.md), vb. azure'a.

Azure dosya eşitleme bulunamazsınız kullanılabilir azure'a otomatik olarak uyum sağlamak için bant genişliği, gecikme süresi gibi çeşitli ağ özellikleri hem de ince ayar yapmak için yönetici denetim teklifi erişim sağlayan bir çalışma yürütürüz. Tüm özellikler şu anda kullanılabilir. Belirli bir davranışı yapılandırmak istiyorsanız, bize [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Ara sunucu
Azure dosya eşitleme uygulamaya özgü ve makine genelindeki proxy ayarlarını destekler.

Sunucusunun tüm trafiğin proxy üzerinden yönlendirilmesini gibi makine genelindeki proxy ayarları Azure dosya eşitleme aracısı için saydamdır.

Uygulamaya özgü proxy ayarları Azure dosya eşitleme trafiği için özel bir ara sunucu yapılandırmasını sağlar. Uygulamaya özgü proxy ayarları 3.0.12.0 Aracı sürüm veya üstü ve aracı yükleme sırasında veya Set-StorageSyncProxyConfiguration PowerShell cmdlet'i kullanılarak yapılandırılabilir.

Uygulamaya özel proxy ayarlarını yapılandırmak için PowerShell komutları:
```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```

## <a name="firewall"></a>Güvenlik duvarı
Bir önceki bölümde belirtildiği gibi bağlantı noktası 443 gereksinimlerini olmasını giden açın. Veri Merkezi, dal veya bölgenizde ilkelerine bağlı olarak, daha fazla trafik Bu bağlantı noktası üzerinden belirli etki alanlarına erişimi kısıtlama istenen gerekli veya olabilir.

Aşağıdaki tabloda iletişim için gereken etki alanları açıklanmaktadır:

| Hizmet | Etki alanı | Kullanım |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | İlk sunucu kayıt çağrısı dahil olmak üzere bu URL için/aracılığıyla (PowerShell gibi) herhangi bir kullanıcının çağrısına gider. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager çağrıları kimliği doğrulanmış bir kullanıcı tarafından yapılması gerekir. Başarılı olması için bu URL'yi, kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.windows.net/ | Azure dosya eşitleme dağıtımı bir parçası olarak, aboneliğin Azure Active Directory'de Hizmet sorumlusu oluşturulur. Bu URL için kullanılır. Bu asıl hakları Azure dosya eşitleme hizmeti için en az bir dizi için temsilci seçme için kullanılır. Azure dosya eşitleme'nin ilk kurulum gerçekleştiren kullanıcı kimliği doğrulanmış bir kullanıcı abonelik sahibi ayrıcalıklara sahip olması gerekir. |
| **Azure Depolama** | &ast;. core.windows.net | Sunucu bir dosya yüklediğinde, ardından sunucu, veri taşıma daha verimli bir şekilde doğrudan depolama hesabındaki Azure dosya paylaşımına konuşurken gerçekleştirir. Sunucuda yalnızca için hedeflenen dosya paylaşımına erişim veren bir SAS anahtarı var. |
| **Azure dosya eşitleme** | &ast;.one.microsoft.com | İlk sunucu kayıt sonrasında sunucu bu bölgede Azure dosya eşitleme hizmeti örneği için bölgesel bir URL alır. Sunucu URL'sini doğrudan ve verimli bir şekilde eşitlendiğini işleme örneğiyle iletişim kurmak için kullanabilirsiniz. |

> [!Important]
> Trafiğe izin verirken &ast;. one.microsoft.com, daha fazlasını eşitleme hizmeti trafiğini sunucudan mümkün. Alt etki alanları altında kullanılabilen pek çok daha fazla Microsoft hizmetleri vardır.

Varsa &ast;. one.microsoft.com çok geniş, yalnızca açık bölgesel Azure dosya eşitleme hizmeti örneklerini vererek sunucu iletişimi sınırlayabilirsiniz. Seçmek için hangi örnekleri için dağıttıktan ve sunucunun kayıtlı depolama eşitleme hizmeti bölgesine bağlıdır. Bu sunucu için izin vermeniz gerekir bölgedir. Yakında yeni iş sürekliliği özellikleri etkinleştirmek için daha fazla URL olacaktır. 

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
> Bu ayrıntılı bir güvenlik duvarı kuralları tanımlarsanız, bu belgenin sık sık kontrol edin ve güvenlik duvarı ayarlarınızı güncel olmayan veya eksik URL listelerinde nedeniyle hizmet kesintilerinden kaçınmak için güvenlik duvarı kurallarını güncelleştir.

## <a name="summary-and-risk-limitation"></a>Summary ve risk sınırlama
Bu belgedeki listeleri, Azure dosya eşitleme şu anda iletişim kuran URL'leri içerir. Güvenlik duvarları, bunlardan alınan yanıtları yanı sıra bu etki alanlarına giden trafiğe izin verecek şekilde mümkün olması gerekir. Microsoft, bu liste güncelleştirildi tutmak çalışır.

Güvenlik duvarı kuralları kısıtlama etki alanının ayarlanmasında güvenliğini artırmak için bir ölçü olabilir. Bu güvenlik duvarı yapılandırmaları kullandıysanız, biri URL'leri eklenebilir ve zaman içinde değişmesi gerektiğini aklınızda bulundurun gerekir. Bu nedenle en son aracı dağıtımını test üzerinde başka bir değişiklik Yönetimi işlemini bir Azure dosya eşitleme Aracısı sürümünden bir parçası olarak bu belgedeki tabloları denetleyin akıllıca ölçümüdür. Bu şekilde güvenlik duvarınızı etki alanlarına en son aracıyı izin verecek şekilde yapılandırıldığından emin olun gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md)
