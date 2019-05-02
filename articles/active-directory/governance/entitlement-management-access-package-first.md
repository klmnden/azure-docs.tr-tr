---
title: Öğretici - Azure AD hak yönetimi (Önizleme) - Azure Active Directory içinde ilk erişim paketinizi oluşturmak
description: Azure Active Directory hak yönetimi (Önizleme) ilk erişim paketinizi oluşturmak için adım adım öğretici.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 04/27/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 354af736d5896214848205f41e429d9bf2c49863
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873529"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management-preview"></a>Öğretici: İlk erişim paketinizi Azure AD hak yönetimi (Önizleme) oluşturma

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tüm kaynakları çalışanları gereksinimi, grupları, uygulamalar ve siteler gibi erişimi yönetme, kuruluşların önemli bir işlevdir. Çalışanların üretken ve artık gerekli olmadığında, kişilerin erişimlerini kaldırmanız için ihtiyaç duydukları erişim doğru düzeyini vermek istediğiniz.

Bu öğreticide, bir BT yöneticisi olarak Woodgrove Bankası için çalışır. Bir paketi iç kullanıcıların Self Servis isteği için bir web projesi için kaynakları oluşturmak için olmak istediniz. İstekleri onay gerektirir ve kullanıcının erişim 30 gün sonra sona erer. Bu öğretici için yalnızca tek bir grup üyeliği web projesi kaynaklardır ancak grupları, uygulamaları veya SharePoint Online siteleri koleksiyonunu olabilir.

