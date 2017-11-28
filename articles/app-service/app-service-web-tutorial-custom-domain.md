---
title: "Azure Web uygulamaları için var olan bir özel DNS ad eşleme | Microsoft Docs"
description: "Bir web uygulaması, mobil uygulama arka ucu veya Azure App Service'teki API uygulamasına varolan özel DNS etki alanı adı (gösterim etki alanında) eklemeyi öğrenin."
keywords: "uygulama hizmeti, azure uygulama hizmeti, etki alanı eşleme, etki alanı adı, varolan etki alanı, ana bilgisayar adı"
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1a0b54e75bd6356ba7ba351d51d5f4a59bd64c75
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Harita Azure Web uygulamaları için varolan bir özel DNS adı

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, Azure Web uygulamaları için var olan bir özel DNS adını eşleştirmek nasıl gösterir.

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir alt etki alanı eşleme (örneğin, `www.contoso.com`) bir CNAME kaydı kullanılarak
> * Kök etki alanını eşlemek (örneğin, `contoso.com`) kullanarak bir A kaydı
> * Joker karakter etki alanını eşlemek (örneğin, `*.contoso.com`) bir CNAME kaydı kullanılarak
> * Etki alanı eşlemesi komut dosyaları ile otomatikleştirme

Ya da kullanabileceğiniz bir **CNAME kaydı** veya bir **bir kayıt** App Service'e bir özel DNS adını eşleştirmek için. 

> [!NOTE]
> Bir kök etki alanı dışındaki tüm özel DNS adları için CNAME kullanmanızı öneririz (örneğin, `contoso.com`).

