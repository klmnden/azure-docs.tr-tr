---
title: Azure harcama sınırı | Microsoft Docs
description: Bu makalede, bir Azure harcama sınırının işleyişi ve nasıl kaldırılacağı açıklanır.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: fb43f29827309fc8986ee6b4653f5edf303cc21d
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490605"
---
# <a name="azure-spending-limit"></a>Azure harcama sınırı

Azure harcama limiti, kredi tutarı harcama engeller. Azure deneme sürümünü veya birden çok aya yayılan kredi içeren teklifler için kaydolun tüm yeni müşteriler varsayılan olarak açık harcama limiti vardır. Harcama sınırı $0 olan. Değiştirilemez. Harcama limiti, bazı abonelikler planları için söz konusu taahhüt planları ve planları Kullandıkça Öde fiyatlandırması ile kullanılamaz. Bkz: [tam listesini Azure tekliflerini ve harcama limiti kullanılabilirliğini](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Harcama limiti ulaşma

Kullanım sonuçlarınızda aylık miktarı harcadıklarını ücretleri Azure aboneliğinize dahil, dağıttığınız hizmetler söz konusu fatura dönemindeki kalan için devre dışı bırakıldı.

Örneğin, aboneliğinize dahil olan tüm kredi harcadığınız, dağıttığınız Azure kaynaklarına üretimden kaldırılır ve Azure sanal makineleriniz durdurulur ve serbest. Veriler depolama hesaplarınızda, salt okunur olarak kullanılabilir.

Abonelik teklifinizi birden çok aya yayılan kredi içeriyorsa, bir sonraki fatura döneminin başında aboneliğiniz otomatik olarak yeniden etkinleştirildi. Ardından Azure kaynaklarınızla bağlantınızı yeniden dağıtın ve depolama hesaplarınız ile veritabanlarınıza tam erişim sağlama.

Azure aboneliğinizin harcama ulaştığında bildirimleri sınırlamak e-posta gönderir. Oturum açma için [hesap Merkezi](https://account.windowsazure.com/Subscriptions) harcama limitine ulaşan abonelikler hakkındaki bildirimleri görmek için.

Ücretsiz deneme aboneliğinde olması ve harcama sınırına ulaştığında, bir plan ile yükseltebilirsiniz [Kullandıkça Öde](billing-upgrade-azure-subscription.md) sınırlamak harcama kaldırmak için fiyatlandırma ve aboneliği otomatik olarak etkinleştirir.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Hesap Merkezi'nde harcama limitini kaldırın

Azure aboneliğinizle ilişkili geçerli ödeme yöntemine var olduğu sürece, harcama sınırını istediğiniz zaman kaldırabilirsiniz. Birden çok aya yayılan kredi içeren Tekliflere, sonraki faturalama dönemi başında harcama limiti de etkinleştirebilirsiniz.

Harcama limitini kaldırmak için şu adımları izleyin:

1. Oturum açma için [hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Bir abonelik seçin. Abonelik, harcama sınırına ulaşılması nedeniyle devre dışı bırakıldıysa, bildirime tıklayın: **Abonelik, harcama sınırına ulaştı ve ücretleri önlemek için devre dışı bırakıldı.** ' A tıklayıp **harcama sınırını Kaldır** içinde **ABONELİK durumu** alan.
1. Size uygun bir seçenek belirleyin.

![Harcama sınırını Kaldır seçeneği](./media/billing-spending-limit/remove-spending-limit.PNG)

| Seçenek | Etki |
| --- | --- |
| Harcama limitini süresiz olarak kaldırma | Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılmaz. |
| Geçerli fatura dönemi için harcama limitini kaldırma | Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılır. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Neden harcama limitini kaldırmak isteyebilirsiniz

Harcama limiti dağıtma veya belirli bir üçüncü taraf ve Microsoft hizmetlerini kullanarak engelleyebilir. Burada bazı durumlarda Burada, aboneliğinizde harcama limitini kaldırmanız gerekir.

-  Oracle ve Azure DevOps Hizmetleri gibi hizmetleri gibi birinci taraf görüntülerini dağıtımını yapmayı planlıyorsunuz. Bu durum, harcama limitiniz neredeyse anında aşılacak neden olur ve aboneliğiniz devre dışı bırakılmasına neden olur.
- Kesintiye istemediğiniz hizmetleriniz var.
- Sanal IP adresleri gibi ayarlar içeren, kaybetmek istemediğiniz hizmet ve kaynaklarınız var. Hizmetler ve kaynaklar serbest olduğunda bu ayarlar kaybolur.

## <a name="turn-on-the-spending-limit-after-removing"></a>Kaldırdıktan sonra harcama sınırını Aç

Bu özellik yalnızca harcama limitini süresiz olarak kaldırılmış olsa kullanılabilir. Bir sonraki fatura döneminin başlangıcında otomatik olarak açmak için değiştirin.

1. Oturum açma için [hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Harcama sınırı seçeneğini değiştirmek için sarı başlığa tıklayın.
1. Seçin **sınırı, sonraki fatura döneminde harcama kapatma \<başlangıç tarihi, faturalama dönemi\>**

## <a name="custom-spending-limit"></a>Harcama limiti özel

Özel harcama sınırları mevcut değildir.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Harcama limiti, tüm ücretleri engellemez

[Dış Bazı hizmetler Azure Market'te yayımlanan](billing-understand-your-azure-marketplace-charges.md) abonelik kredilerinizi kullanılamaz ve harcama limitinizi ayarlandığında bile ayrı bir ücrete tabi. Visual Studio lisansları, Azure Active Directory premium, destek planları ve çoğu üçüncü-taraf markalı hizmetlerin örneklerindendir. Yeni bir dış hizmeti sağladığınızda, hizmetler ayrı olarak faturalandırılan bildiren bir uyarı gösterilir:

![Market satın alma Uyarısı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Bir plan ile yükseltme [Kullandıkça Öde](billing-upgrade-azure-subscription.md) fiyatlandırma.
