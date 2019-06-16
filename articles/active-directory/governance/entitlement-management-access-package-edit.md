---
title: Düzenleme ve Azure AD hak yönetimi (Önizleme) - Azure Active Directory içinde var olan erişim paketini yönetme
description: Düzenleme ve Azure Active Directory hak yönetimi (Önizleme) içinde var olan erişim paketini yönetme hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce51f4df50de50cef06bba699ca7c6f76bc5d166
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833237"
---
# <a name="edit-and-manage-an-existing-access-package-in-azure-ad-entitlement-management-preview"></a>Düzenleme ve Azure AD hak yönetimi (Önizleme) içinde var olan erişim paketini yönetme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir erişim paket ömrü boyunca erişim paket erişimi otomatik olarak yönetir kaynaklarının ve ilkeleri tek seferlik Kurulumu yapmanıza olanak sağlar. Bir erişim Paket Yöneticisi olarak, yeni kaynaklarına kullanıcının erişimi sağlama veya önceki kaynaklardan erişimleri kaldırma hakkında endişelenmeden erişim paket kaynakları dilediğiniz zaman değiştirebilirsiniz. İlkeler, dilediğiniz zaman güncelleştirilebilir, ancak ilke değişiklikleri yeni erişim yalnızca etkiler.

Bu makalede, düzenleme ve mevcut erişim paketlerini yönetme açıklar.

## <a name="add-resource-roles"></a>Kaynak rolleri Ekle

Kaynak rolü kaynakla ilişkili izinler koleksiyonudur. Kaynakları istemek kullanıcılar için kullanılabilir hale kaynak rolleri için erişim paketinizi ekleyerek yoludur. Grupları, uygulamaları ve SharePoint siteleri için kaynak rolleri ekleyebilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Sol menüde **kaynak rolleri**.

1. Tıklayın **kaynak rolleri ekleme** paket sayfasına erişmek için kaynak Rol Ekle açın.

    ![Paket erişim - kaynak rolleri Ekle](./media/entitlement-management-access-package-edit/resource-roles-add.png)

1. Bir grup, uygulama veya SharePoint sitesi eklemek isteyip istemediğinizi bağlı olarak, adımları aşağıdaki kaynak rolü bölümleri birini gerçekleştirin.

### <a name="add-a-group-resource-role"></a>Bir Grup Kaynak rolü Ekle

Hak Yönetimi otomatik olarak erişim paket atandığında bir gruba kullanıcı ekleme olabilir. 

- Bir gruba erişim paketi ve bir kullanıcı bir parçası olduğunda, o erişim paket için kullanıcıyı bu gruba eklenir, zaten mevcut değilse atanır.
- Bir kullanıcının erişim paket atamasını süresi dolduğunda, şu anda aynı grubu içeren başka bir erişim paket atama yoksa bunlar grubundan kaldırılır.

Herhangi bir Azure AD güvenlik grubu veya Office 365 grubu seçebilirsiniz.  Yöneticiler, herhangi bir grubu için bir katalog ekleyebilir; grubun sahibi olmaları durumunda Kataloğu sahipleri kataloğa herhangi bir grubu ekleyebilirsiniz. Aşağıdaki Azure AD kısıtlamaları, bir grup seçerken göz önünde bulundurun:

- Bir konuk dahil olmak üzere, bir kullanıcı grubuna üye olarak eklendiğinde, bu grubun diğer tüm üyeleri görebilir.
- Azure AD, Windows Server Active Azure AD Connect kullanarak Directory'den eşitlenen bir grubun üyeliğini değiştiremezsiniz.  
- Dinamik grup üyeliği ekleyerek veya dinamik grup üyeliği, hak yönetimi ile kullanım için uygun değildir. Bu nedenle, üye çıkarılması güncelleştirilemiyor.

1. Üzerinde **paket erişmek için kaynak rolleri ekleme** sayfasında **grupları** grupları bölmesini açmak için.

1. Erişim paket içerisine dâhil etmek istediğiniz grupları seçin.

    ![Paket erişim - kaynak rolleri - grupları Ekle](./media/entitlement-management-access-package-edit/group-select.png)

1. **Seç**'e tıklayın.

1. İçinde **rol** listesinden **sahibi** veya **üye**.

    Genellikle, üye rolü de seçin. Sahip rolü seçerseniz, kullanıcıların diğer üyeleri veya sahipleri ekleme veya kaldırma sağlayacak.

    ![Paket erişim - Kaynak rolü için bir grup ekleme](./media/entitlement-management-access-package-edit/group-role.png)

1. **Ekle**'yi tıklatın.

    Eklendiğinde mevcut atamaları erişim paketine sahip tüm kullanıcılar otomatik olarak bu grubun üyeleri olacak.

