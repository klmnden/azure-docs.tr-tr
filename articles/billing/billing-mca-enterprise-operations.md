---
title: Microsoft Müşteri sözleşmesi - Azure Kurumsal Anlaşma görevleri gerçekleştirin
description: Kurumsal Anlaşma fatura hesabınıza yeni görevleri tamamlamanızı öğrenin.
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
ms.openlocfilehash: 9404908b7c486801480474c5a2c9ff7688e1de48
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490703"
---
# <a name="complete-enterprise-agreement-tasks-in-your-billing-account-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için fatura hesabınıza Kurumsal Anlaşma görevleri gerçekleştirin

Kuruluşunuzun bir Kurumsal Anlaşma kaydınıza yenilemek için Microsoft Müşteri sözleşmesi oturum açtığını, yeni bir faturalama hesabı sözleşme oluşturulur. Yeni hesabınızda faturalandırmayı Kurumsal anlaşmanızı farklı şekilde düzenlenmiştir. Bu makalede, Kurumsal anlaşmanızı gerçekleştirilen görevleri gerçekleştirmek için yeni bir faturalama hesabı nasıl kullanabileceğiniz açıklanır.

## <a name="billing-organization-in-the-new-account"></a>Yeni hesap kuruluşta faturalama

Aşağıdaki diyagramda, faturalandırma yeni faturalama hesabınızda nasıl düzenlendiği açıklanmaktadır.

![Ea-mca-post-geçiş-hiyerarşi görüntüsü](./media/billing-mca-setup-account/mca-post-transition-hierarchy.png)

