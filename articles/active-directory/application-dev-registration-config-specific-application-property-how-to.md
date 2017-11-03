---
title: "Özel geliştirilmiş bir uygulama için belirli alanları doldurmak nasıl | Microsoft Docs"
description: "Azure AD ile özel bir geliştirilmiş uygulama kaydederken belirli alanları doldurmak nasıl hakkında yönergeler"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 04fd35f238e4dd05486f85b0b16c2ab0c5ae9f30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Özel geliştirilmiş bir uygulama için belirli alanları doldurmak nasıl

Bu makale size uygulama kayıt formunda kullanılabilir tüm alanlar kısa bir açıklamasını [Azure portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Yeni uygulamayı Kaydet

-   Yeni bir uygulama kaydetmek için gidin [Azure portal](https://portal.azure.com).

-   Sol gezinti bölmesinden tıklatın **Azure Active Directory.**

-   Seçin **uygulama kayıtlar** tıklatıp **Ekle**.

-   Uygulama kayıt formu bu açın.

## <a name="fields-in-the-application-registration-form"></a>Uygulama kayıt formu alanları


| Alan            | Açıklama                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Ad             | Uygulamanın adı. En az dört karakter olmalıdır.                |
| Uygulama türü | **Web uygulaması/Web API**: bir web uygulaması, web API veya her ikisini de temsil eden bir uygulama 
| |**Yerel**: bir kullanıcının Cihazınızda veya bilgisayarınızda yüklü bir uygulama           |
| Oturum açma URL'si      | Burada kullanıcıların uygulamanızı kullanmaya oturum açabilirsiniz URL'si                                  |

Yukarıdaki alanları doldurduktan sonra uygulamayı Azure Portalı'nda kayıtlı olması ve uygulama sayfasına yeniden yönlendirilmeniz. **Ayarları** uygulama bölmesi düğmesinde uygulamanızı özelleştirmenize olanak için daha fazla alan Ayarları sayfası açılır. Aşağıdaki tabloda Ayarları sayfasında tüm alanları açıklar. yalnızca bir alt kümesini, bir web uygulaması veya bir yerel uygulamayı oluşturduğunuz bağlı olarak, bu alanlara görür unutmayın.

| Alan           | Açıklama                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uygulama Kimliği  | Bir uygulamayı kaydetme, Azure AD, uygulamanızın bir uygulama kimliği atar Kimliği Azure ad kimlik doğrulama isteklerini uygulamanızda benzersizce gibi kaynaklara erişmek için kullanılan uygulama grafik API'si ister.                                                          |
| Uygulama Kimliği URI'si      | Bu genellikle formunun benzersiz bir URI olmalıdır **https://&lt;Kiracı\_adı&gt;/&lt;uygulama\_adı&gt;.** Bu yetkilendirme grant akışı sırasında için belirteç veren kaynak belirtmek için benzersiz bir tanımlayıcı olarak kullanılır. Verilen erişim belirteci 'aud' talepte haline gelir. |
| Karşıya yeni logo yükle | Bu, uygulamanız için bir logosu yüklemek için kullanabilirsiniz. Logo .bmp, .jpg veya .png biçiminde olmalıdır ve dosya boyutu 100 KB'tan daha az olmalıdır. Görüntü boyutları merkezi görüntü boyutları 94 x 94 piksel ile 215 x 215 piksel olmalıdır.                                                       |
| Giriş sayfası URL'si   | Uygulama kaydı sırasında belirtilen oturum açma URL'si budur.                                                                                                                                                                                                                                              |
| Oturum kapatma URL'si      | Bu tek oturum kapatma oturum kapatma URL'si. Kullanıcı Azure AD ile kullanıcıların oturumlarını temizlediğinde azure AD herhangi bir kayıtlı uygulamayı kullanarak bu URL'yi bir oturum kapatma isteği gönderir.                                                                                                                                       |
| Çoklu kiralanan  | Bu anahtar, uygulamanın birden çok kiracılar tarafından kullanılıp kullanılamayacağını belirtir. Bu genellikle, dış kuruluşlar kendi Kiracı kaydetme ve kuruluşun verilere erişim izni verme uygulamanızı kullanabileceğiniz anlamına gelir.                                                                   |
| Yanıt URL'leri      | Yanıt URL'leri burada Azure AD dönüş uygulamanız tarafından istenen herhangi bir belirtece noktalarıdır.                                                                                                                                                                                                          |
| Yeniden yönlendirme URI'ler   | Yerel uygulamalar için bu kullanıcının burada olması olduğu için aşağıdaki başarılı yetkilendirme gönderilir. Yeniden yönlendirme URI'si uygulamanız OAuth 2.0 istekte sağlayan azure AD onay Portalı'nda kayıtlı değerlerden biri ile eşleşir.                                                            |
| Anahtarlar            | Program aracılığıyla erişim Web API'leri herhangi bir kullanıcı etkileşimi olmadan Azure AD tarafından güvenli hale getirilmiş anahtarları oluşturabilir. Gelen \* \*anahtarları\* \* sayfasında, anahtar açıklaması ve sona erme tarihini girin ve anahtarı oluşturmak için kaydedin. Daha sonra erişim açamayacaksınız gibi güvenli bir yere kaydettiğinizden emin olun.             |

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md)
