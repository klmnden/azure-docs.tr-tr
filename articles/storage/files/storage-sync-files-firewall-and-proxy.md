---
title: Azure dosya eşitleme şirket içi güvenlik duvarı ve proxy ayarları | Microsoft Docs
description: Azure dosya eşitleme şirket ağ yapılandırması
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 11/26/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d9b7296a116ebd06542a53087afbd083dbd3a7eb
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766870"
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

**Uygulamaya özel proxy ayarlarını** Azure dosya eşitleme trafiği için özel bir ara sunucu yapılandırmasına izin verin. Uygulamaya özel ara sunucu ayarlarını, Aracı sürüm 4.0.1.0 ya da daha yeni ve aracı yükleme sırasında veya Set-StorageSyncProxyConfiguration PowerShell cmdlet'i kullanılarak yapılandırılabilir.

Uygulamaya özel proxy ayarlarını yapılandırmak için PowerShell komutları:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
**Makine genelindeki proxy ayarlarının** sunucusunun tüm trafiğin proxy üzerinden yönlendirilmesini olarak Azure dosya eşitleme aracısı için saydamdır.

Makine genelinde proxy ayarlarını yapılandırmak için aşağıdaki adımları izleyin: 

1. .NET uygulamaları için proxy ayarlarını yapılandırma 

   - Bu iki dosyayı düzenleyin:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - Machine.config dosyaları (aşağıda < system.serviceModel > bölümünde) < system.net > bölümüne ekleyin.  IP adresi ve bağlantı noktası proxy sunucusu için 127.0.01:8888 değiştirin. 
     ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
     ```

2. WinHTTP proxy ayarları ayarlayın 

   - Bir yükseltilmiş komut istemi veya var olan proxy ayarı görmek için PowerShell'de aşağıdaki komutu çalıştırın:   

     Netsh winhttp show proxy

   - Bir yükseltilmiş komut istemi veya PowerShell proxy ayarını ayarlamak için aşağıdaki komutu çalıştırın (127.0.01:8888 IP adresi ve bağlantı noktası proxy sunucusu için değiştirin):  

     Netsh winhttp proxy 127.0.0.1:8888 ayarlayın

3. Bir yükseltilmiş komut istemi veya PowerShell aşağıdaki komutu çalıştırarak depolama eşitleme Aracısı hizmetini yeniden başlatın: 

      net stop filesyncsvc

      Not: Otomatik başlatma depolama Eşitleme Aracı (filesyncsvc) hizmeti durdurulduğunda.

## <a name="firewall"></a>Güvenlik duvarı
Bir önceki bölümde belirtildiği gibi bağlantı noktası 443 gereksinimlerini olmasını giden açın. Veri Merkezi, dal veya bölgenizde ilkelerine bağlı olarak, daha fazla trafik Bu bağlantı noktası üzerinden belirli etki alanlarına erişimi kısıtlama istenen gerekli veya olabilir.

Aşağıdaki tabloda iletişim için gereken etki alanları açıklanmaktadır:

| Hizmet | Genel bulut uç noktası | Azure kamu uç noktası | Kullanım |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | https://management.usgovcloudapi.net | İlk sunucu kayıt çağrısı dahil olmak üzere bu URL için/aracılığıyla (PowerShell gibi) herhangi bir kullanıcının çağrısına gider. |
| **Azure Active Directory** | https://login.windows.net | https://login.microsoftonline.us | Azure Resource Manager çağrıları kimliği doğrulanmış bir kullanıcı tarafından yapılması gerekir. Başarılı olması için bu URL'yi, kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.windows.net/ | https://graph.windows.net/ | Azure dosya eşitleme dağıtımı bir parçası olarak, aboneliğin Azure Active Directory'de Hizmet sorumlusu oluşturulur. Bu URL için kullanılır. Bu asıl hakları Azure dosya eşitleme hizmeti için en az bir dizi için temsilci seçme için kullanılır. Azure dosya eşitleme'nin ilk kurulum gerçekleştiren kullanıcı kimliği doğrulanmış bir kullanıcı abonelik sahibi ayrıcalıklara sahip olması gerekir. |
| **Azure Depolama** | &ast;. core.windows.net | &ast;.core.usgovcloudapi.net | Sunucu bir dosya yüklediğinde, ardından sunucu, veri taşıma daha verimli bir şekilde doğrudan depolama hesabındaki Azure dosya paylaşımına konuşurken gerçekleştirir. Sunucuda yalnızca için hedeflenen dosya paylaşımına erişim veren bir SAS anahtarı var. |
| **Azure dosya eşitleme** | &ast;.one.microsoft.com | &ast;.afs.azure.us | İlk sunucu kayıt sonrasında sunucu bu bölgede Azure dosya eşitleme hizmeti örneği için bölgesel bir URL alır. Sunucu URL'sini doğrudan ve verimli bir şekilde eşitlendiğini işleme örneğiyle iletişim kurmak için kullanabilirsiniz. |
| **Microsoft PKI** | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | Azure dosya eşitleme Aracısı yüklendikten sonra PKI URL'si Azure dosya paylaşımı ve Azure dosya eşitleme hizmeti ile iletişim kurmak için gereken Ara sertifikaları yüklemek için kullanılır. OCSP URL'si bir sertifika durumunu denetlemek için kullanılır. |

> [!Important]
> Trafiğe izin verirken &ast;. one.microsoft.com, daha fazlasını eşitleme hizmeti trafiğini sunucudan mümkün. Alt etki alanları altında kullanılabilen pek çok daha fazla Microsoft hizmetleri vardır.

Varsa &ast;. one.microsoft.com çok geniş, Azure dosya eşitleme hizmeti yalnızca dolayımsız bölgesel örneklerini iletişimi vererek sunucu iletişimi sınırlayabilirsiniz. Depolama eşitleme hizmetini dağıttıktan ve sunucuya kayıtlı bölgesindeki seçmek için hangi örneklerdeki bağlıdır. Bu bölge, aşağıdaki tabloda "birincil uç nokta URL'si" adı verilir.

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) nedenleriyle, bir genel olarak yedekli (GRS) depolama hesabı, Azure dosya paylaşımlarını belirtmiş olabilirsiniz. Bu durumda, ardından Azure dosya paylaşımlarınızın üzerinden eşleştirilmiş bölgede kalıcı bölgesel bir kesinti durumunda başarısız olur. Azure dosya eşitleme aynı bölge çiftlerini depolama alanı olarak kullanır. Bu nedenle GRS depolama hesapları kullanıyorsanız, sunucunuzun eşleştirilmiş bölgede Azure dosya eşitleme için iletişim kurmasına izin vermek ek URL'ler etkinleştirmek gerekir. Aşağıdaki tabloda, bu "çiftli bölge" çağırır. Ayrıca, de etkinleştirilmesi gerekir bir traffic manager profil URL'si yok. Bu, ağ trafiği sorunsuz bir şekilde eşleştirilmiş bölge için bir yük devretme durumunda yeniden yönlendirilebilir ve aşağıdaki tabloda "Bulma URL'si" olarak adlandırılan garanti eder.

| Bulut  | Bölge | Birincil uç nokta URL'si | Eşleştirilmiş bölge | Bulma URL'si |
|--------|--------|----------------------|---------------|---------------|
| Genel |Avustralya Doğu | https://kailani-aue.one.microsoft.com | Avustralya Güneydoğu | https://kailani-aue.one.microsoft.com |
| Genel |Avustralya Güneydoğu | https://kailani-aus.one.microsoft.com | Avustralya Doğu | https://tm-kailani-aus.one.microsoft.com |
| Genel | Orta Kanada | https://kailani-cac.one.microsoft.com | Doğu Kanada | https://tm-kailani-cac.one.microsoft.com |
| Genel | Doğu Kanada | https://kailani-cae.one.microsoft.com | Orta Kanada | https://tm-kailani.cae.one.microsoft.com |
| Genel | Orta ABD | https://kailani-cus.one.microsoft.com | Doğu ABD 2 | https://tm-kailani-cus.one.microsoft.com |
| Genel | Doğu Asya | https://kailani11.one.microsoft.com | Güneydoğu Asya | https://tm-kailani11.one.microsoft.com |
| Genel | Doğu ABD | https://kailani1.one.microsoft.com | Batı ABD | https://tm-kailani1.one.microsoft.com |
| Genel | Doğu ABD 2 | https://kailani-ess.one.microsoft.com | Orta ABD | https://tm-kailani-ess.one.microsoft.com |
| Genel | Kuzey Avrupa | https://kailani7.one.microsoft.com | Batı Avrupa | https://tm-kailani7.one.microsoft.com |
| Genel | Güneydoğu Asya | https://kailani10.one.microsoft.com | Doğu Asya | https://tm-kailani10.one.microsoft.com |
| Genel | Birleşik Krallık Güney | https://kailani-uks.one.microsoft.com | Birleşik Krallık Batı | https://tm-kailani-uks.one.microsoft.com |
| Genel | Birleşik Krallık Batı | https://kailani-ukw.one.microsoft.com | Birleşik Krallık Güney | https://tm-kailani-ukw.one.microsoft.com |
| Genel | Batı Avrupa | https://kailani6.one.microsoft.com | Kuzey Avrupa | https://tm-kailani6.one.microsoft.com |
| Genel | Batı ABD | https://kailani.one.microsoft.com | Doğu ABD | https://tm-kailani.one.microsoft.com |
| Devlet | ABD Devleti Arizona | https://usgovarizona01.afs.azure.us | ABD Devleti Texas | https://tm-usgovarizona01.afs.azure.us |
| Devlet | ABD Devleti Texas | https://usgovtexas01.afs.azure.us | ABD Devleti Arizona | https://tm-usgovtexas01.afs.azure.us |

- Yerel olarak yedekli (LRS) veya bölge olarak yedekli (ZRS) depolama hesapları kullanıyorsanız, yalnızca "birincil uç nokta URL'si altında" listelenen URL'sini etkinleştirmek gerekir.

- Genel olarak yedekli (GRS) depolama hesapları kullanıyorsanız, üç URL etkinleştirin.

**Örnek:** Depolama eşitleme hizmetinde dağıttığınız `"West US"` ve sunucunuz ile kaydedin. Bu durumda iletişim kurmak sunucu izni URL'ler şunlardır:

> - https://kailani.one.microsoft.com (birincil uç noktası: Batı ABD)
> - https://kailani1.one.microsoft.com (yük devretme eşleştirilmiş bölge: Doğu ABD)
> - https://tm-kailani.one.microsoft.com (birincil bölge bulma URL'si)

## <a name="summary-and-risk-limitation"></a>Summary ve risk sınırlama
Bu belgedeki listeleri, Azure dosya eşitleme şu anda iletişim kuran URL'leri içerir. Güvenlik duvarları bu etki alanlarına giden trafiğe izin verecek şekilde kurabilmesi gerekir. Microsoft, bu liste güncelleştirildi tutmak çalışır.

Güvenlik duvarı kuralları kısıtlama etki alanının ayarlanmasında güvenliğini artırmak için bir ölçü olabilir. Bu güvenlik duvarı yapılandırmaları kullandıysanız, bir URL eklenir ve hatta zaman içinde değişebileceğini aklınızda bulundurun gerekir. Bu makalede düzenli aralıklarla denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
