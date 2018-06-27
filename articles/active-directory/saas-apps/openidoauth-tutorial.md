---
title: Azure AD uygulama galerisinde Openıd/OAuth uygulamadan yapılandırma adımları | Microsoft Docs
description: Azure AD uygulama galerisinde Openıd/OAuth uygulamayı yapılandırmak için adımlar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 69e9d66458409bbc744416a58ceb508349418a76
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019562"
---
# <a name="steps-to-configure-an-openidoauth-application-from-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde Openıd/OAuth uygulamadan yapılandırma adımları

## <a name="process-of-open-id-application-addition-from-gallery"></a>Galeriden Open ID uygulama toplama işlemi

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi](./media/openidoauth-tutorial/tutorial_general_01.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](./media/openidoauth-tutorial/tutorial_general_02.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi](./media/openidoauth-tutorial/tutorial_general_03.png)

4. Arama kutusuna **uygulama adı**seçin **uygulama istenen** sonuç paneli ve oturum uygulama kadar.

    ![Uygulama ekleme](./media/openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Open ID Connect ve OAuth için uygulama Ekle düğmesi varsayılan olarak devre dışıdır. Kiracı yönetici üzerinde burayı **kaydolma** düğmesine tıklayın ve uygulama onay sağlayın. İle uygulama açıkça eklemenize gerek müşteri Kiracı eklenen ve yapılandırmaları yapın.

    ![Ekle düğmesi](./media/openidoauth-tutorial/addbutton.png)

5. Kayıt bağlantısı tıkladığınızda, oturum açma kimlik bilgileri için Azure AD sayfasına yönlendirilir.

6. Başarılı bir kimlik doğrulamasından sonra kullanıcı, onay sayfasından ve bundan sonra onay kabul sahip, uygulama giriş sayfası görüntülenir.

    > [!NOTE]
    > Müşteriler, yalnızca uygulamanın bir örneği eklemek için izin verilir. Zaten bir eklediyseniz ve onay vermeniz denedi yeniden onu yeniden kiracısında eklenmez. Bu nedenle mantıksal bunlar Kiracı içinde yalnızca bir uygulama örneğini kullanabilirsiniz.

## <a name="authentication-flow-using-openid-connect"></a>Openıd Connect kullanarak kimlik doğrulama akışı

En basit oturum açma akışını aşağıdaki adımları içerir - bunların her biri aşağıda ayrıntılı olarak açıklanmıştır.

![Openıd Connect kullanarak kimlik doğrulama akışı](./media/openidoauth-tutorial/authenticationflow.png)

* **Çok kiracılı uygulama** -birçok kuruluş kullanmak için çok kiracılı uygulama amaçlanmamıştır yalnızca bir kuruluş. Bunlar genellikle yazılım olarak-hizmet (SaaS) uygulamaları bir bağımsız yazılım satıcısı (ISV) tarafından yazılmış olan. Çok kiracılı uygulamalara, bunları kaydetmek için kullanıcı veya yönetici izni gerektiren burada bunlar kullanılacak, her dizin için hazırlanması gerekir. Bir uygulama dizinde kayıtlı ve erişim verilen grafik API'sini veya belki başka bir web API için bu onay işlemi başlar. Bir kullanıcı veya farklı bir kuruluşta uygulamayı kullanmak kaydolun yöneticisinden uygulama gerektiriyor izinleri görüntüleyen bir iletişim kutusu sunulur. Kullanıcı veya yönetici uygulama erişimi için belirtilen veri sağlar ve son olarak uygulamaları kendi dizininde kaydeder uygulamaya ardından onay.

    > [!NOTE]
    > Hangi Kiracı belirlemek için bir mekanizma, uygulamanızın kullanılabilir birden fazla dizine kullanıcılara yapıyorsanız, ihtiyacınız bunlar oturum açtınız. Azure AD içinde tüm dizinlerinden belirli bir kullanıcıyı tanımlamak çok kiracılı uygulama gereken sırada bir kullanıcının kendi dizinde aramak bir tek kiracılı uygulama yeterlidir. Bu görevi gerçekleştirmek için Azure AD herhangi bir çok kiracılı uygulama oturum açma istekleri, bir kiracı özel uç noktası yerine burada yönlendirebilirsiniz ortak bir kimlik doğrulama uç noktası sağlar. Bu uç nokta [ https://login.microsoftonline.com/common ](https://login.microsoftonline.com/common) Azure AD'de tüm dizinler için bir kiracı özel uç noktası olabilir ancak [ https://login.microsoftonline.com/contoso.onmicrosoft.com ](https://login.microsoftonline.com/contoso.onmicrosoft.com). Ortak bir uç oturum açma, oturum kapatma ve belirteç doğrulama sırasında birden çok kiracıya işlemek için gerekli mantığı gerekir çünkü Uygulamanızı geliştirirken dikkat edilmesi özellikle önemlidir.

Çeşitli kuruluşlar arasında kolayca erişilebilen ve kullanımı kolay onay kabul ettikten sonra varsayılan olarak azure AD ekibi çok kiracılı uygulama yükseltir.

## <a name="what-is-consent-framework"></a>Onay Framework nedir?

Azure AD onay framework çok kiracılı web ve yerel istemci uygulamaları geliştirmek kolaylaştırır. Bu uygulamalar, uygulama kayıtlı olandan farklı bir Azure AD kiracısı oturum açma ana kadar kullanıcı hesapları tarafından izin. Web API'ları (Azure Active Directory, Intune ve Office 365'te hizmetlerine erişmek için) Microsoft grafik API'si gibi ve web API'leri yanı sıra diğer Microsoft Hizmetleri API'leri erişmek de gerekebilir. Bir kullanıcı veya yönetici dizin verilerine erişme gerektirebilir kendi dizininde kaydedilecek soran bir uygulamaya izin vermiş framework dayanır. İzin verilen sonra istemci uygulamasının kullanıcı adına Microsoft Graph API çağrısı ve gerektiği gibi bilgileri kullanın.

[Microsoft Graph API](https://graph.microsoft.io/) (takvimleri ve Exchange, siteler ve SharePoint, OneDrive belgelerden, OneNote, Planlayıcısı, Excel çalışma kitaplarından görevlerden defterlerinden listelerinden iletileri gibi Office 365'te verilere erişim sağlar , kullanıcıların ve grupların Azure AD'den ve diğer veri nesneleri daha fazla Microsoft bulut hizmetlerinden yanı sıra vb.).

Aşağıdaki adımlar nasıl onayı deneyimi uygulama geliştiricisi ve kullanıcı için çalışır gösterir.

1. Belirli bir kaynak/API'sine erişim izni istemek için gereken bir web istemci uygulaması olduğunu varsayın. Azure portal yapılandırması aynı anda izin istekleri bildirmek için kullanılır. Diğer yapılandırma ayarları gibi bunlar uygulamanın Azure AD kaydını bir parçası haline gelir:

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Uygulamanızın izinlerini güncelleştirildi, uygulaması çalıştıran ve yaklaşık ilk kez kullanmak için bir kullanıcı olduğunu göz önünde bulundurun. Uygulamanın ilk gerekir Azure AD'den ait bir yetkilendirme kodu elde / son noktanın yetkilendirilmesi. Yetkilendirme kodu sonra yeni bir erişim edinmeli ve belirteç yenilemek için kullanılabilir.

3. Kullanıcı zaten doğrulanmış, Azure AD değil, / yetkilendirmek için oturum açma uç noktası ister.

    ![Kimlik Doğrulaması](./media/openidoauth-tutorial/authentication.png)

4. Kullanıcı oturum açtıktan sonra Azure AD kullanıcı bir onay sayfasına gerekip gerekmediğini belirler. Bu belirleme, kullanıcı (veya kuruluşun yönetici) zaten uygulama izin verilip üzerinde temel alır. İzin zaten verilmiş değil, Azure AD kullanıcıdan izin ve çalışması için gerekli olan gerekli izinleri görüntüler. Onay iletişim kutusunda görüntülenen izinler kümesini Azure portalında izinlere temsilci olarak seçilen dosyalardan eşleşmesi.

    ![Onay sayfası](./media/openidoauth-tutorial/consentpage.png)

Diğer bir kiracı yöneticisinin iznini gerektirirken bazı izinler için normal bir kullanıcı tarafından rıza.

## <a name="whats-the-difference-between-admin-consent-and-user-consent"></a>Yönetici onayı kullanıcı izni arasındaki fark nedir?

Yönetici olarak, aynı zamanda tüm kullanıcılar adına bir uygulamanın temsilci izinleri kiracınızda sayılırsınız. Yönetici onayı onay iletişim Kiracı her kullanıcı için görüntülenmesini engeller ve Azure portalında Yönetici rolüne sahip kullanıcılar tarafından yapılabilir. Ayarlar sayfasında uygulamanız için gerekli izinler'i tıklatın ve izinler düğmesine tıklayın.

![İzin ver](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> İzinler düğmesini kullanarak açık izin verme ADAL.js kullanan tek sayfa uygulamaları için (SPA) şu anda gereklidir. Erişim belirteci istendiğinde, aksi takdirde uygulama başarısız olur.

Yalnızca uygulama izinleri her zaman bir kiracı yönetici izni gerektirir. Uygulamanızı bir yalnızca uygulama izni ister ve uygulamada oturum açmak bir kullanıcı çalışırsa, kullanıcı onayı mümkün değil bildiren bir hata iletisi görüntülenir.

Uygulamanızı yönetici izni gerektiren izinleri kullanıyorsa, yönetici eylemi burada başlatabilirsiniz düğmesini veya bağlantısını gibi hareket olması gerekir. Bu eylem ayrıca istemi içeren her zamanki OAuth2/Openıd Connect yetkilendirme isteği için uygulamanızı gönderir istek = admin_consent sorgu dizesi parametresi. = Admin_consent parametresini yönetici seçtiği ve hizmet sorumlusu müşterinin Kiracı oluşturulduğunda, sonraki oturum açma istekleri istemi gerekmez. Yönetici istenen izinleri kabul edilebilir olduğuna karar olduğundan, hiçbir diğer kullanıcılar Kiracı o noktadan izin istenir. Kiracı Yöneticisi uygulamalara onayı normal kullanıcılara devre dışı bırakabilirsiniz. Bu özelliği devre dışıysa, yönetici onayı her zaman uygulamanın kiracısında kullanılması gereklidir. Devre dışı son kullanıcı izin ile uygulamanızı test etmek istiyorsanız, yapılandırma anahtarı bulabilirsiniz [Azure portal](https://portal.azure.com/) içinde [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) altında bölümünde **Enterprise uygulamaları**

İstemi admin_consent = parametresi da yönetici onayı gerektirmeyen izinleri gerektiren uygulamalar tarafından kullanılabilir. Bu olduğunda kullanılacak bir uygulama nerede Kiracı Yönetici "kaydolduğunda" bir deneyim gerekiyorsa bir saat ve başka kullanıcıların bu noktadan itibaren izin istenir örnektir.

Bir uygulama yönetici izni gerektirir ve sorulmadan yönetici oturum açtığında admin_consent parametresi yalnızca kendi kullanıcı hesabı için geçerli uygulama için yönetici başarıyla izin olduğunda, gönderilen =. Normal kullanıcı hala oturum açın veya uygulamaya onayı mümkün olmaz. Bu özellik, Kiracı yönetici diğer kullanıcılar erişime izin vermeden önce uygulamanızın keşfedin vermek istiyorsanız yararlıdır.