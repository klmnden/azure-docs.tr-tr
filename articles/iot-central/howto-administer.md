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
ms.openlocfilehash: a43febf1e78f80451b6aeed19e095b2c313d3216
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284862"
---
# <a name="how-to-administer-your-application"></a>Uygulamanızı yönetme

Microsoft Azure IOT Central bir uygulama oluşturduktan sonra kullanabileceğiniz **Yönetim** yönetmek için Azure IOT Central kullanıcı arabiriminin bir bölümü. Gidilecek **Yönetim** bölümünde, seçin **Yönetim** sol gezinti menüsünde.

**Yönetim** bölümünde:

- Kullanıcıları yönetme

- Rolleri yönetme

- Fatura bilgileri görüntüleyin

- Uygulama ayarlarını yönet

- Ücretsiz bir deneme süresini uzatın

İçinde **Yönetim** bölüm, çeşitli yönetim görevleri için bağlantılarla birlikte ikincil Gezinti Menüsü.

Erişimi ve kullanımı **Yönetim** bölümünde olmanız gerekir **yönetici** Azure IOT Central uygulamasına rolü. Azure IOT Central bir uygulama oluşturursanız, otomatik olarak atanan **yönetici** o uygulama için rol. *Kullanıcıları yönetme* bölümünde bu makalede açıklanır daha nasıl atanacağı hakkında **yönetici** diğer kullanıcılara rol.

## <a name="change-application-name"></a>Uygulama adını değiştir

Uygulamanızın adını değiştirmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

Üzerinde **uygulama ayarları** ettiğiniz bir ad girin, sayfa **uygulama adı** alan ve ardından **Kaydet**.

## <a name="change-the-application-url"></a>Uygulama URL'sini değiştirme

Uygulamanızın URL'sini değiştirmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

![Uygulama Ayarları sayfası](media\howto-administer\image0-a.png)

Üzerinde **uygulama ayarları** ettiğiniz URL'sini girin **URL** alan ve ardından **Kaydet**. URL'niz, en fazla 200 karakter uzunluğunda olabilir. URL kullanılabilir değilse, bir doğrulama hatası görürsünüz.

> [!Note]
> URL'niz değiştirirseniz, eski URL'nizi başka bir Azure IOT Central müşteri tarafından alınabilir. Bu durumda, artık kullanmak için kullanılabilir. URL'niz değiştirdiğinizde, eski URL artık çalışır ve bunu kullanmak için yeni URL ilgili kullanıcılarınıza bildirmeniz gerekir.

## <a name="change-the-application-image"></a>Değişiklik uygulama görüntüsü

Azure IOT Central bir uygulamada görüntüleri kullanma hakkında daha fazla bilgi için bkz. [hazırlama ve karşıya yükleme görüntüleri, Azure IOT Central uygulamasına](howto-prepare-images.md).

## <a name="copy-an-application"></a>Bir uygulamayı kopyalama

Tüm cihaz örnekleri, cihaz verileri geçmişi ve kullanıcı verileri hariç herhangi bir uygulamanın bir kopyasını oluşturabilirsiniz. Kopyalama için ücret ödersiniz Ücretli bir uygulama olacaktır. Başka bir uygulama kopyalayarak deneme uygulama oluşturulamıyor.

Bir uygulama kopyalamak için gidin **uygulama ayarları** sayfasında ve tıklayın **kopyalama** düğmesi.

![Uygulama Ayarları sayfası](media\howto-administer\appCopy1.png)

Tıklayarak **kopyalama** düğmesi, seçebileceğiniz bir ad, URL, AAD dizini, aboneliği ve Azure bölgesine kopyalama, uygulamanız tarafından oluşturulan yeni bir uygulama için bir iletişim kutusu açılır. Bu alanların her biri için değerleri seçin ve ardından **kopyalama** düğmesini devam etmek istediğinizi onaylayın. Bu değerleri makalesinde hakkında girin gerekenler hakkında daha fazla bilgi [bir uygulamanın nasıl oluşturulacağını](howto-create-application.md).

