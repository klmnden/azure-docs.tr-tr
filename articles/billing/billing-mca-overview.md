---
title: Bir Microsoft Müşteri sözleşmesi - Azure fatura hesabınıza ile çalışmaya başlama | Microsoft Docs
description: Fatura hesabı için bir Microsoft Müşteri sözleşmesi anlama
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: banders
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: ea625a61ed600dbaa22fef85987e9570a6fb7dbc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371465"
---
# <a name="get-started-with-your-billing-account-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için fatura hesabınıza ile çalışmaya başlama

Bir faturalama hesabı, Azure'ı kullanmak için Microsoft ile oturum her anlaşma için oluşturulur. Faturalandırmayı yönetmek ve maliyetleri izlemek için fatura hesabınıza kullanın. Faturalandırma birden çok hesaba erişim sağlayabilirsiniz. Örneğin, Azure için kişisel projeleriniz için kaydolmanız. Ayrıca, kuruluşunuzun Kurumsal Anlaşma veya Microsoft Müşteri sözleşmesi aracılığıyla azure'a erişimi olabilir. Bu senaryoların her biri için ayrı bir fatura hesap gerekir.

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="understand-billing-account"></a>Fatura hesabı anlama

Microsoft Müşteri sözleşmesi için fatura hesabınıza faturaları ve ödeme yöntemlerini yönetmenize olanak sağlayan bir veya daha fazla fatura profillerini içerir. Her fatura profili fatura profilin fatura maliyetlerini düzenlemenize olanak sağlayan bir veya daha fazla fatura bölümleri içerir.

Aşağıdaki diyagramda faturalama hesabıyla faturalama profilleri ve fatura bölümleri arasındaki ilişki gösterilir.

![Microsoft Müşteri sözleşmesi için fatura hiyerarşisini gösteren diyagram](./media/billing-mca-overview/mca-billing-hierarchy.png)

Fatura hesabındaki rolleri, yüksek düzeyde izinlere sahiptir. Varsayılan olarak, yalnızca genel Yöneticiler, kuruluşunuzun Azure Active Directory'de fatura hesap erişim elde edin. Bu rolleri, faturaları görüntülemek ve Finans veya BT yöneticileri gibi kuruluşunuzun tamamı için maliyetleri izlemek için gereken kullanıcılara atanmalıdır. Daha fazla bilgi için [Faturalama hesabı rolleri ve görevleri](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="understand-billing-profiles"></a>Fatura profilleri anlama

Fatura ve ödeme yöntemlerinizi yönetme için bir faturalandırma profili kullanın. Bir aylık fatura Azure abonelik ve faturalandırma profili kullanılarak satın alınan diğer ürünler için oluşturulur. Fatura ödemek için ödeme yöntemleri kullanın.

Faturalandırma profili için fatura hesabınıza otomatik olarak oluşturulur. Ek faturalar ayarlanacak yeni faturalandırma profilleri oluşturabilirsiniz. Örneğin, farklı faturalar her departman veya proje için kuruluşunuzda isteyebilirsiniz.

Fatura profilin fatura maliyetlerini düzenlemek için fatura bölümleri de oluşturabilirsiniz. Azure abonelikleri ve bir fatura bölümü için satın alınan ürünleri giderleri bölümünde gösterilir. Tüm bölümler fatura için fatura profilin fatura ücretleri içerir.

