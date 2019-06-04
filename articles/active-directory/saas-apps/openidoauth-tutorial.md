---
title: Bir Azure AD uygulama galerisinde Openıd/OAuth uygulamasından yapılandırma | Microsoft Docs
description: Bir Azure AD uygulama galerisinde Openıd/OAuth uygulamasından yapılandırma adımları.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 166452b052313397f1ec17adb59cad3c20fab1f9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497477"
---
# <a name="configure-an-openidoauth-application-from-the-azure-ad-app-gallery"></a>Bir Azure AD uygulama Galerisi Openıd/OAuth uygulaması yapılandırma

## <a name="process-of-adding-an-openid-application-from-the-gallery"></a>Galeriden bir Openıd uygulama ekleme işlemi

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi](common/select-azuread.png))

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Seçin **yeni uygulama** iletişim kutusunun üst kısmındaki.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, uygulama adı yazın. Sonuç panelden istediğiniz uygulamayı seçin ve uygulamaya kaydolun.

    ![Sonuç listesinde Openıd](common/search-new-app.png)

    > [!NOTE]
    > Uygulamalar, Openıd Connect ve OAuth için **Ekle** düğmesi, varsayılan olarak devre dışıdır. Burada, Kiracı Yöneticisi kayıt seçmelisiniz düğmesi ve uygulamaya onay sağlayın. Uygulama ardından, nerede yapılandırmalar yapabilirsiniz müşteri kiracıya eklenir. Uygulamayı açıkça eklemenize gerek yoktur.

    ![Ekle düğmesi](./media/openidoauth-tutorial/addbutton.png)

5. Kaydolma bağlantısı seçtiğinizde, oturum açma kimlik bilgilerini Azure Active Directory (Azure AD) sayfasına yönlendirilirsiniz.

6. Başarılı kimlik doğrulamadan sonra onay sayfasından onayı kabul etmiş olursunuz. Bundan sonra uygulama giriş sayfası görüntülenir.

    > [!NOTE]
    > Uygulamanın yalnızca bir örneğini ekleyebilirsiniz. Zaten bir eklendi ve yeniden izin sağlamanız denedi, onu yeniden kiracıda eklenmeyecek. Bu nedenle mantıksal olarak, kiracıda yalnızca bir uygulama örneği kullanabilirsiniz.

## <a name="authentication-flow-using-openid-connect"></a>Openıd Connect'i kullanarak kimlik doğrulama akışı

En temel oturum açma akışını aşağıdaki adımları içerir:

