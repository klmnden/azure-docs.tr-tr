---
title: Etkin DNS adını - Azure App Service'e geçirme | Microsoft Docs
description: Azure App Service için kapalı kalma süresi olmadan canlı bir siteye zaten atanmış özel bir DNS etki alanı adı geçirmeyi öğrenin.
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
ms.custom: seodec18
ms.openlocfilehash: 6215230a52bcb5c44f54747b447dc5f64e6af650
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130390"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Etkin DNS adını Azure App Service'e geçirme

Bu makalede, etkin bir DNS adına geçirme işlemini göstermektedir [Azure App Service](../app-service/overview.md) kapalı kalma süresi olmadan.

Canlı siteyi ve onun DNS etki alanı adını App Service'e geçirme, Canlı trafik, DNS adı zaten hizmet veriyor. Etkin DNS adını App Service uygulamanızı sıd'lerde bağlayarak, geçiş sırasında kapalı kalma süresi DNS çözümlemesi önleyebilirsiniz.

DNS çözümlemesi kapalı kalma süresi hakkında endişeleniyoruz değil olup [mevcut bir özel DNS adını Azure App Service'e eşlemek](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır tamamlamak için:

- [App Service uygulamanızı ücretsiz katmanında olmadığından emin olun](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-the-domain-name-preemptively"></a>Etki alanı adı sıd'lerde bağlama

Özel bir etki alanı sıd'lerde bağladığınızda, DNS kayıtlarınızı herhangi bir değişiklik yapmadan önce aşağıdaki her iki gerçekleştirirsiniz:

- Etki alanı sahipliğini doğrulama
- Uygulamanız için etki alanı adını etkinleştirme

Son olarak, özel DNS adını eski sitesinden App Service uygulamasına geçiş yaptığınızda, DNS çözümlemesi kapalı kalma süresi olmadan olacaktır.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Etki alanı doğrulama kaydı

Etki alanı sahipliğini doğrulamak için bir TXT kaydı ekleyin. Gelen TXT kaydını eşler _awverify.&lt; alt etki alanı >_ için  _&lt;uygulamaadı >. azurewebsites.net_. 

Gereksinim duyduğunuz TXT kaydı, geçirmek istediğiniz DNS kaydını bağlıdır. Örnekler için aşağıdaki tabloya bakın (`@` genellikle kök etki alanını temsil eder):

| DNS kaydı örneği | TXT konak | TXT değeri |
| - | - | - |
| \@ (kök) | _awverify_ | _&lt;appname>.azurewebsites.net_ |
| www (sub) | _awverify.www_ | _&lt;appname>.azurewebsites.net_ |
| \* (joker karakter) | _awverify.\*_ | _&lt;appname>.azurewebsites.net_ |

DNS kayıtları sayfası geçirmek istediğiniz DNS adı kayıt türü unutmayın. App Service eşlemeleri CNAME ve A kayıtları destekler.

> [!NOTE]
> CloudFlare gibi belirli sağlayıcıları `awverify.*` geçerli kaydı değildir. Kullanım `*` yalnızca yerine.

> [!NOTE]
> Joker karakter `*` kayıtları alt etki alanlarını bir CNAME'İNİZ'ın kayıtla doğrulama olmaz. Açıkça her alt etki alanı için bir TXT kaydı oluşturmanız gerekebilir.


### <a name="enable-the-domain-for-your-app"></a>Etki alanı için uygulamanızı etkinleştirme

İçinde [Azure portalında](https://portal.azure.com), uygulama sayfasının sol gezinti bölmesinde seçin **özel etki alanları**. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

İçinde **özel etki alanları** sayfasında **+** yanındaki simge **konak adı Ekle**.

![Konak adı ekleme](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Gibi TXT kaydı eklediğiniz tam etki alanı adı yazın `www.contoso.com`. Joker karakter etki alanı (gibi \*. contoso.com), joker karakter etki alanıyla eşleşen herhangi bir DNS adı kullanabilirsiniz. 

**Doğrula**'yı seçin.

**Konak adı ekle** düğmesi etkinleştirilir. 

Emin olun **konak adı kayıt türü** geçirmek istediğiniz DNS kayıt türüne ayarlanır.

**Konak adı ekle**'yi seçin.

![Uygulamaya DNS adı ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Yeni konak adının uygulamanın **Özel etki alanları** sayfasına yansıtılması biraz zaman alabilir. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

![CNAME kaydı eklenir](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Özel DNS adınızı, Azure uygulamanızda şimdi etkinleştirildi. 

## <a name="remap-the-active-dns-name"></a>Etkin DNS adını yeniden eşleme

Yapmak için tek şey sol, etkin DNS kaydınızı App Service'ı işaret etmek için'yeniden eşleniyor. Sağ şimdi bunu hala eski sitenize işaret eder.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Uygulamanın IP adresini (yalnızca bir kayıt) kopyalayın

CNAME kaydını yeniden eşleme işlemi, bu bölümü atlayın. 

Bir A kaydını yeniden eşlemek için gösterilen App Service uygulamanın dış IP adresi gerekir. **özel etki alanları** sayfası.

Kapat **konak adı Ekle** seçerek sayfası **X** sağ üst köşedeki. 

**Özel etki alanları** sayfasında, uygulamanın IP adresini kopyalayın.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>DNS kaydını güncelleştirme

Geri DNS kayıtları sayfasında etki alanı sağlayıcınızın yeniden eşlemek için DNS kaydı seçin.

İçin `contoso.com` kök etki alanı örneğinde, aşağıdaki tabloda örnek gibi A veya CNAME kaydını yeniden eşleyin: 

| FQDN örneği | Kayıt türü | Host | Değer |
| - | - | - | - |
| contoso.com (kök) | A | `@` | [Uygulamanın IP adresini kopyalama](#info) bölümünden IP adresi |
| www\.contoso.com (sub) | CNAME | `www` | _&lt;appname>.azurewebsites.net_ |
| \*. contoso.com (joker karakter) | CNAME | _\*_ | _&lt;appname>.azurewebsites.net_ |

Ayarlarınızı kaydedin.

DNS sorgularının DNS yayma hemen yapıldıktan sonra App Service uygulamanızı çözümleme başlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

App Service için özel bir SSL sertifikası bağlama hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Azure App Service'e mevcut özel bir SSL sertifikasını bağlama](app-service-web-tutorial-custom-ssl.md)