Canlı site ve DNS etki alanı adını App Service'e geçirmek için bkz [etkin bir DNS adı Azure App Service'e geçirme](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Bir App Service uygulaması oluşturma](/azure/app-service/), veya başka bir öğretici için oluşturduğunuz bir uygulama kullanın.
* Bir etki alanı adı satın alın ve etki alanı sağlayıcınızın (örneğin, GoDaddy) DNS kayıt defterine erişim olduğundan emin olun.

  Örneğin, DNS girişlerini eklemek için `contoso.com` ve `www.contoso.com`, DNS ayarlarını yapılandırın olmalıdır `contoso.com` kök etki alanı.

  > [!NOTE]
  > Ad, göz önünde bulundurun var olan bir etki alanı yoksa [Azure portalını kullanarak bir etki alanı satın alma](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>Uygulamayı hazırlayın

Özel bir DNS adı bir web uygulamanızın için web uygulaması eşlemek için [uygulama hizmeti planı](https://azure.microsoft.com/pricing/details/app-service/) Ücretli katmanı olmalıdır (**paylaşılan**, **temel**, **standart**, veya  **Premium**). Bu adımda, App Service uygulaması desteklenen fiyatlandırma katmanı olan olduğundan emin olun.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Açık [Azure portal](https://portal.azure.com) ve Azure hesabınızla oturum açın.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Azure portalında uygulama gidin

Sol menüden seçin **uygulama hizmetleri**ve ardından uygulama adını seçin.

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/select-app.png)

Uygulama Hizmeti uygulamasının yönetim sayfasına bakın.  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanı denetleyin

Uygulama sayfanın sol gezinti bölmesinde kaydırın **ayarları** bölümünde ve seçin **(uygulama hizmeti planı) ölçeklendirme**.

![Büyütme menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlığı ile vurgulanır. Uygulama içinde olmadığından emin olmak için kontrol edin **serbest** katmanı. İçinde özel DNS desteklenmiyor **serbest** katmanı. 

![Fiyatlandırma katmanı denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Uygulama hizmeti planı değilse **serbest**, Kapat **fiyatlandırma katmanınızı seçin** sayfasında ve geçin [bir CNAME kaydı eşleme](#cname).

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a>Uygulama hizmeti planı ölçeklendirme

Boş olmayan katmanları birini seçin (**paylaşılan**, **temel**, **standart**, veya **Premium**). 

**Seç**'e tıklayın.

![Fiyatlandırma katmanı denetleyin](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Aşağıdaki bildirim görürseniz, bir ölçeklendirme işlemi tamamlanır.

![Ölçek işlemi onayı](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Harita bir CNAME kaydı

Eğitmen örnekte için bir CNAME kaydı ekleyin `www` alt etki alanı (örneğin, `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>CNAME kaydı oluşturun.

Uygulamanın varsayılan ana bilgisayar adı için bir alt etki alanı eşlemek için bir CNAME kaydı ekleyin (`<app_name>.azurewebsites.net`, burada `<app_name>` , uygulamanızın adıdır).

İçin `www.contoso.com` etki alanı örneği adı eşleyen bir CNAME kaydı ekleyin `www` için `<app_name>.azurewebsites.net`.

CNAME ekledikten sonra DNS kayıtları sayfasında aşağıdaki gibi görünür:

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>CNAME kaydı eşleme Azure içinde etkinleştir

Azure Portalı'nda uygulama sayfası sol gezinti bölmesinde seçin **özel etki alanları**. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

İçinde **özel etki alanları** sayfası uygulama, özel tam DNS adı Ekle (`www.contoso.com`) listesi.

Seçin  **+**  yanındaki simge **ana bilgisayar adını eklemek**.

![Ana bilgisayar adı ekleyin](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

İçin bir CNAME kaydı gibi eklenen tam etki alanı adı yazın `www.contoso.com`. 

Seçin **doğrulamak**.

**Ana bilgisayar adını eklemek** düğmesi etkinleştirilir. 

Olduğundan emin olun **ana bilgisayar adı kayıt türü** ayarlanır **CNAME (www.example.com veya herhangi bir alt etki alanı)**.

Seçin **ana bilgisayar adını eklemek**.

![DNS adı için uygulama ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Uygulamanın içinde yansıtılması yeni ana bilgisayar adı için biraz zaman alabilir **özel etki alanları** sayfası. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

![CNAME kaydı eklendi](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Bir adımı eksik veya herhangi bir yerde yazmış daha önce sayfanın sonundaki bir doğrulama hatası görürsünüz.

![Doğrulama hatası](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Bir A kaydı eşleme

Eğitmen örnekte kök etki alanı için bir A kaydı ekleyin (örneğin, `contoso.com`). 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Uygulamanın IP adresini kopyalayın

Bir A kaydı eşlemek için uygulamanın dış IP adresi gerekir. Bu IP adresi uygulamanın içinde bulabilirsiniz **özel etki alanları** Azure portalında sayfası.

Azure Portalı'nda uygulama sayfası sol gezinti bölmesinde seçin **özel etki alanları**. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

İçinde **özel etki alanları** sayfasında, uygulamanın IP adresini kopyalayın.

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a>A kaydını oluşturun

Bir uygulama için bir A kaydı eşlemek için uygulama hizmeti gerektirir **iki** DNS kayıtları:

- Bir **A** uygulamanın IP adresine eşlemek için kayıt.
- A **TXT** uygulamanın varsayılan hostname eşlemek için kayıt `<app_name>.azurewebsites.net`. Uygulama hizmeti bu kaydı yalnızca yapılandırması sırasında özel etki alanı sahibi olduğunu doğrulamak için kullanır. Özel etki alanınız doğrulandı ve App Service'te yapılandırdıktan sonra bu TXT kaydı silebilirsiniz. 

İçin `contoso.com` etki alanı örneği aşağıdaki tabloya göre A ve TXT kayıtlarını oluşturun (`@` genellikle kök etki alanını temsil eder). 

| Kayıt türü | Host | Değer |
| - | - | - |
| A | `@` | IP adresinden [uygulamanın IP adresini kopyalayın](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Kayıtları eklendiğinde, DNS kayıtları sayfasında aşağıdaki gibi görünür:

![DNS kayıtları sayfasında](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>Uygulama A kaydı eşleştirmeyi etkinleştir

Uygulamanın edilene **özel etki alanları** sayfasında Azure Portalı'nda, özel tam DNS adı ekleyin (örneğin, `contoso.com`) listesi.

Seçin  **+**  yanındaki simge **ana bilgisayar adını eklemek**.

![Ana bilgisayar adı ekleyin](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

A kaydı için aşağıdaki gibi yapılandırılmış tam etki alanı adı yazın `contoso.com`.

Seçin **doğrulamak**.

**Ana bilgisayar adını eklemek** düğmesi etkinleştirilir. 

Olduğundan emin olun **ana bilgisayar adı kayıt türü** ayarlanır **bir kayıt (example.com)**.

Seçin **ana bilgisayar adını eklemek**.

![DNS adı için uygulama ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Uygulamanın içinde yansıtılması yeni ana bilgisayar adı için biraz zaman alabilir **özel etki alanları** sayfası. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

![Bir kayıt eklendi](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Bir adımı eksik veya herhangi bir yerde yazmış daha önce sayfanın sonundaki bir doğrulama hatası görürsünüz.

![Doğrulama hatası](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Bir joker karakter etki alanına Eşle

Eğitmen örnekte, eşleme bir [joker DNS adına](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (örneğin, `*.contoso.com`) için bir CNAME kaydı ekleyerek App Service uygulaması. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>CNAME kaydı oluşturun.

Uygulamanın varsayılan ana bilgisayar adı için bir joker karakter ad eşlemek için bir CNAME kaydı ekleyin (`<app_name>.azurewebsites.net`).

İçin `*.contoso.com` etki alanı örneği CNAME kaydı adı eşler `*` için `<app_name>.azurewebsites.net`.

CNAME eklendiğinde, DNS kayıtları sayfasında aşağıdaki gibi görünür:

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>Uygulamasında CNAME kaydı eşleştirmeyi etkinleştir

Uygulama için joker karakter adıyla eşleşen tüm alt etki alanı artık ekleyebilirsiniz (örneğin, `sub1.contoso.com` ve `sub2.contoso.com` eşleşen `*.contoso.com`). 

Azure Portalı'nda uygulama sayfası sol gezinti bölmesinde seçin **özel etki alanları**. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Seçin  **+**  yanındaki simge **ana bilgisayar adını eklemek**.

![Ana bilgisayar adı ekleyin](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Joker karakter etki alanıyla eşleşen bir tam etki alanı adı yazın (örneğin, `sub1.contoso.com`) ve ardından **doğrulama**.

**Ana bilgisayar adını eklemek** düğmesi etkinleştirilir. 

Olduğundan emin olun **ana bilgisayar adı kayıt türü** ayarlanır **CNAME kaydı (www.example.com veya herhangi bir alt etki alanı)**.

Seçin **ana bilgisayar adını eklemek**.

![DNS adı için uygulama ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Uygulamanın içinde yansıtılması yeni ana bilgisayar adı için biraz zaman alabilir **özel etki alanları** sayfası. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

Seçin  **+**  yeniden joker karakter etki alanıyla eşleşen başka bir ana bilgisayar adını eklemek için simge. Örneğin, ekleyin `sub2.contoso.com`.

![CNAME kaydı eklendi](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Tarayıcıda test

Daha önce yapılandırılmış DNS adları göz atın (örneğin, `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, ve `sub2.contoso.com`).

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="resolve-404-error-web-site-not-found"></a>404 hatası "bulunamadı Web Site" çözümlenemiyor

Özel etki alanınızı URL'sine göz atarken bir HTTP 404 (bulunamadı) hata alırsanız, etki alanınız için IP adresini kullanarak uygulamanızın çözümler doğrulayın <a href="https://www.whatsmydns.net/" target="_blank">WhatsmyDNS.net</a>. Aksi durumda, aşağıdaki nedenlerden biri olabilir:

- Yapılandırılmış özel etki alanı, bir A kaydı ve/veya bir CNAME kaydı eksik.
- Tarayıcı istemci eski IP adresi, etki alanınızın önbelleğe. Önbellek ve test DNS çözümlemesi yeniden temizleyin. Bir Windows makinesinde önbellek ile temizleyin `ipconfig /flushdns`.

<a name="virtualdir"></a>

## <a name="direct-default-url-to-a-custom-directory"></a>Özel bir dizin için doğrudan varsayılan URL

Varsayılan olarak, uygulama hizmeti, uygulama kodunuzun kök dizininin web isteği yönlendirir. Ancak, bazı web çerçeveleri kök dizininde başlatmayın. Örneğin, [Laravel](https://laravel.com/) başlayacağını `public` alt dizin. Devam etmek için `contoso.com` DNS örnek, bu tür bir uygulama olacaktır, erişilebilir `http://contoso.com/public`, ancak doğrudan istediğinizden emin `http://contoso.com` için `public` dizin yerine. Bu adım, DNS çözümlemesi, ancak sanal dizin özelleştirme kullanılmaz.

Bunu yapmak için seçin **uygulama ayarları** , web uygulama sayfasının sol gezinti içinde. 

Sayfasında, kök sanal dizini altındaki `/` işaret `site\wwwroot` varsayılan olarak, uygulama kodunuzun kök dizini olan. İşaret edecek şekilde değiştirmeniz `site\wwwroot\public` bunun yerine, örneğin ve değişikliklerinizi kaydedin. 

![Sanal dizin özelleştirme](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

İşlemi tamamlandıktan sonra uygulamayı sağ sayfa (örneğin, http://contoso.com) kök yolundaki döndürmelidir.

## <a name="automate-with-scripts"></a>Komut dosyalarıyla otomatikleştirme

Komut dosyaları ile özel etki alanlarını yönetimini kullanarak otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure CLI 

Aşağıdaki komutu bir App Service uygulaması için yapılandırılmış bir özel DNS adı ekler. 

```bash 
az webapp config hostname add \
    --webapp-name <app_name> \
    --resource-group <resource_group_name> \ 
    --hostname <fully_qualified_domain_name> 
``` 

Daha fazla bilgi için bkz: [bir web uygulaması için özel bir etki alanı eşleme](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

Aşağıdaki komutu bir App Service uygulaması için yapılandırılmış bir özel DNS adı ekler. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Daha fazla bilgi için bkz: [bir web uygulaması için özel bir etki alanı Ata](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir alt etki alanı CNAME kaydı kullanılarak eşleme
> * Bir A kaydı kullanarak bir kök etki alanına Eşle
> * Bir CNAME kaydı kullanılarak bir joker karakter etki alanına Eşle
> * Etki alanı eşlemesi komut dosyaları ile otomatikleştirme

Bir web uygulaması için özel bir SSL sertifikası bağlama konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure Web uygulamaları için var olan özel bir SSL sertifikası bağlama](app-service-web-tutorial-custom-ssl.md)