### <a name="add-an-application-resource-role"></a>Uygulama kaynak rolü Ekle

Azure AD'ye otomatik olarak kullanıcıların SaaS uygulamaları hem de kullanıcı erişim paket atandığında Azure AD'ye Federasyon, kuruluşunuzun uygulamaları dahil olmak üzere bir Azure AD kuruluş uygulaması için erişim atama olabilir. Federasyon çoklu oturum açma aracılığıyla Azure AD ile tümleştirilen uygulamalar için Federasyon belirteçleri uygulamaya atanmış kullanıcılar için Azure AD vereceğiz.

Uygulamalar birden çok rol sahip olabilir. Bu uygulamanın birden fazla rol varsa bir uygulamaya bir erişim paket eklerken, bu kullanıcılar için uygun rolü belirtmek gerekir.  Uygulamalar geliştiriyorsanız, uygulamalarınıza nasıl makalede bu rolleri hakkında daha fazla sağlanan okuyabilirsiniz [SAML belirtecinde verilen rol talep yapılandırma](../develop/active-directory-enterprise-app-role-management.md).

Bir uygulama rolü erişim paketinin bir parçası olduğunda:

- Bir kullanıcı bu erişimi paket atandığında, kullanıcının bu uygulama rolüne eklenir, zaten mevcut değilse.
- Bir kullanıcının erişim paket atamasını süresi dolduğunda, uygulama rolü içeren başka bir erişim paketi atamaya sahip olmadıkları sürece uygulamadan erişimleri kaldırılacak.

Bir uygulama seçerken bazı noktalar şunlardır:

- Uygulamaları, kendi rolleri için atanan gruplar da olabilir.  Daha sonra uygulama My erişim Portalı'nda erişim paketinin bir parçası olarak kullanıcıya görünür olmaz ancak bir erişim paketinde bir gruba uygulama rolü yerine eklemeyi seçebilirsiniz.

1. Üzerinde **paket erişmek için kaynak rolleri ekleme** sayfasında **uygulamaları** uygulama bölmesini açın.

1. Erişim paket içerisine dâhil etmek istediğiniz uygulamaları seçin.

    ![Paket erişim - kaynak rolleri - uygulama ekleme](./media/entitlement-management-access-package-edit/application-select.png)

1. **Seç**'e tıklayın.

1. İçinde **rol** listesinde, bir uygulama rolü seçin.

    ![Paket erişim - bir uygulama için kaynak rolü ekleme](./media/entitlement-management-access-package-edit/application-role.png)

1. **Ekle**'yi tıklatın.

    Eklendiğinde mevcut atamaları erişim paketine sahip tüm kullanıcılar otomatik olarak erişim için bu uygulamayı verilir.

### <a name="add-a-sharepoint-site-resource-role"></a>Bir SharePoint site Kaynak rolü Ekle

Erişim paket atandığında azure AD'ye otomatik olarak kullanıcılar erişim için bir SharePoint Online sitesi veya SharePoint Online site koleksiyonu atayabilirsiniz.

1. Üzerinde **paket erişmek için kaynak rolleri ekleme** sayfasında **SharePoint siteleri** seçin SharePoint Online siteleri bölmesini açmak için.

1. Erişim paket içerisine dâhil etmek istediğiniz SharePoint Online siteleri seçin.

    ![Paket erişim - kaynak rolleri - seçin SharePoint Online sitesine ekleme](./media/entitlement-management-access-package-edit/sharepoint-site-select.png)

1. **Seç**'e tıklayın.

1. İçinde **rol** listesinde, bir SharePoint Online site rolü seçin.

    ![Paket erişim - bir SharePoint Online sitesi için kaynak rolü ekleme](./media/entitlement-management-access-package-edit/sharepoint-site-role.png)

1. **Ekle**'yi tıklatın.

    Eklendiğinde mevcut atamaları erişim paketine sahip tüm kullanıcılar otomatik olarak erişim için bu SharePoint Online sitesine verilir.

## <a name="remove-resource-roles"></a>Kaynak rolleri kaldırın

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Sol menüde **kaynak rolleri**.

1. Kaynak rolleri listesinde, kaldırmak istediğiniz kaynak rolü bulun.

1. Üç nokta simgesine tıklayın ( **...** ) ve ardından **Kaynak rolü Kaldır**.

    Mevcut atamaları erişim paketine sahip tüm kullanıcılar otomatik olarak bu kaynak rolüne, kaldırıldığında iptal kendi erişebilir.

## <a name="add-a-new-policy"></a>Yeni İlke Ekle