| Kurumsal Anlaşma   | Microsoft Müşteri Sözleşmesi    |
|------------------------|--------------------------------------------------------|
| Kayıt            | Kuruluşunuzda, Kurumsal Anlaşma kaydınıza benzer faturalandırmayı yönetmek için bir faturalandırma profili kullanın. Kurumsal Yöneticiler fatura profilinin sahipleri haline gelir. Fatura profilleri hakkında daha fazla bilgi için bkz: [fatura profillerini anlayabilir](billing-mca-overview.md#billing-profiles).
| Departman            | Maliyetlerinizi, Kurumsal Anlaşma kaydınıza departmanlara benzer düzenlemek için bir fatura bölümü kullanın. Fatura bölümler bölüm haline gelir ve departman yöneticilerinin sahipleri ilgili fatura bölümlerin olur. Fatura bölümleri hakkında daha fazla bilgi için bkz: [anlayın fatura bölümleri](billing-mca-overview.md#invoice-sections). |
| Hesap               | Kurumsal anlaşmanızı oluşturulan hesapları yeni faturalandırma hesabında desteklenmiyor. Hesap aboneliklerini departmanı için ilgili fatura bölümüne ait. Hesap sahipleri oluşturabilir ve fatura bölümlerinin aboneliklerini yönetin. |

## <a name="changes-for-enterprise-administrators"></a>Kuruluş Yöneticileri için değişiklikler

Kuruluş Yöneticileri, bir Microsoft Müşteri sözleşmesi yenilenmiş Kurumsal Anlaşma aşağıdaki değişiklikler uygulanır.

- Kaydınız için bir faturalandırma profili oluşturulur. Kurumsal Anlaşma kaydınıza gibi kuruluşunuz için faturalandırmayı yönetmek için faturalandırma profili kullanacaksınız. Fatura profilleri hakkında daha fazla bilgi için [fatura profillerini anlayabilir](billing-mca-overview.md#billing-profiles).
- Bir fatura bölümü, Kurumsal Anlaşma kaydınıza her bir departman için oluşturulur. Fatura aşağıdaki bölümlerde, bölümlerinizden yönetmek için kullanacaksınız. Ek bölümler ayarlanacak yeni fatura bölümleri oluşturabilirsiniz. Fatura bölümleri hakkında daha fazla bilgi için bkz: [fatura bölümleri anlamak](billing-mca-overview.md#invoice-sections).
- Azure aboneliği Oluşturucu rolü, diğer Kurumsal Anlaşma kaydı oluşturulan hesapları gibi Azure aboneliği oluşturma izni vermek için fatura bölümleri kullanacaksınız.
- Kullanacağınız [Azure portalında](https://portal.azure.com) Azure EA portal yerine kuruluşunuz için faturalandırmayı yönetmek için.

Yeni Fatura hesap aşağıdaki rolleri verilmiştir:

**Faturalandırma Profil sahibi** -anlaşma imzalandığında oluşturduğunuz fatura profilindeki faturalandırma profili sahip rolü atanır. Rol, kuruluşunuz için faturalandırma yönetmenizi sağlar. Ücretleri ve faturaları görüntülemek, fatura maliyetlerini düzenlemek, ödeme yöntemlerini Yönet ve kuruluşunuzun faturalarına erişimi denetler.

**Fatura bölümüne sahip** -Kurumsal Anlaşma kaydınıza departmanları için oluşturulan tüm fatura bölümlerde fatura bölümüne sahip rolü atanır. Rol sayesinde kimlerin Azure abonelikleri oluşturabilir ve diğer ürünleri satın alın.

### <a name="view-charges-and-credits-balance-for-your-organization"></a>Kuruluşunuz için giderleri ve kredi bakiyesi görüntüleyin

Fatura profilinizi, kuruluşunuzun Kurumsal Anlaşma kaydınıza benzer ücretleri ve Azure kredisi bakiyesi izlemek için kullanın.

Faturalandırma profili kredi bakiyesi görüntüleme bilgi edinmek için [fatura profiliniz için Azure kredi bakiyesi izlemek](billing-mca-check-azure-credits-balance.md).

Faturalandırma profili için ücretleri görüntüleme hakkında bilgi edinmek için [Microsoft Müşteri anlaşmanın faturasında ücretlerini anlama](billing-mca-understand-your-bill.md).

### <a name="view-charges-for-a-department"></a>Bölümün ücretleri görüntüle

Bir fatura bölümü, Kurumsal anlaşmanızı olduğu her departman için oluşturulur. Azure portalında bir fatura bölümü ücretleri görüntüleyebilirsiniz. Daha fazla bilgi için [görüntüleme işlemleri fatura bölümleri](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

### <a name="view-charges-for-an-account"></a>Bir hesap için ücretleri görüntüle

Kurumsal Anlaşma kaydınıza oluşturulan hesapları yeni faturalandırma hesabında desteklenmez. Hesap aboneliklerini departmanı için ilgili fatura bölümüne ait. Hesap sahipleri oluşturabilir ve fatura bölümlerinin aboneliklerini yönetin.

Bir hesaba ait abonelikleri için toplam maliyet görüntülemek için her abonelik için bir maliyet merkezi ayarlamanız gerekir. Ardından Azure kullanım ve Ücret csv dosyası abonelik maliyet merkezi tarafından filtre uygulamak için kullanabilirsiniz.

### <a name="download-usage-and-charges-csv-price-sheet-and-tax-documents"></a>İndirme kullanımı ve ücretleri csv, fiyat ve vergi belgeleri

Bir aylık Fatura Fatura hesabınıza her fatura profilinde oluşturulur. Her bir fatura Azure kullanım ve Ücret csv dosyası, fiyat ve vergi belge (varsa) indirebilirsiniz. Ayrıca, geçerli ayın ücretler Azure kullanım ve Ücret csv dosyası indirebilirsiniz.

Azure kullanım ve Ücret csv dosyalarını indirme öğrenmek için bkz. [Microsoft Müşteri sözleşmenizi kullanımı indir](billing-download-azure-daily-usage.md#download-usage-for-your-microsoft-customer-agreement).

Fiyat listesini indirme hakkında bilgi edineceksiniz için bkz: [indirmek için Microsoft Müşteri sözleşmenizi fiyatlandırma](billing-ea-pricing.md#microsoft-customer-agreement-pricing).

Vergi belgelerini indirmek öğrenmek için bkz: [vergi belgeleri görüntülemek için Microsoft Müşteri sözleşmenizi](billing-mca-download-tax-document.md#view-and-download-tax-documents).

### <a name="add-an-additional-enterprise-administrator"></a>Ek Kurumsal Yönetici Ekle

Kullanıcılara bildirmek için fatura profiline görünümü erişmesini ve kendi Kurumunuz için faturalandırma yönetin. Kullanabileceğiniz **erişim denetimi (IAM)** erişim vermek için Azure portalında sayfası.  Faturalandırma profili rolleri hakkında daha fazla bilgi için bkz: [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

Sağlamak için fatura profilinize erişmek öğrenmek için bkz: [Azure portalındaki faturalandırma rolleri yönetme](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-department"></a>Yeni bir bölüm oluşturun

Kurumsal Anlaşma kaydınıza departmanlara gibi ihtiyaçlarınıza göre maliyetlerinizi düzenlemek için bir fatura bölümü oluşturun. Azure portalında yeni bir fatura bölüm oluşturabilirsiniz. Daha fazla bilgi için bkz. [maliyetlerinizi düzenlemek için fatura bölümler oluşturma](billing-mca-section-invoice.md).

### <a name="create-a-new-account"></a>Yeni hesap oluşturun

Kullanıcılar, Kurumsal Anlaşma kaydı oluşturulan hesapları gibi Azure aboneliği oluşturmak üzere izin vermeniz fatura bölümlerde Azure aboneliği Oluşturucu rolü atayın. Daha fazla bilgi için [diğerlerinin Azure abonelikleri oluşturabilmesi için izinler verebilirsiniz](billing-mca-create-subscription.md#give-others-permission).

## <a name="changes-for-department-administrators"></a>Bölüm Yöneticiler için değişiklikler

Bir Microsoft Müşteri sözleşmesi yenilenmiş olan Kurumsal Anlaşma departman yöneticilerinin aşağıdaki değişiklikler uygulanır.

- Bir fatura bölümü, Kurumsal Anlaşma kaydınıza her bir departman için oluşturulur. Fatura bölümü rapordan, departmanları yönetmek için kullanacaksınız. Fatura bölümleri hakkında daha fazla bilgi için bkz: [fatura bölümleri anlamak](billing-mca-overview.md#invoice-sections).
- Diğer Kurumsal Anlaşma kaydı oluşturulan hesapları gibi Azure aboneliği oluşturma izni vermek için fatura bölümünde Azure abonelik Oluşturucu rolü kullanacaksınız.
- Azure EA portal yerine kuruluşunuz için faturalandırmayı yönetmek için Azure portalını kullanacaksınız.

Yeni Fatura hesap aşağıdaki rolleri verilmiştir:

**Fatura bölümüne sahip** -Kurumsal anlaşmasına sahip departmanları için oluşturulan fatura bölümündeki fatura bölümüne sahip rolü atanır. Rol, Görünüm ve izleme ücretleri ve denetimi kullanan Azure abonelikleri oluşturabilir ve diğer ürünler için fatura bölüm satın sağlar.

### <a name="view-charges-for-your-department"></a>Bölümünüze ilişkin ücretleri görüntüle

Azure portalının içinde bölümünüze ilişkin ücretleri oluşturulan fatura bölümü için görüntüleyebilirsiniz [maliyet Yönetimi + faturalandırma sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).

### <a name="add-an-additional-department-administrator"></a>Ek bölüm yönetici Ekle

Bir fatura bölümü, Kurumsal anlaşmanızı olduğu her departman için oluşturulur. Kullanabileceğiniz **erişim Control(IAM)** başkalarının görüntülemek ve fatura bölüm yönetmek için erişim vermek için Azure portalında sayfası. Fatura bölüm rolleri hakkında daha fazla bilgi edinmek için [fatura bölüm rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Sağlayın, fatura bölümüne erişmek öğrenmek için bkz: [Azure portalındaki faturalandırma rolleri yönetme](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-account-in-your-department"></a>Departmanınız içinde yeni bir hesap oluşturun

Kullanıcılar departmanınız için oluşturulan fatura bölümünde Azure abonelik Oluşturucu rolü atayın. Daha fazla bilgi için [diğerlerinin Azure abonelikleri oluşturabilmesi için izinler verebilirsiniz](billing-mca-create-subscription.md#give-others-permission).

### <a name="view-charges-for-accounts-in-your-departments"></a>Bölümlerinizden hesapları için ücretleri görüntüle

Kurumsal Anlaşma kaydınıza oluşturulan hesapları yeni faturalandırma hesabında desteklenmez. Hesap aboneliklerini departmanı için ilgili fatura bölümüne ait. Hesap sahipleri oluşturabilir ve fatura bölümlerinin aboneliklerini yönetin.

Kuruluşunuzda bir hesaba ait abonelikleri için toplam maliyet görüntülemek için her abonelik için bir maliyet merkezi ayarlamanız gerekir. Ardından Azure kullanım ve Ücret dosya abonelik maliyet merkezi tarafından filtre uygulamak için kullanabilirsiniz.

## <a name="changes-for-account-owners"></a>Hesap sahipleri için değişiklikler

Hesap sahipleri, Kurumsal Anlaşma Azure abonelikleri yeni fatura hesap oluşturma izni alın. Mevcut Azure aboneliklerinizi departmanınız için oluşturulan fatura bölümüne ait. Hesabınız bir bölüme ait değilse, aboneliklerinizin varsayılan fatura bölümü adlı bir fatura bölümüne ait.

Ek Azure abonelikleri oluşturabilmesi için aşağıdaki rol yeni fatura hesap sunulur.

**Azure aboneliği Oluşturucusu** -Kurumsal Anlaşma, bölümünüze ilişkin oluşturulan fatura bölümünde azure abonelik Oluşturucu rolü atanır. Hesabınız bir bölüme ait değilse varsayılan fatura bölümü adlı bir bölüm Azure aboneliği Oluşturucu rolünde olursunuz. Rol, Azure abonelikleri için fatura bölümünde oluşturmanızı sağlar.

### <a name="create-an-azure-subscription"></a>Bir Azure aboneliği oluşturun

Azure abonelikleri için fatura bölümünüzü Azure portalında oluşturabilirsiniz. Daha fazla bilgi için [ek bir Azure aboneliği için Microsoft Müşteri sözleşmesi oluşturma](billing-mca-create-subscription.md)

### <a name="view-charges-for-your-account"></a>Hesabınız için ücretleri görüntüle

Bir hesaba ait aboneliklerinin görüntülemek için Git [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında. Abonelik sayfasında, aboneliğinize ilişkin ücretleri görüntüler.

### <a name="view-charges-for-a-subscription"></a>Abonelik ücretleri görüntüle

Bir abonelik için ücret ya da görüntüleyebilirsiniz üzerinde [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ya da Azure analysis maliyeti. Azure maliyet analizi hakkında daha fazla bilgi için bkz. [maliyet analizi ile maliyetleri analiz](../cost-management/quick-acm-cost-analysis.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Fatura hesabı için bir Microsoft Müşteri sözleşmesi anlama](billing-mca-overview.md)
- [Faturanızı anlama](billing-understand-your-bill.md)
- [Faturanızı anlama](billing-understand-your-invoice.md)
- [Faturalandırma sahipliğini diğer kullanıcıların Azure aboneliği edinin](billing-mca-request-billing-ownership.md)
