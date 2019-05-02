---
title: Web API'leri - Microsoft kimlik platformu erişmek için bir uygulama yapılandırma
description: Microsoft kimlik platformuyla kaydedilen uygulamaları yeniden yönlendirme URI’leri, kimlik bilgileri veya web API’lerine erişmek için izinleri içerecek şekilde yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: celested
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e5adafc251735fd25b819921514bf6d1d3c3955
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918873"
---
# <a name="quickstart-configure-a-client-application-to-access-web-apis"></a>Hızlı Başlangıç: Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma

Bir web/gizli istemci uygulamasının kimlik doğrulaması gerektiren bir yetkilendirme akışına dahil olabilmesi (ve erişim belirteci alabilmesi) için güvenli kimlik bilgileri kullanması gerekir. Azure portal tarafından desteklenen varsayılan kimlik doğrulaması yöntemi istemci kimliği ve gizli anahtar kullanımıdır.

Ayrıca, istemcinin bir kaynak uygulaması tarafından kullanıma sunulan web API'sine (Microsoft Graph API gibi) erişebilmesi için, onay çerçevesi istenen izinlere bağlı olarak istemcinin gerekli izni almasını sağlar. Varsayılan olarak, tüm uygulamalar Microsoft Graph API’sinden izinleri seçebilir. [Graph API “Oturum açma ve kullanıcı profilini okuma” izni](https://developer.microsoft.com/graph/docs/concepts/permissions_reference#user-permissions) varsayılan olarak seçilir. İstenen her web API'si için [iki izin türü](developer-glossary.md#permissions) arasından seçim yapabilirsiniz:

* **Uygulama izinleri** - İstemci uygulamanızın web API'sine kendisi olarak (kullanıcı bağlamı olmadan) doğrudan erişmesi gerekir. Bu izin türü için yönetici onayı gerekir ve bu tür genel (masaüstü ve mobil) istemci uygulamalarında kullanılamaz.
* **Temsilcili izinler** - İstemci uygulamanızın web API'sine oturum açmış kullanıcı olarak erişmesi gerekir ancak erişim seçilen izinle sınırlı olur. Yönetici onayı gerektirmediği sürece bu izin türü bir kullanıcı tarafından verilebilir.

  > [!NOTE]
  > Bir uygulamaya temsilcili izin eklenmesi kiracı içindeki kullanıcılara otomatik olarak onay vermez. Yöneticinin tüm kullanıcıların adına onay vermemesi durumunda kullanıcıların yine çalışma zamanında eklenen temsilcili izinler için el ile onay vermesi gerekir.

Bu hızlı başlangıçta, uygulamanızı aşağıdakiler için yapılandırmayı öğreneceksiniz:

* [Uygulamanıza yeniden yönlendirme URI’leri ekleme](#add-redirect-uris-to-your-application)
* [Web uygulamanız için kimlik bilgilerini ekleyin](#add-credentials-to-your-web-application)
* [Web API’lerine erişim izinleri ekleme](#add-permissions-to-access-web-apis)

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki önkoşulları karşıladığınızdan emin olun:

* Diğer kullanıcılar veya uygulamalar tarafından kullanılması gereken uygulamaları derleme konusunda önemli olan desteklenen [izinler ve onaylar](v2-permissions-and-consent.md) hakkında bilgi edinin.
* Uygulamaların kaydedilmiş olduğu bir kiracı kullanın.
  * Kayıtlı uygulama yoksa, [Microsoft kimlik platformu ile uygulamaları kaydetmeyi öğrenin](quickstart-register-app.md).
* Azure portalında uygulama kaydı için Önizleme deneyimine katılın. Bu hızlı başlangıçtaki adımlar yeni kullanıcı arabirimine karşılık gelir ve yalnızca Önizleme deneyimine geçmeyi kabul ettiyseniz çalışır.

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

Uygulamayı yapılandırmadan önce, aşağıdaki adımları izleyin:

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve ardından **Uygulama kayıtları (Önizleme)** seçeneğini belirleyin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtiğinizde, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.
1. Uygulamanızı web API'lerine erişmek üzere yapılandırmak için adımları izleyin: 
    * [Uygulamanıza yeniden yönlendirme URI’leri ekleme](#add-redirect-uris-to-your-application)
    * [Web uygulamanız için kimlik bilgilerini ekleyin](#add-credentials-to-your-web-application)
    * [Web API’lerine erişim izinleri ekleme](#add-permissions-to-access-web-apis)

## <a name="add-redirect-uris-to-your-application"></a>Uygulamanıza yeniden yönlendirme URI’leri ekleme

[![Web ve genel istemci uygulamaları için özel yeniden yönlendirme URI’leri ekleme](./media/quickstart-update-azure-ad-app-preview/authentication-redirect-uris-expanded.png)](./media/quickstart-update-azure-ad-app-preview/authentication-redirect-uris-expanded.png#lightbox)

Uygulamanıza yeniden yönlendirme URI’si eklemek için:

1. Uygulamanın **Genel Bakış** sayfasında, **Kimlik doğrulaması** bölümünü seçin.

1. Web ve genel istemci uygulamalarına özel bir yeniden yönlendirme URI’si eklemek için:

   1. **Yeniden yönlendirme URI’si** bölümün bulun.
   1. **Web** veya **Genel istemci (mobil ve masaüstü)** seçenekleri arasından oluşturduğunuz uygulama türünü seçin.
   1. Uygulamanız için Yeniden Yönlendirme URI’sini girin.
      * Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin `http://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır.
      * Genel uygulamalar için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Uygulamanıza özgü bir değer girin; örneğin https://MyFirstApp.

1. Ortak istemciler (mobil, masaüstü) için önerilen Yeniden Yönlendirme URI'lerini seçemk için bu adımları izleyin:

    1. **Ortak istemciler (mobil, masaüstü) için önerilen Yeniden Yönlendirme URI'leri** bölümünü bulun.
    1. Onay kutularını kullanarak uygulamanız için uygun Yeniden Yönlendirme URI’lerini seçin.

## <a name="add-credentials-to-your-web-application"></a>Web uygulamanıza kimlik bilgileri ekleme

[![Sertifikalar ve istemci gizli dizileri ekleme](./media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)](./media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png#lightbox)

Web uygulamanıza kimlik bilgileri eklemek için:

1. Uygulamanın **Genel Bakış** sayfasında, **Sertifikalar ve gizli diziler** bölümünü seçin.

1. Bir sertifika eklemek için aşağıdaki adımları izleyin:

    1. **Sertifikayı karşıya yükle**’yi seçin.
    1. Yüklemek istediğiniz dosyayı seçin. Şu dosya türlerinden biri olmalıdır: .cer, .pem, .crt.
    1. **Add (Ekle)** seçeneğini belirleyin.

1. Bir istemci gizli dizisi eklemek için aşağıdaki adımları izleyin:

    1. **Yeni istemci gizli dizisi**’ni seçin.
    1. İstemci gizli diziniz için bir açıklama ekleyin.
    1. Bir süre seçin.
    1. **Add (Ekle)** seçeneğini belirleyin.

> [!NOTE]
> Yapılandırmada yaptığınız değişiklikleri kaydettikten sonra en sağdaki sütunda istemci gizli dizi değeri gösterilir. İstemci uygulamanızın kodunda kullanmak üzere **değeri kopyalamayı unutmayın**. Bu sayfadan ayrıldıktan sonra bu değere erişemezsiniz.

## <a name="add-permissions-to-access-web-apis"></a>Web API’lerine erişim izinleri ekleme

[![API izinleri ekleme](./media/quickstart-update-azure-ad-app-preview/api-permissions-expanded.png)](./media/quickstart-update-azure-ad-app-preview/api-permissions-expanded.png#lightbox)

İstemcinizden kaynak API'lerine erişim izni eklemek için:

1. Uygulamanın **Genel Bakış** sayfasında, **API izinleri** bölümünü seçin.
1. **İzin ekleyin** düğmesini seçin.
1. Varsayılan olarak, görünüm **Microsoft API’leri** arasından seçim yapmanızı sağlar. İlgilendiğiniz API’leri seçin:
    * **Microsoft API’leri** - Microsoft Graph gibi Microsoft API’leri için izinleri seçmenize olanak sağlar.
    * **Kuruluşumun kullandığı API’ler** - Kuruluşunuz tarafından kullanıma sunulan API’ler veya kuruluşunuzun tümleştirdiği API’ler için izinleri seçmenize olanak sağlar.
    * **API’lerim** - Kullanıma sunduğunuz API’ler için izinleri seçmenize olanak sağlar.
1. API’leri seçtikten sonra, **API İzinleri İsteme** sayfasını görürsünüz. API hem temsilci hem de uygulama izinlerini kullanıma sunuyorsa, uygulamanız için gereken izin türünü seçin.
1. İşiniz bittiğinde, **İzinleri ekle** seçeneğini belirleyin. İzinlerin kaydedilip tabloya eklendiği **API izinleri** sayfasına dönersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalar için diğer ilgili uygulama yönetimi hızlı başlangıçları hakkında bilgi edinin:

* [Microsoft kimlik platformu ile uygulama kaydetme](quickstart-register-app.md)
* [Bir uygulamayı web API'lerini kullanıma sunacak şekilde yapılandırma](quickstart-configure-app-expose-web-apis.md)
* [Bir uygulama tarafından desteklenen hesapları değiştirme](quickstart-modify-supported-accounts.md)
* [Microsoft kimlik platformu ile kaydedilmiş bir uygulamayı kaldırma](quickstart-remove-app.md)

Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Azure Active Directory ile uygulama geliştirirken kullanmanız gereken markalama yönergeleri hakkında daha fazla bilgi edinmek için bkz. [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md).
