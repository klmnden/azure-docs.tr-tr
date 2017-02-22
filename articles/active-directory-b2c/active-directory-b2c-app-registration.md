---
title: "Azure Active Directory B2C: Uygulama kaydı | Microsoft Belgeleri"
description: "Uygulamanızı Azure Active Directory B2C&quot;ye kaydetme"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: fd22e9596feecbc12e577a4abfb47552e1b6e520
ms.openlocfilehash: da8f083cb7bca59501df080036e789a0fb75731e


---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Uygulamanızı kaydetme
## <a name="prerequisite"></a>Önkoşul
Tüketicinin kaydolmasını ve oturum açmasını kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısına kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin. Söz konusu makaledeki tüm adımları izledikten sonra B2C özellikleri dikey penceresi Başlangıç panonuza sabitlenir.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>B2C özellikleri dikey penceresine gitme
B2C özellikleri dikey penceresini Başlangıç panonuza sabitlediyseniz dikey pencereyi, B2C kiracısının Genel Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açar açmaz görürsünüz.

Ayrıca dikey pencereye, [Azure portalındaki](https://portal.azure.com/) **Diğer hizmetler**’e tıklayarak ve ardından sol gezinti bölmesinde **Azure AD B2C** araması yaparak erişebilirsiniz.

> [!IMPORTANT]
> B2C özellikleri dikey penceresine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
> 
> 

## <a name="register-an-application"></a>Bir uygulamayı kaydetme
1. Azure portalındaki B2C özellikleri dikey penceresinde **Applications (Uygulamalar)** seçeneğine tıklayın.
2. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
3. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso B2C uygulaması"na girebilirsiniz.
4. Web tabanlı bir uygulama yazıyorsanız **Include web app / web API (Web uygulamasını/web API'sini dahil et)** düğmesini **Yes (Evet)** olarak değiştirin. **Yanıt URL'leri**, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri getirdiği uç noktalardır. Örneğin, `https://localhost:44316/` girin. Web uygulamanız aynı zamanda Azure AD B2C ile güvenliği sağlanan bazı web API’lerini çağıracaksa **Anahtarı Oluştur** düğmesine tıklayarak bir **Uygulama Gizli Anahtarı** da oluşturmak istersiniz.
   
   > [!NOTE]
   > **Uygulama Gizli Anahtarı** önemli bir güvenlik kimlik bilgisidir ve güvenliği uygun şekilde sağlanmalıdır.
   > 
   > 
5. Mobil bir uygulama yazıyorsanız **Include native client (Yerel istemci ekle)** düğmesini **Yes (Evet)** olarak değiştirin. Sizin için otomatik olarak oluşturulan varsayılan **Yeniden Yönlendirme URI'sini** kopyalayın.
6. Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.
7. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.

> [!IMPORTANT]
> B2C özellikleri dikey penceresinde oluşturulan uygulamaların aynı konumda yönetilmesi gerekir. B2C uygulamalarını PowerShell veya başka bir portal kullanarak düzenlerseniz desteklenmez duruma gelirler ve Azure AD B2C ile çalışma olasılığı düşüktür.
> 
> 

## <a name="build-a-quick-start-application"></a>Hızlı Başlangıç Uygulaması oluşturma
Azure AD B2C'ye kayıtlı bir uygulamaya sahip olduğunuza göre başlamak ve çalıştırmak için hızlı başlangıç öğreticilerimizden birini tamamlayabilirsiniz. İşte birkaç öneri:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]




<!--HONumber=Feb17_HO1-->


