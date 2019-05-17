---
title: Bir uygulamanın yayımcı etki alanı yapılandırma | Azure
description: Bir uygulamanın yayımcı etki alanı bilgilerini nerede gönderilen kullanıcılarınıza yapılandırmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, zachowd
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d47075f9e18b299341a98983ffb8a47389fd7063
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540231"
---
# <a name="how-to-configure-an-applications-publisher-domain-preview"></a>Nasıl yapılır: Bir uygulamanın yayımcı etki alanı (Önizleme) yapılandırma

Bir uygulamanın yayımcı etki alanı üzerinde kullanıcılara görüntülenen [uygulamanın onay istemi](application-consent-experience.md) bilgilerine nereden gönderilen bilmesini sağlamak üzere. Bir yayımcı etki alanı olmayan 21 Mayıs 2019'den sonra kaydedilen çok kiracılı uygulamaları görünmesini olarak **doğrulanmamış**. Çok kiracılı uygulamaları tek bir kuruluş dizini dışına hesaplarını destekleyen uygulamalar şunlardır; Örneğin, tüm Azure AD hesapları desteklemek veya tüm Azure AD hesapları ve kişisel Microsoft hesapları destekler.

## <a name="new-applications"></a>Yeni uygulamalar

Yeni uygulamayı kaydedin, uygulamanızı yayımcı etki alanı varsayılan bir değere ayarlanabilir. Değer, burada uygulama, özellikle bir kiracıda kayıtlı uygulama ve etki alanlarını doğrulandı mı sahip Kiracı Kiracı olmadığını kayıtlı üzerinde bağlıdır.

Kiracı doğrulanmış etki alanları varsa, uygulamanın yayımcı etki alanı varsayılan olarak kiracının doğrulanmış birincil etki olacaktır. Hiçbir kiracının doğrulanmış etki alanları (uygulama bir kiracıda kayıtlı değil, durumdur), uygulamanın yayımcı etki alanı ayarlama olup olmadığını null.

Aşağıdaki tabloda, yayımcı etki alanı değerinin varsayılan davranışını özetlemektedir.  

| Kiracı doğrulanmış etki alanları | Yayımcı etki alanının varsayılan değer |
|-------------------------|----------------------------|
| null  | null  |
| *.onmicrosoft.com | *.onmicrosoft.com |
| - *.onmicrosoft.com<br/>-EtkiAlanı1.com<br/>-EtkiAlanı2.com (birincil) | EtkiAlanı2.com |

Bir çok kiracılı uygulama yayımcı etki ayarlanmamış veya biten bir etki alanına ayarlanır. onmicrosoft.com, uygulamanın onay istemi gösterir **doğrulanmamış** yayımcı etki alanı yerine.

## <a name="grandfathered-applications"></a>Grandfathered uygulamalar

Uygulamanızı 21 Mayıs 2019'dan önce kaydedildiyse, uygulamanızın onay istemi değil Göster **doğrulanmamış** yayımcı etki alanı ayarlamadıysanız. Kullanıcılar bu bilgileri, uygulamanızın onay istemi görebilmeniz için yayımcı etki alanı değeri ayarlamanızı öneririz.

## <a name="configure-publisher-domain-using-the-azure-portal"></a>Azure portalını kullanarak bir yayımcı etki alanını yapılandırma

Uygulamanızın yayımcı etki alanı ayarlamak için aşağıdaki adımları izleyin.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

1. Birden fazla Azure AD kiracısında hesabınız varsa:
   1. Sayfanın sağ üst köşedeki menüden profilinizi seçin ve ardından **dizini Değiştir**.
   1. Oturumunuz, uygulamanızı oluşturmak için istediğiniz Azure AD kiracısına değiştirin.

1. Gidin [Azure Active Directory > Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) bulup yapılandırmak istediğiniz uygulamayı seçin.

   Uygulamayı seçtikten sonra uygulamanın göreceğiniz **genel bakış** sayfası.

1. Uygulamanın gelen **genel bakış** sayfasında **markalama** bölümü.

1. Bulma **yayımcı etki alanı** alan ve aşağıdaki seçeneklerden birini seçin:

   - Seçin **bir etki alanı yapılandırma** bir etki alanı önce yapılandırmadıysanız.
   - Seçin **güncelleme etki alanı** bir etki alanı zaten yapılandırılmışsa.

Uygulamanızı bir kiracıda kayıtlı değilse, aralarından seçim yapabileceğiniz iki sekme görürsünüz: **Doğrulanmış bir etki alanı seçin** ve **yeni bir etki alanını doğrulama**.

Uygulamanızı bir kiracıda kayıtlı değilse, yalnızca uygulamanız için yeni bir etki alanı doğrulama seçeneğine görürsünüz.

