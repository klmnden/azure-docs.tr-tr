---
title: Azure Red Hat OpenShift için bir Azure AD uygulama kaydı ve kullanıcı oluşturun | Microsoft Docs
description: Hizmet sorumlusu oluşturma, bir istemci gizli anahtarı ve kimlik doğrulama geri çağırma URL'si oluşturun ve Microsoft Azure Red Hat OpenShift kümenizdeki uygulamaları test etmek için yeni bir Active Directory kullanıcı oluşturma hakkında bilgi edinin.
author: tylermsft
ms.author: twhitney
ms.service: openshift
manager: jeconnoc
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/06/2019
ms.openlocfilehash: de3f3c30848d26ea399bcccc29a6149a149f6a55
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078528"
---
# <a name="create-an-azure-ad-app-registration-and-user-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için bir Azure AD uygulama kaydı ve kullanıcı oluşturma

Microsoft Azure Red Hat OpenShift kümenizi adına görevleri gerçekleştirmek için izinler gerekiyor. Kuruluşunuz, hizmet sorumlusu olarak kullanılacak bir Azure Active Directory (Azure AD) uygulama kaydı zaten yoksa, oluşturmak için bu yönergeleri izleyin.

Bu konuda, yeni bir oluşturma hakkında yönergeler sonucuna Azure Red Hat OpenShift kümenizde çalışan uygulamalara erişmek için gereken bir Azure AD kullanıcısı.

Azure AD kiracısı henüz oluşturmadıysanız,'ndaki yönergeleri izleyin [Azure Red Hat OpenShift için Azure AD kiracısı oluşturma](howto-create-tenant.md) bu yönergeleri ile devam etmeden önce.

## <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturma

Azure ad özelliklerini kullanmak isteyen bir uygulama, bir Azure AD kiracısında önce kaydedilmelidir. Azure AD uygulama bulunduğu URL, bir kullanıcının kimliği doğrulandıktan sonra yanıtların gönderileceği URL, uygulama ve benzeri tanımlayan URI'yi gibi uygulamanızın ayrıntılarını vererek kayıt işlemini içerir.

