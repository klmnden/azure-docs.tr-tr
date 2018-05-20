---
title: Azure IOT merkezi bir uygulamayı yönetme | Microsoft Docs
description: Bir yönetici olarak Azure IOT merkezi uygulamanızı yönetme
services: iot-central
author: TanmayBhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: b60b9e851a3b6612964e67e7764ad8d43d606b4e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-administer-your-application"></a>Uygulamanızı yönetme

Microsoft Azure IOT merkezi bir uygulama oluşturduktan sonra kullanabileceğiniz **Yönetim** yönetmek için Azure IOT merkezi kullanıcı arabirimi bölümü. Gitmek için **Yönetim** bölümünde, seçin **Yönetim** sol gezinti menüsünde.

**Yönetim** bölüm olanak sağlar:

- Kullanıcıları yönet

- Rolleri yönetme

- Faturalama bilgileri görüntüleyin

- Uygulama ayarlarını yönet

- Ücretsiz deneme genişletme

İçinde **Yönetim** bölümünde, çeşitli yönetim görevlerini bağlantılarla ikincil Gezinti Menüsü yoktur.

Erişmek ve kullanmak üzere **Yönetim** bölümünde olmalısınız içinde **yönetici** Azure IOT merkezi uygulamasında rol. Azure IOT merkezi bir uygulama oluşturursanız, otomatik olarak atanır **yönetici** rolü bu uygulama için. *Kullanıcıları yönetme* bu makalenin bölümünde açıklanmaktadır daha atama hakkında **yönetici** diğer kullanıcılara rol.

## <a name="change-application-name"></a>Uygulama adı değiştir

Uygulamanızın adını değiştirmek için gitmek için ikincil Gezinti menüsünde kullanın **uygulama ayarları** sayfasındaki **Yönetim** bölümü.

Üzerinde **uygulama ayarları** sayfasında, tercih ettiğiniz bir ad girin **uygulama adı** alan ve ardından **kaydetmek**.

## <a name="change-the-application-url"></a>Uygulama URL'si değiştirme

Uygulamanızı URL'sini değiştirmek için gitmek için ikincil Gezinti menüsünde kullanın **uygulama ayarları** sayfasındaki **Yönetim** bölümü.

![Uygulama Ayarları sayfası](media\howto-administer\image0-a.png)

Üzerinde **uygulama ayarları** sayfasında, tercih ettiğiniz URL'sini girin **URL** alan ve ardından **kaydetmek**. URL'niz, en fazla 200 karakter uzunluğunda olabilir. URL kullanılabilir durumda değilse, bir doğrulama hatası bakın

> [!Note]
> URL'niz değiştirirseniz, eski URL'nizi başka bir Azure IOT merkezi müşteri tarafından alınabilir. Bu durumda, artık kullanmak için kullanılabilir. URL'niz değiştirdiğinizde, eski URL artık çalışır ve kullanmak için yeni URL ilgili kullanıcılarınıza bildirmeniz gerekir.

## <a name="change-the-application-image"></a>Değişiklik uygulama görüntüsü

Azure IOT merkezi bir uygulamada görüntüleri kullanma hakkında daha fazla bilgi için bkz: [hazırlama ve karşıya yükleme görüntüleri Azure IOT merkezi uygulamanıza](howto-prepare-images.md).

## <a name="delete-an-application"></a>Bir uygulamayı Sil

Uygulamanızı silmek için gitmek için ikincil Gezinti menüsünde kullanın **uygulama ayarları** sayfasındaki **Yönetim** bölümü.

Seçin **silmek**.

> [!Note]
> Bir uygulama modelinizden silmek uygulama ile ilişkili tüm verileri siler. Bir uygulamayı silmek için de kaynaklarını silmek için hakları olmalıdır uygulama oluştururken seçtiğiniz Azure aboneliğinde. Daha fazla bilgi için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek üzere Use Role-Based erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Azure IOT Orta rollerinde

Roller, kuruluşunuz içinde çeşitli Azure IOT yönetim görevlerini gerçekleştirebilir kimin için etkinleştirin. Azure IOT Orta uygulamanızı kullanıcılara atayabileceğiniz üç rol yok. Roller, her uygulama tarafından atanır. Aynı kullanıcı farklı rolleri farklı uygulamalara sahip olabilir. Aynı kullanıcı can uygulama içinde birden çok rol atayabilirsiniz.

### <a name="administrator"></a>Yönetici

Kullanıcılar **yönetici** rol Azure IOT merkezi bir uygulamada tüm işlevlere erişimi vardır.

Bir uygulama oluşturmaya kullanıcı otomatik olarak atanır **yönetici** rol. Ayrıca en az bir kullanıcı her zaman olmalıdır **yönetici** rol.

### <a name="application-builder"></a>Uygulama Oluşturucu

Kullanıcılar **uygulama Oluşturucu** rol uygulamayı yönetme dışında bir Azure IOT merkezi uygulamadaki her şeyi yapabilir.

