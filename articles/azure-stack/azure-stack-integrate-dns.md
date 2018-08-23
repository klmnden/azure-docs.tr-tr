---
title: Azure Stack veri merkezi tümleştirmesi - DNS
description: Azure Stack DNS, DNS veri merkezi ile tümleştirmeyi öğrenin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: b4935dc95ccf525c0a40b10dcc8c59ec8aba710e
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42059750"
---
# <a name="azure-stack-datacenter-integration---dns"></a>Azure Stack veri merkezi tümleştirmesi - DNS
Azure Stack uç noktaları erişebilmesi için (`portal`, `adminportal`, `management`, `adminmanagement`vb..)  Dış Azure yığını, Azure Stack'te kullanmak istediğiniz DNS bölgeleri barındıran bir DNS sunucuları ile Azure Stack DNS hizmetleri tümleştirmeniz gerekir.

## <a name="azure-stack-dns-namespace"></a>Azure Stack DNS ad alanı
Azure Stack dağıtırken DNS ile ilgili bazı önemli bilgileri sağlamak için gerekli değildir.


|Alan  |Açıklama  |Örnek|
|---------|---------|---------|
|Bölge|Azure Stack dağıtımınıza coğrafi konumu.|`east`|
|Dış etki alanı adı|Azure Stack dağıtımınız için kullanmak istediğiniz bölgenin adı.|`cloud.fabrikam.com`|
|İç etki alanı adı|Azure stack'teki altyapı hizmetleri için kullanılan iç bölgesinin adı.  Dizin hizmeti ile tümleşik ve özel (ulaşılamıyor gelen Azure Stack dağıtımı dışında).|`azurestack.local`|
|DNS ileticisi|DNS sorguları, DNS bölgelerini ve kayıtlarını Azure Stack, Kurumsal intranet veya genel internet'dışında barındırılan iletmek için kullanılan DNS sunucuları.|`10.57.175.34`<br>`8.8.8.8`|
|Adlandırma ön eki (isteğe bağlı)|İçin Azure Stack altyapısını rol örneği makine adları istediğiniz adlandırma önek.  Sağlanmazsa, varsayılan değer `azs`.|`azs`|

Azure Stack dağıtımı ve uç noktalarına tam etki alanı adı (FQDN) bölge parametresi ve dış etki alanı adı parametresi birleşimidir. Örnek değerler, önceki tabloda kullanarak, bu Azure Stack dağıtımı için FQDN şu adı olacaktır:

`east.cloud.fabrikam.com`

Bu nedenle, bu dağıtım için uç noktalar bazılarının örnekleri aşağıdaki URL'ler gibi görünür:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Bu örnek DNS ad alanı için bir Azure Stack dağıtımı kullanmak için aşağıdaki koşullar gereklidir:

- Bölge `fabrikam.com` bir etki alanı kayıt şirketi, dahili Kurumsal DNS sunucusuna veya her ikisiyle de, ad çözümlemesi gereksinimleri bağlı olarak kaydedilir.
- Alt etki alanı `cloud.fabrikam.com` bölgenin altında mevcut `fabrikam.com`.
- Bölgeleri barındıran bir DNS sunucuları `fabrikam.com` ve `cloud.fabrikam.com` Azure Stack dağıtımdan ulaşılabilir.

Azure Stack uç noktaları ve örneklerin dışında Azure Stack için DNS adlarını çözümlemek DNS sunucularını Azure Stack için kullanmak istediğiniz üst bölgeyi barındıran bir DNS sunucularıyla dış DNS bölgesini barındıran tümleştirme gerekir.


## <a name="resolution-and-delegation"></a>Çözümleme ve temsilci seçme

İki tür DNS sunucusu bulunur:

- Yetkili bir DNS sunucusu DNS bölgelerini barındırır. Bu sunucu, yalnızca bu bölgelerdeki kayıtlar için DNS sorgularını yanıtlar.
- Özyinelemeli bir DNS sunucusu DNS bölgelerini barındırmıyor. Bu sunucu, tüm DNS sorgularını yanıtlamak için yetkili DNS sunucularını çağırarak ihtiyacı olan verileri toplar.

Azure Stack yetkili hem de içeren ve yinelenen DNS sunucuları. Yinelenen sunucuları adları her şeyin özel bölge iç ve dış Genel DNS bölgelerinin hariç olmak üzere, Azure Stack dağıtımı için çözmek için kullanılır. 