Fatura profilleri rollerinde faturaları ve ödeme yöntemlerini görüntülemek ve yönetmek için izinlere sahip. Bu roller, faturalar gibi muhasebe ekibi üyelerinin, kuruluşunuzda ödeme kullanıcılar atayın. Daha fazla bilgi için [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="monthly-invoice-generated-for-each-billing-profile"></a>Her fatura profili için oluşturulan bir aylık fatura

Bir aylık fatura her fatura profili için fatura tarihi üzerinde oluşturulur. Faturanın önceki ay için tüm ücretleri içerir.

Faturayı görüntülemek, belgeleri ve e-posta, Azure portalında, gelecek fatura almak için ayarını değiştirin. Daha fazla bilgi için [bir Microsoft Müşteri sözleşmesi faturaları indirmesine](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

### <a name="invoices-paid-through-payment-methods"></a>Fatura ödeme yöntemlerini Ücretli

Her fatura profili, fatura ödemek için kullanılan, kendi ödeme yöntemleri vardır. Aşağıdaki ödeme yöntemleri desteklenir:

| Tür             | Tanım  |
|------------------|-------------|
|Azure KREDİLERİ    |  Krediler, faturayla ödeme yapmam gerekiyor tutarı hesaplamak için toplam faturalandırılan miktar otomatik olarak uygulanır. Daha fazla bilgi için [fatura profiliniz için Azure kredi bakiyesi izlemek](billing-mca-check-azure-credits-balance.md). |
|Onay veya kablo aktarımı | Alacak miktarı ödeyebilirsiniz faturası için onay veya kablo üzerinden aktarım. Fatura ödeme yönergeleri verilir. |

### <a name="control-azure-marketplace-and-reservation-purchases-by-applying-policies"></a>İlkeleri uygulayarak denetimi Azure Market ve rezervasyon satın alma

Bir faturalandırma profili kullanılarak gerçekleştirilen satın alma işlemleri denetlemek üzere ilkeler uygulayın. Azure ayırmaları ve Market ürünleri satın devre dışı bırakmak için ilkeler ayarlayabilir. İlkeleri uygulandığında, fatura profilinde fatura bölümleri için oluşturulan abonelikleri Azure ayırmaları ve Market ürünleri satın almak için kullanılamaz.

### <a name="allow-users-to-create-azure-subscriptions-by-enabling-azure-plans"></a>Azure planları etkinleştirerek Azure abonelikleri oluşturabilmesi kullanıcılara izin ver

Azure planları, faturalandırma profili oluşturduğunuzda otomatik olarak etkinleştirilir. Faturalandırma profili tüm fatura bölümlerde bu planları erişin. Fatura bölümüne erişimi olan kullanıcılar, Azure abonelikleri oluşturabilmesi için planlar kullanın. Faturalandırma profil için bir Azure planını etkinleştirilmediği sürece Azure abonelikleri oluşturabilmesi olamaz. Aşağıdaki Azure planları, Microsoft Müşteri sözleşmesi için hesapları faturalama desteklenir:

| Planlama             | Tanım  |
|------------------|-------------|
|Microsoft Azure-planı   | Kullanıcıların herhangi bir iş yükünü çalıştırmak üzere abonelik oluşturmasına izin verin. Daha fazla bilgi için [Microsoft Azure planlama](https://azure.microsoft.com/offers/ms-azr-0017g/) |
|Microsoft Azure geliştirme ve Test planlama | Geliştirme için kısıtlı abonelikler oluşturmak Visual Studio aboneleri izin verin veya test iş yükleri. Bu abonelik, Azure portalında daha düşük fiyatlar ve özel sanal makine görüntülerine erişim gibi avantajlardan yararlanabilirsiniz. Daha fazla bilgi için [Microsoft Azure geliştirme ve test için planlama](https://azure.microsoft.com/offers/ms-azr-0148g/)|

## <a name="understand-invoice-sections"></a>Fatura bölümleri anlama

Fatura profilin fatura maliyetlerini düzenlemek için fatura bölümler oluşturun. Örneğin, departman, takım veya proje maliyetlerini düzenlemek istiyorsanız ancak kuruluşunuz için tek bir fatura gerekebilir. Bu senaryoda, her bölüm, takım veya proje için fatura bölümü oluşturduğunuz tek bir fatura profili sahip.

Bir fatura bölümü oluşturulduğunda, diğerleri bölümü için Azure aboneliği oluşturma izni verebilirsiniz. Ardından tüm kullanım ücretleri ve abonelik satın alma işlemleri fatura uygun bölümüne yansıtılır.

Fatura bölümünde rolleri Azure abonelikleri oluşturan Denetim izinlerine sahip. Bu roller, takımların mühendislik müşteri adayları ve teknik mimarlar gibi kuruluşumuz için Azure ortamı ayarlama kullanıcılar atayın. Daha fazla bilgi için [fatura bölüm rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

Fatura hesabınıza hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure'da Microsoft Müşteri sözleşmesi yönetici rollerini anlama](billing-understand-mca-roles.md)
- [Microsoft Müşteri sözleşmesi için ek bir Azure aboneliği oluşturun](billing-mca-create-subscription.md)
- [Maliyetlerinizi düzenlemek için fatura bölümler oluşturma](billing-mca-section-invoice.md)