### <a name="to-verify-a-new-domain-for-your-app"></a>Uygulamanız için yeni bir etki alanını doğrulamak için

1. Adlı bir dosya oluşturun `microsoft-identity-association.json` ve aşağıdaki JSON kod parçacığını yapıştırın.

   ```json
   {
      "associatedApplications": [
        {
           "applicationId": "{YOUR-APP-ID-HERE}"
        }
      ]
    }
   ```

1. Yer tutucusunu değiştirin *{YOUR-uygulama-kimliği-HERE}* uygulamanıza karşılık gelen uygulama (istemci) kimliği.

1. Dosyayı ana: `https://{YOUR-DOMAIN-HERE}.com/.well-known/microsoft-identity-association.json`. Yer tutucusunu değiştirin *{YOUR-DOMAIN-HERE}* doğrulanmış etki alanıyla eşleşecek şekilde.

1. Tıklayın **doğrulayın ve etki alanı kaydedin** düğmesi.

### <a name="to-select-a-verified-domain"></a>Doğrulanmış bir etki alanı seçin

- Kiracınızda doğrulanmış etki alanlarına, etki alanlarından birini **doğrulanmış bir etki alanı seçin** açılır.

## <a name="implications-on-the-app-consent-prompt"></a>Uygulama üzerindeki etkileri izin alma istemi

Yayımcı etki alanını yapılandırmak, kullanıcıların uygulama izni istemde gördükleri üzerinde bir etkisi yoktur. Onay istemi bileşenlerden tam olarak anlamak için bkz: [uygulama onay deneyimleri anlama](application-consent-experience.md).

Aşağıdaki tabloda, 21 Mayıs 2019'den önce oluşturulan uygulamaların davranışını açıklar.

![21 Mayıs 2019'den önce oluşturulan uygulamaların onay istemi](./media/howto-configure-publisher-domain/old-app-behavior-table.png)

Davranış 21 Mayıs 2019'den sonra oluşturulan yeni uygulamalar için yayımcı etki alanı ve uygulamanın türüne bağlıdır. Aşağıdaki tabloda, yapılandırmaları farklı birleşimlerle görmeyi beklemeniz değişiklikleri açıklar.

![21 Mayıs 2019'den sonra oluşturulan uygulamalar için izin alma istemi](./media/howto-configure-publisher-domain/new-app-behavior-table.png)

## <a name="implications-on-redirect-uris"></a>Yeniden yönlendirme URI'leri üzerindeki etkileri

Kullanıcılar herhangi bir iş veya Okul hesabı veya kişisel Microsoft hesapları ile oturum açma uygulamaları ([çok kiracılı](single-and-multi-tenant-apps.md)) belirtirken bazı sınırlamalara tabi yeniden yönlendirme olduğu URI'ler.

### <a name="single-root-domain-restriction"></a>Tek bir kök etki alanı kısıtlaması

Çok kiracılı uygulamalar için yayımcı etki alanı değeri null, uygulamalar için ayarlandığında bir yeniden yönlendirme URI'leri tek kök etki alanı paylaşmak için Yasak. Örneğin, aşağıdaki değerleri birleşimi için izin verilmiyor kök etki alanı, contoso.com, fabrikam.com eşleşmiyor.

```
"https://contoso.com",
"https://fabrikam.com",
```

### <a name="subdomain-restrictions"></a>Alt etki alanı kısıtlamaları

Alt etki alanlarını izin verilir, ancak kök etki alanını açıkça kaydetmeniz gerekir. Örneğin, aşağıdaki bir URI'leri paylaşmak sırada tek bir kök etki alanı birleşimi izin verilmiyor.

```
"https://app1.contoso.com",
"https://app2.contoso.com",
```

Ancak, geliştirici kök etki alanını açıkça eklerse, birlikte izin verilir.

```
"https://contoso.com",
"https://app1.contoso.com",
"https://app2.contoso.com",
```

### <a name="exceptions"></a>Özel Durumlar

Aşağıdaki durumlarda tek bir kök etki alanı kısıtlaması tabi değildir:

- Tek kiracılı uygulamalar ya da tek bir dizinde hesapları hedefleyen uygulamalar
- Yeniden yönlendirme URI'leri gibi localhost kullanımı
- Özel düzenleri (olmayan HTTP veya HTTPS) ile yeniden yönlendirme URI'leri

## <a name="configure-publisher-domain-programmatically"></a>Yayımcı etki alanı programlama yapılandırma

Şu anda bir yayımcı etki alanı programlı bir şekilde yapılandırmak için REST API veya PowerShell desteği yoktur.