Kimin bir erişim paketini talep edebilir belirttiğiniz şekilde bir ilke oluşturmaktır. Farklı kullanıcı kümeleri için farklı onay ve sona erme ayarları atamaları verilecek izin vermek istiyorsanız, tek bir erişim paketi için birden çok ilke oluşturabilirsiniz. Tek bir ilke, iç ve dış kullanıcılar için aynı erişim paketini atamak için kullanılamaz. Ancak, aynı erişim pakette--iki ilke oluşturabilirsiniz. bir iç kullanıcılar ve dış kullanıcılar için. Bir kullanıcıya uygulanan birden çok ilke varsa, atanacak istediğiniz ilkeyi seçin, isteği zamanında istenir.

Aşağıdaki diyagramda, mevcut bir erişim pakete yönelik bir ilke oluşturmak için üst düzey bir işlem gösterilir.

![Bir ilke işlemi oluşturma](./media/entitlement-management-access-package-edit/policy-process.png)

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **ilkeleri** ardından **ilke Ekle**.

1. Bir ad ve ilke için bir açıklama yazın.

    ![İlke adı ve açıklaması ile oluşturma](./media/entitlement-management-access-package-edit/policy-name-description.png)

1. Seçiminiz için temel **erişim isteğinde bulunabileceği kullanıcılar**, aşağıdaki ilke bölümlerden birine adımları gerçekleştirin.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="edit-an-existing-policy"></a>Var olan bir ilkeyi Düzenle

Herhangi bir zamanda bir ilkeyi düzenleyebilirsiniz. Bir ilke için sona erme tarihi değiştirirseniz, zaten bekleyen bir onay ya da durum onaylanan istekler için sona erme tarihi değiştirmez.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **ilkeleri** ve düzenlemek istediğiniz ilkenin'ye tıklayın.

    **İlke ayrıntıları** sayfanın alt kısmında bölmesi açılır.

    ![Paket erişim - ilke ayrıntıları bölmesi](./media/entitlement-management-access-package-edit/policy-details.png)

1. Tıklayın **Düzenle** ilkeyi düzenlemek için.

    ![Erişim package - Düzenleme İlkesi](./media/entitlement-management-access-package-edit/policy-edit.png)

1. İşiniz bittiğinde tıklayın **güncelleştirme**.

## <a name="directly-assign-a-user"></a>Doğrudan kullanıcı atama

Bazı durumlarda, böylece kullanıcıların erişim paket isteyen işlem yapması gerekmez belirli kullanıcılar için bir erişim paketi doğrudan atamak isteyebilirsiniz. Doğrudan kullanıcılara atamak için erişim paket yönetici doğrudan atamaları izin veren bir ilke olması gerekir.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Sol menüde **atamaları**.

1. Tıklayın **yeni atama** kullanıcı erişim pakete Ekle açın.

    ![Atamaları - ekleme kullanıcıya erişim paket](./media/entitlement-management-access-package-edit/assignments-add-user.png)

1. Tıklayın **kullanıcı ekleme** erişim paketi atamak istediğiniz kullanıcıları seçin.

