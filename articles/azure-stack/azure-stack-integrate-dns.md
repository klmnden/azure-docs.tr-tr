---
title: "Azure yığın datacenter tümleştirmesi - DNS"
description: "Azure yığın DNS'yi veri merkeziniz ile DNS tümleştirme öğrenin"
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: 
ms.openlocfilehash: 5bdac2f3e6082f9449800fe2d4b303e2d59ade46
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="azure-stack-datacenter-integration---dns"></a>Azure yığın datacenter tümleştirmesi - DNS
Azure yığın uç noktaları erişebilmeleri için (`portal`, `adminportal`, `management`, `adminmanagement`vb..)  Dış Azure yığınından Azure yığınında kullanmak istediğiniz DNS bölgeleri barındıran DNS sunucularını Azure yığın DNS hizmetleri tümleştirileceğini gerekir.

## <a name="azure-stack-dns-namespace"></a>Azure yığın DNS ad alanı
Azure yığın dağıtırken DNS ile ilgili bazı önemli bilgileri sağlamak için gerekli değildir.


|Alan  |Açıklama  |Örnek|
|---------|---------|---------|
|Bölge|Azure yığın dağıtımınızı coğrafi konumu.|`east`|
|Dış etki alanı adı|Azure yığın dağıtımınız için kullanmak istediğiniz bölge adı.|`cloud.fabrikam.com`|
|İç etki alanı adı|Azure yığınında altyapı hizmetleri için kullanılan iç bölge adı.  Dizin hizmeti ile tümleşik ve özel (ulaşılamıyor gelen Azure yığın dağıtımına dışında).|`azurestack.local`|
|DNS ileticisi|DNS sorguları, DNS bölgeleri ve Azure yığını, şirket intraneti veya ortak Internet dışında barındırılan kayıtları iletmek için kullanılan DNS sunucuları.|`10.57.175.34`<br>`8.8.8.8`|
|Adlandırma önekini (isteğe bağlı)|Adlandırma önekini sağlamak için Azure yığın altyapı rolü örnek makine adları istiyor.  Sağlanmazsa, varsayılan değer `azs`.|`azs`|

Azure yığın dağıtımı ve uç noktaları tam etki alanı adını (FQDN) bölge parametresi ve dış etki alanı adı parametresi birleşimidir. Önceki tabloda örnekleri değerleri kullanarak, bu Azure yığın dağıtım için FQDN şu adı olacaktır:

`east.cloud.fabrikam.com`

Bu nedenle, bu dağıtım için uç noktaları bazı örnekleri aşağıdaki URL'ler gibi görünür:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Bir Azure yığın dağıtım için bu örnek DNS ad alanını kullanmak için aşağıdaki koşullar gereklidir:

- Bölge `fabrikam.com` , bir etki alanı kayıt şirketi, iç kurumsal bir DNS sunucusu veya her ikisi de, ad çözümlemesi gereksinimlerinize bağlı olarak kaydedilir.
- Alt etki alanı `cloud.fabrikam.com` altında bölgesi var `fabrikam.com`.
- Bölgeleri barındıran DNS sunucularını `fabrikam.com` ve `cloud.fabrikam.com` Azure yığın dağıtımdan erişilebilen.

Azure yığın uç noktaları ve dış Azure yığından örnekleri için DNS adlarını çözmek Azure yığını için kullanmak istediğiniz üst bölgeyi barındıran DNS sunucuları ile dış DNS bölgesini barındıran DNS sunucularını tümleştirmeniz gerekir.


## <a name="resolution-and-delegation"></a>Çözümleme ve temsilci seçme

İki tür DNS sunucusu bulunur:

- Yetkili bir DNS sunucusu DNS bölgelerini barındırır. Bu sunucu, yalnızca bu bölgelerdeki kayıtlar için DNS sorgularını yanıtlar.
- Yinelemeli bir DNS sunucusu DNS bölgelerini barındırmaz. Bu sunucu, tüm DNS sorgularını yanıtlamak için yetkili DNS sunucularını çağırarak ihtiyacı olan verileri toplar.

Azure yığın yetkili hem de içerir ve yinelemeli DNS sunucuları. Özyinelemeli sunucuları adlarını her şeyi iç özel hem de dış ortak DNS bölgesine dışında bu Azure yığın dağıtım için çözmek için kullanılır. 

