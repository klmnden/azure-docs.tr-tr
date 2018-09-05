---
title: Azure harcama sınırı anlama | Microsoft Docs
description: Azure harcama sınırı nasıl çalıştığı ve nasıl kaldırılacağı açıklanmaktadır.
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
ms.date: 06/15/2018
ms.author: genli
ms.openlocfilehash: 448622f0406eb709c8d94d60722edb4ef00f42de
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669923"
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Azure harcama limiti ve nasıl kaldırılacağını anlama

Azure harcama, kredi tutarı harcama önlemek için mevcut. Birden çok aya yayılan kredi içeren tekliflere ve deneme için kaydolun tüm yeni müşteriler varsayılan olarak açık harcama limiti vardır. Harcama sınırı $0 olan. Değiştirilemez. Harcama limiti; Kullandıkça Öde abonelikleri ve taahhüt planları gibi abonelik türlerinde kullanılamaz. Bkz: [tam listesini Azure tekliflerini ve harcama limiti kullanılabilirliğini](https://azure.microsoft.com/support/legal/offer-details/).

**Fatura uyarılarını mi arıyorsunuz?** Bkz: [Azure abonelikleri için fatura veya kredi uyarıları ayarlama](billing-set-up-alerts.md).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Harcama limitine ulaştığımda ne olur?

Dağıttığınız Hizmetleri aboneliğinizle aylık miktarı harcadıklarını ücretleri kullanım sonuçlarınızda dahil edilirse, söz konusu fatura dönemindeki kalan için devre dışı bırakılır. 

Örneğin, aboneliğinize dahil olan tüm kredi harcadığınız, dağıttığınız bulut Hizmetleri üretimden kaldırılır ve Azure sanal makineleriniz durdurulur ve serbest. Depolama hesaplarınız ve veritabanlarınızdaki verilerinde bir salt okunur şekilde kullanılabilir.

Abonelik teklifinizi birden çok aya yayılan kredi içeriyorsa, bir sonraki fatura döneminin başında, aboneliğiniz otomatik olarak yeniden etkinleştirilmesi. Ardından bulut hizmetlerinizi yeniden dağıtın ve depolama hesaplarınız ile veritabanlarınıza tam erişim sağlama.

Aboneliğinizin harcama sınırına ulaştığınızda e-posta bildirimleri göndereceğiz. Oturum [hesap Merkezi](https://account.windowsazure.com/Subscriptions), ve harcama limitine ulaşan abonelikler hakkındaki bildirimleri görebilirsiniz.

Ücretsiz deneme sürümünüz ve harcama sınırına ulaştığında, yapabilecekleriniz [Kullandıkça Öde aboneliğine yükseltme](billing-upgrade-azure-subscription.md) harcama limitini kaldırın ve aboneliğiniz otomatik olarak yeniden etkinleştirildi.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Hesap Merkezi'nde harcama limitini kaldırın

Aboneliğinizle ilişkili geçerli bir ödeme yöntemi olduğu sürece, harcama limitini istediğiniz zaman kaldırabilirsiniz. Birden çok aya yayılan kredi içeren tekliflere da harcama limitini sonraki faturalama dönemi başında yeniden etkinleştirebilirsiniz.

Harcama limitini kaldırmak için şu adımları izleyin:

1. Oturum [hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Bir abonelik seçin. Abonelik, harcama sınırına ulaşılması nedeniyle devre dışı ise bu bildirime tıklayın: "Abonelik harcama sınırına ulaştı ve ücretleri önlemek için devre dışı bırakıldı." ' A tıklayıp **harcama sınırını Kaldır** içinde **ABONELİK durumu** alan.
1. Size uygun bir seçenek belirleyin.

![Harcama sınırını Kaldır seçeneği](./media/billing-spending-limit/remove-spending-limit.PNG)

|Seçenek|Etki|
|-------|-----|
|Harcama limitini süresiz olarak kaldırma|Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılmaz.|
|Geçerli fatura dönemi için harcama limitini kaldırma|Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılır.|

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Neden harcama limitini kaldırmak istiyor?

Harcama limiti dağıtma veya belirli bir üçüncü taraf ve Microsoft hizmetlerini kullanarak engelleyebilir. Aboneliğinizdeki harcama limitini kaldırmanızı gerektiren senaryolar aşağıda belirtilmiştir.

* Oracle gibi birinci taraf görüntülerini ve Visual Studio Team Services gibi hizmetleri dağıtmayı planlıyorsunuz. Bu senaryo, harcama limitiniz neredeyse anında aşılacak neden olur ve aboneliğiniz devre dışı bırakılmasına neden olur.
* Kesintiye uğramaması gereken hizmetleriniz var.
* Sanal IP adresleri gibi ayarlar içeren, kaybetmek istemediğiniz hizmet ve kaynaklarınız var. Hizmetler ve kaynaklar serbest olduğunda bu ayarlar kaybolur.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Harcama limitini kaldırmadan sonra üzerinde nasıl kapatırım?

Bu özellik yalnızca harcama limitini süresiz olarak kaldırılmış olsa kullanılabilir. Bir sonraki fatura döneminin başlangıcında otomatik olarak açmak için değiştirin.

1. Oturum [hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Harcama sınırı seçeneğini değiştirmek için sarı başlığa tıklayın.
1. Seçin **sınırı, sonraki fatura döneminde harcama kapatma \<başlangıç tarihi, faturalama dönemi\>**

### <a name="how-do-i-set-a-custom-spending-limit"></a>Özel bir harcama sınırına nasıl ayarlayabilirim?

Size özel sınırları bugün harcama yok. Bununla birlikte, kabul etmek [denetlemek için faturalama uyarılarını kullanın, harcama](billing-set-up-alerts.md).

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Harcama limiti azure'dan tüm ücretlerden mu?

[Dış Bazı hizmetler Azure Market'te yayımlanan](billing-understand-your-azure-marketplace-charges.md) abonelik kredilerinizi kullanılamaz ve harcama limitinizi ayarlandığında bile ayrı bir ücrete tabi. Visual Studio lisansları, Azure Active Directory premium, destek planları ve çoğu üçüncü-taraf markalı hizmetlerin örneklerindendir. Yeni bir dış hizmeti sağladığınızda, hizmetler ayrı olarak faturalandırılan bildiren bir uyarı gösterilir:

![Market satın alma Uyarısı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
