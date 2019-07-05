---
title: Bir Microsoft Müşteri sözleşmesi - Azure faturalandırma hesabınızı ayarlama
description: Fatura hesabınızı bir Microsoft Müşteri sözleşmesi ayarlama konusunda bilgi edinin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 947bfe85d94a5d11eeb54bd6b24c4c515af024d4
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490687"
---
# <a name="set-up-your-billing-account-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için fatura hesabınızı ayarlama

Kurumsal Anlaşma kaydınıza süresi doldu veya süresi için kayıt yenilemek için bir Microsoft Müşteri sözleşmesi oturum olur. Bu makale, mevcut fatura Kurulumdan sonra yapılan değişiklikleri açıklar ve yeni fatura hesabınıza ayarlama işleminde size yol gösterir. Yenileme, aşağıdaki adımları içerir:

1. Yeni Microsoft Müşteri Sözleşmesi'ni kabul edin. Microsoft alan temsilcinizle ayrıntılarını anlamak ve yeni sözleşme kabul edecek şekilde çalışır.
2. Yeni Microsoft Müşteri sözleşmesi için oluşturulan yeni Faturalama hesabı ayarlayın.

Fatura hesabı ayarlamak için Azure aboneliğinin faturalandırma yeni hesaba Kurumsal Anlaşma kaydınıza geçiş gerekir. Kurulum, aboneliklerinizdeki çalışan Azure Hizmetleri etkisi yoktur. Bununla birlikte, faturalandırma için aboneliklerinizi yöneteceksiniz şeklini değiştirir.

