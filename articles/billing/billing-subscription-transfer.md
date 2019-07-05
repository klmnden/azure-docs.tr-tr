---
title: Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarabilir | Microsoft Docs
description: Başka bir hesap ve süreci hakkında sık sorulan bazı sorular (SSS) için bir Azure aboneliğinin faturalandırma sahipliğini aktarmak açıklar
keywords: Azure aboneliği, azure aktarım aboneliği aktarmayı, başka bir hesabı, azure yeni abonelik sahibi azure aboneliği taşımak, başka bir hesabı, azure aktarım faturalama azure aboneliği Aktarım
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff3c57c31526119ab81225a1c70b163173be937
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514430"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Bir Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarma

Kuruluşunuz çıkıyorsunuz veya başka bir hesaba faturalandırılmaya aboneliğinizi istiyorsanız Azure aboneliğinizin faturalandırma sahipliğini devretmek isteyebilirsiniz. Faturalandırma sahipliğini başka bir hesaba aktarma gibi faturalandırma görevlerini gerçekleştirmek için yeni hesap izni Yöneticiler ödeme yöntemini değiştir, harcamalarını görüntülemek ve aboneliği iptal sağlar.

Aboneliğinizi türünü değiştir ancak faturalandırma sahipliğini tutmak istiyorsanız, bkz. [Azure aboneliğinizi başka bir teklife geç](billing-how-to-switch-azure-offer.md). Abonelikteki kaynakları yönetebilir denetim istiyorsanız bkz [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Bir kurumsal Agreement(EA) müşterisiyseniz, kuruluş yöneticileri hesapları arasında abonelik faturalandırma sahipliğini aktarabilir. Daha fazla bilgi için [Kurumsal Anlaşma (EA) abonelik faturalandırma sahipliğini](#transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Bir Azure aboneliğinin faturalandırma sahipliğini aktarma

1. Oturum [Azure portalında](https://portal.azure.com) devretmek istediğiniz aboneliğin fatura hesap yöneticisi olarak. Yönetici olup olmadığınızı öğrenmek için bkz: [sık sorulan sorular](#faq).

1. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Seçin **abonelikleri** sol bölmesinden. Erişiminizi bağlı olarak, fatura bir kapsam seçin ve ardından gerekebilir **abonelikleri** veya **Azure abonelikleri**.

1. Seçin **başka bir hesaba aktarma** aktarmak istediğiniz abonelik için. 

   ![Aktarım için abonelik seçin](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Bir faturalama aboneliğin yeni sahibi olmanız ve ardından hesap yöneticisi olan bir kullanıcı e-posta adresini girin **gönderme aktarım isteği**.

    > [!IMPORTANT]
    >
    > Aboneliğinizin faturalandırma sahipliğini başka bir Azure AD'de kullanıcının hesabı aktarırsanız Kiracı, tüm [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) Abonelikteki kaynakları yönetmek için atamaları kalıcı olarak kaldırılır. Yalnızca yeni sahip, abonelik kaynakları yönetmek için erişim gerekir. Daha fazla bilgi için [başka bir Azure AD kiracısında bir kullanıcı için aboneliğin aktarılması](../active-directory/managed-identities-azure-resources/known-issues.md).
  
    ![Aktarım Sayfa Gönder](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. Kullanıcı, aktarım isteğini gözden geçirmek için yönergeler içeren bir e-posta alır.

   ![Alıcı için abonelik aktarım e-posta](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Aktarım İsteği onaylamak için kullanıcı e-postada bağlantısını seçer ve yönergeleri izler. Kullanıcı aboneliği ödemesi için kullanılan bir ödeme yöntemi seçin etmesi gerekir. Kullanıcının bir Azure hesabınız yoksa, ayrıca, bunlar yeni bir hesap için kaydolmanız gerekir. 

   ![İlk abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![İkinci abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![İkinci abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Başarılı! Abonelik artık aktarılır.

## <a name="transferring-subscription-to-an-account-in-another-azure-ad-tenant"></a>Abonelik başka bir Azure AD kiracısında bir hesaba aktarma

Azure için kaydolduğunuzda, bir Azure Active Directory (AD) kiracısı sizin için oluşturulur. Kiracı hesabınızın temsil eder. Kiracı, aboneliklerinize ve kaynaklarınıza erişimi yönetmek için kullanın.

Yeni bir abonelik oluşturduğunuzda, hesabınızın Azure AD kiracısında barındırılır. Diğerleri, abonelik ve kaynaklara erişim sağlamak istiyorsanız, bunları kiracınıza davet etmek gerekir. Bu yardımcı, aboneliklerinize ve kaynaklarınıza erişimi denetleyebilirsiniz.

Başka bir Azure AD kiracısında bir hesap, abonelik faturalandırma sahipliğini aktarmak, aboneliğin yeni hesap kiracıya taşındıktan. Tüm kullanıcıları, grupları veya sahip olan hizmet sorumlularını [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) yönetmek için abonelikte kaynak erişimlerini kaybeder. Yalnızca kullanıcı aktarım İsteğiniz kabul eden yeni hesabında kaynakları yönetmek için erişim gerekir. İlk olarak erişimi olan kullanıcılar için erişim sağlamak için yeni sahibin gerekir [el ile bu aboneliğe kullanıcı eklemek](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).


## <a name="transferring-visual-studio-microsoft-partner-network-mpn-and-pay-as-you-go-devtest-subscriptions"></a>Visual Studio, Microsoft iş ortağı ağı (MPN) ve Kullandıkça Öde geliştirme ve Test abonelikleri aktarma

Visual Studio ve Microsoft iş ortağı ağı abonelikleri aylık yinelenen Azure kredisi ilişkili vardır. Bu abonelikler aktardığınızda, kredinizin hedef fatura hesap kullanılamıyor. Abonelik kredi hedef Faturalama hesabı kullanır. Örneğin Bob, Gamze'nin 9 Eylül hesapta ve Jane Visual Studio Enterprise aboneliğine taşıyorsa aktarımı kabul eder. Transfer tamamlandıktan sonra abonelik kredi Gamze'nin hesabında kullanarak başlatır. Kredi 9 her ay sıfırlanır. 


<a id="EA"></a>

## <a name="transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions"></a>Kurumsal Anlaşma (EA) abonelik faturalandırma sahipliğini aktarma

Kuruluş Yöneticisi bir kayıt içindeki hesapları arasında abonelik sahipliğini aktarabilir. Daha fazla bilgi için [hesap sahipliğini aktarma](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) EA portalında.

<a id="CSP"></a>

## <a name="next-steps-after-accepting-billing-ownership"></a>Faturalandırma sahipliğini kabul sonraki adımlar

Bir Azure aboneliğinin faturalandırma sahipliğini onayladığınızda, sonraki adımları gözden geçirmenizi öneririz:

1. Gözden geçirin ve Hizmet Yöneticisi ve ortak Yöneticiler diğer RBAC rollerini güncelleştirin. Daha fazla bilgi için bkz. [ekleme veya değiştirme Azure aboneliği yöneticileri](billing-add-change-azure-subscription-administrator.md) ve [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
1. Dahil olmak üzere bu aboneliğin hizmetleriyle ilişkili kimlik bilgilerini güncelleştirin:
   1. Yönetici hakları abonelik kaynaklarına kullanıcı yönetim sertifikaları. Daha fazla bilgi için [oluşturup Azure için bir yönetim sertifikası yükleme](../cloud-services/cloud-services-certs-create.md)
   1. Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md)
   1. Azure sanal makineler Hizmetleri için uzaktan erişim kimlik bilgilerini ister.
1. Bir iş ortağıyla çalışıyorsanız, iş ortağı Kimliğini bu abonelikte güncelleştirmeyi göz önünde bulundurun. İş ortağı kimliği güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com). Daha fazla bilgi için [şekilde Azure hesaplarınızı için bir iş ortağı kimliği Bağla](billing-partner-admin-link-started.md)

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Abonelik aktarımı Azure portalında aşağıda listelenen abonelik türleri için kullanılabilir. Şu anda aktarımı için desteklenmiyor [ücretsiz deneme](https://azure.microsoft.com/offers/ms-azr-0044p/) veya [içinde Aç (AIO) Azure](https://azure.microsoft.com/offers/ms-azr-0111p/) abonelikler. Geçici bir çözüm için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md). Diğer abonelikler aktarmak istediğiniz [sponsorluk](https://azure.microsoft.com/offers/ms-azr-0036p/) veya destek planları, [Azure desteği ile iletişim kurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) 
- [MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure-planı](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\* [EA portal aracılığıyla](#EA).

\*\* Yalnızca oturum sırasında ayarlama Azure Web sitesinde oluşturulan hesaplar için desteklenir. 

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Sık sorulan sorular (SSS) Gönderenler için

Bu SSS, Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarma kullanıcılara uygulayın.

### <a name="whoisaa"></a> Bir fatura hesap yöneticisinin kim?

Faturalama yöneticisi hesabınız için fatura bilgilerini Yönetme iznine sahip bir kişidir. Faturalandırma erişim yetkiniz [Azure portalında](https://portal.azure.com) ve abonelikler, Görünüm ve ödeme faturalar veya güncelleştirme oluşturmak gibi çeşitli faturalandırma görevlerini gerçekleştirme ödeme yöntemleri.

Faturalama Yöneticisi olduğunuz abonelikleri tanımlamak için aşağıdaki adımları kullanın:

1. Ziyaret [maliyet Yönetimi + Azure portalındaki faturalandırma sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. Seçin **abonelikleri** sol bölmesinden. Erişiminizi bağlı olarak, fatura bir kapsam seçin ve ardından gerekebilir **abonelikleri** veya **Azure abonelikleri**
1. Abonelik sayfasında, bir faturalama yöneticisi olduğunuz tüm abonelikler listeler.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Her şeyi aktarım mu? Kaynak grupları, VM'ler, diskler ve çalışan başka hizmetler de dahil olmak üzere?

Tüm kaynaklarınızı yeni hesaba aktarımı Web siteleri, disk ve VM'lerin ister. Bununla birlikte, hesabınız için başka bir Azure AD'de aboneliği transfer ederseniz Kiracı, tüm [yönetici rolleri](billing-add-change-azure-subscription-administrator.md) ve [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) abonelik atamaları [olmayan Aktarım](#transferring-subscription-to-an-account-in-another-azure-ad-tenant). Ayrıca, [uygulama kayıtları](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) ve başka bir kiracıya özgü Hizmetleri ile birlikte abonelik aktarılmıyor.

### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Bir hesaba başka bir ülkede sahipliğini aktarabilir miyim?
Ne yazık ki, platformlar arası ülke, Azure portalında aktarımları gerçekleştirilemiyor. Ülkelerde, aboneliğinizin aktarılmasına yönelik [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>İki hesap yönetici ortağıyım. Bir abonelik my hesapları birinden diğerine aktarabilirim?
Evet, kendi hesapları arasında abonelik aktarabilirsiniz. Abonelikleri, hesapları arasında aktarmak için yukarıdaki adımları kullanabilmeniz için hesaplarınızı iki farklı kullanıcı hesaplarını kavramsal olarak kabul edilir.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Abonelik aktarımı, hizmet kapalı kalma süresi içinde sonuç vermez?

Bir kullanıcıya aynı Azure AD kiracısında bir aboneliği aktarırsanız aboneliğinde çalışan kaynaklar için herhangi bir etkisi yoktur.  Ancak, başka bir kullanıcıya aboneliği aktarırsanız Kiracı, tüm kullanıcılara, gruplara veya sahip olan hizmet sorumlularını [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) yönetmek için abonelikte kaynak erişimlerini kaybeder. 

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Alıcı, kullanım ve fatura geçmişinizi erişimi var mı?

Alıcı yalnızca bilgileri, aboneliğiniz için son ayın maliyetidir. Kullanım ve fatura geçmişinizi geri kalanını abonelikle aktarılmaz

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Nasıl yeni abonelik için veri ve hizmetlerinizi Azure Aboneliğimi için geçiş yaparım?

Aboneliğin sahipliğini devretmek olamaz, kaynaklarınızı el ile geçirebilirsiniz. Bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Ben bir Visual Studio veya Microsoft iş ortağı ağı aboneliği transfer ederseniz, kredilerimi abonelikte yeni bir hesap ile ileri gerçekleştirmek mu?

Hayır, krediniz, yeni hesap kullanılamıyor. Aktarım isteği kabul eden kullanıcının aktarım isteği kabul etmek için Visual Studio Lisansı olmalıdır. Abonelik, kullanıcının hesabında kullanılabilen Visual Studio kredisi kullanır. Daha fazla bilgi için [aktarma Visual Studio, Microsoft iş ortağı ağı (MPN) ve Kullandıkça Öde geliştirme ve Test abonelikleri](#transferring-visual-studio-microsoft-partner-network-mpn-and-pay-as-you-go-devtest-subscriptions)


## <a name="frequently-asked-questions-faq-for-recipients"></a>Sık sorulan sorular (SSS) alıcılar için

Bu SSS, başka bir hesabı bir Azure aboneliğinin faturalandırma sahipliğini ele kullanıcılara uygulanır.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Başka bir hesap bir abonelik faturalandırma sahipliğini üzerinden alırsa, hesap devam etmek, kaynağı erişimi kullanıcıların musunuz?

Abonelik hesabı aynı Azure AD kiracısı, tüm kullanıcılara, gruplara veya sahip olan hizmet sorumlularını aktarılıyorsa [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) Abonelikteki kaynakları yönetmek için kendi erişimi korur. Abonelik için RBAC erişimi olan kullanıcıları görmek için aşağıdaki adımları kullanın:

1. Ziyaret [Azure portalında abonelik sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Kontrol edin ve ardından istediğiniz aboneliği seçin **erişim denetimi (IAM)** sol bölmesinden.
1. Seçin **rol atamaları** sayfanın üst. Rol atamaları sayfası abonelikte RBAC erişimine sahip tüm kullanıcılar listelenir.

Abonelik başka bir Azure AD kiracısı, tüm kullanıcılara, gruplara veya sahip olan hizmet sorumlularını hesap aktarılıyorsa [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) yönetmek için abonelikte kaynak erişimlerini kaybeder. Bunlar RBAC artık erişiminiz yoksa bile, ancak bunlar yine de dahil olmak üzere bazı güvenlik mekanizmaları aracılığıyla aboneliğe erişim olabilir:

* Yönetici hakları abonelik kaynaklarına kullanıcı yönetim sertifikaları. Daha fazla bilgi için [oluşturup Azure yönetim sertifikası karşıya](../cloud-services/cloud-services-certs-create.md).
* Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).
* Azure sanal makineler Hizmetleri için uzaktan erişim kimlik bilgilerini ister.

Alıcı gerekiyorsa kısıtlamak, kullanıcıların kaynaklara erişmek hizmetle ilişkili herhangi bir gizli anahtar güncelleştirme düşünmelisiniz. En fazla kaynak, aşağıdaki adımları kullanarak güncelleştirilebilir:

  1. [Azure Portal](https://portal.azure.com) oturum açın.
  2. Hub menüsünde **tüm kaynakları**.
  3. Kaynak seçin.
  4. Kaynak sayfasında tıklatın **ayarları**. Burada görüntüleyebilir ve var olan gizli anahtarları güncelleştirme.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Fatura dönemi ortasında bir abonelik faturalandırma sahipliğini üzerinden alırsa, ben tüm faturalama döngüsü için ödeme yapmam gerekir mi?

Hesabınız, aktarımı ve sonraki sürümlerde zamandan bildirilen tüm kullanımlar için ödeme sorumludur. Aktarım önce gerçekleşen ancak daha sonra bildirilen bazı kullanım olabilir. Kullanım, hesabınızın fatura dahil edilir.

### <a name="can-i-use-a-different-payment-method"></a>Farklı bir ödeme yöntemi kullanabilir miyim?

Evet. Aktarım isteği kabul edilirken hesabınızla bağlantılı ya da yeni bir ödeme yöntemi eklemeniz mevcut ödeme yöntemini seçebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

### <a id="no-button"></a> "Abonelik aktarımı" düğmesini neden göremiyorum?

Ne yazık ki, Self Servis abonelik aktarımı için fatura hesabınıza kullanılamaz. Kurumsal Anlaşma (EA) hesabı Azure portalındaki faturalandırma sahipliğini dışarı aktarılması şu anda desteklemiyoruz. Üstelik, Microsoft satış çalışırken oluşturulan Microsoft Müşteri sözleşmesi hesaplar, abonelik faturalandırma sahipliğini aktarma desteklemez. 

### <a id="no-button"></a> Aboneliğimi'neden olmayan türü desteği aktarımı? 

Ne yazık ki, tüm türü abonelikler, fatura sahipliğinin aktarılmasını desteklemez. Aktarımlarını destekleyen bir abonelik türleri listesini görüntülemek için bkz: [desteklenen abonelik türleri](#supported-subscription-types)

### <a id="no-button"></a> Bir erişim reddedildi hatası abonelik faturalandırma sahipliğini aktarmak çalıştığınızda neden mesajı alıyorum? 

Microsoft Azure planlama abonelik aktarımı çalıştığınız ve gerekli izni yoksa, bu hatayı görürsünüz. Microsoft Azure plan aboneliği aktarmayı sahibi veya abonelik faturalanacağı fatura bölümüne katkıda bulunanı olmanız gerekir. Daha fazla bilgi için [fatura bölümü için abonelikleri yönetme](billing-understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirin ve Hizmet Yöneticisi ve ortak Yöneticiler diğer RBAC rollerini güncelleştirin. Daha fazla bilgi için bkz. [ekleme veya değiştirme Azure aboneliği yöneticileri](billing-add-change-azure-subscription-administrator.md) ve [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
