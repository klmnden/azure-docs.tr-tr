---
title: Azure IOT Central bir uygulamayı yönetme | Microsoft Docs
description: Bir yönetici olarak Azure IOT Central uygulamanızı yönetme
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1bb0bc0aa7ad6bbbad502832ba8e0a96f36de428
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268319"
---
# <a name="administer-your-iot-central-application"></a>IOT Central uygulamanızı yönetme

Microsoft Azure IOT Central bir uygulama oluşturduktan sonra kullanabileceğiniz **Yönetim** yönetmek için Azure IOT Central kullanıcı arabiriminin bir bölümü. Gitmek için **Yönetim** bölümünden **Yönetim** sol gezinti menüsünde.

**Yönetim** bölümünde:

- Kullanıcıları yönetme

- Rolleri yönetme

- Fatura bilgileri görüntüleyin

- Uygulama ayarlarını yönet

- Ücretsiz deneme olanağı sunuyoruz

İçinde **Yönetim** bölümünde, ikincil Gezinti Menüsü çeşitli yönetim görevlerini bağlantılar içeriyor.

Erişimi ve kullanımı **Yönetim** bölümünde olmanız gerekir **yönetici** rolü için bir Azure IOT Central uygulamasına. Azure IOT Central bir uygulama oluşturursanız, otomatik olarak atanmış **yönetici** o uygulama için rol. *Kullanıcıları yönetme* bölümünde bu makalede açıklanır daha nasıl atanacağı hakkında **yönetici** diğer kullanıcılara rol.

## <a name="change-application-name"></a>Uygulama adını değiştir

Uygulamanızın adını değiştirmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

Üzerinde **uygulama ayarları** ettiğiniz bir ad girin, sayfa **uygulama adı** alan. Daha sonra **Kaydet**’e tıklayın.

## <a name="change-the-application-url"></a>Uygulama URL'sini değiştirme

Uygulamanızın URL'sini değiştirmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

![Uygulama Ayarları sayfası](media\howto-administer\image0-a.png)

Üzerinde **uygulama ayarları** ettiğiniz URL'sini girin **URL** alan ve ardından **Kaydet**. URL'niz, en fazla 200 karakter uzunluğunda olabilir. URL kullanılabilir durumda değilse, bir doğrulama hatası görürsünüz.

> [!Note]
> URL'niz değiştirirseniz, eski URL'nizi başka bir Azure IOT Central müşteri tarafından alınabilir. Bu durumda, artık kullanmak için kullanılabilir. URL'niz değiştirdiğinizde, eski URL artık çalışır ve kullanmak için yeni URL ilgili kullanıcılarınıza bildirmeniz gerekir.

## <a name="change-the-application-image"></a>Değişiklik uygulama görüntüsü

Azure IOT Central bir uygulamada görüntüleri kullanma hakkında daha fazla bilgi için bkz. [hazırlama ve karşıya yükleme görüntüleri, Azure IOT Central uygulamasına](howto-prepare-images.md).

## <a name="copy-an-application"></a>Bir uygulamayı kopyalama

Tüm cihaz örnekleri, cihaz verileri geçmişi ve kullanıcı verileri hariç herhangi bir uygulamanın bir kopyasını oluşturabilirsiniz. Kopyalama için ücret ödersiniz Ücretli bir uygulama olacaktır. Başka bir uygulama kopyalayarak deneme uygulama oluşturulamıyor.

Bir uygulama kopyalamak için Git **uygulama ayarları** sayfası. Ardından **kopyalama** düğmesi.

![Uygulama Ayarları sayfası](media\howto-administer\appCopy1.png)

Seçme **kopyalama** düğme, bir adı, URL, Azure AD dizini, abonelik ve Azure bölgesi uygulamanızı kopyalayarak oluşturulacak yeni bir uygulama için bir iletişim kutusunu açar. Bu alanların her biri için değerler seçin. Ardından **kopyalama** düğmesini devam etmek istediğinizi onaylayın. Bu makalede bu değerler hakkında girin gerekenler hakkında daha fazla bilgi [bir uygulamanın nasıl oluşturulacağını](howto-create-application.md).