- Yerine [EA portal](https://ea.azure.com), Azure hizmetlerinizi ve faturalandırma, içinde yöneteceksiniz [Azure portalında](https://portal.azure.com).
- Ücretleriniz için aylık, dijital bir fatura alırsınız. Görüntüleyebilir ve Azure maliyet Yönetimi + faturalandırma sayfasını faturaya analiz edin.
- Departmanlar ve Kurumsal Anlaşma kaydınıza hesabında yerine, yönetmek ve fatura düzenlemek için fatura yapısı ve yeni hesabı kapsamlardan kullanacaksınız.

Kuruluma başlamadan önce şunları öneririz:

- **Yeni Fatura hesabınıza anlama**
  - Yeni hesabınıza kuruluşunuz için faturalandırma basitleştirir. [Yeni Fatura hesabınıza hızlı bir genel bakış](billing-mca-overview.md)
- **Kurulumu tamamlamak için erişiminizi doğrulayın**
  - Yalnızca belirli yönetim izinlerine sahip kullanıcılar, Kurulum tamamlayabilirsiniz. Olup olmadığını denetlemek [Kurulumu tamamlamak için gerekli erişimle](#access-required-to-complete-the-setup).
- **Değişiklikler fatura hiyerarşinize anlama**
  - Yeni hesabı faturalama, Kurumsal Anlaşma kaydınıza farklı şekilde düzenlenmiştir. [Fatura hiyerarşinize yeni hesabı değişiklikleri anlamak](#understand-changes-to-your-billing-hierarchy).
- **Değişiklikler, faturalama yöneticileri erişimi anlama**
  - Kurumsal Anlaşma kaydınıza yöneticilerini yeni hesabın fatura kapsamlarda erişim elde edin. [Anlamak erişimleri değişiklikleri](#changes-to-billing-administrator-access).
- **Yeni hesap tarafından değiştirilir görünümü Kurumsal Anlaşma özellikleri**
  - Yeni hesap özellikleri yerine Kurumsal Anlaşma kaydınıza özelliklerini görüntüleyin.
- **En sık sorulan soruların yanıtlarını görüntüle**
  - Görünüm [ek bilgi](#additional-information) kurulumu hakkında daha fazla bilgi için.

## <a name="access-required-to-complete-the-setup"></a>Kurulumu tamamlamak için gereken erişim

Kurulumu tamamlamak için aşağıdaki erişim gerekir:

- Microsoft Müşteri sözleşmesi imzalandığında oluşturduğunuz fatura profilinin sahibi. Fatura profilleri hakkında daha fazla bilgi için bkz: [fatura profillerini anlayabilir](billing-mca-overview.md#billing-profiles).

- Kuruluş Yöneticisi otomatik olarak yenilenir kayıt.

### <a name="if-youre-not-an-enterprise-administrator-on-the-enrollment"></a>Kayıt bir kuruluş yöneticisi değilseniz

Fatura hesabınızın kurulumunu tamamlamak için kayıt Kurumsal yöneticileri isteyebilir.

1. Microsoft Müşteri sözleşmesi imzalandığında size gönderilen e-posta bağlantıyı kullanarak Azure portalında oturum açın.

2. Kuruluşunuzda başkasının sözleşmesi imzalı veya e-posta yoksa, aşağıdaki bağlantıyı kullanarak oturum açın. Değiştirin **enrollmentNumber** kayıt sayısıyla Kurumsal anlaşmanızı yenilendi.

   `https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber`

3. İstek göndermek istediğiniz kuruluş yöneticileri'ni seçin.

   ![Kuruluş yöneticilerinin davet gösteren ekran görüntüsü](./media/billing-mca-setup-account/ea-mca-invite-admins.png)

4. Seçin **gönderme isteği**.

   Yöneticiler Kurulumu tamamlamak için yönergeleri içeren bir e-posta alırsınız.

### <a name="if-youre-not-an-owner-of-the-billing-profile"></a>Faturalandırma profili sahibi değilseniz

Kuruluşunuzdaki kullanıcı fatura profilindeki sahibi olarak kim Microsoft Müşteri sözleşmesi imzalı eklenir. Böylece Kurulumu tamamlamak bir sahip olarak eklemek için Kullanıcı isteği.  <!-- Todo Are there any next steps -->

## <a name="understand-changes-to-your-billing-hierarchy"></a>Değişiklikler fatura hiyerarşinize anlama

Yeni faturalandırma hesabınız faturalandırma için Gelişmiş faturalandırma ve maliyet yönetimi özellikleri sağlayarak kuruluşunuzun basitleştirir. Aşağıdaki diyagramda, faturalama yeni faturalandırma hesabında nasıl düzenlendiği açıklanmaktadır.

![Ea-mca-post-geçiş-hiyerarşi görüntüsü](./media/billing-mca-setup-account/mca-post-transition-hierarchy.png)

1. Faturalama hesabı, Microsoft Müşteri sözleşmenizi için faturalandırmayı yönetmek için kullanın. Fatura hesabı hakkında daha fazla bilgi edinmek için [Faturalama hesabı anlamak](billing-mca-overview.md#your-billing-account).
2. Kuruluşunuzda, Kurumsal Anlaşma kaydınıza benzer faturalandırmayı yönetmek için faturalandırma profili kullanın. Kurumsal Yöneticiler fatura profilinin sahipleri haline gelir. Fatura profilleri hakkında daha fazla bilgi için bkz: [fatura profillerini anlayabilir](billing-mca-overview.md#billing-profiles).
3. Kurumsal Anlaşma kaydınıza departmanlara benzeyen, gereksinimlerinize göre maliyetlerinizi düzenlemek için bir fatura bölümü kullanın. Fatura bölümler bölüm haline gelir ve departman yöneticilerinin sahipleri ilgili fatura bölümlerin olur. Fatura bölümleri hakkında daha fazla bilgi için bkz: [fatura bölümleri anlamak](billing-mca-overview.md#invoice-sections).
4. Kurumsal anlaşmanızı oluşturulan hesapları yeni faturalandırma hesabında desteklenmiyor. Hesap aboneliklerini departmanı için ilgili fatura bölümüne ait. Hesap sahipleri oluşturabilir ve fatura bölümlerinin aboneliklerini yönetin.

## <a name="changes-to-billing-administrator-access"></a>Yönetici erişimi faturalandırma değişiklikleri

Erişimleri bağlı olarak, Kurumsal Anlaşma kaydınıza üzerinde faturalama yöneticileri fatura kapsamlar yeni hesap erişim elde edin. Aşağıdaki tabloda Kurulum sırasında değişiklik erişim açıklanmaktadır:

| Mevcut bir rolü | POST geçiş rolü |
| --- | --- |
| **Kuruluş Yöneticisi (okuma yalnızca = Hayır)** | **-Fatura Profil sahibi** </br> Fatura profilinde her şeyi yönetme </br> - **Fatura bölümüne sahip tüm fatura bölümleri** </br> Fatura bölümlerde her şeyi yönetme |
| **Kuruluş Yöneticisi (okuma Evet yalnızca =)** | **-Fatura profil okuyucusu** </br> Fatura hesabı üzerinde salt okunur görünümü her şeyin</br>**-Tüm Fatura bölümüne fatura bölüm okuyucusu**</br> Fatura bölümündeki her şeyin salt okunur görünümü|
| **Departman Yöneticisi (okuma yalnızca = Hayır)** |**-İlgili departmanı için oluşturulan fatura bölümünde fatura bölüm sahibi** </br>Fatura bölümündeki her şeyi yönetme|
| **Departman Yöneticisi (okuma Evet yalnızca =)**|**-İlgili departmanı için oluşturulan fatura bölümünde fatura bölüm okuyucusu**</br> Fatura bölümündeki her şeyin salt okunur görünümü|
| **Hesap sahibi** | **-İlgili departmanı için oluşturulan fatura bölümünde azure aboneliği Oluşturucusu** </br>  Azure abonelikleri için fatura bölümü oluşturma|

Azure Active Directory kiracısı, Microsoft Müşteri sözleşmesi imzalanırken yeni fatura hesap için seçilir. Kuruluşunuz için bir kiracı yoksa, yeni bir kiracı oluşturulur. Kiracı, kuruluşunuzun Azure Active Directory içinde temsil eder. Kuruluşunuzdaki genel Kiracı yöneticileri Kiracı uygulama ve kuruluşunuzda veri erişimi yönetmek için kullanın.

Yeni hesabınıza yalnızca Microsoft Müşteri sözleşmesi imzalanırken seçilen Kiracı kullanıcıları destekler. Kurumsal sözleşmeniz üzerinde yönetici iznine sahip kullanıcılar Kiracı parçası ise bunlar Kurulum sırasında yeni faturalandırma hesaba erişim kazanırsınız. Kiracı parçası değillerse, davet sürece yeni bir fatura hesabınıza erişmek mümkün olmayacaktır.

Kullanıcıları davet ederken, kiracınıza Konuk kullanıcı olarak eklenir ve fatura hesap erişin. Kullanıcıları davet etmek için Konuk erişimi Kiracı için açık olması gerekir. Daha fazla bilgi için [Konuk erişimi Azure Active Directory'de](https://docs.microsoft.com/microsoftteams/teams-dependencies#control-guest-access-in-azure-active-directory). Konuk erişimine kapatılırsa, açmak kiracınızın genel yönetici ile iletişime geçin. <!-- Todo - How can they find their global administrator -->

## <a name="view-replaced-features"></a>Değiştirilen özelliklerini görüntüleyin

Aşağıdaki Kurumsal anlaşmanın özellikleri varsayılan olarak, bir Microsoft Müşteri sözleşmesi fatura hesabındaki yeni özellikler ile değiştirilir.

### <a name="enterprise-agreement-accounts"></a>Kurumsal Anlaşma hesapları

Kurumsal Anlaşma kaydınıza oluşturulan hesapları yeni faturalandırma hesabında desteklenmiyor. İlgili departmanı için oluşturulan fatura bölümüne hesabın aboneliklerini ait. Hesap sahipleri Azure aboneliği creators haline gelir ve oluşturabilir ve fatura bölümlerinin aboneliklerini yönetin.

### <a name="notification-contacts"></a>Bildirim ilgili kişileri

Bildirim ilgili kişileri Azure Kurumsal anlaşmasına hakkında iletişim e-posta gönderilir. Yeni faturalandırma hesabında desteklenmez. Azure KREDİLERİ ve faturalar hakkında e-posta, fatura hesabınıza erişim fatura profillerine sahip kullanıcılara gönderilir.

### <a name="spending-quotas"></a>Harcama kotası

Kurumsal Anlaşma kaydınıza departmanları için ayarladığınız harcama kotalarını yeni fatura hesabındaki bütçe ile değiştirilir. Bütçe her harcama kotası kaydınız departmanlara ayarlamak için oluşturulur. Bütçe hakkında daha fazla bilgi için bkz. [oluşturup Azure bütçelerini yönetmek](../cost-management/manage-budgets.md).

### <a name="cost-centers"></a>Maliyet merkezi

Kurumsal Anlaşma kaydınıza Azure abonelikleri üzerinde ayarlanan maliyet merkezi bir denetim yapılmaz yeni fatura hesap. Bununla birlikte, maliyet merkezlerinin Departmanlar ve Kurumsal Anlaşma hesapları için desteklenmez.

## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümlerde, fatura hesabınızı ayarlama hakkında ek bilgi sağlar.

### <a name="no-service-downtime"></a>Hizmet kapalı kalma süresi olmadan

Aboneliğinizdeki Azure hizmetleri kesintisiz olarak çalışmaya devam eder. Yalnızca Azure aboneliklerinizin faturalama ilişkisini geçiririz. Mevcut kaynaklar, kaynak grupları veya yönetim grupları üzerinde herhangi bir etki söz konusu olmayacaktır.

### <a name="user-access-to-azure-resources"></a>Azure kaynaklarına kullanıcının erişimi

Azure RBAC (rol tabanlı erişim denetimi) ile ayarlanmış olan Azure kaynaklarına erişimi, geçiş sırasında etkilenmez.

### <a name="azure-reservations"></a>Azure ayırmalar

Kurumsal Anlaşma kaydınıza içinde Azure rezervasyon yeni fatura hesabınıza taşınır. Geçiş sırasında, aboneliklerinizde uygulanan rezervasyon indirimlerinde herhangi bir değişiklik olmaz.

### <a name="azure-marketplace-products"></a>Azure Market ürünleri

Azure Marketi ürünlerden Kurumsal Anlaşma kaydınıza abonelikleri birlikte taşınır. Geçiş sırasında Market ürünlerin hizmet erişimi herhangi bir değişiklik olmayacak.

### <a name="support-plan"></a>Destek planı

Destek avantajları, geçişin bir parçası olarak aktarılmıyor. İçin yeni faturalandırma hesabınızdaki Azure abonelik avantajlarından yararlanabilmek için yeni bir destek planı satın alın.

### <a name="past-charges-and-balance"></a>Geçmiş ücretleri ve Dengeleme

Geçiş öncesinde ücretleri ve kredi bakiyesi Kurumsal Anlaşma kaydınıza Azure portalı üzerinden görüntülenebilir. <!--Todo - Add a link for this-->

### <a name="when-should-the-setup-be-completed"></a>Kurulumun tamamlanması gereken ne zaman?

Kurumsal Anlaşma kaydınıza süresi dolmadan önce fatura hesabınıza Kurulumu tamamlayın. Kaydınıza süresi dolarsa Azure aboneliklerinizi Hizmetleri'nde hala açık kesintisiz çalışmaya devam eder. Ancak, perakende oranlarına Hizmetleri için ücretlendirilirsiniz.

### <a name="changes-to-the-enterprise-agreement-enrollment-after-the-setup"></a>Kurumsal Anlaşma kaydınıza Kurulumdan sonra değişiklikler

Azure geçişi el ile yeni faturalandırma hesaba taşınıp sonra Kurumsal Anlaşma kaydı için oluşturulan abonelikler. Daha fazla bilgi için [Azure abonelikleri diğer kullanıcılardan sahipliğini fatura](billing-mca-request-billing-ownership.md). Geçişten sonra satın alınan Azure ayırmaları taşımak [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Ayrıca, Faturalama hesabı için erişim geçişten sonra kullanıcılar sağlayabilir. Daha fazla bilgi için [Azure portalındaki faturalandırma rollerini yönetme](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)

### <a name="revert-the-transition"></a>Geçişi geri döndür

Geçiş geri alınamaz. Azure aboneliklerinizin faturalandırma yeni fatura hesabınıza geçirilir sonra Kurumsal Anlaşma kaydınıza geri dönemezsiniz.

### <a name="closing-your-browser-during-setup"></a>Kurulum sırasında tarayıcının kapanması

Tıklayarak önce **başlangıç geçiş**, tarayıcıyı kapatabilirsiniz. Kullanarak e-postada aldığınız bağlantının kurulumu için geri dönün ve geçişi başlatın. Geçiş başladıktan sonra Tarayıcıyı kapatın, geçişinizi üzerinde çalışmaya devam eder. Geçişinizi en son durumunu izlemek için geçiş durumu sayfasına geri dönün. Geçiş tamamlandığında bir e-posta alırsınız.

## <a name="complete-the-setup-in-the-azure-portal"></a>Azure portalında kurulumunu tamamlayın

Kurulumu tamamlamak için yeni bir faturalama hesabı ve Kurumsal Anlaşma kaydınıza erişmeniz gerekir. Daha fazla bilgi için [kümesi fatura hesabınıza tamamlamak için gerekli erişimle](#access-required-to-complete-the-setup).

1. Microsoft Müşteri sözleşmesi imzalandığında size gönderilen e-posta bağlantıyı kullanarak Azure portalında oturum açın.

2. Kuruluşunuzda başkasının sözleşmesi imzalı veya e-posta yoksa, aşağıdaki bağlantıyı kullanarak oturum açın. Değiştirin **enrollmentNumber** kayıt sayısıyla Kurumsal anlaşmanızı yenilendi.

   `https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber`

3. Seçin **başlangıç geçiş** Kurulum ve son adımda. Başlangıç geçiş seçtikten sonra:

    ![Kurulum Sihirbazı'nı gösteren ekran görüntüsü](./media/billing-mca-setup-account/ea-mca-set-up-wizard.png)

    - Kurumsal Anlaşma hiyerarşinize karşılık gelen faturalandırma bir hiyerarşiye yeni fatura hesap oluşturulur. Daha fazla bilgi için [değişiklikler fatura hiyerarşinize anlamak](#understand-changes-to-your-billing-hierarchy).
    - Kuruluşunuz için faturalandırmayı yönetmek devam eder, Kurumsal Anlaşma kaydınıza yöneticilerini yeni fatura hesabınıza erişim verilir.
    - Azure aboneliklerinizin faturalandırma yeni hesaba geçirilir. **Azure hizmetlerinizi bu geçiş sırasında hiçbir etkisi olmayacak. Bunlar herhangi bir kesinti çalışmaya devam**.
    - Azure ayırmaları varsa, yeni fatura hesabınıza aynı indirim ve terimi taşınır. Ayırma indirimi, geçiş sırasında uygulanacak devam eder.

4. Geçiş durumunu izleyebilirsiniz **geçiş durumu** sayfası.

   ![Geçiş durumu gösteren ekran görüntüsü](./media/billing-mca-setup-account/ea-mca-set-up-status.png)

## <a name="validate-billing-account-set-up"></a>Fatura hesabı ayarlama doğrula

 Yeni Fatura hesabınıza düzgün ayarlandığından emin olmak için aşağıdakileri doğrulayın:

### <a name="azure-subscriptions"></a>Azure abonelikleri

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminize bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** ve ardından faturalandırma profili.

4. Seçin **Azure abonelikleri** solundan.

   ![Aboneliklerin listesini gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-subscriptions-post-transition.png)

Kurumsal Anlaşma kaydınıza yeni fatura hesabınıza geçişi yapılır azure abonelikleri Azure abonelikler sayfasında görüntülenir. Herhangi bir abonelik eksik olduğunu düşünüyorsanız, Azure portalında el ile aboneliğin faturalama geçiş. Daha fazla bilgi için [faturalandırma sahipliğini diğer kullanıcıların Azure aboneliği edinin](billing-mca-request-billing-ownership.md)

### <a name="azure-reservations"></a>Azure ayırmalar

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Fatura bölümü seçin. Erişiminize bağlı olarak, bir faturalama hesabı veya faturalama profili seçmeniz gerekebilir.  Fatura hesabı ya da fatura profili seçin **fatura bölümleri** ve ardından bir fatura bölümü.

    ![Fatura bölüm post geçiş listesini gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Seçin **tüm ürünleri** solundan.

5. Arama **ayrılmış**.

    ![Abonelikleri post geçiş listesini gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-azure-reservations-post-transition.png)

Kurumsal Anlaşma kaydınıza yeni fatura hesabınıza taşınır azure ayırmaları tüm ürünler sayfasında görüntülenir. Tüm Azure ayırmaları Kurumsal Anlaşma kaydınıza taşınır doğrulamak tüm fatura bölümleri için adımları yineleyin. Tüm Azure ayırma eksik olduğunu düşünüyorsanız [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ayırma yeni faturalandırma hesabına taşınamıyor.

### <a name="access-of-enterprise-administrators-on-the-billing-profile"></a>Kurumsal Yöneticiler fatura profilindeki erişim

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Kaydınız için oluşturulan faturalandırma profili seçin. Erişiminize bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.  Fatura hesap sayfasında **profilleri faturalama** ve ardından faturalandırma profili.

4. Seçin **erişim denetimi (IAM)** solundan.

   ![Kurumsal Yöneticiler post geçişin erişim gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-ea-admins-access-post-transition.png)

Kurumsal Yöneticiler, profil okuyucular Fatura olarak profili sahipleri salt okuma izinlerine sahip Yöneticiler listelenir Kurumsal çalışırken Fatura olarak listelenir. Herhangi bir kurumsal yönetici erişimini eksik olduğunu düşünüyorsanız, bunları Azure portalında erişim verebilirsiniz. Daha fazla bilgi için [Azure portalındaki faturalandırma rolleri yönetme](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="access-of-enterprise-administrators-department-administrators-and-account-owners-on-invoice-sections"></a>Kuruluş Yöneticileri, departman yöneticilerinin ve fatura bölümlerde hesap sahipleri erişim

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-search-cost-management-billing.png).

3. Fatura bölümü seçin. Fatura bölümler Kurumsal Anlaşma kayıtları kendi ilgili bölümlerin aynı ada sahip. Erişiminizi bağlı olarak, bir faturalandırma profili ya da bir faturalama hesabı seçmeniz gerekebilir. Faturalandırma profili veya faturalama hesabı, seçmek **fatura bölümleri** ve ardından bir fatura bölümü seçin.

   ![Fatura bölüm post geçiş listesini gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Seçin **erişim denetimi (IAM)** solundan.

    ![Bölüm ve sonrası geçiş yöneticileri erişimi hesap erişim gösteren ekran görüntüsü](./media/billing-mca-setup-account/billing-mca-department-account-admins-access-post-transition.png)

Departmanındaki hesap sahipleri Azure aboneliği creators listelenmekle birlikte kurumsal yöneticiler ve departman fatura bölüm sahipleri ya da fatura bölüm okuyucu listelenir. Kurumsal Anlaşma kaydınıza içindeki tüm bölümler için erişim denetimi tüm fatura bölümleri için yineleyin. Tüm departmanı bir parçası olmayan bir hesap sahipleri adlı bir fatura bölümüne izin alacak **varsayılan fatura bölümü**. Herhangi bir yönetici erişimini eksik olduğunu düşünüyorsanız, bunları Azure portalında erişim verebilirsiniz. Daha fazla bilgi için [Azure portalındaki faturalandırma rolleri yönetme](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni faturalandırma hesabınız ile çalışmaya başlama](billing-mca-overview.md)

- [Microsoft Müşteri sözleşmesi için fatura hesabınıza Kurumsal Anlaşma görevleri gerçekleştirin](billing-mca-enterprise-operations.md)

- [Fatura hesabınıza erişimi yönetme](billing-understand-mca-roles.md)