![Openıd Connect'i kullanarak kimlik doğrulama akışı](./media/openidoauth-tutorial/authenticationflow.png)

### <a name="multitenant-application"></a>Çok kiracılı uygulama 
Çok müşterili uygulamalarda pek çok kuruluşta, kullanım için tasarlanmamış yalnızca bir kuruluş. Bir bağımsız yazılım satıcısı (ISV) tarafından yazılan genellikle yazılım olarak-hizmet (SaaS) uygulamalarıyla şunlardır. 

Çok kiracılı uygulamaları nerede bunlar kullanılacak her dizinde sağlanması gerekir. Bunlar, bunları kaydetmek için kullanıcı veya yönetici onayı gerektirir. Bir uygulama dizinde kayıtlı ve Graph API'sini ya da belki başka bir web API'si için erişim verilir, bu onay işlemini başlatır. Uygulamayı kullanmak, bir kullanıcının veya yöneticinin farklı bir kuruluştan kaydolduğunda, bir iletişim kutusu, uygulamanız için gereken izinleri görüntüler. 

Kullanıcı veya yönetici uygulamaya onay verebilir. Onay belirtilen uygulama erişmesini sağlayan ve son olarak dizinde uygulama kaydeder.

> [!NOTE]
> Hangi Kiracı belirlemek için bir mekanizma, uygulamanızın kullanılabilir birden çok dizini kullanıcılara yapıyorsanız, ihtiyacınız bulundukları. Tek kiracılı bir uygulama, yalnızca bir kullanıcı için kendi dizinde aramak gerekir. Azure AD'de belirli bir kullanıcı tarafından tüm dizinleri tanımlamak çok kiracılı bir uygulama gerekir.
> 
> Bu görevi gerçekleştirmek için Azure AD oturum açma istekleri bir kiracıya özgü uç nokta yerine herhangi bir çok kiracılı Uygulama yeri yönlendirebilir ortak bir kimlik doğrulama uç noktası sağlar. Bu uç nokta [ https://login.microsoftonline.com/common ](https://login.microsoftonline.com/common) Azure ad'deki tüm dizinler için. Bir kiracıya özgü uç nokta olabilir [ https://login.microsoftonline.com/contoso.onmicrosoft.com ](https://login.microsoftonline.com/contoso.onmicrosoft.com). 
>
> Ortak uç nokta, uygulamanızı geliştirirken dikkate almak önemlidir. Oturum açma, oturum kapatma ve belirteç doğrulama sırasında birden fazla Kiracı işlemek için gerekli mantığı gerekir.

Varsayılan olarak, çok kiracılı uygulamaları Azure AD'ye yükseltir. Kuruluşlar arasında kolayca erişim ve onayı kabul ettikten sonra bunların kullanımı kolaydır.

## <a name="consent-framework"></a>Onay çerçevesi

Azure AD onay çerçevesine, çok kiracılı web ve yerel istemci uygulamaları geliştirmek için kullanabilirsiniz. Bu uygulamalar, uygulama kayıtlı olduğu farklı Azure AD kiracısı oturum açma ana kadar kullanıcı hesapları tarafından tanır. Web API'leri gibi erişmek de gerekebilir:
- Azure AD erişim, Intune ve Office 365 Hizmetleri için Microsoft Graph API. 
- Diğer Microsoft Hizmetleri API'ler.
- Kendi web API'leri. 

Bir kullanıcı veya yönetici, dizinde kayıtlı ister bir uygulamaya onay vermiş framework dayanır. Kayıt, dizin verilerini erişim gerektirebilir. İzin verilen sonra istemci uygulama kullanıcı adına Microsoft Graph API'sini çağırmak ve gerektiği gibi bilgileri kullanın.

[Microsoft Graph API](https://developer.microsoft.com/graph/) gibi Office 365'te verilere erişim sağlar:

- Takvimler ve Exchange'e iletileri.
- Siteler ve SharePoint listelerinden.
- Onedrive'dan belgeler.
- OneNote Not defterlerinden.
- Planner görevlerini.
- Excel çalışma kitapları.

Graph API'si de erişim kullanıcıları ve grupları Azure ad ve diğer veri nesneleri için daha fazla Microsoft bulut hizmetlerinden sağlar.

Aşağıdaki adımlar nasıl çalışır uygulama geliştiriciler ve kullanıcı için onay deneyimi gösterir:

1. Belirli bir kaynağa veya API erişim izni istemek için gereken bir web istemci uygulaması olduğunu kabul edelim. Azure portal, yapılandırma sırasında izin isteklerini bildirmek için kullanılır. Diğer yapılandırma ayarları gibi uygulamanın Azure AD'ye kayıtları bir parçası haline gelir. İzleme ihtiyacınız izni istek yolu için aşağıdaki adımları:

    a. Tıklayarak **uygulama kayıtları** sol tarafındaki menüsünün ve açık uygulama yazarak, uygulamanızın arama kutusuna adı.

    ![Graph API](./media/openidoauth-tutorial/application.png)

    b. Tıklayın **API izinleri görüntüle**.

    ![Graph API](./media/openidoauth-tutorial/api-permission.png)

    c. Tıklayarak **bir izin eklemek**.

    ![Graph API](./media/openidoauth-tutorial/add-permission.png)

    d. Tıklayarak **Microsoft Graph**.

    ![Graph API](./media/openidoauth-tutorial/microsoft-graph.png)

    e. Gerekli seçenekleri arasından seçim **temsilci izinleri** ve **uygulama izinleri**.

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Uygulama izinleri güncelleştirildi göz önünde bulundurun. Uygulamayı çalıştıran ve bir kullanıcı ilk kez kullanmak hakkında sağlamaktır. Uygulamanın ilk gerekiyor son noktanın yetkilendirilmesi Azure AD'den bir yetkilendirme kodu alın. Yetkilendirme kodu, ardından belirteci yenileyin ve yeni bir erişim almak için kullanılabilir.

3. Kullanıcının kimliği zaten varsa, Azure AD / authorize oturum açma uç noktası ister.

    ![Kimlik Doğrulaması](./media/openidoauth-tutorial/authentication.png)

4. Kullanıcı oturum açtıktan sonra Azure AD, kullanıcının bir onay sayfası gerekip gerekmediğini belirler. Bu belirleme, kullanıcı (veya onun kuruluşunun yönetici) zaten uygulama onay verilip üzerinde temel alır.

   İzin verilmedi, Azure AD kullanıcıdan onayı ister ve çalışması için gerekli izinleri görüntüler. Onay iletişim kutusunda görüntülenen izinlere temsilci izinleri Azure portalında seçili olanlarla eşleşmesi.

    ![Onay sayfası](./media/openidoauth-tutorial/consentpage.png)

Normal bir kullanıcı için bazı izinler onay verebilir. Diğer izinler bir kiracı yönetici onayı gerektirir.

## <a name="difference-between-admin-consent-and-user-consent"></a>Yönetici onayı ve kullanıcı onayı arasındaki fark

Bir yönetici olarak, aynı zamanda tüm kullanıcılar adına uygulamanın temsilci izinleri için kiracınızda onay verebilir. Yönetici onayı onay iletişim kutusu, kiracıdaki her kullanıcı için görüntülenmesini engeller. Yönetici rolüne sahip kullanıcılar, Azure portalında onay sağlayabilir. Gelen **ayarları** uygulamanızın, seçin sayfasında **gerekli izinler** > **yönetici onayı vermek**.

![İzinler düğmesi](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> Kullanarak açık izin verme **yönetici onayı vermek** düğmesi, artık ADAL.js kullanan tek sayfalı uygulamalar için (Spa'lar) gereklidir. Erişim belirteci istendiğinde, aksi takdirde uygulama başarısız olur.

Yalnızca uygulama izinleri, her zaman bir kiracı yönetici onayı gerektirir. Uygulamanızı bir salt uygulama izni isteklerini ve bir kullanıcı uygulamada oturum açmaya çalışırsa bir hata iletisi görüntülenir. Kullanıcı onayı sağlayamadığı ileti yazar.

Uygulamanız yönetici onayı gerektiren izinler kullanıyorsa, yönetim eylemi başlayabileceğiniz bir düğme veya bağlantı gibi hareket olması gerekir. Bu eylem için uygulamanızın gönderdiği normal OAuth2/Openıd Connect yetkilendirme isteği isteğidir. Bu istek içerir *istemi admin_consent =* sorgu dizesi parametresi. 

Yönetici onay verdi ve müşteri kiracısında hizmet sorumlusu oluşturulduktan sonra sonraki oturum açma istekleri gerekmeyen *istemi admin_consent =* parametresi. Yönetici, istenen izinleri kabul edilebilir olduğunu karar verdi çünkü başka hiçbir kullanıcı kiracıda o noktadan ilerideki onayı istenir.

Kiracı Yöneticisi, uygulamalara izin vermesi normal kullanıcılar için devre dışı bırakabilirsiniz. Bu özellik devre dışı bırakılırsa, yönetici onayı her zaman kiracısında kullanılmak üzere uygulama için gerekli değildir. Devre dışı son kullanıcı onayı ile uygulamanızı test etmek istiyorsanız, yapılandırma anahtarı bulabilirsiniz [Azure portalında](https://portal.azure.com/). İçinde [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) bölümüne **kurumsal uygulamalar**.

*İstemi admin_consent =* parametresi yönetici onayı gerektirmeyen izinleri isteyen uygulamalar tarafından da kullanılabilir. Burada Kiracı Yönetici "kaydolursa" bir zaman başka hiçbir kullanıcıların ve o noktadan onay üzerinde istenir bir deneyim gerektiren bir uygulama buna bir örnektir.

Bir uygulama, yönetici onayı gerektirir ve bir yönetici olmadan oturum açtığında Imagine *istemi admin_consent =* gönderilen parametre. Yönetici başarıyla uygulamaya onay verdiğinde, yalnızca kendi kullanıcı hesabı için geçerlidir. Normal kullanıcıların oturum açın veya uygulama onayı olacaktır. Bu özellik, Kiracı Yöneticisi diğer kullanıcılar erişime izin vermeden önce uygulamanızın keşfedin vermek istiyorsanız yararlıdır.
