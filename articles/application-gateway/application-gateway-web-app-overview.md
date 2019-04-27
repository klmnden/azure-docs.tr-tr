---
title: Azure Application Gateway ile Azure App service gibi çok kiracılı arka uçlar genel bakış
description: Bu sayfada, Application Gateway’in çok kiracılı arka uç desteği için genel bir bakış sunulmuştur.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/18/2019
ms.author: victorh
ms.openlocfilehash: 8434340bb7ed95cc36115c05048b2b67682b5796
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831340"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>App service gibi çok kiracılı arka için Application Gateway desteği sona erer

Çok kiracılı mimari tasarımlarında web sunucuları, birden fazla Web sitesi web sunucusu örneğinde çalışıyor. Ana bilgisayar adları, barındırılan farklı uygulamalar arasında ayırt etmek için kullanılır. Varsayılan olarak, application gateway istemciden gelen HTTP ana bilgisayar üst bilgisini değiştirmez ve üst bilgiyi değiştirilmemiş halde arka uca gönderir. NIC gibi arka uç havuzu üyeleri için bu çalışır, sanal makine ölçek kümeleri, genel IP adresleri, iç IP adreslerini ve FQDN gibi bunlar bir belirli ana bilgisayar üst bilgisini veya SNI uzantısını doğru uç noktaya çözümlemek için kullanmayın. Ancak, Azure App service web apps ve Azure API management gibi çok kiracılı ve özel ana bilgisayar üst bilgisini veya SNI uzantısını doğru uç noktaya çözümlemek için dayanır, çok sayıda hizmet vardır. Genellikle, sırayla DNS adını uygulama ağ geçidi ile ilişkili olan uygulama, DNS adını arka uç hizmeti etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istek ana bilgisayar üstbilgisi arka uç hizmeti ana bilgisayar adı ile aynı değil. Ana bilgisayar üstbilgisi arka uç uygulama ağ geçidi'ndeki istekteki arka uç hizmeti ana bilgisayar adını değiştirilmediği sürece bu nedenle, çok kiracılı arka uçlar istek doğru uç noktaya çözümlemek mümkün değildir. 

Application gateway, arka uç ana bilgisayar adına göre istekteki HTTP ana bilgisayar üst bilgisi geçersiz kılma olanağı tanıyan bir özellik sunar. Bu özellik, Azure App service web apps ve API yönetimi gibi çok kiracılı arka uçlar için destek sağlar. Bu özellik hem v1 ve v2 standart hem de WAF SKU'ları için kullanılabilir. 