![Azure yığın DNS mimarisi](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Azure yığından dış DNS adları çözme

Azure yığın dışında uç noktaları için DNS adlarını çözümlemek için (örneğin: www.bing.com), Azure yığını için Azure yığın olmayan yetkili DNS isteklerini iletmek için kullanabileceğiniz DNS sunucuları sağlamanız gerekir. Dağıtım için Azure yığın isteklerini iletir DNS sunucuları (DNS ileticisi alanında) dağıtım çalışma sayfasındaki gereklidir. Bu alan en az iki sunucu için hataya dayanıklılık sağlar. Bu değerleri Azure yığın dağıtımı başarısız olur.

### <a name="configure-conditional-dns-forwarding"></a>Koşullu DNS iletmeyi Yapılandır

> [!IMPORTANT]
> Bu yalnızca bir AD FS dağıtımı için geçerlidir.

Ad çözümlemesi ile var olan DNS altyapınızın etkinleştirmek için koşullu iletme yapılandırın.

Koşullu iletici eklemek için ayrıcalıklı uç noktası kullanmanız gerekir.

Bu yordam için Azure yığınında ayrıcalıklı uç noktası ile iletişim kurabilir veri merkezi ağınızı bir bilgisayar kullanın.

1. (Yönetici olarak çalıştır) yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktası IP adresine bağlanın. CloudAdmin kimlik doğrulaması için kimlik bilgilerini kullanın.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Ayrıcalıklı uç noktasına bağlandıktan sonra aşağıdaki PowerShell komutunu çalıştırın. Kullanmak istediğiniz DNS sunucularının IP adresleri ve etki alanı adı ile sağlanan örnek değerleri değiştirin.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Dış Azure yığından Azure yığın DNS adları çözme
Yetkili dış DNS bölge bilgilerini tutmak olanları ve herhangi bir kullanıcı tarafından oluşturulan bölgesini sunucularıdır. Bölge temsilcisi veya dış Azure yığından Azure yığın DNS adlarını çözümlemek için koşullu iletme etkinleştirmek için bu sunucular ile tümleştirin.

## <a name="get-dns-server-external-endpoint-information"></a>DNS sunucusu dış uç nokta bilgileri alma

DNS altyapınızın Azure yığın dağıtımınızı tümleştirmek için aşağıdaki bilgiler gereklidir:

- DNS sunucusu FQDN
- DNS sunucusu IP adresleri

Azure yığın DNS sunucuları için FQDN'leri aşağıdaki biçime sahiptir:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Örnek değerler, FQDN'ler için DNS kullanarak sunucuları şunlardır:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Bu bilgiler ayrıca adlı bir dosya tüm Azure yığın dağıtımlarda sonunda oluşturulur `AzureStackStampDeploymentInfo.json`. Bu dosya bulunan `C:\CloudDeployment\logs` dağıtım sanal makinenin klasör. Azure yığın dağıtımınız için hangi değerlerin kullanılan emin değilseniz, buradan değerleri alabilirsiniz.

Dağıtım sanal makine artık mevcut değil veya erişilemez durumda, ayrıcalıklı uç noktasına bağlanmak ve çalışan değerlerini edinebilirsiniz `Get-AzureStackInfo` PowerShell cmdlet'i. Ayrıcalıklı uç noktası hakkında daha fazla bilgi için (burada makalesi bağlamak Ekle) bakın.

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Azure yığınına koşullu iletme ayarlama

Azure yığın DNS altyapınızın ile tümleştirmek için basit ve en güvenli yolu, üst bölgeyi barındıran sunucudan bölgenin koşullu iletme yapmaktır. Üst bölgeyi barındıran Azure yığın dış DNS ad alanınız için DNS sunucuları üzerinde doğrudan denetim varsa, bu yaklaşım önerilir.

DNS ile koşullu iletme nasıl alışık değilseniz, şu TechNet makalesine bakın: [bir etki alanı adı için bir koşullu ileticisi atamak](https://technet.microsoft.com/library/cc794735), veya DNS çözümünüze belirli belgeleri.

Şirket etki alanı adınızı bir alt etki alanı gibi aramak için dış, Azure yığın DNS bölgesi burada belirttiğiniz senaryolarda, koşullu iletme kullanılamaz. DNS temsilcisi yapılandırılması gerekir.

Örnek:

- Kurumsal DNS etki alanı adı: `contoso.com`
- Azure yığın dış DNS etki alanı adı: `azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>Azure yığınına dış DNS bölgesi için temsilci seçme

DNS adları dışında bir Azure yığın dağıtımına çözülebilir olması, DNS temsilcisini ayarlamanız gerekir.

Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS Yönetim sayfasında NS kayıtlarını düzenleyin ve bölge için NS kayıtlarını Azure yığınında fiyatlarla değiştirin.

Çoğu DNS kaydedicilerin temsilci seçmeyi tamamlamak için iki DNS sunucusu en az sağlamanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)
