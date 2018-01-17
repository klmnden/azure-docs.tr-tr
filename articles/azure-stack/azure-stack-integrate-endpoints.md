---
title: "Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama"
description: "Azure yığın uç noktaları, veri merkezinizde yayımlama öğrenin"
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 01/16/2018
ms.author: victorh
keywords: 
ms.openlocfilehash: 1cc74cb2214918d6bfd0c0827cf5d9832b84f317
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure yığın altyapı rolleri için çeşitli uç (VIP'ler - sanal IP adresleri) ayarlar. Bu Vıp'lerin ortak IP adresi havuzundan ayrılır. Her VIP yazılım tanımlı ağ katmanı bir erişim denetim listesi (ACL) ile korunmaktadır. ACL, fiziksel anahtarlar (TORs ve BMC) arasında daha fazla çözüm sağlamlaştırmak için de kullanılır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki Mimari diyagramı ACL'ler ve farklı ağ katmanları gösterilmektedir:

![Mimari diyagramı](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktalarını ve protokolleri (gelen)

Dış ağlara yayımlama Azure yığın uç noktalar için gerekli altyapı VIP'ler aşağıdaki tabloda listelenmiştir. Liste her gösterir uç noktası, gerekli bağlantı noktası ve protokol. SQL kaynak sağlayıcısı ve diğerleri gibi ek kaynak sağlayıcıları için gerekli uç noktaları belirli bir kaynak sağlayıcısı dağıtım belgelerinde ele alınmıştır.

İç altyapı yayımlama Azure yığını için gerekli olmadıklarını olduğundan VIP'ler listelenmez.

> [!NOTE]
> Azure yığın işleci tarafından hiçbir denetimiyle kullanıcıların kendileri tarafından tanımlanan kullanıcı VIP'ler dinamik.


|Endpoint (VIP)|DNS ana bilgisayar bir kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (Yönetici)|Adminportal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Kaynak Yöneticisi (Yönetici)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (kullanıcı)|Portal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (kullanıcı)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graf|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Sertifika iptal listesi|CRL.*&lt;bölge >.&lt; FQDN >*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP VE UDP|53|
|Anahtar kasası (kullanıcı)|*.Vault.*  &lt;bölge >.&lt; FQDN > *|TCP|443|
|Anahtar kasası (Yönetici)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|TCP|443|
|Depolama Kuyruğu|&#42;. sıra.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;. Tablo.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama blobu|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL'ler (giden)

Azure yığını yalnızca saydam proxy sunucuları destekler. Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için şu bağlantı noktalarını ve URL'ler giden iletişim için izin vermelidir:


|Amaç|URL|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Market dağıtım|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Düzeltme Eki & Güncelleştir|https://&#42;.azureedge.net|HTTPS|443|
|Kayıt|https://management.azure.com|HTTPS|443|
|Kullanım|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|

## <a name="firewall-publishing"></a>Güvenlik Duvarı yayımlama

Önceki bölümde listelenen bağlantı noktaları için geçerli Azure yığın hizmetlerini var olan bir güvenlik duvarı üzerinden yayımlarken gelen iletişimi.

Güvenli Azure yığın yardımcı olmak için bir güvenlik duvarı aygıtı kullanmanızı öneririz. Ancak, bu kesin bir gereklilik değildir. Güvenlik duvarları ile dağıtılmış hizmet engelleme (DDOS) saldırıları ve içerik denetimi gibi yardımcı olmakla birlikte, aynı zamanda BLOB'lar, tablolar ve Kuyruklar gibi Azure storage Hizmetleri için bir işleme ayak haline gelebilir.

(Azure AD veya AD FS) kimlik modelini temel alan bu olabilir veya AD FS uç noktasına yayımlamak için gerekli değildir. Bağlantısı kesilmiş dağıtım modu kullanılırsa, AD FS uç noktasına yayımlamanız gerekir. (Daha fazla bilgi için veri merkezi tümleştirme kimlik konusuna bakın.)

Azure Resource Manager (Yönetici), Yönetici portalı ve anahtar kasası (Yönetici) uç noktaları dış yayımlama mutlaka gerektirmez. Senaryoya bağlıdır. Örneğin, bir hizmet sağlayıcısı olarak, saldırı yüzeyini sınırlayabilir ve yalnızca Azure yığınından ağınızdaki ve Internet'ten yönetmek isteyebilirsiniz.

Bir kurumsal kuruluş için dış ağ mevcut bir kurumsal ağ olabilir. Böyle bir senaryoda, şirket ağından Azure yığın çalışması için bu uç yayımlamanız gerekir.

## <a name="edge-firewall-scenario"></a>Sınır güvenlik duvarı senaryosu

Bir sınır dağıtımında Azure yığın doğrudan (ISS tarafından sağlanan) sınır yönlendiricisi arkasında olan veya olmayan bir güvenlik duvarı önüne dağıtılır.

![Bir Azure yığın kenar dağıtım mimari diyagramı](media/azure-stack-integrate-endpoints/Integrate-Endpoints-02.png)

Genellikle, genel olarak yönlendirilebilir IP adreslerinin genel VIP havuzu için bir sınır dağıtım dağıtım zamanında belirtilir. Bu senaryo bir kullanıcı Azure gibi genel bir bulut gibi tam Self denetimli bulut deneyimi deneyimi sağlar.

### <a name="using-nat"></a>NAT kullanarak

Ek yükü nedeniyle Önerilmemesine rağmen yayımlama uç noktaları için ağ adresi çevirisi (NAT) kullanabilirsiniz. Kullanıcılar tarafından tam olarak denetlenir uç nokta yayımlama için bu NAT kuralı her kullanıcı bir kullanıcı kullanıyor olabilecek tüm bağlantı noktalarını içeren VIP gerektirir.

Azure VPN tüneli için Azure ile karma bulut senaryosunda NAT kullanarak bir uç nokta ayarlamayı desteklemiyor başka bir konudur.

## <a name="enterpriseintranetperimeter-network-firewall-scenario"></a>İntranet/Enterprise/çevre ağ güvenlik duvarı senaryosu

Bir intranet/enterprise/çevre dağıtımında, genellikle çevre ağındaki (DMZ olarak da bilinir) bir parçası olan ikinci bir güvenlik duvarı dışında Azure yığını dağıtılır.

![Azure yığın güvenlik duvarı senaryosu](media/azure-stack-integrate-endpoints/Integrate-Endpoints-03.png)

Genel olarak yönlendirilebilir IP adreslerinin Azure yığın ortak VIP havuzu için belirtilmişse, bu adresleri mantıksal olarak çevre ağına ait ve birincil güvenlik duvarında yayımlama kuralları gerektirir.

### <a name="using-nat"></a>NAT kullanarak

Genel olarak yönlendirilebilir IP adreslerinin Azure yığın ortak VIP havuzu için kullanılıyorsa, NAT ikincil güvenlik duvarında Azure yığın uç noktaları yayımlamak için kullanılır. Bu senaryoda, yayımlama kuralları birincil güvenlik duvarı kenarı ötesine ve ikincil Güvenlik Duvarı'nı yapılandırmanız gerekir. NAT kullanmak istiyorsanız, aşağıdaki noktaları göz önünde bulundurun:

- NAT, kullanıcıların kendi uç noktaları ve kendi yayımlama kuralları yazılım tanımlı ağ (SDN) yığınında denetlemek için güvenlik duvarı kuralları yönetirken yükü ekler. Kullanıcıların yayımlanan kendi VIP'ler almak ve bağlantı noktası listesini güncelleştirmek için Azure yığın işleci başvurmanız gerekir.
- NAT kullanımı kullanıcı deneyimini sınırlar sırasında tam denetim işlecine yayımlama istekleri üzerinden verir.
- Azure ile karma bulut senaryosu için Azure VPN tüneli için NAT'ı kullanarak bir uç nokta ayarlamayı desteklemiyor göz önünde bulundurun.


## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın datacenter tümleştirmesi - güvenlik](azure-stack-integrate-security.md)