---
title: Kullanım ve maliyet uyarılarla harcama izleme | Microsoft Docs
description: Nasıl Yardım maliyet uyarılar bu makalede kullanımı ve Azure maliyet Yönetimi'nde harcamayı izleyin.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: alavital
ms.custom: ''
ms.openlocfilehash: f1bf62596b6edcc6fff6572e431f3a777be93f05
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002086"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Kullanımı ve harcamayı izleyin maliyet uyarılarını kullanın

Bu makale anlamak ve Azure'ı izlemek için maliyet Yönetimi uyarılar kullanmanıza yardımcı olur. kullanımı ve harcamayı. Azure kaynakları tüketilir alarak maliyet uyarıları otomatik olarak oluşturulur. Uyarılar, maliyet yönetimi ve fatura uyarıları birlikte tek bir yerde tüm etkin gösterir. Tüketiminizi belirli bir eşiğe ulaştığında, maliyet Yönetimi tarafından uyarılar oluşturulur. Maliyet uyarılar üç tür vardır: Bütçe uyarıları, kredi uyarılar ve harcama kotası uyarılar bölümü.

## <a name="budget-alerts"></a>Bütçe uyarıları

Bütçe uyarılar size bildirir, harcama, kullanım veya maliyet göre ulaşır ya da tanımlanmış aşıyor [Uyarı koşulu bütçenin](tutorial-acm-create-budgets.md). Maliyet Yönetimi bütçeleri, Azure portalını kullanarak oluşturulur veya [Azure tüketim](https://docs.microsoft.com/rest/api/consumption) API.

Azure portalında bütçelerini maliyet tarafından tanımlanır. Azure tüketim API'sini kullanarak, bütçeler, tüketim, kullanım veya maliyet tarafından tanımlanır. Bütçe uyarıları hem maliyeti hem de kullanım tabanlı bütçelerini destekler. Bütçe uyarısı koşullar karşılandığında her bütçe uyarıları otomatik olarak üretilir. Azure portalında tüm maliyet uyarıları görüntüleyebilirsiniz. Bir uyarı oluşturulduğu zaman içinde maliyet uyarılar gösterilmektedir. Uyarı e-posta, bütçe uyarı alıcılarının listesinde kişilere de gönderilir.

## <a name="credit-alerts"></a>Kredi uyarılar

Azure kredisi, parasal taahhütler tüketilen kredi uyarılar size bildirir. Kurumsal anlaşmalar bir kuruluşlarıyla parasal taahhütler içindir. Kredi Uyarıları, % 90 ve Azure kredi bakiyeniz, % 100 otomatik olarak üretilir. Bir uyarı oluşturulduğu zaman maliyet uyarıları ve hesap sahipleri için gönderilen e-posta yansıtılır.

## <a name="department-spending-quota-alerts"></a>Departman harcama kotası uyarıları

Departman harcama kotası sabit bir eşiğe ulaştığında departmanı harcama kotası uyarılar sizi bilgilendirir. Harcama kotalarını EA Portalı'nda yapılandırılır. Bir eşiği yerine getirildiğinde departmanı sahiplerine e-posta oluşturur ve maliyet uyarılar gösterilir. Örneğin, % 50 veya kotanın %75.

## <a name="supported-alert-features-by-offer-categories"></a>Desteklenen teklif kategorilere göre uyarı özellikleri

Uyarı türleri için destek (Microsoft teklif) sahip bir Azure hesabı türüne bağlıdır. Aşağıdaki tabloda, desteklenen uyarı özellikleri gösterilmektedir. tarafından çeşitli Microsoft sunar. Microsoft teklifleri tam listesini görüntüleyebileceğiniz [anlamak maliyet Yönetimi verilerine](understand-cost-mgt-data.md).

| Uyarı türü | Kurumsal Anlaşma | Microsoft Müşteri Sözleşmesi | Web direct/kullandıkça-As-You-Öde |
|---|---|---|---|
| Bütçe | ✔ | ✔ | ✔ |
| Kredi | ✔ |✘ | ✘ |
| Departman harcama kotası | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Uyarıları maliyet görüntüle

Maliyet uyarıları görüntülemek için Azure portal ve select istenen kapsama açın **bütçelerini** menüsünde. Kullanım **kapsam** zehirli farklı bir kapsama geçin. Seçin **uyarılar maliyet** menüsünde. Kapsamlar hakkında daha fazla bilgi için bkz: [anlayın ve kapsamlı iş](understand-work-scopes.md).

![Uyarılar, maliyet Yönetimi'nde gösterilen örnek görüntüsü](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Etkin ve kapatılmış uyarıların toplam sayısı maliyeti uyarılar sayfasında görünür.

Uyarı türünün tüm uyarıları gösterir. Bütçe uyarısı nedeni neden oluşturuldu ve geçerli bütçe adını gösterir. Her uyarı, oluşturulduğu, tarihi gösterir, durumu ve uyarının uygulandığı kapsam (abonelik veya yönetim grubu).

Olası durumlar **etkin** ve **kapatıldı**. Etkin durumu, uyarı hala geçerli olduğunu gösterir. Uyarı artık ilgili olarak ayarlanacak birisi işaretlenmiş kapatıldı durumu gösterir.

Bir uyarının ayrıntılarını görüntülemek için listeden seçin. Uyarı ayrıntıları uyarı hakkında daha fazla bilgi gösterir. Bütçe uyarılar, bütçe bağlantısını içerir. Bir bütçe uyarısı için bir öneri varsa, öneri bağlantısını da gösterilir. Bütçe, kredi ve departman harcama kotası uyarıları burada uyarının kapsam maliyetlerini keşfedebilirsiniz maliyet analizi analiz etmek için bir bağlantı vardır. Aşağıdaki örnek, uyarı ayrıntılarını içeren bir bölüm için harcama gösterir.

![Uyarı ayrıntıları içeren bir bölüm için harcama gösteren örnek resim](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Kapatılmış bir uyarının ayrıntılarını görüntülediğinizde, el ile gerçekleştirilen eylem gerektiğinde yeniden etkinleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Örnek görüntü gösterme kapatın ve yeniden etkinleştirme seçenekleri](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Ayrıca bkz.

- Zaten bir bütçe oluşturmadınız ve bütçe uyarısı koşulları ayarlayın, tamamlamak [oluştur ve bütçelerini yönetmek](tutorial-acm-create-budgets.md) öğretici.
