---
title: Azure harcama sınırı anlama | Microsoft Docs
description: Azure harcama sınırı nasıl çalıştığını ve nasıl kaldırılacağı açıklanır
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 6049e3614b63bfabee6721dcaa83008eb3306493
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069991"
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Azure harcama sınırı ve nasıl kaldırılacağı anlama

Harcama sınırı Azure kredi tutarın üzerinde harcama önlemek için mevcut. Kaydolan deneme veya birden çok ay içinde krediler içeren teklifler için tüm yeni müşteriler varsayılan olarak açıktır harcama sınırı vardır. Harcama sınırını $0'dır. Değiştirilemez. Harcama limiti; Kullandıkça Öde abonelikleri ve taahhüt planları gibi abonelik türlerinde kullanılamaz. Bkz: [Azure teklifleri ve harcama sınırını kullanılabilirliğini tam listesi](https://azure.microsoft.com/support/legal/offer-details/).

**Uyarıları faturalama için mi arıyorsunuz?** Bkz: [Azure abonelikler için faturalama veya kredi uyarıları ayarlama](billing-set-up-alerts.md).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Harcama limitine ulaştığımda ne olur?

Dağıttığınız Hizmetleri aboneliğinizle kullanım sonuçlarınızda aylık tutarlar tüketebilir ücretleri dahil edilirse, bu fatura dönemi geri kalanı için devre dışı bırakılır. 

Örneğin, aboneliğinizde yer alan tüm kredi harcadığı zaman dağıttığınız bulut Hizmetleri üretimden kaldırılır ve Azure sanal makinelerinizi durduruldu ve XML'deki ayrılmış. Depolama hesapları ve veritabanları veriler salt okunur bir biçimde kullanılabilir.

Abonelik teklifiniz krediler birden fazla ay içeriyorsa, sonraki fatura döneminde başlangıcında, aboneliğinizi otomatik olarak yeniden etkinleştirilmesi. Ardından, bulut hizmetlerinize dağıtmanız ve depolama hesapları ve veritabanları tam erişimi vardır.

Aboneliğiniz harcama sınırına olduğunda size e-posta bildirimleri göndereceğiz. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/Subscriptions), ve bildirimleri harcama limitine ulaştığınız abonelikleri hakkında bölümüne bakın.

Ücretsiz deneme sürümü varsa ve harcama sınırına ulaştığında, şunları yapabilirsiniz [Kullandıkça Öde yükseltme](billing-upgrade-azure-subscription.md) harcama sınırını kaldırmak ve aboneliğiniz otomatik olarak yeniden etkinleştirilebilir.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Hesap Merkezi'nde harcama limitini kaldırın

Aboneliğinizle ilişkili geçerli bir ödeme yöntemi olduğu sürece, harcama limitini istediğiniz zaman kaldırabilirsiniz. Birden çok ay içinde iade sahip teklifleri için harcama sınırı, sonraki fatura döneminde başında yeniden etkinleştirebilirsiniz.

Harcama limitini kaldırmak için şu adımları izleyin:

1. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/Subscriptions).
1. Bir abonelik seçin.
. Aboneliğin harcama sınırına ulaşıldı nedeniyle devre dışıysa, bu bildirime tıklayın: "Abonelik harcama sınırına ulaştı ve Ücret oluşmasını önlemek için devre dışı bırakıldı." Aksi takdirde tıklatın **harcama sınırı Kaldır** içinde **ABONELİK durumunu** alanı.
1. Size uygun bir seçenek belirleyin.

![Harcama sınırı kaldırmak için bir seçenek seçme](./media/billing-spending-limit/remove-spending-limit.PNG)

|Seçenek|Etki|
|-------|-----|
|Harcama limitini süresiz olarak kaldırma|Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılmaz.|
|Geçerli fatura dönemi için harcama limitini kaldırma|Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılır.|

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Neden için harcama sınırını kaldırmak istiyor?

Harcama sınırını dağıtma veya bazı üçüncü taraf ve Microsoft hizmetlerini kullanarak engelleyebilir. Aboneliğinizdeki harcama limitini kaldırmanızı gerektiren senaryolar aşağıda belirtilmiştir.

* Oracle gibi birinci taraf görüntülerini ve Visual Studio Team Services gibi hizmetleri dağıtmayı planlıyorsunuz. Bu senaryo, harcama limitine hemen aşmasına neden olur ve aboneliğiniz devre dışı bırakılmasına neden olur.
* Kesintiye uğramaması gereken hizmetleriniz var.
* Sanal IP adresleri gibi ayarlar içeren, kaybetmek istemediğiniz hizmet ve kaynaklarınız var. Hizmet ve kaynakları serbest ayrılmış olduğunda bu ayarları kaybolur.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Kaldırmadan sonra harcama sınırı üzerinde nasıl kapatırım?

Bu özellik yalnızca harcama sınırını süresiz olarak kaldırıldığında kullanılabilir. Sonraki fatura döneminde başlangıcında otomatik olarak Aç şekilde değiştirin.

1. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/Subscriptions).
1. Harcama sınırı seçeneğini değiştirmek için sarı başlığını tıklatın.
1. Seçin **sınırı sonraki fatura döneminde harcama kapatma \<başlangıç tarihi, faturalama dönemi\>**

### <a name="how-do-i-set-a-custom-spending-limit"></a>Özel bir harcama sınırı nasıl ayarlarım?

Size özel sınırları bugün harcamanız yok. Ancak, siz de seçebilirsiniz [denetlemek için faturalama uyarılarını kullanın, harcamanız](billing-set-up-alerts.md).

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Harcama sınırını azure'dan tüm ücret oluşmasını önlemek mu?

[Bazı dış hizmetler Azure Marketi'nde yayımlanan](billing-understand-your-azure-marketplace-charges.md) abonelik kredinizi kullanılamaz ve hatta harcama limitine ayarlandığında ayrı bir ücrete tabi. Visual Studio lisansları, Azure Active Directory premium, destek planları ve Hizmetleri markalı çoğu üçüncü taraf örnek verilebilir. Yeni bir dış hizmet sağladığınızda Hizmetleri ayrı ayrı faturalandırılır size bildirmek için bir uyarı gösterilir:

![Market satın uyarı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