![Azure Stack DNS mimarisi](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Azure Stack dış DNS adları çözme

Azure Stack dış uç noktaları için DNS adlarını çözümlemek için (örneğin: www.bing.com), Azure Stack, Azure Stack değil yetkili DNS istekleri iletmek için kullandığı DNS sunucularını sağlamanız gerekir. Dağıtım için Azure Stack isteklerini iletir DNS sunucuları (DNS ileticisi alanında) dağıtım çalışma gerekir. Bu alan en az iki sunucu için hataya dayanıklılık sağlar. Bu değerler Azure Stack dağıtım başarısız olur.

### <a name="configure-conditional-dns-forwarding"></a>Koşullu DNS iletme'yi yapılandırma

> [!IMPORTANT]
> Bu, yalnızca bir AD FS dağıtımı için geçerlidir.

Ad çözümlemesi, mevcut bir DNS altyapısıyla etkinleştirmek için koşullu iletme yapılandırın.

Koşullu ileticisi eklemek için ayrıcalıklı uç nokta kullanmanız gerekir.

Bu yordam için Azure Stack'te ayrıcalıklı uç noktası ile iletişim kurabilen veri merkezi ağınızı bir bilgisayar kullanın.

1. (Yönetici olarak çalıştır) yükseltilmiş Windows PowerShell oturumu açın ve ayrıcalıklı uç noktanın IP adresine bağlanın. CloudAdmin kimlik doğrulaması için kimlik bilgilerini kullanın.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Ayrıcalıklı uç noktasına bağlandıktan sonra aşağıdaki PowerShell komutunu çalıştırın. Kullanmak istediğiniz DNS sunucularının IP adresleri ve etki alanı adı ile sağlanan örnek değerleri değiştirin.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Dış Azure Stack Azure Stack DNS adları çözme
Yetkili dış DNS bölge bilgileri tutmak olanları ve herhangi bir kullanıcı tarafından oluşturulmuş bölge sunucularıdır. Bölge temsilcisi ya da dış Azure Stack Azure Stack DNS adları çözümlemek için koşullu iletme etkinleştirmek için bu sunucular ile tümleştirin.

## <a name="get-dns-server-external-endpoint-information"></a>DNS sunucusu dış uç nokta bilgileri Al

Azure Stack dağıtımınıza DNS altyapınızın ile tümleştirmek için aşağıdaki bilgiler gereklidir:

- DNS sunucusu FQDN
- DNS sunucusu IP adresleri

Azure Stack DNS sunucuları için FQDN'lerin aşağıdaki biçime sahiptir:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Örnek değerleri, DNS için FQDN'leri kullanarak sunucuları şunlardır:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Bu bilgiler ayrıca sonunda, tüm Azure Stack dağıtımlarda adlı bir dosya oluşturulur `AzureStackStampDeploymentInfo.json`. Bu dosya bulunan `C:\CloudDeployment\logs` dağıtım sanal makinenin klasör. Azure Stack dağıtımınız için değerleri kullanılan emin değilseniz, buradan değerleri alabilirsiniz.

Dağıtım sanal makine artık kullanılamaz veya erişilemez durumda, ayrıcalıklı uç noktaya bağlanmak ve çalıştırarak değerlerini edinebilirsiniz `Get-AzureStackInfo` PowerShell cmdlet'i. Daha fazla bilgi için [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md).

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Azure Stack koşullu iletme ayarlama

Azure Stack, DNS altyapısıyla tümleştirme basit ve en güvenli yolu, üst bölgeyi barındıran sunucudan bölgenin koşullu iletme yapmaktır. Üst bölgeyi barındıran Azure Stack dış DNS ad alanınız için DNS sunucuları üzerinde doğrudan denetim varsa, bu yaklaşım önerilir.

DNS ile koşullu iletme yapma hakkında bilgi sahibi değilseniz, şu TechNet makalesine bakın: [koşullu ileticisi atamak için bir etki alanı adı](https://technet.microsoft.com/library/cc794735), veya DNS çözümünüze özgü belgelere.

Şirket etki alanı adınızı bir alt etki alanı gibi aramak için harici, Azure Stack DNS bölgesi burada belirttiğiniz senaryolarda, koşullu iletme kullanılamaz. DNS temsilcisi yapılandırılması gerekir.

Örnek:

- Kurumsal DNS etki alanı adı: `contoso.com`
- Azure Stack dış DNS etki alanı adı: `azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>Azure Stack için dış DNS bölgesini temsilci olarak görevlendirme

Bir Azure Stack dağıtımı dışında çözülebilir olması DNS adları için DNS temsilci seçmeyi ayarlama gerekir.

Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS Yönetim sayfasında NS kayıtlarını düzenleyin ve bölge için NS kayıtlarını Azure Stack dışındaki değiştirin.

Çoğu DNS kaydedicilerin temsilci seçmeyi tamamlamak için iki DNS sunucuları en az sağlamanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)