### <a name="application-operator"></a>Uygulama işleci

Kullanıcılar **uygulama işleci** rol için erişim değilsiniz **uygulama Oluşturucu** sayfası. Bunlar uygulama yönetmek için kullanılamaz.

## <a name="manage-users"></a>Kullanıcıları yönet

Uygulama Yöneticileri kullanıcıların uygulama roller atayabilirsiniz.

### <a name="add-users"></a>Kullanıcı ekle

Oturum açma ve Azure IOT merkezi bir uygulamaya erişim önce her kullanıcı bir kullanıcı hesabı olmalıdır. Microsoft Accounts (MSA) ve Azure Active Directory (AAD) hesapları Azure IOT merkezi desteklenir. Azure Active Directory grupları şu anda Azure IOT Merkezi içinde desteklenmez.

Daha fazla bilgi için bkz: [Microsoft hesabı Yardım](https://support.microsoft.com/products/microsoft-account?category=manage-account) ve [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Azure IOT merkezi bir uygulama bir kullanıcı hesabı eklemek için gitmek için ikincil Gezinti menüsünde kullanın **kullanıcılar** sayfasındaki **Yönetim** bölümü.

    ![Kullanıcı listesi](media\howto-administer\image1.png)

1. Üzerinde **kullanıcılar** sayfasında, **+ Ekle kullanıcı** bir kullanıcı eklemek için.

    ![Kullanıcı Ekle](media\howto-administer\image2.png)

1. Azure IOT merkezi uygulamanıza bir kullanıcı eklemek için kullanıcıdan bir rol seçin **rol** açılır. Rolleri hakkında daha fazla bilgi edinin *Azure IOT merkezi rollerinde* bu makalenin.

    ![Rolü seçimi](media\howto-administer\image3.png)

    > [!NOTE]
    >  Toplu olarak kullanıcı eklemek için eklemek istediğiniz tüm kullanıcıların kullanıcı kimliklerini girin noktalı virgülle ayrılmış. Bir rol seçin **rol** açılır ve seçin **kaydetmek**.

1. Bir kullanıcı ekledikten sonra bir giriş o kullanıcı için görünür **kullanıcılar** sayfası.

    ![Kullanıcı listesi](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>Kullanıcılara atanan rollerini Düzenle

Roller, bir kez assinged değiştirilemez. Bir kullanıcıya atanmış rolünü değiştirmek için kullanıcı silin ve farklı bir rol kullanıcıyla yeniden ekleyin.

### <a name="delete-users"></a>Kullanıcıları silme

Kullanıcı silmek için bir veya daha fazla onay kutularını kontrol **kullanıcılar** sayfasında ve ardından **silmek**.

## <a name="view-your-bill"></a>Faturanızı görüntülemek

Faturanızı görüntülemek için gidin **faturalama** sayfasındaki **Yönetim** bölümünde ve seçin **faturalama**. Azure faturalama sayfasına yeni bir pencerede açılır ve Azure IOT merkezi uygulamaların her biri için fatura görebilirsiniz.

## <a name="convert-your-trial-to-a-paid-application"></a>Denemenizi Ücretli bir uygulamaya Dönüştür

IOT Orta hesaplanan sonra denemenizi Ücretli bir uygulamaya dönüştürebilirsiniz. Bu Self Servis işlemi tamamlamak için aşağıdaki adımları izleyin:

1. İkincil Gezinti menüsünde gezinmek için kullanın **faturalama** sayfasındaki **Yönetim** bölümü. Deneme sürümünüzü genişletilmiş yapmadıysanız, sayfa aşağıdaki gibi görünür:

    ![Ücretsiz deneme durumu](media/howto-administer/freetrial.png)

2. Tıklatın **için ücretli Dönüştür**. Deneme sürümünüzü genişlettiyseniz henüz açılır aşağıdaki gibi görünür:

    Açılır pencerede uygun Azure Active Directory kiracısı ve IOT Orta uygulamanız için kullanmak istediğiniz Azure aboneliği seçin.

    ![Ücretsiz deneme genişletme](media/howto-administer/extend.png)

3. Tıklattıktan sonra **Dönüştür**, denemenizi Ücretli bir uygulamaya dönüştürülür ve fatura başlatın.

## <a name="extend-your-free-trial"></a>Ücretsiz denemenizi genişletme

Varsayılan olarak, tüm ücretsiz deneme 7 gün için kullanılabilir. Deneme sürenizi 30 gün artırmak istiyorsanız şu adımları izleyin:

1. İkincil Gezinti menüsünde gezinmek için kullanın **faturalama** sayfasındaki **Yönetim** bölümü:

1. Tıklatın **deneme uzatmak**. Açılır pencerede IOT Orta uygulamanız için kullanılacak uygun Azure Active Directory Kiracı ve sonra da Azure aboneliğini seçin:

1. Ardından **Genişlet**. Deneme sürümünüzü şimdi 30 gün boyunca geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızı yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz şablonu ayarlayın](howto-set-up-template.md)