1. İçinde [Azure portalında](https://portal.azure.com), kiracınıza kullanıcı adınızı üst altında göründüğünden emin olun portal'ın sağ:

    ![Kiracı portalı ekran görüntüsü, sağ üst bölümde listelenen][tenantcallout] yanlış kiracıya gösterilirse, sağ üst köşesindeki kullanıcı adına tıklayın ve ardından tıklayın **dizini Değiştir**, doğru kiracıdan seçip **tüm Dizinleri** listesi.

2. Açık [uygulama kayıtları dikey penceresinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) tıklatıp **yeni uygulama kaydı**.
3. İçinde **Oluştur** bölmesinde, uygulama nesnesi için bir kolay ad girin.
4. Emin **uygulama türü** ayarlanır *Web uygulaması/API'si*.
5. Oluşturma bir **oturum açma URL'si** şu biçimi kullanarak:

    ```
    https://<cluster-name>.<azure-region>.cloudapp.azure.com
    ```

    . . . Burada `<cluster-name>` hedeflenen Azure Red Hat OpenShift kümenizin adıdır ve `<azure-region>` olduğu [Azure Red Hat OpenShift kümenizin bulunduğu Azure bölgesine](supported-resources.md#azure-regions). Örneğin, küme adınızı olması durumunda `contoso`, ve onu oluşturacağınız `eastus` için gireceğiniz tam etki alanı adı (FQDN) bölge **oturum açma URL'si** olacaktır `https://contoso.eastus.cloudapp.azure.com`

    > [!IMPORTANT]
    > Küme adının tamamen küçük harflerden ve FQDN URL benzersiz olmalıdır.
    > Kümeniz için yeterince belirgin bir ad seçtiğinizden emin olun.

    Küme adınızı not edin ve oturum açma URL'si — daha sonra küme üzerinde çalışan uygulamalara erişim gerekir. Küme adı olarak anılacaktır `CLUSTER_NAME`ve bu oturum açma URL'si olarak `FQDN` içinde [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.

6. Olun, **oturum açma URL'si** değeri, yeşil bir onay işareti ile doğrular. (Odak tanesi için SEKME tuşuna basın *oturum açma URL'si* alan ve doğrulama denetimi gerçekleştirin.)
7. Tıklayın **Oluştur** düğmesini Azure AD uygulaması oluşturur.
8. Üzerinde **kayıtlı uygulama** görüntülenen sayfa aşağı kopyalama **uygulama kimliği**. Bu değer anılacaktır `APPID` içinde [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.

    ![Uygulama Kimliği textbox'ın ekran görüntüsü][appidimage]
    
Azure uygulama nesneleri hakkında daha fazla bilgi için bkz. [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

Yeni bir oluşturma hakkında bilgi edinmek için Azure AD uygulaması, bakın [bir uygulamayı Azure Active Directory v1.0 uç noktası ile kaydetme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

### <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Artık uygulamanızı Azure Active Directory kimlik doğrulaması için bir istemci gizli anahtarı oluşturmak hazırsınız.

1. Gelen **kayıtlı uygulama** sayfasında, şirket **ayarları** kayıtlı uygulama ayarlarını açın.
2. Üzerinde **ayarları** görüntülenen bölmesi **anahtarları**.  **Anahtarları** bölmesi görünür.
![Portalda anahtar Oluştur sayfasının ekran görüntüsü][createkeyimage]
3. Bir anahtarı **açıklama**.
4. İçin bir değer ayarlamanız **Expires**, örneğin *2 yıl içinde*.
5. Tıklayın **Kaydet** ve anahtar değeri olarak görünür.
6. Anahtar değerini kopyalayın. Bu değer anılacaktır `SECRET` içinde [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.

### <a name="create-a-reply-url"></a>Yanıt URL'si oluşturun

Azure AD kullanan uygulama nesnesi *yanıt URL'si* uygulama yetkilendirirken kullanmak için geri çağırma belirtmek için. Bir web API'si veya web uygulaması söz konusu olduğunda, yanıt URL'si Azure AD kimlik doğrulaması başarılı olursa, bir belirteç de kimlik doğrulaması yanıtını burada gönderir konumdur.

Daha küçük uyumsuzluğu (sondaki eğik çizgi eksik, farklı büyük/küçük harf) oturum açmanız mümkün olmayacaktır ve belirteç verme işleminin başarısız olmasına neden olur.

1. Geri Git **ayarları** bölmesinde (tıklayabilirsiniz **ayarları** yer alan içerik haritasındaki portalın üst kısmındaki), tıklatıp **yanıt URL'leri** sağ.  **Yanıt URL'leri** bölmesi görünür.
2. Listedeki ilk yanıt URL'si olacaktır `FQDN` adım 6'değerinden [yeni bir uygulama kaydı oluşturma](#create-a-new-app-registration). Düzenleme ve ekleme `/oauth2callback/Azure%20AD`.  Örneğin, yanıt URL'si şimdi şuna benzer olmalıdır `https://mycluster.eastus.cloudapp.azure.com/oauth2callback/Azure%20AD`
3. Tıklayın **Kaydet** yanıt URL'si kaydetmek için.

## <a name="create-a-new-active-directory-user"></a>Yeni bir Active Directory kullanıcısı oluşturma

Azure Red Hat OpenShift kümenizde çalışan uygulamayı imzalamak için kullanılacak Active Directory'de yeni bir kullanıcı oluşturun.

1. Git [kullanıcılar - tüm kullanıcılar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) dikey penceresi.
2. Tıklayın **+ yeni kullanıcı**. **Kullanıcı** bölmesi görünür.
3. Girin bir **adı** bu kullanıcı için istediğiniz.
4. Oluşturma bir **kullanıcı adı** ile oluşturduğunuz Kiracı adını temel `.onmicrosoft.com` sonuna eklenir. Örneğin, `yourUserName@yourTenantName.onmicrosoft.com`. Bu kullanıcı adı yazın. Kümenizde uygulamayı kullanmak oturum açmanız gerekir.
5. Tıklayın **dizin rolü** seçip **genel yönetici** ve ardından **Tamam** bölmesinin alt kısmındaki.
6. Ortasında **kullanıcı** bölmesinde tıklayın **Göster parola** ve geçici parolasını kaydedin. İlk kez oturum sonra sıfırlayın istenir.
7. Bölmenin altında tıklatın **Oluştur** kullanıcı oluşturmak için.

## <a name="resources"></a>Kaynaklar

* [Uygulamalar ve Azure Active Directory'de Hizmet sorumlusu nesneleri](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)  
* [Hızlı Başlangıç: Azure Active Directory v1.0 uç noktası ile bir uygulamayı kaydetme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)  

[appidimage]: ./media/howto-create-tenant/get-app-id.png
[createkeyimage]: ./media/howto-create-tenant/create-key.png
[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="next-steps"></a>Sonraki adımlar

Tüm karşılamanızın varsa [Azure Red Hat OpenShift önkoşulları](howto-setup-environment.md), ilk kümenizi oluşturmak hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
