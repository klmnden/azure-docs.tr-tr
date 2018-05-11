---
title: Özel geliştirilmiş bir uygulama için belirli alanları doldurmak nasıl | Microsoft Docs
description: Azure AD ile özel bir geliştirilmiş uygulama kaydederken belirli alanları doldurmak nasıl hakkında yönergeler
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cd4313efb5d08842ba12ec00e6e5160214800d56
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Özel geliştirilmiş bir uygulama için belirli alanları doldurmak nasıl

Bu makalede, kullanılabilir tüm alanlar uygulama kayıt formunda kısa bir açıklamasını sağlar [Azure portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Yeni uygulamayı Kaydet

-   Yeni bir uygulama kaydetmek için gidin [Azure portal](https://portal.azure.com).

-   Sol gezinti bölmesinden tıklatın **Azure Active Directory.**

-   Seçin **uygulama kayıtlar** tıklatıp **Ekle**.

-   Uygulama kayıt formu bu açın.

## <a name="fields-in-the-application-registration-form"></a>Uygulama kayıt formu alanları


| Alan            | Açıklama                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Ad             | Uygulamanın adı. En az dört karakter olmalıdır.                |
| Uygulama Türü | **Web uygulaması/Web API**: bir web uygulaması, web API veya her ikisini de temsil eden bir uygulama 
| |**Yerel**: bir kullanıcının Cihazınızda veya bilgisayarınızda yüklü bir uygulama           |
| Oturum Açma URL'si      | Burada kullanıcıların uygulamanızı kullanmaya oturum açabilirsiniz URL'si                                  |

Yukarıdaki alanları doldurduktan sonra uygulama Azure portalında kaydedilir ve uygulama sayfasına yönlendirilirsiniz. **Ayarları** uygulama bölmesi düğmesinde uygulamanızı özelleştirmenize olanak için daha fazla alan Ayarları sayfası açılır. Aşağıdaki tabloda Ayarları sayfasında tüm alanları açıklar. yalnızca bir alt kümesini, bir web uygulaması veya bir yerel uygulamayı oluşturduğunuz bağlı olarak, bu alanlara görür unutmayın.

| Alan           | Açıklama                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uygulama Kimliği  | Bir uygulamayı kaydetme, Azure AD, uygulamanızın bir uygulama kimliği atar Kimliği Azure ad kimlik doğrulama isteklerini uygulamanızda benzersizce gibi kaynaklara erişmek için kullanılan uygulama grafik API'si ister.                                                          |
| Uygulama Kimliği URI'si      | Bu genellikle formunun benzersiz bir URI olmalıdır **https://&lt;Kiracı\_adı&gt;/&lt;uygulama\_adı&gt;.** Bu yetkilendirme grant akışı sırasında için belirteç veren kaynak belirtmek için benzersiz bir tanımlayıcı olarak kullanılır. Verilen erişim belirteci 'aud' talepte haline gelir. |
| Karşıya yeni logo yükle | Bu, uygulamanız için bir logosu yüklemek için kullanabilirsiniz. Logo .bmp, .jpg veya .png biçiminde olmalıdır ve dosya boyutu 100 KB'tan daha az olmalıdır. Görüntü boyutları merkezi görüntü boyutları 94 x 94 piksel ile 215 x 215 piksel olmalıdır.                                                       |
| Giriş sayfası URL'si   | Uygulama kaydı sırasında belirtilen oturum açma URL'si budur.                                                                                                                                                                                                                                              |
| Oturum Kapatma URL'si      | Bu tek oturum kapatma oturum kapatma URL'si. Kullanıcı Azure AD ile kullanıcıların oturumlarını temizlediğinde azure AD herhangi bir kayıtlı uygulamayı kullanarak bu URL'yi bir oturum kapatma isteği gönderir.                                                                                                                                       |
| Çok kiracılı  | Bu anahtar, uygulamanın birden çok kiracılar tarafından kullanılıp kullanılamayacağını belirtir. Bu genellikle, dış kuruluşlar kendi Kiracı kaydetme ve kuruluşun verilere erişim izni verme uygulamanızı kullanabileceğiniz anlamına gelir.                                                                   |
| Yanıt URL'leri      | Yanıt URL'leri Azure AD uygulamanız tarafından istenen herhangi bir belirtece döndüğü noktalarıdır.                                                                                                                                                                                                          |
| Yeniden Yönlendirme URI'leri   | Yerel uygulamalar için kullanıcı başarılı yetkilendirme sonrasında burada gönderilen budur. Yeniden yönlendirme URI'si uygulamanız OAuth 2.0 istekte sağlayan azure AD onay Portalı'nda kayıtlı değerlerden biri ile eşleşir.                                                            |
| Anahtarlar            | Program aracılığıyla erişim Web API'leri herhangi bir kullanıcı etkileşimi olmadan Azure AD tarafından güvenli hale getirilmiş anahtarları oluşturabilir. Gelen \* \*anahtarları\* \* sayfasında, anahtar açıklaması ve sona erme tarihini girin ve anahtarı oluşturmak için kaydedin. Daha sonra erişim açamayacaksınız gibi güvenli bir yere kaydettiğinizden emin olun.             |

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
