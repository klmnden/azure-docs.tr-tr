---
title: Azure uygulama teklifi yayımlama | Azure Market
description: İşlem ve Azure uygulaması Azure Marketi'nde teklif yayımlamak için gereken adımları açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: pabutler
ms.openlocfilehash: 2326ce1a591d1276dbaf9c7f3238f7214e5134ab
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942901"
---
# <a name="publish-azure-application-offer"></a>Azure uygulama teklifini yayımlama

Şirket bilgileri sağlayarak bir teklif oluşturduktan sonra **yeni teklif** sayfasında teklif yayımlayabilirsiniz. Seçin **Yayımla** yayımlama işlemini başlatmak için.

Aşağıdaki diyagramda "Canlı gitmek" bir teklif için yayımlama işlemi ana adımları gösterir.

![Teklif yayımlama adımları](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımları ayrıntılı bir açıklaması

Aşağıdaki tabloda, listeler ve her yayımlama adımlarını açıklar ve her adımı tamamlamak için tahmini süre sağlar.  "Gün" iş günü tanımlanan tahmin, hafta sonları ve tatiller hariç tutun.

|  **Yayımlama Adım**           | **saat**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | < 15 dk    | Bilgi sunan ve ayarlar doğrulanır sunar.                        |
| Gelir etkileyen ayarları doğrulama | < 15 dk  | Azure kaynak kullanım attribution teklif için denetlenir.             |
| Sertifika                  | < 1 gün     | Teklif, Azure sertifika ekibi tarafından analiz edilir. Bu teklif, virüsler, kötü amaçlı yazılım, emniyet uyumluluk ve güvenlik sorunları için taranır. Bu teklif, tüm uygunluk ölçütlerini karşıladığını görmek için denetlenir. Daha fazla bilgi için [önkoşulları](./cpp-prerequisites.md). Bir sorun bulunursa geri bildirim sağlanır. |
| Sürücü doğrulama testi          | < 2 saat   | (İsteğe bağlı) Bir Test sürüşüne varsa, Microsoft, dağıtılan çoğaltılan ve olduğunu doğrular.  |
| Paketleme ve müşteri adayı oluşturma kaydı | 1 saatten az  | Teklife ilişkin teknik varlıkları müşteri kullanılmak üzere hazırlanmıştır ve müşteri adayı sistemleri yapılandırılmalı ve dağıtılmalıdır. |
|  Yayımcı oturum kapatma             |  El ile    | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklif Önizleme için kullanıma sunulmuştur.  Teklifinizi (adımlarda teklif bilgi) seçili Aboneliklerdeki tüm gereksinimleri karşıladığından emin doğrulamak için dağıtabilirsiniz.  Teklif doğruladıktan sonra seçin **Go Live** için teklifinizi sonraki adıma geçebilirsiniz. |
| Microsoft gözden geçirme                | 7 - 14 gün | Bütünlüklü olarak Microsoft Azure uygulamanızı inceler ve sorunları bulunursa size e-posta.  Bu adımı uzunluğunu uygulama, sorunları ortaya çıkardı ve onlara nasıl kısa bir süre içinde yanıt karmaşıklığına bağlıdır.  |
| Canlı                           | < 1 gün | Teklif serbest, belirli bölgelerde çoğaltılır ve genel kullanıma sunulan. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

Yayımlama işlemini izleyebilirsiniz **durumu** bulut iş ortağı Portalı'nda teklifinizi için sekmesinde.

![Bir Azure uygulaması teklif durumu sekmesi](./media/offer-status-tab.png)

Yayımlama işlemini tamamladıktan sonra teklifinizi listelenir [Microsoft Azure Marketi'nde uygulama kategorisi](https://azuremarketplace.microsoft.com/marketplace/apps/).

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](../../cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Teklifiniz, Yayımlama durumunu görüntüleme yanı sıra **durumu** sekmesi de hata iletileri ve nerede bir sorunla karşılaştık. herhangi bir yayımlama adım görüşleri görüntüler.  Sorun önemlidir, ardından yayımlama iptal edildi.  Ardından, bildirilen sorunları düzeltin ve teklif yeniden yayımlamanız gerekir.  Çünkü **Microsoft gözden geçirme** adımı temsil eden ilgili teknik varlıkları (özellikle Azure Resource Manager şablonu) teklifiniz ve kapsamlı bir gözden geçirme, sorunları çekme isteği (PR) bağlantıları gibi tipik olarak sunulur.  Görüntülemek ve bu Pr'ler için yanıt vermek bir açıklama görmek [işleme İnceleme geri](./cpp-handling-review-feedback.md).


## <a name="next-steps"></a>Sonraki adımlar

Bir veya daha yayımlama adımları hatalarla, bunları düzeltin ve teklifinizi yeniden yayımlayın.  Kritik sorunlar karşılaşılırsa **Microsoft gözden geçirme** adım gerekir [gözden geçirme geri bildirimi işlemek](./cpp-handling-review-feedback.md) Microsoft erişerek ekibin Azure DevOps deposunda gözden geçirin.

Bir Azure uygulamasına başarıyla yayımlandıktan sonra yapabilecekleriniz [mevcut teklifi güncelleştirme](./cpp-update-existing-offer.md) değişen iş ya da teknik gereksinimlerine yansıtacak şekilde. 
