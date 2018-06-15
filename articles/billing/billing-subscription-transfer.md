---
title: Başka bir hesap için Azure aboneliği sahipliğini aktarma | Microsoft Docs
description: Başka bir kullanıcı ve süreci hakkında bazı sık sorulan sorular (SSS) için bir Azure aboneliği aktarım açıklar
keywords: Azure abonelik, azure aktarımı abonelik aktarımı, başka bir hesap, azure yeni abonelik sahibi azure aboneliği taşımak, azure aboneliği başka bir hesaba aktarın
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 110e2f611ba8bfc42fe17de6aa4487683db4a414
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069887"
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Başka bir hesap için bir Azure aboneliği sahipliğini aktarma

Aboneliğiniz Hesap Yöneticisi değiştirmek ve abonelik fatura sahipliğini el için başka bir kullanıcı hesabı Center'da aktarın. Aboneliğinizi farklı bir teklif değiştirmek için bkz: [Azure aboneliğinizi başka bir teklife geç](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
> 
> Yeni bir Azure AD aboneliği transfer ederseniz Kiracı, tüm rol atamalarını [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) kaynak kiracıdan kalıcı olarak silinir ve hedef Kiracı geçirilmez.

## <a name="transfer-ownership-of-an-azure-subscription"></a>Bir Azure aboneliği sahipliğini aktarma

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Adresinde oturum açın [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesabı yönetici olarak Aboneliğin hesap yöneticisi kim olduğunu bulmak için bkz: [sık sorulan sorular](#faq).

1. Aktarılacak abonelik seçin.

1. Denetleyerek, aboneliğinizin Self Servis aktarım için uygun olduğunu doğrulayın **teklif** ve **Teklif kimliği** ile [desteklenen teklifleri listesi](#supported).

   ![Teklif hesap Merkezi'nde abonelik Kimliğini doğrulayın](./media/billing-subscription-transfer/image0.png)
1. **Aboneliği devret**'e tıklayın.

   ![Azure hesabı abonelikleri sekmesi](./media/billing-subscription-transfer/image1.png)
1. Alıcıyı belirtin.

   > [!IMPORTANT]
   > 
   > Yeni bir Azure AD aboneliği transfer ederseniz Kiracı, tüm rol atamalarını [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) kaynak kiracıdan kalıcı olarak silinir ve hedef Kiracı geçirilmez.

   ![Aktarım abonelik iletişim kutusu](./media/billing-subscription-transfer/image2.PNG)

1. Alıcı otomatik olarak, kabul bağlantısı içeren bir e-posta alır.

   ![Abonelik aktarımı alıcı'e-posta](./media/billing-subscription-transfer/image3.png)
1. Alıcı bağlantıya tıklar ve yönergeleri izler; bu arada ödeme bilgilerini de girer.

   ![İlk abonelik aktarımı web sayfası](./media/billing-subscription-transfer/image4.png)

   ![İkinci abonelik aktarımı web sayfası](./media/billing-subscription-transfer/image5.png)
1. Başarılı! Abonelik artık aktarılır.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (Kurumsal Sözleşme) müşteriler için abonelik sahipliğini aktarma

Kuruluş Yöneticisi bir kayıt içinde abonelikleri sahipliğini aktarabilirsiniz. Başlamak için bkz: [hesap sahipliğini aktarma](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) EA Portalı'nda.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Bir aboneliğin sahipliğini kabul sonraki adımlar

1. Hesabı yöneticisiyle sunulmuştur Gözden geçirin ve Hizmet Yöneticisi, ortak yöneticilere ve diğer RBAC rolleri güncelleştirin. Daha fazla bilgi için bkz: [abonelik ya da hizmetleri yönetmek ekleme veya değiştirme Azure yönetici rolleri](billing-add-change-azure-subscription-administrator.md).
1. Dahil olmak üzere bu aboneliğin hizmetleriyle ilişkili kimlik bilgilerini güncelleştirin:
   1. Kullanıcının yönetici hakları abonelik kaynaklarına yönetim sertifikaları. Daha fazla bilgi için bkz: [oluşturun ve bir yönetim sertifikası için Azure yükleyin](../cloud-services/cloud-services-certs-create.md)
   1. Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için bkz: [Azure storage hesapları hakkında](../storage/common/storage-create-storage-account.md)
   1. Azure sanal makineleri Hizmetleri için uzaktan erişim kimlik bilgilerini ister. 
1. [Bu abonelik için faturalama uyarılarını güncelleştirin](billing-set-up-alerts.md) adresindeki [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). 
1. Bir iş ortağı ile çalışıyorsanız, iş ortağı Kimliğini bu abonelikte güncelleştirmeyi deneyin. İş ortağı Kimliğini güncelleştirebilirsiniz [Azure portal](https://portal.azure.com).

<a id="supported"></a>

## <a name="whats-supported"></a>Ne desteklenir:

Self Servis abonelik aktarımı teklifleri veya aşağıdaki tabloda listelenen abonelik türleri için kullanılabilir. Ücretsiz deneme sürümü şu anda aktaramaz veya [içinde Aç (AIO) Azure](https://azure.microsoft.com/offers/ms-azr-0111p/) abonelikleri. Geçici bir çözüm için bkz: [yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md). Gibi diğer abonelikler aktarmak için [sponsorluk](https://azure.microsoft.com/offers/ms-azr-0036p/) veya destek planları, [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Teklif Adı                                                                             | Teklif Numarası |
|----------------------------------------------------------------------------------------|--------------|
| [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR-0017P        |
| [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR-0025P        |
| [MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR-0062P        |
| [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR-0003P        |
| [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR-0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR-0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR-0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR-0060P        |

\* [EA portalı yoluyla](#EA)

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

### <a name="whoisaa"></a> Aboneliğin hesap yöneticisi kim?

Hesap Yöneticisi kaydolup veya Azure aboneliği satın kullanıcıdır. Bunlar erişim yetkiniz [hesap Merkezi'nde](https://account.azure.com/Subscriptions) ve abonelikleri oluşturma, aboneliklerinizi iptal edin, bir abonelik için faturalama değiştirmek veya Hizmet Yöneticisi değiştirme gibi çeşitli yönetim görevlerini gerçekleştirebilirsiniz. Bir abonelik için hesap yöneticinize olan emin değilseniz, öğrenmek için aşağıdaki adımları kullanın.

1. Ziyaret [Azure portalında abonelikleri sayfası](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Denetleyin ve altında aramak istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Her şeyi aktarım mu? Kaynak grupları, VM'ler, diskleri ve diğer çalışan hizmetleri de dahil olmak üzere?

Tüm kaynaklarınıza VM'ler, diskler ve Web siteleri aktarımı için yeni sahibin ister. Ancak, tüm [yönetici rolleri](billing-add-change-azure-subscription-administrator.md) ve [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) ayarladığınız ilkeleri farklı dizinlerde aktarım değil. Ayrıca, [uygulama kayıtlar](../active-directory//develop/active-directory-integrating-applications.md) ve diğer Kiracı özgü hizmetler boyunca aktarım yok.

### <a id="no-button"></a> Neden "abonelik aktarma" düğmesini görmüyorum?

Ne yazık ki, Self Servis abonelik aktarımı teklif veya ülke için kullanılamaz. Aboneliğinizi aktarmayı [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Abonelik aktarımı herhangi bir hizmet kesintisi yol mu?

Hizmet üzerinde etkisi yoktur. Aboneliğin aktarılması abonelik geçerli hesap yöneticisi altında iptal eder ve alıcının hesabı altında bir abonelik oluşturur. Yeni Abonelik için temel Azure Hizmetleri ilişkilidir. Abonelik kimliği aynı kalır.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Abonelik için dizini değiştirmek için bu işlemi nasıl kullanırım?

Bir Azure aboneliğiniz Hesap Yöneticisi ait dizinde oluşturulur. Dizini değiştirmek için hedef dizininde bir kullanıcı hesabı için abonelik aktarın. Bu kullanıcının aktarımı kabul etmek için adımları tamamlandığında, abonelik otomatik olarak hedef dizinine taşınır.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Fatura sahipliğini başka bir kuruluştan bir abonelik üzerinden izlerseniz Kaynaklarım erişiminiz devam eder?

Abonelik için başka bir kiracı aktardıysanız, önceki Kiracı ile ilişkilendirilen kullanıcılar abonelik erişimi kaybedersiniz. Bir kullanıcı bir Hizmet Yöneticisi veya ortak yönetici artık olsa bile, yine de dahil olmak üzere, başka bir güvenlik mekanizması abonelikle erişimleri:

* Kullanıcının yönetici hakları abonelik kaynaklarına yönetim sertifikaları. Daha fazla bilgi için bkz: [oluşturun ve Azure için yönetim sertifikası karşıya](../cloud-services/cloud-services-certs-create.md).
* Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için bkz: [Azure storage hesapları hakkında](../storage/common/storage-create-storage-account.md).
* Azure sanal makineleri Hizmetleri için uzaktan erişim kimlik bilgilerini ister.

Alıcı kullanıcıların kaynaklara erişimini kısıtlamak gerekirse, hizmetle ilişkilendirilmiş gizli güncelleştirmeyi düşünmelisiniz. En fazla kaynak, aşağıdaki adımları kullanarak güncelleştirilebilir:

  1. [Azure Portal](https://portal.azure.com) gidin.
  2. Hub menüsünde seçin **tüm kaynakları**.
  3. Kaynak seçin.
  4. Kaynak dikey penceresinde tıklayın **ayarları**. Burada görebilirsiniz ve var olan gizli güncelleştirin.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Abonelik faturalama döngüsü ortasında aktarırsanız, alıcı ödeme tüm faturalama döngüsü mu?

Gönderen, aktarım tamamlandıktan noktaya kadar bildirildi kullanım için ödeme sorumludur. Alıcı, sonraki sürümlerde aktarımı zamandan bildirilen kullanım sorumludur. Aktarım önce gerçekleşen ancak daha sonra bildirildi bazı kullanım olabilir. Kullanım alıcının fatura dahil edilir.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Alıcı, kullanım ve Fatura geçmişi erişimi var mı?

  Bilgiler yalnızca alıcıya kullanılabilir son fatura miktarıdır veya ilk fatura başlatmasından önce abonelik aktarılan varsa geçerli bakiye oluşturulur. Kullanım ve fatura geçmişini geri kalanı abonelikle aktarılmaz.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>Teklif aktarım sırasında değiştirilebilir mi?

Teklif aynı kalması gerekir. Teklifiniz değiştirmek için bkz: [Azure aboneliğinizi başka bir teklife geç](billing-how-to-switch-azure-offer.md).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-country"></a>Başka bir ülkede bir kullanıcı hesabı için bir abonelik aktarabilirim?

Hayır, başka bir ülkede kullanıcı hesabına bir aboneliğin aktarılması desteklenmiyor. Alıcının kullanıcı hesabının aynı ülkede olması gerekir.

### <a name="can-the-recipient-use-a-different-payment-method"></a>Alıcı, farklı bir ödeme yöntemi kullanabilir miyim?

Evet. Ancak abonelik faturalama geçmişi iki hesap arasında bölünür.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>Bir Azure aboneliği transfer sonra ödeme yöntemini etkileniyor mu?

Abonelik aktarımı kabul etmek için bir kredi kartı veya ödeme yöntemini benzer abonelik için ödeme yapmak sağlanmalıdır. Örneğin, Bob bir abonelik için Jane aktarır ve Jane aktarımı kabul eder, Jane abonelik için ödeme yapmak için bir ödeme yöntemi sağlamanız gerekir. Transfer işlemi tamamlandıktan sonra Jane değil Bob abonelik için faturalandırılır.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Nasıl veri ve Hizmetleri Azure Aboneliğim için yeni abonelik geçişini miyim?

Abonelik sahipliği aktaramaz, kaynaklarınızın el ile geçirebilirsiniz. Bkz: [yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.