![Ana bilgisayar geçersiz kılma](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> ASE Azure uygulama hizmeti, bir çok kiracılı kaynak aksine ayrılmış bir kaynak olduğundan bu Azure App service ortamı (ASE) için geçerli değildir.

## <a name="override-host-header-in-the-request"></a>İstekte konak üst bilgisi geçersiz kıl

Bir ana bilgisayar geçersiz kılma işlemi belirtme olanağı tanımlanan [HTTP ayarları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) ve kural oluşturma sırasında herhangi bir arka uç havuzuna uygulanabilir. Ana bilgisayar üst bilgisini ve SNI uzantısını çok kiracılı arka uçlar için geçersiz kılma aşağıdaki iki yolu desteklenir:

- Ana bilgisayar adı açık bir şekilde girilmemişse HTTP ayarlarında sabit bir değere ayarlama yeteneği. Bu özellik, ana bilgisayar üstbilgisi burada belirli HTTP ayarlarının uygulandığı arka uç havuzu için tüm trafik için şu değer kılınır sağlar. Uçtan uca SSL kullanılırken, geçersiz kılınan bu ana bilgisayar adı SNI uzantısında kullanılır. Bu özellik, burada bir arka uç havuzu grubunun gelen müşteri ana bilgisayar üst bilgisinden farklı bir ana bilgisayar üst bilgisi beklediği senaryolara olanak sağlar.

- IP veya FQDN arka uç havuzu üyelerinin ana bilgisayar adı türetme olanağını. HTTP ayarları ayrıca bir bağımsız arka uç havuzu üyesinden ana bilgisayar adı türetme seçeneğiyle yapılandırdıysanız ana bilgisayar adı FQDN'den bir arka uç havuzu üyesinin dinamik olarak çekmek için bir seçenek sağlar. Uçtan uca SSL kullanılırken, bu ana bilgisayar adı FQDN’den türetilir ve SNI uzantısında kullanılır. Bu özellik, burada bir arka uç havuzunun Azure web apps gibi iki veya daha fazla çok kiracılı PaaS hizmetine sahip olabilir ve her üyesine isteğin ana bilgisayar üstbilgisi, FQDN'den türetilmiş ana bilgisayar adını içeren senaryolara olanak sağlar. Bu senaryoyu uygulamak için bir anahtar olarak adlandırılan HTTP ayarlarında kullandığımız [çekme arka uç adresi ana bilgisayar adını](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-backend-address) , dinamik olarak geçersiz özgün istek arka uç havuzunda belirtilen bir ana bilgisayar üstbilgisi.  Arka uç havuzu FQDN "contoso11.azurewebsites.net" ve "contoso22.azurewebsites.net" içeriyorsa, örneğin, contoso.com olan özgün isteğin ana bilgisayar üst bilgisini contoso11.azurewebsites.net veya contoso22.azurewebsites.net geçersiz kılınır ne zaman isteği uygun arka uç sunucusuna gönderilir. 

  ![web uygulaması senaryosu](./media/application-gateway-web-app-overview/scenario.png)

Bu özellik sayesinde müşteriler, HTTP ayarları ve özel araştırmalardaki seçenekleri uygun yapılandırmaya göre belirtebilir. Bu ayar daha sonra bir dinleyici ve arka uç havuzu için bir kural kullanarak bağlanır.

## <a name="special-considerations"></a>Özel durumlar

### <a name="ssl-termination-and-end-to-end-ssl-with-multi-tenant-services"></a>SSL sonlandırma ve uçtan uca SSL ile çok kiracılı hizmetler

Çok kiracılı hizmetler ile SSL sonlandırma hem de SSL şifrelemesi uçtan uca desteklenir. Application Gateway SSL sonlandırma için SSL sertifikası için uygulama ağ geçidi dinleyici eklenmesi gereken devam eder. Ancak, Azure App service web Apps'e beyaz arka uçları'application Gateway'de gerektirmeyen gibi uçtan uca SSL'durumunda güvenilir Azure Hizmetleri. Bu nedenle, herhangi bir kimlik doğrulama sertifikası eklemenize gerek yoktur. 

![uçtan uca SSL](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

Yukarıdaki resimde dikkat edin, App service arka uç olarak seçildiğinde kimlik doğrulama sertifikaları eklemek için bir gereksinimi yoktur.

### <a name="health-probe"></a>Durum yoklaması

Ana bilgisayar üstbilgisi için geçersiz kılma **HTTP ayarları** yalnızca istek ve akışı etkiler. Sistem durumu araştırma davranışını etkilemez. Uçtan uca işlevselliğin çalışması için hem araştırma hem de HTTP ayarları doğru yapılandırmayı yansıtacak şekilde değiştirilmelidir. Özel araştırmalar araştırma yapılandırmasında bir ana bilgisayar üst bilgisi belirtme olanağı sağlamanın yanı sıra, şu anda yapılandırılmış HTTP ayarlarından ana bilgisayar üstbilgisi türetme olanağını da desteklemektedir. Bu yapılandırma, araştırma yapılandırmasındaki `PickHostNameFromBackendHttpSettings` parametresi kullanılarak belirtilebilir.

### <a name="redirection-to-app-services-url-scenario"></a>App Service'nın URL senaryosu için yeniden yönlendirme

Burada uygulama hizmetinden gelen yanıt olarak ana bilgisayar adı doğrudan son kullanıcı tarayıcıya senaryolar olabilir *. azurewebsites.net ana bilgisayar adı yerine uygulama ağ geçidiyle ilişkilendirilmiş etki alanı. Bu sorun oluşabilir olduğunda:

- App Service'inizde yapılandırıldı yeniden yönlendirme var. Yeniden yönlendirme isteği sonunda eğik çizgi ekleme olarak basit olabilir.
- Sahip olduğunuz bir Azure AD kimlik doğrulaması yeniden yönlendirme neden olur.

Bu gibi durumlarda çözmek için bkz [yeniden yönlendirme için App service'nın URL sorun giderme](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).

## <a name="next-steps"></a>Sonraki adımlar

Azure App service web uygulaması gibi çok kiracılı uygulaması ile application Gateway ziyaret ederek bir arka uç havuzu üyesi olarak ayarlamayı öğrenmenin [Application Gateway ile yapılandırma App Service web uygulamaları](https://docs.microsoft.com/azure/application-gateway/create-web-app)