![Uygulama Ayarları sayfası](media\howto-administer\appCopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra uygulamanız üzerinde görünen bağlantıyı tıklatarak kopyalayarak oluşturulan yeni uygulama gitmek mümkün olmayacak **uygulama ayarları** sayfası.

![Uygulama Ayarları sayfası](media\howto-administer\appCopy3.png)

> [!Note]
> Bir uygulamayı kopyalama kuralları veya Eylemler tanımını kopyalar. Ancak özgün uygulamanıza erişimi olan kullanıcılar için kopyalanan uygulamayı kopyalanmaz olduğundan, kullanıcıları kullanıcılar önkoşul olan e-posta gibi eylemleri el ile eklemeniz gerekir.

## <a name="delete-an-application"></a>Bir uygulamayı Sil

Uygulamanızı silmek için gitmek için ikincil Gezinti Menüsü kullanın **uygulama ayarları** sayfasını **Yönetim** bölümü.

Seçin **Sil**.

> [!Note]
> Bir uygulama modelinizden silmek, uygulama ile ilişkili tüm verileri siler. Bir uygulamayı silmek için de kaynakları silmek için hakları olmalıdır uygulama oluştururken seçtiğiniz Azure aboneliğinde. Daha fazla bilgi için bkz. [Use Role-Based erişim denetimi, Azure abonelik kaynaklarınıza erişimi yönetmek için](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Azure IOT Central rolleri

Rolleri kim, kuruluşunuzun içindeki çeşitli Azure IOT Central görevleri gerçekleştirebilirsiniz için denetimi etkinleştir. Azure IOT Central, uygulamanızın kullanıcılara atayabileceğiniz üç rol yok. Roller, her bir uygulama tarafından atanır. Aynı kullanıcı farklı uygulamalarda farklı rollere sahip olabilir. Aynı kullanıcı can uygulama içinde birden çok rol atayabilirsiniz.

### <a name="administrator"></a>Yönetici

Kullanıcıların **yönetici** rolü Azure IOT Central bir uygulamada tüm işlevlere erişimi vardır.

Uygulama oluşturma kullanıcı otomatik olarak atanır **yönetici** rol. Ayrıca en az bir kullanıcı her zaman olmalıdır **yönetici** rol.

### <a name="application-builder"></a>Uygulama Oluşturucu

Kullanıcıların **uygulama Oluşturucusu** rol bir Azure IOT Central uygulamasına uygulamayı yönetme dışında her şeyi yapabilir.

### <a name="application-operator"></a>Uygulama işleci

Kullanıcıların **uygulama işleci** rol erişmeye değilsiniz **uygulama Oluşturucusu** sayfası. Bunlar, uygulamayı yönetemezsiniz.

## <a name="manage-users"></a>Kullanıcıları yönetme

Uygulama Yöneticileri kullanıcıların uygulama roller atayabilirsiniz.

### <a name="add-users"></a>Kullanıcı ekle

Oturum açın ve bir Azure IOT Central uygulamasına erişmek için önce her kullanıcının bir kullanıcı hesabı olması gerekir. Microsoft Accounts (MSA) ve Azure Active Directory (AAD) hesabı Azure IOT Central desteklenir. Azure Active Directory grupları şu anda Azure IOT Central ' desteklenmez.

Daha fazla bilgi için bkz. [Microsoft hesabı Yardım](https://support.microsoft.com/products/microsoft-account?category=manage-account) ve [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Bir kullanıcı hesabı için bir Azure IOT Central uygulamasına eklemek için gitmek için ikincil Gezinti Menüsü kullanın **kullanıcılar** sayfasını **Yönetim** bölümü.

    ![Kullanıcı listesi](media\howto-administer\image1.png)

1. Üzerinde **kullanıcılar** sayfasında **+ Ekle kullanıcı** kullanıcı eklemek için.

    ![Kullanıcı Ekleme](media\howto-administer\image2.png)

1. Bir kullanıcı Azure IOT Central uygulamanıza eklediğinizde, kullanıcının bir rol seçin **rol** açılır. Roller hakkında daha fazla bilgi *Azure IOT Central rollerinde* bu makalenin.

    ![Rolü seçimi](media\howto-administer\image3.png)

    > [!NOTE]
    >  Toplu olarak kullanıcı eklemek için eklemek istediğiniz tüm kullanıcıları kullanıcı kimliklerini girin noktalı virgülle ayrılmış. Bir rolden seçin **rol** seçin ve açılır **Kaydet**.

1. Bir kullanıcı ekledikten sonra bir giriş o kullanıcı için görünür **kullanıcılar** sayfası.

    ![Kullanıcı listesi](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>Kullanıcılara atanan rollerini Düzenle

Rolleri, bir kez assinged değiştirilemez. Bir kullanıcıya atanan role değiştirmek için kullanıcıyı silin ve yeniden farklı bir rol ile kullanıcı ekleyin.

### <a name="delete-users"></a>Kullanıcıları Sil

Kullanıcıları silmek için üzerinde bir veya daha fazla onay kutularını işaretleyin **kullanıcılar** sayfasında ve sonra **Sil**.

## <a name="view-your-bill"></a>Faturanızı görüntüleyin

Faturanızı görüntülemek için gidin **faturalama** sayfasını **Yönetim** bölümünde ve seçin **faturalama**. Azure faturalandırma sayfasındaki yeni bir sekmede açılır ve uygulamaların her biri, Azure IOT Central için faturada görebilirsiniz.

## <a name="convert-your-trial-to-a-paid-application"></a>Denemenizi Ücretli bir uygulamaya dönüştürme

IOT Central değerlendirdiniz sonra denemenizi Ücretli bir uygulamaya dönüştürme yapabilirsiniz. Bu Self Servis işlemi tamamlamak için aşağıdaki adımları izleyin:

1. İkincil Gezinti menüsünde gezinmek için kullanın **faturalama** sayfasını **Yönetim** bölümü. Deneme süreniz uzatıldıysa henüz sayfa aşağıdaki gibi görünür:

    ![Ücretsiz deneme sürümü durumu](media/howto-administer/freetrial.png)

2. Tıklayın **Ücretli sürüme dönüştürme**. Deneme süreniz uzatıldıysa henüz açılır aşağıdaki gibi görünür:

    Açılır pencerede uygun Azure Active Directory kiracısı'i ve ardından IOT Central uygulamanız için kullanmak istediğiniz Azure aboneliğini seçin.

    ![Ücretsiz deneme süresini uzatın](media/howto-administer/extend.png)

3. Tıkladıktan sonra **dönüştürme**, denemenizi Ücretli bir uygulamaya dönüştürülür ve faturalandırılan başlatın.

## <a name="extend-your-free-trial"></a>Ücretsiz deneme sürenizi uzatın

Varsayılan olarak, tüm ücretsiz denemeleri 7 gün boyunca kullanılabilir. Deneme sürenizi 30 gün artırmak istiyorsanız, aşağıdaki adımları izleyin:

1. İkincil Gezinti menüsünde gezinmek için kullanın **faturalama** sayfasını **Yönetim** bölümü:

1. Tıklayın **deneme süresini uzatın**. Açılır pencerede IOT Central uygulamanız için kullanılacak uygun bir Azure Active Directory kiracısı ve sonra da Azure aboneliği seçin:

1. Ardından **Genişlet**. Deneme süreniz, şimdi 30 gün boyunca geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızı yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz şablonu ayarlama](howto-set-up-template.md)