![Uygulama Ayarları sayfası](media\howto-administer\appCopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra kopyalama, uygulamanız tarafından oluşturulan yeni uygulama gidebilirsiniz. Uygulamaya Git üzerinde görünen bağlantıyı seçin **uygulama ayarları** sayfası.

![Uygulama Ayarları sayfası](media\howto-administer\appCopy3.png)

> [!Note]
> Bir uygulamayı kopyalama, kurallar veya Eylemler tanımını kopyalar. Ancak özgün uygulamanıza erişimi olan kullanıcılar için kopyalanan uygulamayı kopyalanmaz olduğundan, kullanıcıları kullanıcılar önkoşul olan e-posta gibi eylemleri el ile eklemeniz gerekir.

## <a name="delete-an-application"></a>Bir uygulamayı Sil

Uygulamanızı silmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

Seçin **Sil**.

> [!Note]
> Bir uygulamayı kalıcı olarak silmek, uygulama ile ilişkili tüm verileri siler.  Bir uygulamayı silmek için de kaynakları silmek için izinleri olmalıdır uygulama oluştururken seçtiğiniz Azure aboneliğinde. Daha fazla bilgi için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimini kullanma](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Azure IOT Central rolleri

Rolleri kimin kuruluşunuzdaki çeşitli Azure IOT Central görevleri gerçekleştirebilirsiniz denetim sağlıyor. Azure IOT Central, uygulamanızın kullanıcılara atayabileceğiniz üç rol yok. Roller, her bir uygulama tarafından atanır. Aynı kullanıcı farklı uygulamalarda farklı rollere sahip olabilir. Aynı kullanıcı bir uygulama içinde birden çok rol atayabilirsiniz.

### <a name="administrator"></a>Yönetici

Kullanıcıların **yönetici** rolü Azure IOT Central bir uygulamada tüm işlevlere erişimi vardır.

Bir uygulamayı oluşturan kullanıcı otomatik olarak atanır **yönetici** rol. Ayrıca en az bir kullanıcı her zaman olmalıdır **yönetici** rol.

### <a name="application-builder"></a>Uygulama Oluşturucu

Kullanıcıların **uygulama Oluşturucusu** rol bir Azure IOT Central uygulamasına uygulamayı yönetme dışında her şeyi yapabilir.

### <a name="application-operator"></a>Uygulama işleci

Kullanıcıların **uygulama işleci** rol erişmeye değilsiniz **uygulama Oluşturucusu** sayfası. Bunlar, uygulamayı yönetemezsiniz.

## <a name="manage-users"></a>Kullanıcıları yönetme

Uygulama Yöneticileri kullanıcıların uygulama roller atayabilirsiniz.

### <a name="add-users"></a>Kullanıcı ekle

Oturum açın ve bir Azure IOT Central uygulamasına erişmek için önce her kullanıcının bir kullanıcı hesabı olması gerekir. Microsoft Accounts (MSA) ve Azure Active Directory (Azure AD) hesaplarını Azure IOT Central desteklenir. Azure Active Directory grupları şu anda Azure IOT Central ' desteklenmez.

Daha fazla bilgi için [Microsoft hesabı Yardım](https://support.microsoft.com/products/microsoft-account?category=manage-account) ve [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Bir kullanıcı hesabı için bir Azure IOT Central uygulamasına eklemek için gitmek için ikincil Gezinti Menüsü kullanın **kullanıcılar** sayfasını **Yönetim** bölümü.

    ![Kullanıcı listesi](media\howto-administer\image1.png)

1. Bir kullanıcı eklemek için **kullanıcılar** sayfasında **+ Ekle kullanıcı**.

    ![Kullanıcı ekle](media\howto-administer\image2.png)

1. Kullanıcının bir rol seçin **rol** açılan menüsü. Roller hakkında daha fazla bilgi *Azure IOT Central rollerinde* bu makalenin.

    ![Rolü seçimi](media\howto-administer\image3.png)

    > [!NOTE]
    >  Kullanıcıları toplu halde eklemek için noktalı virgülle ayrılmış eklemek istediğiniz tüm kullanıcıların kimliklerini kullanıcı girin. Bir rolden seçin **rol** açılan menüsü. Daha sonra **Kaydet**’e tıklayın.

1. Bir kullanıcı ekledikten sonra bir giriş o kullanıcı için görünür **kullanıcılar** sayfası.

    ![Kullanıcı listesi](media\howto-administer\image4.png)

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Kullanıcılara atanan ve rollerini Düzenle

Rol atanmış oldukları sonra değiştirilemez. Bir kullanıcıya atanmış rol değiştirmek için kullanıcıyı silin ve ardından farklı bir rolüyle kullanıcıyı ekleyin.

### <a name="delete-users"></a>Kullanıcıları Sil

Kullanıcıları silmek için üzerinde bir veya daha fazla onay kutularını işaretleyin **kullanıcılar** sayfası. Ardından **Sil**’i seçin.

## <a name="view-your-bill"></a>Faturanızı görüntüleyin

Faturanızı görmek için Git **faturalama** sayfasını **Yönetim** bölümü. Ardından **faturalama**. Azure faturalama sayfasını fatura uygulamaların her biri, Azure IOT Central için görebileceğiniz yeni bir sekmede açılır.

## <a name="convert-your-trial-to-a-paid-application"></a>Denemenizi Ücretli bir uygulamaya dönüştürme

IOT Central değerlendirdiniz sonra Ücretli bir uygulamaya denemenizi dönüştürebilirsiniz. Bu Self Servis işlemi tamamlamak için aşağıdaki adımları izleyin:

1. İkincil Gezinti Menüsü için Git kullanın **faturalama** sayfasını **Yönetim** bölümü. Deneme süreniz uzatıldıysa henüz sayfası aşağıdaki ekran görüntüsüne benzer görünür:

    ![Ücretsiz deneme sürümü durumu](media/howto-administer/freetrial.png)

2. Seçin **Ücretli sürüme dönüştürme**. Deneme süreniz uzatıldıysa henüz açılır penceresi aşağıdaki ekran görüntüsüne benzer görünür:

    ![Ücretsiz deneme süresini uzatın](media/howto-administer/extend.png)

3. Açılır pencerede uygun Azure Active Directory kiracısı ve ardından Azure aboneliğini IOT Central uygulamanız için kullanılacağını seçin.

3. Seçtikten sonra **dönüştürme**, faturalandırılan, deneme dönüştürür ve Ücretli bir uygulamaya başlayın.

## <a name="extend-your-free-trial"></a>Ücretsiz deneme sürenizi uzatın

Varsayılan olarak, tüm ücretsiz denemeleri yedi gün boyunca kullanılabilir. Deneme sürenizi 30 gün artırmak istiyorsanız, aşağıdaki adımları izleyin:

1. İkincil Gezinti Menüsü için Git kullanın **faturalama** sayfasını **Yönetim** bölümü.

1. Seçin **deneme süresini uzatın**. Açılır pencerede IOT Central uygulamanız için kullanılacak uygun Azure Active Directory kiracısı ve sonra da Azure aboneliğini seçin.

1. Ardından **Genişlet**. Deneme süreniz, şimdi 30 gün boyunca geçerlidir.

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Denetim düzlemi işlemleri için Azure SDK'ları kullanın

IOT Central Azure Resource Manager SDK'sı paketleri, düğüm, Python, C#, Ruby, Java ve Git için kullanılabilir. Oluşturmanızı, Listele, Güncelleştir veya IOT Central uygulamaları silin sağlayan bu kitaplıkları destek denetim düzlemi işlemleri IOT Central için. Kimlik doğrulaması başa çıkmak için de Yardımcıları sağlanır ve hata işleme, her dil için özeldir. 

Azure Resource Manager SDK'ları, kullanma örnekleri bulabilirsiniz [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Daha fazla bilgi için Github'da bu paketleri göz atın.

| Dil | Havuz | Paket |
| ---------| ---------- | ------- |
| Node | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Başlayın | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızı yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz şablonu ayarlama](howto-set-up-template.md)
