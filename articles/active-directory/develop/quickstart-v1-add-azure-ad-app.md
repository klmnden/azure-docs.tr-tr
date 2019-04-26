---
title: Azure Active Directory v1.0 uç noktasına uygulama kaydetme
description: Bir uygulamayı Azure Active Directory (Azure AD) v1.0 uygulamasına nasıl ekleyip kaydedeceğinizi öğrenin.
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
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15e77767e16ce6ae02c6f1f93891d5acf4bfcb6f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299524"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v10-endpoint"></a>Hızlı Başlangıç: Azure Active Directory v1.0 uç noktasına uygulama kaydetme

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Kurumsal geliştiriciler ve hizmet olarak yazılım (SaaS) sağlayıcıları, hizmetleri için güvenli oturum açma ve yetkilendirme imkanı sunmak için Azure Active Directory (Azure AD) ile tümleştirilebilen ticari bulut hizmetleri veya iş kolu uygulamaları geliştirebilir. Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir.

Azure AD'nin özelliklerini kullanmak isteyen her uygulama önce bir Azure AD kiracısı olarak kaydedilmelidir. Bu kayıt işlemi Azure AD'ye uygulamanızın bulunduğu konumun URL'si, bir kullanıcının kimliği doğrulandıktan sonra yanıtların gönderileceği URL, uygulamayı tanımlayan URI gibi ayrıntılarının verilmesini içerir.

Bu hızlı başlangıçta Azure portalındaki **Uygulama kayıtları** deneyimini kullanarak Azure AD’de uygulama ekleme ve kaydetme işlemleri gösterilir.

> [!NOTE]
> Yeni bir uygulama mı kaydediyorsunuz? Azure portalında yeni **Uygulama kayıtları (Önizleme)** deneyimini kullanın. Başlamak için bkz. [Uygulama kaydetme (Önizleme)](quickstart-register-app.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için uygulamalarınızı kaydetmek için kullanabileceğiniz bir Azure AD kiracınız olduğundan emin olun. Henüz bir kiracınız yoksa [nasıl kiracı alınabileceğini öğrenin](quickstart-create-new-tenant.md).

## <a name="register-a-new-application-using-the-azure-portal"></a>Yeni bir uygulamayı Azure portalını kullanarak kaydetme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin.
1. **Uygulama kayıtları**'nı, sonra da **Yeni uygulama kaydı**'nı seçin.

    ![Yeni uygulama kaydetme](./media/quickstart-v1-integrate-apps-with-azure-ad/add-app-registration.png)

1. **Oluştur** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 

   - **Adı:** Anlamlı uygulama adı girin
   - **Uygulama türü:**
     - Bir cihaza yerel olarak yüklenen [istemci uygulamaları](developer-glossary.md#client-application) için **Yerel**'i seçin. Bu ayar OAuth ortak [yerel istemcileri](developer-glossary.md#native-client) için kullanılır.
     - Güvenli bir sunucuya yüklenen [istemci uygulamaları](developer-glossary.md#client-application) ve [kaynak/API uygulamaları](developer-glossary.md#resource-server) için **Web uygulaması/API'si**'ni seçin. Bu ayar OAuth gizli [web istemcileri](developer-glossary.md#web-client) ve ortak [kullanıcı aracısı tabanlı istemciler](developer-glossary.md#user-agent-based-client) için kullanılır. Aynı uygulama gerek bir istemciyi, gerekse kaynağı/API'yi sunabilir.
   - **Oturum açma URL'si:** İçin "Web uygulaması / API'si" uygulamaları, uygulamanızın temel URL'si girin. Örneğin `http://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır. 
   - **Yeniden yönlendirme URI'si:** "Yerel" uygulamaları için Azure AD'nin belirteç yanıtlarını döndürmek için kullanılan URI girin. Uygulamanıza özgü bir değer girin; örneğin `http://MyFirstAADApp`

     ![Yeni bir uygulamayı kaydetme - oluşturma](./media/quickstart-v1-integrate-apps-with-azure-ad/add-app-registration-create.png)

     Web uygulamalarına veya yerel uygulamalara özgü örnekler istiyorsanız, belgelerimizdeki **Hızlı Başlangıçlar**'a bakın.

1. Bittiğinde **Oluştur**’u seçin.

    Azure AD, uygulamanıza benzersiz bir Uygulama Kimliği atar ve uygulamanızın ana kayıt sayfasına yönlendirilirsiniz. Web uygulaması ya da yerel uygulama olmasına bağlı olarak uygulamanıza ek özellikler eklemek için değişik seçenekler sunulur.

      > [!NOTE]
      > Yeni kaydedilen bir web uygulaması, varsayılan olarak **yalnızca** aynı kiracıdan kullanıcıların oturum açmasına izin verecek şekilde yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar

- Onay vermeye genel bir bakış için bkz. [Azure AD onay çerçevesi](consent-framework.md).
- Uygulama kaydınızda kimlik bilgileri, izinler, başka kiracılardan kullanıcıların oturum açmasını etkinleştirme gibi ek yapılandırma özelliklerini etkinleştirme için bkz. [Azure AD'de bir uygulamayı güncelleştirme](quickstart-v1-update-azure-ad-app.md).
- Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve bunlar aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).
- Azure Active Directory ile uygulama geliştirirken kullanmanız gereken markalama yönergeleri hakkında daha fazla bilgi edinmek için bkz. [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md).