![Senaryoya genel bakış](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir access paketi ile bir grubu bir kaynak olarak oluşturun.
> * Approver belirleyin
> * Bir iç kullanıcıyı erişim paket nasıl isteyebilir gösterme
> * Erişim isteği Onayla

Bir Azure AD Premium P2 veya Enterprise Mobility + Security E5 lisansı, yoksa, ücretsiz bir oluşturma [Enterprise Mobility + Security E5 deneme sürümü](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1).

## <a name="prerequisites"></a>Önkoşullar

Azure AD hak yönetimi (Önizleme) kullanmak için aşağıdaki lisanslardan birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 lisansı

## <a name="step-1-set-up-users-and-group"></a>1. Adım: Kullanıcılar ve Grup ayarlayın

Kaynak dizini paylaşmak için bir veya daha fazla kaynak var. Bu adımda, oluşturduğunuz adlı bir grubu **mühendislik grubu** hak yönetimi için hedef kaynak Woodgrove Bank dizine. Ayrıca iç bir istek sahibi ayarlarsınız.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

![Kullanıcılar ve gruplar oluşturma](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. Oturum [Azure portalında](https://portal.azure.com) olarak genel yönetici veya Kullanıcı Yöneticisi.  

1. Sol gezinti bölmesinde tıklayın **Azure Active Directory**.

1. Oluşturun veya aşağıdaki iki kullanıcı yapılandırın. Bu adları veya farklı adlar kullanabilirsiniz. **Admin1** şu anda oturum açmaya olarak kullanıcı olabilir.

    | Ad | Dizin rolü | Açıklama |
    | --- | --- | --- |
    | **admin1** | Genel yönetici<br/>-veya-<br/>Sınırlı yönetici (Kullanıcı Yöneticisi) | Yönetici ve onaylayan |
    | **Requestor1** | Kullanıcı | İç istek sahibi |

    Bu öğretici için yönetici ve onaylayan aynı kişidir ancak onaylayanlar olacak şekilde bir veya daha fazla kişi genellikle belirleyin.

1. Bir Azure AD güvenlik oluşturma adlı grubu **mühendislik grubu** üyelik türü olan **atanan**.

    Bu grup, hak yönetimi için hedef kaynak olacaktır. Grup üyeleri başlatmak için boş olmalıdır.

## <a name="step-2-create-an-access-package"></a>2. Adım: Bir erişim paketi oluşturun

Bir *erişim paket* kullanıcı gerekli bir projede çalışmak veya işlerini gerçekleştirmek için tüm kaynaklara paketidir. Erişim paketleri adlı kapsayıcılarda tanımlanmış *katalogları*. Bu adımda, oluşturduğunuz bir **Web projesi erişim paketi** içinde **genel** Kataloğu.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

![Bir erişim paketi oluşturun](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Azure portalında sol gezinti bölmesinde tıklayın **Azure Active Directory**.

1. Sol menüde **Kimlik Yönetimi**

1. Sol menüde **erişim paketleri**.  Görürseniz **erişim reddedildi**, bir Azure AD Premium P2 lisansı bu dizinde mevcut olduğundan emin olun.

1. Tıklayın **yeni erişim paket**.

    ![Azure portalında hak yönetimi](./media/entitlement-management-access-package-first/access-packages-list.png)

1. Üzerinde **Temelleri** sekmesinde, adı **Web projesi erişim paketi** ve açıklama **mühendislik web projesi için paket erişim**.

1. Bırakın **Kataloğu** ayarlamak açılır listede **genel**.

    ![Yeni erişim package - temel sekmesi](./media/entitlement-management-access-package-first/basics.png)

1. Tıklayın **sonraki** açmak için **kaynak rolleri** sekmesi.

    Bu sekmede, erişim paket içerisine dâhil etmek izinleri seçin.

1. tıklayın **grupları**.

1. Grupları bölmesinde bulun ve seçin **mühendislik grubu** daha önce oluşturduğunuz grup.

    Varsayılan olarak, iç ve dış gruplar gördüğünüz **genel** Kataloğu. Dışında bir grubu seçtiğinizde **genel** Kataloğu'na da eklenecektir **genel** Kataloğu.

    ![Yeni erişim package - kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Tıklayın **seçin** grup listesine eklenecek.

1. İçinde **rol** aşağı açılan listesinden **üye**.

    ![Yeni erişim package - kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles.png)

1. Tıklayın **sonraki** açmak için **ilke** sekmesi.

1. Ayarlama **ilk ilkesi oluşturma** geç **sonra**.

    Sonraki bölümde ilkesi oluşturur.

    ![Yeni erişim package - ilke sekmesi](./media/entitlement-management-access-package-first/policy.png)

1. Tıklayın **sonraki** açmak için **gözden geçir + Oluştur** sekmesi.

    ![Yeni erişim package - gözden geçir + sekme oluşturma](./media/entitlement-management-access-package-first/review-create.png)

1. Erişim paket ayarlarını gözden geçirin ve ardından **Oluştur**.

    Katalog henüz etkinleştirilmediğinden erişim paket kullanıcılara görünür olmaz, bir ileti görebilirsiniz.

    ![Yeni erişim package - görünür değil iletisi](./media/entitlement-management-access-package-first/not-visible.png)

1. **Tamam** düğmesine tıklayın.

    Birkaç dakika sonra erişim paket başarıyla oluşturulmuş bir bildirim görmeniz gerekir.

## <a name="step-3-create-a-policy"></a>3. Adım: İlke oluştur

A *ilke* kuralları veya erişim pakete guardrails tanımlar. Bu adımda, kaynak dizininde erişim paket isteği için belirli bir kullanıcıya izin veren bir ilke oluşturun. Ayrıca isteklerinin onaylanması gerekir ve kimin onaylayan olacağını belirtin.

![Paket erişim ilkesi oluşturma](./media/entitlement-management-access-package-first/elm-access-package-policy.png)

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. İçinde **Web projesi erişim paketi**, sol menüde **ilkeleri**.

    ![Erişim paket ilkeleri listesi](./media/entitlement-management-access-package-first/policies-list.png)

1. Tıklayın **ilke Ekle** Oluştur İlkesi'ni açmak için.

1. Adı **iç istek sahibine ilke** ve açıklama **web projesi kaynaklarına erişimi istemek için bu dizindeki kullanıcılara**.

1. İçinde **erişim isteğinde bulunabileceği kullanıcılar** bölümünde **dizininizdeki kullanıcılar için**.

    ![İlke oluşturma](./media/entitlement-management-access-package-first/policy-create.png)

1. Ekranı aşağı kaydırarak **kullanıcıları ve grupları seçin** 'ye tıklayın **kullanıcılar ve gruplar ekleme**.

1. Belirli kullanıcılar ve grupları bölmesinde seçin **Requestor1** kullanıcı daha önce oluşturduğunuz ve ardından **seçin**.

1. İçinde **istek** bölümünde, **onayı iste** için **Evet**.

1. İçinde **onaylayanları seçin** bölümünde **onaylayan Ekle**.

1. Select onaylayanlar bölmesinde seçin **Admin1** daha önce oluşturduğunuz ve ardından **seçin**.

    Bu öğretici için yönetici ve onaylayan aynı kişidir ancak onaylayan olarak başka bir kişiye atayabilirsiniz.

1. İçinde **sona erme** bölümünde, **erişim paket süresi** için **gün sayısı**.

1. Ayarlama **erişim süresi dolduktan sonra** için **30** gün.

1. İçin **ilkesini etkinleştir**, tıklayın **Evet**.

    ![İlke ayarları oluşturma](./media/entitlement-management-access-package-first/policy-create-settings.png)

1. Tıklayın **Oluştur** oluşturmak için **iç istek sahibine ilke**.

1. Web projesi erişim paketi sol menüde **genel bakış**.

1. Kopyalama **My erişim portalı bağlantısı**.

    Bu bağlantı bir sonraki adımda kullanacaksınız.

    ![Erişim pakete genel bakış - My erişim portalı bağlantısı](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-4-request-access"></a>4. Adım: Erişim izni iste

Bu adımda gerçekleştirdiğiniz adımları **iç istek sahibi** ve erişim paketine erişim isteyin. İstek sahipleri isteklerinde My erişim portalı adlı bir sitede kullanarak gönderin. My erişim portalı erişim paketleri için istekler, bunlar zaten erişimi ve onların istek geçmişini görüntüleme erişimi paketleri Bkz istek sahipleri sağlar.

**Önkoşul rolü:** İç istek sahibi

1. Azure portalında oturum açın.

1. Yeni bir tarayıcı penceresinde önceki adımda kopyaladığınız My erişim portalı bağlantısına gidin.

1. My erişim portalında oturum açın **Requestor1**.

    Görmelisiniz **Web projesi erişim paketi**.

1. Gerekli olursa **açıklama** sütuna, ayrıntıları erişim paketi hakkında görüntülemek için oka tıklayın.

    ![My erişim portalı - erişim paketleri](./media/entitlement-management-shared/my-access-access-packages.png)

1. Paketi seçmek için onay işaretine tıklayın.

1. Tıklayın **erişim isteği** isteği erişim bölmesini açmak için.

1. İçinde **İş Gerekçesi** gerekçe yazın **web proje üzerinde çalışan**.

1. Ayarlama **belirli bir süre için istek** geç **Evet**.

1. Ayarlama **başlangıç tarihi** için bugün ve **bitiş tarihi** yarın için.

    ![My erişim portalı - erişim isteği](./media/entitlement-management-shared/my-access-request-access.png)

1. **Gönder**'e tıklayın.

1. Sol menüde **istek geçmişi** isteğiniz gönderildi doğrulayın.

## <a name="step-5-approve-access-request"></a>5. Adım: Erişim isteği Onayla

Bu adımda, olarak oturum **onaylayan** kullanıcı ve iç istek sahibi erişim isteğini onaylayın. İstek sahipleri istek göndermek için kullanabilir onaylayanları aynı My erişim portalı kullanın. My erişim portalı kullanarak, onaylayanlar onayları görüntüleyin ve onaylayabilir veya istekleri reddetme.

**Önkoşul rolü:** Onaylayan

1. My erişim portalında oturum açın.

1. Oturum [My erişim portalı](https://myaccess.microsoft.com) olarak **Admin1**.

1. Sol menüde **onaylar**.

1. Üzerinde **bekleyen** sekmesinde, bulmak **Requestor1**.

    Requestor1 istekten görmüyorsanız, birkaç dakika bekleyin ve yeniden deneyin.

1. Tıklayın **görünümü** erişim isteği bölmesini açmak için bağlantı.

1. Tıklayın **onaylama**.

1. İçinde **neden** nedenini yazın **erişim web projesi için onaylanmış**.

    ![My erişim portalı - erişim isteği](./media/entitlement-management-shared/my-access-approve-request.png)

1. Tıklayın **Gönder** kararınız göndermek için.

    Başarıyla onaylandığı bir ileti görürsünüz.

## <a name="step-6-validate-that-access-has-been-assigned"></a>6. Adım: Erişim atanmış olduğunu doğrulayın

Erişim isteği, bu adımda, onaylanmış olduğunuza emin olun **iç istek sahibi** erişim atanan paket ve artık üyesi oldukları **mühendislik grubu** grubu.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. My erişim portalında oturum açın.

1. Oturum [Azure portalında](https://portal.azure.com) olarak **Admin1**.

1. Tıklayın **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri**.

1. Bulun ve tıklatın **Web projesi erişim paketi**.

1. Sol menüde **istekleri**.

    Requestor1 ve durumu ile iç istek sahibine ilke görmelisiniz **teslim edildi**.

1. İstek, istek ayrıntılarını görmek için tıklayın.

    ![Erişim package - İstek Ayrıntıları](./media/entitlement-management-access-package-first/request-details.png)

1. Sol gezinti bölmesinde tıklayın **Azure Active Directory**.

1. Tıklayın **grupları** açın **mühendislik grubu** grubu.

1. Tıklayın **üyeleri**.

    Görmelisiniz **Requestor1** bir üyesi olarak listelenir.

    ![Mühendislik grubu üyeleri](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-7-clean-up-resources"></a>7. Adım: Kaynakları temizleme

Bu adımda, yapılan değişiklikleri kaldırın ve Sil **Web projesi erişim paketi** erişim paket.

**Önkoşul rolü:**  Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Açık **Web projesi erişim paketi**.

1. Tıklayın **atamaları**.

1. İçin **Requestor1**, üç noktaya tıklayın (**...** ) ve ardından **erişimi kaldırmak**.

    Durum teslim edildi süresi doldu olarak değişir.

1. Tıklayın **ilkeleri**.

1. İçin **iç istek sahibine ilke**, üç noktaya tıklayın (**...** ) ve ardından **Sil**.

1. Tıklayın **kaynak rolleri**.

1. İçin **mühendislik grubu**, üç noktaya tıklayın (**...** ) ve ardından **Kaynak rolü Kaldır**.

1. Erişim paketlerin listesini açın.

1. İçin **Web erişim projeler**, üç noktaya tıklayın (**...** ) ve ardından **Sil**.

1. Azure Active Directory'ye oluşturduğunuz gibi herhangi bir kullanıcı silme **Requestor1** ve **Admin1**.

1. Silme **mühendislik grubu** grubu.

## <a name="next-steps"></a>Sonraki adımlar

Hak Yönetimi'nde yaygın senaryo adımları hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Yaygın senaryolar](entitlement-management-scenarios.md)
