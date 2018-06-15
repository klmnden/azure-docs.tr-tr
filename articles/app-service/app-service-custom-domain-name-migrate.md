---
title: Etkin bir DNS adı Azure App Service'e geçirme | Microsoft Docs
description: Azure App Service'e herhangi kesinti olmadan dinamik bir siteye zaten atanmış özel bir DNS etki alanı adı geçirmek öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: cd04be2046a23901471cb7bd0da9e0ed2d514d0d
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
ms.locfileid: "27566500"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Etkin bir DNS adı Azure App Service'e geçirme

Bu makalede, etkin bir DNS adı geçirmek nasıl gösterilmektedir [Azure App Service](../app-service/app-service-web-overview.md) herhangi kesinti olmadan.

Uygulama hizmeti için Canlı bir site ve DNS etki alanı adını geçirdiğinizde, o DNS adını dinamik trafik zaten hizmet veriyor. Uygulama hizmeti uygulamanızı erken önlem active DNS adı bağlayarak geçiş sırasında kapalı kalma süresi DNS çözümlemesi ile önleyebilirsiniz.

Kapalı kalma süresi DNS çözümlemesi ile ilgili endişeleniyoruz değil, bkz: [Azure Web uygulamaları için var olan bir özel DNS ad eşleme](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Önkoşullar

Bu yöntem tamamlamak için:

- [Uygulama hizmeti uygulamanızı ücretsiz katmanında olmadığından emin olun](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-the-domain-name-preemptively"></a>Etki alanı adı erken önlem bağlama

Özel bir etki alanı erken önlem bağladığınızda, hem de DNS kayıtlarınızı herhangi bir değişiklik yapmadan önce aşağıdakileri gerçekleştirirsiniz:

- Etki alanı sahipliğini doğrulama
- Uygulamanız için etki alanı adını etkinleştir

Son olarak, özel DNS ad eski siteden App Service uygulaması geçirdiğinizde, DNS çözümlemesi kapalı kalma süresi olacaktır.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Etki alanı doğrulama kaydı oluşturma

Etki alanı sahipliği doğrulamak için bir TXT kaydı ekleyin. TXT kaydı gelen eşler _awverify.&lt; alt etki alanı >_ için  _&lt;uygulamaadı >. azurewebsites.net_. 

Gereksinim duyduğunuz TXT kaydı geçirmek istediğiniz DNS kaydını bağlıdır. Örnekler için aşağıdaki tabloya bakın (`@` genellikle kök etki alanını temsil eder):

| DNS kaydı örneği | TXT ana bilgisayar | TXT değeri |
| - | - | - |
| @ (root) | _awverify_ | _&lt;uygulamaadı >. azurewebsites.net_ |
| www (alt) | _awverify.www_ | _&lt;uygulamaadı >. azurewebsites.net_ |
| \*(joker karakterler) | _awverify.\*_ | _&lt;uygulamaadı >. azurewebsites.net_ |

DNS kayıtları sayfasında, geçirmek istediğiniz DNS adı kayıt türünü unutmayın. Uygulama hizmeti CNAME ve A kayıtlarını eşlemelerini destekler.

### <a name="enable-the-domain-for-your-app"></a>Etki alanı için uygulamanızı etkinleştirme

İçinde [Azure portal](https://portal.azure.com), uygulama sayfanın sol gezinti bölmesinde seçin **özel etki alanları**. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

İçinde **özel etki alanları** sayfasında,  **+**  yanındaki simge **ana bilgisayar adını eklemek**.

![Ana bilgisayar adı ekleyin](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

TXT kaydı gibi eklenen tam etki alanı adı yazın `www.contoso.com`. Bir joker karakter etki alanı için (gibi \*. contoso.com), joker karakter etki alanıyla eşleşen herhangi bir DNS adı kullanabilirsiniz. 

Seçin **doğrulamak**.

**Ana bilgisayar adını eklemek** düğmesi etkinleştirilir. 

Olduğundan emin olun **ana bilgisayar adı kayıt türü** geçiş yapmak istediğiniz DNS kaydı türünü ayarlayın.

Seçin **ana bilgisayar adını eklemek**.

![DNS adı için uygulama ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Uygulamanın içinde yansıtılması yeni ana bilgisayar adı için biraz zaman alabilir **özel etki alanları** sayfası. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

![CNAME kaydı eklendi](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Özel DNS adınızı Azure uygulamanızı şimdi etkinleştirildi. 

## <a name="remap-the-active-dns-name"></a>Etkin DNS adı yeniden eşleme

Yapmak için tek şey, etkin DNS kaydınız App Service'e işaret edecek şekilde yeniden eşleme. Sağ şimdi onu hala eski sitenize işaret eder.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Uygulamanın IP adresini (yalnızca bir kayıt) kopyalayın

Bir CNAME kaydı yeniden eşleme, bu bölümü atlayabilirsiniz. 

Bir A kaydını yeniden eşlemek için gösterilen uygulama hizmeti uygulamanın dış IP adresine gereksinim **özel etki alanları** sayfası.

Kapat **ana bilgisayar adını eklemek** seçerek sayfa **X** sağ üst köşedeki. 

İçinde **özel etki alanları** sayfasında, uygulamanın IP adresini kopyalayın.

![Azure App portalında gezinme](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>DNS kaydını güncelleştirin

Geri DNS kayıtları sayfasında, etki alanı sağlayıcısının yeniden eşlemek için DNS kaydı seçin.

İçin `contoso.com` kök etki alanı örneği, aşağıdaki tabloda yer alan örnekler gibi A veya CNAME kaydı yeniden eşleyin: 

| FQDN örneği | Kayıt türü | Host | Değer |
| - | - | - | - |
| contoso.com (root) | A | `@` | IP adresinden [uygulamanın IP adresini kopyalayın](#info) |
| www.contoso.com (alt) | CNAME | `www` | _&lt;uygulamaadı >. azurewebsites.net_ |
| \*. contoso.com (joker karakterler) | CNAME | _\*_ | _&lt;uygulamaadı >. azurewebsites.net_ |

Ayarlarınızı kaydedin.

DNS sorguları hemen DNS'ye yayma işlemi gerçekleştikten sonra uygulama hizmeti uygulamanızı çözme başlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama hizmeti için özel bir SSL sertifikası bağlama öğrenin.

> [!div class="nextstepaction"]
> [Azure Web uygulamaları için var olan özel bir SSL sertifikası bağlama](app-service-web-tutorial-custom-ssl.md)