1. İçinde **seçin, ilke** listesinde, var olan bir ilke seçin [yok (doğrudan atama Yöneticisi yalnızca)](#policy-none-administrator-direct-assignments-only) ayarı.

    Bu erişim paket bu tür bir ilke yoksa tıklayabilirsiniz **yeni ilke Oluştur** eklemek için.

1. Seçili kullanıcılar ataması başlangıç ve bitiş için tarih ve saat olarak ayarlayın. Bitiş tarihi sağlanmazsa, ilkenin sona erme ayarları kullanılacaktır.

1. İsteğe bağlı olarak, doğrudan atama tutulması için bir gerekçe belirtin.

1. Tıklayın **Ekle** seçili kullanıcıların erişim pakete doğrudan atamak için.

    Birkaç dakika sonra tıklayın **Yenile** atamalar listesinde kullanıcıları görmek için.

## <a name="view-who-has-an-assignment"></a>Bir ataması görüntüle

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **atamaları** etkin atamalar listesini görmek için.

1. Belirli bir atama ek ayrıntıları görmek için tıklayın.

1. Tüm kaynak rolleri düzgün bir şekilde sağlanmış yoktu atamaları listesini görmek için filtre durum tıklayıp **dağıtma**.

    Kullanıcının karşılık gelen isteği bulma tarafından teslim hataları hakkında ek ayrıntılar görebilirsiniz **istekleri** sayfası.

1. Süresi dolan atamaları görmek için filtre durum tıklayıp **süresi dolan**.

1. Filtrelenmiş liste bir CSV dosyasını indirmek için tıklayın **indirme**.

## <a name="view-requests"></a>İstekleri görüntüle

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **istekleri**.

1. Ek ayrıntıları görmek için belirli bir istek'i tıklatın.

## <a name="view-a-requests-delivery-errors"></a>Bir isteğin teslim hataları görüntüleyin

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **istekleri**.

1. Görüntülemek istediğiniz isteği seçin.

    İstek, herhangi bir teslim hata varsa, isteği durum olacaktır **Undelivered** ve alt **kısmen teslim**.

    İsteğin ayrıntı bölmesinde teslim hataları varsa, teslim hataların sayısını olacaktır.

1. Sayı tüm isteğin teslim hataları görmek için tıklayın.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Yalnızca, henüz teslim edilmemiş bekleyen bir isteği iptal edebilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Tıklayın **istekleri**.

1. İsteği iptal etmek istediğiniz tıklayın

1. İstek ayrıntıları bölmesinde **iptal isteği**.

## <a name="copy-my-access-portal-link"></a>My erişim portalı bağlantıyı Kopyala

Dizininizdeki çoğu kullanıcının, My erişim portalında oturum açın ve otomatik olarak talep edebilir erişim paketlerin bir listesi görmek. Ancak, henüz dizininizde olmayan dış iş ortağı kullanıcılar için erişim paket isteği için kullanabilecekleri bir bağlantı şirketlerde gerekecektir. Erişim paket dış kullanıcılar için etkindir ve dış kullanıcı dizini için bir ilke sahip olduğu sürece, dış kullanıcı erişim paket isteği için My erişim portalı bağlantısını kullanabilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Genel bakış sayfasında kopyalamak **My erişim portalı bağlantısı**.

    ![Erişim pakete genel bakış - My erişim portalı bağlantısı](./media/entitlement-management-shared/my-access-portal-link.png)

1. E-posta veya dış iş ortağınıza bağlantıyı gönderin. Bağlantıyı erişim paket isteği için kendi kullanıcılarla paylaşabilirler.

## <a name="change-the-hidden-setting"></a>Gizli ayarını değiştirin

Erişim paketler, varsayılan olarak bulunabilir. Bu erişim paket isteği bir kullanıcı bir ilke izin veriyorsa, bunlar otomatik olarak kendi My erişim Portalı'nda listelenen erişim paket göreceği anlamına gelir.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Genel bakış sayfasında tıklayın **Düzenle**.

1. Ayarlama **gizli** ayarı.

    Varsa kümesine **Hayır**, erişim paket kullanıcının My erişim Portalı'nda listelenmez.

    Varsa kümesine **Evet**, erişim paket kullanıcının My erişim Portalı'nda listelenmez. Doğrudan sahip bir kullanıcı, erişim paket görüntüleyebilir tek yol budur **My erişim portalı bağlantısı** erişim paketi.

## <a name="delete"></a>Sil

Etkin kullanıcı ataması yok, erişim paket yalnızca silinebilir.

**Önkoşul rolü:** Kullanıcı Yöneticisi, katalog sahibi veya erişim Paket Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri** ve erişim paketini açın.

1. Sol menüde **atamaları** ve tüm kullanıcıların erişimini kaldırabilirsiniz.

1. Sol menüde **genel bakış** ve ardından **Sil**.

1. Görünür silme iletisi tıklayın **Evet**.

## <a name="when-are-changes-applied"></a>Değişikliklerin ne zaman uygulanır

Hak Yönetimi'nde, günde birkaç kez atama ve erişim paketlerinizi kaynaklarında toplu değişiklikler Azure AD'ye işler. Atama olun veya kaynak rolleri erişim paketinizin değiştirmek, bu değişikliği Azure AD'de yapılacak 24 saate kadar sürebilir için süreyi yanı sıra diğer Microsoft Online Services bu değişiklikleri yaymak için alır veya SaaS uygulamasına bağlı s. Değişikliğinizi birkaç nesneleri etkileyen, değişiklik büyük olasılıkla yalnızca sonra diğer Azure AD bileşenleri daha sonra değiştirmek ve SaaS uygulamaları güncelleştirmek algılar, Azure AD'de uygulamak için birkaç dakika sürer. Değişikliğinizi binlerce nesneyi etkiliyorsa, değişikliğin daha uzun sürer. Örneğin, bir erişim paketiniz 2 uygulamaları ve 100 kullanıcı atamaları varsa ve erişim paketi bir SharePoint site rolü eklemek karar verirseniz, bir gecikme olabilir kadar tüm kullanıcıları, SharePoint site rolünü bir parçasıdır. Azure AD denetim günlüğü, Azure AD sağlama günlüğü ve SharePoint site denetim günlüklerini ilerlemeyi izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [İstek işlemini ve e-posta bildirimleri](entitlement-management-process.md)
