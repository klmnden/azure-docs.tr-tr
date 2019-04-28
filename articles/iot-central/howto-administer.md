---
title: Azure IOT Central bir uygulamayı yönetme | Microsoft Docs
description: Bir yönetici olarak Azure IOT Central uygulamanızı yönetme
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 29ded279e2a76940049c257b954b1dae75f14836
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118910"
---
# <a name="administer-your-iot-central-application"></a>IOT Central uygulamanızı yönetme

Bir IOT Central uygulaması oluşturduktan sonra gidebilirsiniz **Yönetim** bölümünü:

- Uygulama ayarlarını yönet
- Kullanıcıları yönetme
- Rolleri yönetme
- Faturanızı görüntüleyin
- Deneme sürümünüzü Kullandıkça Öde aboneliğine dönüştürün.
- Verileri dışarı aktarma
- Cihaz bağlantısını yönetme
- Erişim belirteçleri kullanma

Erişimi ve kullanımı **Yönetim** bölümünde olmanız gerekir **yönetici** rolü için bir Azure IOT Central uygulamasına. Azure IOT Central bir uygulama oluşturursanız, otomatik olarak atanmış **yönetici** o uygulama için rol. [Kullanıcıları Yönet](#manage-users) bölümünde bu makalede açıklanır daha nasıl atanacağı hakkında **yönetici** diğer kullanıcılara rol.

## <a name="manage-application-settings"></a>Uygulama ayarlarını yönet

### <a name="change-application-name-and-url"></a>Değişiklik uygulama adı ve URL
İçinde **uygulama ayarları** sayfasını, adını ve uygulamanızın URL'sini değiştirin ve ardından seçin **Kaydet**.

![Uygulama Ayarları sayfası](media/howto-administer/image0-a.png)

> [!Note]
> URL'niz değiştirirseniz, eski URL'nizi başka bir Azure IOT Central müşteri tarafından alınabilir. Bu durumda, artık kullanmak için kullanılabilir. URL'niz değiştirdiğinizde, eski URL artık çalışır ve kullanmak için yeni URL ilgili kullanıcılarınıza bildirmeniz gerekir.

### <a name="prepare-and-upload-image"></a>Görüntüleri hazırlama ve karşıya yükleme
Uygulama görüntüsü değiştirmek için bkz [hazırlama ve karşıya yükleme görüntüleri, Azure IOT Central uygulamasına](howto-prepare-images.md).

### <a name="copy-an-application"></a>Bir uygulamayı kopyalama
Tüm cihaz örnekleri, cihaz verileri geçmişi ve kullanıcı verileri hariç herhangi bir uygulamanın bir kopyasını oluşturabilirsiniz. Kopyalama için ücret ödersiniz bir Kullandıkça Öde uygulamasıdır. Bu şekilde deneme uygulama oluşturulamıyor.

Seçin **kopyalama**. İletişim kutusunda, yeni bir Kullandıkça Öde uygulama için ayrıntıları girin. Ardından **kopyalama** devam etmek istediğinizi onaylayın. Bu formdaki alanları hakkında daha fazla bilgi [uygulama oluşturma](quick-deploy-iot-central.md) hızlı başlangıç.

![Uygulama Ayarları sayfası](media/howto-administer/appcopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra görünen bağlantıyı kullanarak uygulamanızı kopyalayarak oluşturulan yeni uygulama gidebilirsiniz.

![Uygulama Ayarları sayfası](media/howto-administer/appCopy3.png)

> [!Note]
> Bir uygulamayı kopyalama, kurallar ve Eylemler tanımını kopyalar. Ancak özgün uygulamanıza erişimi olan kullanıcılar için kopyalanan uygulamayı kopyalanmaz olduğundan el ile kullanıcılar için bir önkoşul olan e-posta gibi eylemleri için kullanıcılar eklemeniz gerekir. Genel kurallar ve Eylemler içinde yeni uygulama güncel olduklarından emin olmak için kontrol etmek için bir fikirdir.

### <a name="delete-an-application"></a>Bir uygulamayı Sil

> [!Note]
> Bir uygulamayı silmek için de kaynakları silmek için izinleri olmalıdır uygulama oluştururken seçtiğiniz Azure aboneliğinde. Daha fazla bilgi için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimini kullanma](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Kullanım **Sil** IOT Central uygulamanız kalıcı olarak silmek için düğmeyi. Kalıcı olarak bunu, uygulama ile ilişkili tüm verileri siler.

## <a name="manage-users"></a>Kullanıcıları yönetme

### <a name="add-users"></a>Kullanıcı ekle

Oturum açın ve bir Azure IOT Central uygulamasına erişmek için önce her kullanıcının bir kullanıcı hesabı olması gerekir. Microsoft Accounts (MSA) ve Azure Active Directory (Azure AD) hesaplarını Azure IOT Central desteklenir. Azure Active Directory grupları şu anda Azure IOT Central ' desteklenmez.

Daha fazla bilgi için [Microsoft hesabı Yardım](https://support.microsoft.com/products/microsoft-account?category=manage-account) ve [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Bir kullanıcı bir IOT Central uygulamasına eklemek için Git **kullanıcılar** sayfasını **Yönetim** bölümü.

    ![Kullanıcı listesi](media/howto-administer/image1.png)

1. Bir kullanıcı eklemek için **kullanıcılar** sayfasında **+ Ekle kullanıcı**.

1. Kullanıcının bir rol seçin **rol** açılan menüsü. Roller hakkında daha fazla bilgi [rolleri yönetme](#manage-roles) bu makalenin.

    ![Rolü seçimi](media/howto-administer/image3.png)

    > [!NOTE]
    >  Kullanıcıları toplu halde eklemek için noktalı virgülle ayrılmış eklemek istediğiniz tüm kullanıcıların kimliklerini kullanıcı girin. Bir rolden seçin **rol** açılan menüsü. Daha sonra **Kaydet**’e tıklayın.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Kullanıcılara atanan ve rollerini Düzenle

Rol atanmış oldukları sonra değiştirilemez. Bir kullanıcıya atanmış rol değiştirmek için kullanıcıyı silin ve ardından farklı bir rolüyle kullanıcıyı ekleyin.

### <a name="delete-users"></a>Kullanıcıları Sil

Kullanıcıları silmek için üzerinde bir veya daha fazla onay kutularını işaretleyin **kullanıcılar** sayfası. Ardından **Sil**’i seçin.

## <a name="manage-roles"></a>Rolleri yönetme

Rol, kuruluşunuzda kimlerin IOT Central çeşitli görevleri gerçekleştirebilirsiniz sağlar. Üç rol, uygulamanızın kullanıcılara atayabileceğiniz vardır.

### <a name="administrator"></a>Yönetici

Kullanıcıların **yönetici** rolü bir uygulamada tüm işlevlere erişimi vardır.

Bir uygulamayı oluşturan kullanıcı otomatik olarak atanır **yönetici** rol. Ayrıca en az bir kullanıcı her zaman olmalıdır **yönetici** rol.

### <a name="application-builder"></a>Uygulama Oluşturucu

Kullanıcıların **uygulama Oluşturucusu** rolü bir uygulamada uygulamayı yönetmek dışında her şeyi yapabilir. Başka bir deyişle, Oluşturucular, düzenlemek ve cihaz şablonları ve cihazları silmek, cihaz kümeleri yönetme, oluşturup analiz ve işleri çalıştırabilirsiniz. Oluşturucular erişimine sahip olmaz **Yönetim** uygulama bölümü.

### <a name="application-operator"></a>Uygulama İşletmeni

Kullanıcıların **uygulama işleci** rolü cihaz şablonlarda değişiklik yapılamıyor ve uygulamayı yönetemezsiniz. Bu işleçler eklemeyi ve silmeyi cihazları, cihaz kümeleri yönetebilir ve analiz ve işleri çalıştırma anlamına gelir. İşleçler, erişim sahibi olmaz **uygulama Oluşturucusu** ve **Yönetim** sayfaları.

## <a name="view-your-bill"></a>Faturanızı görüntüleyin

Faturanızı görmek için Git **faturalama** sayfasını **Yönetim** bölümü. Azure faturalama sayfasını fatura uygulamaların her biri, Azure IOT Central için görebileceğiniz yeni bir sekmede açılır.

### <a name="convert-your-trial-to-pay-as-you-go"></a>Deneme sürümünüzü Kullandıkça Öde aboneliğine dönüştürün.

Deneme uygulamanız için bir Kullandıkça Öde uygulama dönüştürebilirsiniz. Bu tür uygulamaları arasındaki farklar aşağıda verilmiştir.

- **Deneme** uygulamaları, süresi dolmadan 7 gün ücretsiz kullanılabilir. Süresi dolmadan önce herhangi bir anda Kullandıkça Öde uygulamasına dönüştürülebilir.
- **Kullandıkça Öde** uygulamaları, ilk 5 cihaz ücretsiz olmak üzere cihaz başına ücretlendirilir.

[Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

Bu Self Servis işlemi tamamlamak için aşağıdaki adımları izleyin:

1. Git **faturalama** sayfasını **Yönetim** bölümü.

    ![Deneme durumu](media/howto-administer/freetrialbilling.png)

1. Seçin **Kullandıkça Öde aboneliğine dönüştürmek**.

    ![Deneme Dönüştür](media/howto-administer/convert.png)

1. Uygun Azure Active Directory seçin ve sonra Azure aboneliği Kullandıkça Öde uygulamanız için kullanılacak.

1. Seçtikten sonra **dönüştürme**, uygulamanız artık Kullandıkça Öde uygulamasıdır ve faturalandırılan başlatın.

## <a name="export-data"></a>Verileri dışarı aktarma

Etkinleştirebilirsiniz **verileri sürekli dışarı aktarma** ölçümleri, cihazları ve cihaz şablonları verileri Azure Blob Depolama hesabınıza aktarmak. Kullanma hakkında daha fazla bilgi edinin [verilerinizi dışarı](howto-export-data.md).

## <a name="manage-device-connection"></a>Cihaz bağlantısını yönetme

Anahtarlar ve sertifikalar buraya kullanarak uygulamanızı bir ölçekte cihazları bağlayın. Daha fazla bilgi edinin [cihazları bağlama](concepts-connectivity.md).

## <a name="use-access-tokens"></a>Erişim belirteçleri kullanma

Geliştirici araçları kullanmaya erişim belirteçleri oluşturun. Şu anda IOT Central Gezgini cihaz iletilerini ve özellikleri ve ayarlarında yapılan değişiklikleri izlemek için bir geliştirici aracı kullanılabilir yoktur. Daha fazla bilgi edinin [IOT Central Gezgini](howto-use-iotc-explorer.md).

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Denetim düzlemi işlemleri için Azure SDK'ları kullanın

IOT Central Azure Resource Manager SDK'sı paketleri, düğüm, Python, C#, Ruby, Java ve Git için kullanılabilir. Oluşturmanızı, Listele, Güncelleştir veya IOT Central uygulamaları silin sağlayan bu kitaplıkları destek denetim düzlemi işlemleri IOT Central için. Kimlik doğrulaması başa çıkmak için de Yardımcıları sağlanır ve hata işleme, her dil için özeldir. 

Azure Resource Manager SDK'ları, kullanma örnekleri bulabilirsiniz [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Daha fazla bilgi için Github'da bu paketleri göz atın.

| Dil | Depo | Paket |
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
