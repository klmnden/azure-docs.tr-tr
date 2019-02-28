---
title: Azure Application Gateway ile App Service – App Service'nın URL yeniden yönlendirme sorunlarını giderme
description: Bu makale, Azure Application Gateway, Azure App Service ile kullanıldığında, yeniden yönlendirme sorunu gidermeye ilişkin bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: 7a645574a75a040c3b0218714363cf85e0384e68
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959842"
---
# <a name="troubleshoot-application-gateway-with-app-service--redirection-to-app-services-url"></a>App Service ile Application Gateway’de sorun giderme: App Service’in URL’sine yeniden yönlendirme

 Application Gateway, App Service'nın URL Burada sunulan ile yeniden yönlendirme sorunları tanılamak ve gidermek öğrenin.

## <a name="overview"></a>Genel Bakış

App Service Application Gateway, arka uç havuzundaki'e yönelik bir genel yapılandırma ve uygulama kodunuzda yapılandırılmış bir yeniden yönlendirme varsa, uygulama ağ geçidi eriştiğinizde, görebilirsiniz, uygulamayı doğrudan tarayıcı tarafından yönlendirilir Hizmet URL'si.

Bu sorun, aşağıdaki ana nedenlerden ötürü oluşabilir:

- App Service'inizde yapılandırıldı yeniden yönlendirme var. Yeniden yönlendirme isteği sonunda eğik çizgi ekleme olarak basit olabilir.
- Sahip olduğunuz bir Azure AD kimlik doğrulaması yeniden yönlendirme neden olur.
- Application Gateway HTTP ayarlarında "Arka uç adresi alanından konak adı seçin" anahtar etkinleştirdiniz.
- Kayıtlı uygulama hizmetiniz ile özel etki alanınız yok.

## <a name="sample-configuration"></a>Örnek yapılandırma

- HTTP dinleyicisi: Temel veya çok siteli
- Arka uç adres havuzu: App Service
- HTTP ayarları: "Ana bilgisayar adı arka uç adresi etkin seçin"
- Araştırması: "Ana bilgisayar adını HTTP ayarlarından etkin seçin"

## <a name="cause"></a>Nedeni

Bir App Service yalnızca özel etki alanı ayarlarını, yapılandırılmış konak adı ile varsayılan olarak erişilebilir, "example.azurewebsites.net" ve Application Gateway ile App Service veya ile kaydedilmemiş bir ana bilgisayar adı kullanarak uygulama hizmetinize erişmek istiyorsanız Uygulama ağ geçidinin FQDN'ye sahip ana bilgisayar özgün istek App Service'nın ana bilgisayar adı için geçersiz kılınacak.

Application Gateway ile bunun için "Arka uç Kimden ana bilgisayar adı adresi Seç" anahtar HTTP ayarlarında ve araştırma için çalışmaya kullanıyoruz, araştırma yapılandırmasında "Arka uç HTTP ayarları gelen konak adı seçin" kullanırız.

![appservice-1](.\media\troubleshoot-app-service-redirection-app-service-url\appservice-1.png)

App Service bir yeniden yönlendirme yaptığında bu nedenle, konum üst bilgisi "example.azurewebsites.net" ana bilgisayar adı yerine özgün ana bilgisayar adı aksi şekilde yapılandırılmadıkça kullanır. Örnek istek ve yanıt üst bilgileri aşağıdaki kontrol edebilirsiniz.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
Yukarıdaki örnekte, yanıt üst bilgisi durum kodu 301 yeniden yönlendirme ve location üst bilgisini App Service'nın ana bilgisayar adı yerine özgün ana bilgisayar adı "www.contoso.com" içerdiğini fark.

## <a name="solution"></a>Çözüm

App Service'e bir ana bilgisayar geçersiz kılma yapmak yerine de biz Application Gateway alan aynı ana bilgisayar üstbilgisi geçmelidir mümkün değilse, uygulama tarafında, ancak bir yeniden yönlendirme kalmayarak Bu sorun çözülebilir.

Bunu sonra App Service yönlendirme (varsa) uygulama ağ geçidine işaret aynı özgün ana bilgisayar üst yapar ve kendi.

Bunu başarmak için özel bir etki alanına sahip ve Aşağıda sözü edilen süreci izleyin.

- App Service özel etki alanı listesi etki alanına kaydedin. App Service'nın FQDN'sine işaret eden özel etki alanınızda bunun için bir CNAME olmalıdır. Daha fazla bilgi için [mevcut bir özel DNS adını Azure App Service'e eşlemek](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](.\media\troubleshoot-app-service-redirection-app-service-url\appservice-2.png)

- Bu yapıldıktan sonra App Service ana bilgisayar adı "www.contoso.com" kabul etmeye hazırdır. Şimdi uygulama ağ geçidinin FQDN'sine işaret etmek için bir DNS CNAME girişiniz değiştirin. Örneğin, "appgw.eastus.cloudapp.azure.com".

- Bir DNS sorgusu yaptığınızda etki alanınız "www.contoso.com" için uygulama ağ geçidinin FQDN çözümler emin olun.

- "Arka uç HTTP ayarları gelen konak adı seçin" devre dışı bırakmak için özel bir araştırma ayarlayın. Bu, portaldan araştırma ayarlarında onay işaretini kaldırarak yapılabilir ve PowerShell kullanarak değil - PickHostNameFromBackendHttpSettings kümesi AzApplicationGatewayProbeConfig komutta geçiş yapabilirsiniz. Uygulama ağ geçidinden gönderilen araştırma isteklerine bu ana bilgisayar üst bilgisinde taşıyacağı şekilde araştırma ana bilgisayar adı alanına, App Service'nın FQDN'si "example.azurewebsites.net" girin.

  > [!NOTE]
  > Sonraki adım yaparken, lütfen bu noktada etkin "Arka uç Kimden ana bilgisayar adı adresi Seç" anahtarı, HTTP ayarları hala sahip olduğundan, özel bir araştırma, arka uç HTTP ayarları için ilişkili olmadığından emin olun.

- "Arka uç Kimden ana bilgisayar adı adresi Seç" devre dışı bırakmak için uygulama ağ geçidinizin HTTP ayarları ayarlayın. Portaldan bu yapılabilir onay kutusunun işaretini kaldırarak ve PowerShell kullanarak değil kümesi AzApplicationGatewayBackendHttpSettings komutta - PickHostNameFromBackendAddress geçin.

- Özel araştırma arka uç HTTP ayarları için yeniden ilişkilendirin ve arka uç sistem durumu sağlıklı olup olmadığını doğrulayın.

- Bunu yaptıktan sonra artık aynı ana bilgisayar adı "www.contoso.com" uygulama ağ geçidi için App Service İleri ve aynı ana bilgisayar adına yeniden yönlendirme gerçekleşir. Örnek istek ve yanıt üst bilgileri aşağıdaki kontrol edebilirsiniz.
```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: [www.contoso.com](http://www.contoso.com)

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
```
## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).
