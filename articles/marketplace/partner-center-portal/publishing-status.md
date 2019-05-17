---
title: Kendi ticari Market teklifi Yayımlama durumunu denetleyin
description: Onay durumunu doğrulama, sertifika ve önizleme adımları ticari Market üzerinden bir teklif Microsoft Partner Center'da yayımlamak için gerekli.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 461c9f3f3725ba27410088ca19f1ec050375adf2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806154"
---
# <a name="check-the-publishing-status-of-your-commercial-marketplace-offer"></a>Kendi ticari Market teklifi Yayımlama durumunu denetleyin

Geçerli görüntüleyebileceğiniz **yayımlama durumu** üzerinde **genel bakış sunan** sekmesinde [ticari Marketi portalı](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) iş ortağı Merkezi'nde.

## <a name="automated-validation"></a>Otomatik doğrulama

Yayımlama sürecinde ilk adım, otomatik doğrulamaları kümesidir. Her doğrulama adımının teklifinizi oluşturulmasında etkinleştirmek için seçtiğiniz bir özelliğe karşılık gelir. Bu özellik etkinleştirilmediyse doğrulama yayımlama sonraki adıma devam atlar. Her doğrulama denetimi, yayımlama durumu onaylanmadan önce tamamlanmış olması gerekir.

- **Satın alma akış Kurulum sunar (> 10 dakika)**

Bu adımda, teklifinizi müşterilerin Azure Portalı aracılığıyla satın alımlarda olarak getirilmesi emin olun. Bu adım, yalnızca Microsoft satılan teklifler için geçerlidir.

- **Test sürücü veri doğrulama (~ 5 dakika)**

Bu adımda, biz test sürüşü teklif teknik yapılandırma bölümünde sağlanan verileri doğrulayın. Test sürücü işlevselliği test ve onaylandı. Bu adım yalnızca etkin bir test sürüşüne ile teklifler için geçerlidir.

- **Test sürücüsü (yaklaşık 30 dakika) sağlama**

Veri ve önceki adımda, test sürüşünüz işlevselliğini doğrulama sonra bu adımda dağıtmak ve test sürüşünüz örneklerini çoğaltmak, böylece bunlar müşterinin kullanıma hazır olur.  Bu adım yalnızca etkin bir test sürüşüne ile teklifler için geçerlidir.

- **Yönetim doğrulama ve kayıt sağlama (> 15 dakika)**

Bu adımda, müşteri adayı Yönetimi sisteminizde müşteri adayları teklif kurulumunda sağlanan Ayrıntılar temel alabilir onaylayın. Bu adım yalnızca sağlama yönetimi etkin teklifler için geçerlidir.

## <a name="certification"></a>Sertifika

Yayımlanmakta önce sertifikalı teklifler ticari Market iş ortağı Merkezi'nde gönderilen gerekir. Teklifler geçmeleri sıkı, bazı otomatik test ve diğerleri gibi bir denetimi el ile gönderilen [Azure Market sertifika ilkeleri](https://docs.microsoft.com/legal/marketplace/general-policies). Yayımlama akıştaki bir sonraki adıma devam etmeden önce gönderimleri için sertifika uygun işaretlenmelidir sunar.

### <a name="types-of-validation-that-take-place-during-certification"></a>Doğrulama sırasında sertifika gerçekleşmesi türleri

Gönderilen her teklif için sertifika işlemi dahil doğrulama üç düzeyi vardır.

- Yayımcı iş uygunluk
- İçerik doğrulama
- Teknik doğrulama

#### <a name="publisher-business-eligibility"></a>Yayımcı iş uygunluk

Her bir teklif türü bir dizi yayımcı karşılaması gereken temel uygunluk ölçütlerini denetler. Uygunluk ölçütleri, yayımcının MPN durumu, tutulan uzmanlıklar, uzmanlık düzeyi, vb. içerebilir.

#### <a name="content-validation"></a>İçerik doğrulama

İçerik doğrulaması sırasında teklifinizi oluştururken girdiğiniz bilgileri denetlenir kalite ve ilgi düzeyi. Bu denetimler, fiyatlandırma, kullanılabilirlik, ilişkilendirilmiş planları vb. ayrıntıları listeleme Market'e girdilerinizi gözden. Azure Market ve/veya Appsource'ta listeleme ölçütlerini karşılamak için size teklifinizi içerdiğini doğrular:

- Teklif doğru şekilde açıklayan bir başlık;
- kapsamlı bir genel bakış ve değer önerisi sağlar, iyi-yazılan açıklamaları;
- Kalite ekran görüntüleri ve videoları eşlik eden; ve
- Bu teklif Microsoft platformları ve araçlarıyla nasıl kullanacağını bir açıklama.

İçerik doğrulama ölçütlerini ile ilgili daha fazla bilgi edinmek [genel ilkeleri listeleyen](https://docs.microsoft.com/legal/marketplace/general-policies#10-general-listing-policies).

#### <a name="technical-validation"></a>Teknik doğrulama

Teknik doğrulama sırasında aşağıdaki denetimleri (paket veya ikili) indirim uygulanır.
- Kötü amaçlı yazılım için tarama
- İzlenen ağ çağrıları
- Analiz paketi
- Teklife ilişkin gerçek işlevselliğini tam tarama

Teklif sağlam olduğundan emin olmak için çeşitli platformlara ve sürümlere test edilir.

Bu belgenin teknik yapılandırma bölümü teklife için gereken belirli yapılandırma ayrıntılarını gözden geçirin.

### <a name="certification-failure-report"></a>Sertifika hatası raporu

Teklifinizi sertifika geçtiyse gözden geçirme tamamlandığında, ardından bu yayımlama sürecinde bir sonraki adıma taşır. Teklifinizi herhangi biri listeyi, teknik veya ilke denetimleri başarısız olursa veya bu tür bir teklif göndermek uygun değilse, bir sertifika hatası raporu oluşturulur ve size e-posta ile.

Bu rapor, gözden geçirme notları birlikte başarısız oldu. tüm ilkeleri açıklamalarını içerir. Bu e-posta raporunu gözden geçirin, teklifinizin güncelleştirmeler yapma, tüm sorunları ele gerektiği ve teklifi kullanarak yeniden [ticari Marketi portalı](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) iş ortağı Merkezi'nde. (Bir teklif olarak geçirme sertifika kadar gerektiği gibi birçok kez yeniden gönderebilirsiniz).

## <a name="preview-creation"></a>Önizleme oluşturma

Sırasında **oluşturma Önizleme** adım oluştururuz teklifinizi sürümünü teklifinizi Önizleme bölümünde belirtilen hedef kitle için erişilebilir.

## <a name="publisher-approval"></a>Yayımcı onayı

Bu adımda, gözden geçirin ve teklif Önizleme son yayımlama adım önce onaylamanız size bir istekle almayacağınızı.

Teklifinizi Microsoft yoluyla satmak için seçtiyseniz, edinme ve teklifinizi, bu Önizleme onay aşamasında gereksinimlerinizi karşıladığından emin olmak için dağıtımı test etmek mümkün olacaktır. Teklifinizin henüz pubic Market'te kullanılabilir olmaz. Test ve bu Önizleme onaylama sonra seçmeniz gerekecek **Go-Live** üzerinde [ **genel bakış sunan** ](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) Pano.

Bu önizleme aşamasında teklif değişiklik yapmak istiyorsanız, düzenleyin ve yeni bir önizleme yayımlamak için yeniden gönderin. Makaleye göz atın [mevcut Marketi tekliflerini güncelleştirme](#update-existing-marketplace-offers) daha fazla değişiklik hakkında ayrıntılı bilgi için.

Teklifinizi zaten canlı ve Market'te genel erişime açık ise, siz seçene kadar yaptığınız tüm güncelleştirmeleri Canlı gitmiyor **Go-live** üzerinde [ **genel bakış sunan** ](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) Pano.

### <a name="publish-offer-to-the-public"></a>Ortak teklifi yayımlama

İş ortağı merkezi oturum açın ve teklife erişebilirsiniz. İçin yönlendirilirsiniz **genel bakış sunan** sayfası. Bu sayfanın en üstünde bir seçenek görürsünüz **yayınlayın**. Seçin **yayınlayın,** ve onayladıktan sonra teklifin ortak yayımlanan başlar. Teklif Canlı hale geldiğinde bir e-posta bildirimi alırsınız.

## <a name="publish"></a>Yayımlama

İçin seçmiş **yayınlayın** teklifiniz Market'te kullanılabilir hale vardır, bir dizi aracılığıyla Canlı teklif yalnızca önizleme gibi yapılandırıldığından emin olmak için basamaklı nihai doğrulama denetimleri Teklifin sürümü.

- **Satın alma akış Kurulum sunar (> 10 dakika)**

Bu adımda, teklifinizi müşterilerin Azure Portalı aracılığıyla satın alımlarda olarak getirilmesi emin olun. Bu adım, yalnızca Microsoft satılan teklifler için geçerlidir.

- **Test sürücü veri doğrulama (~ 5 dakika)**

Bu adımda, biz test sürüşü teklif teknik yapılandırma bölümünde sağlanan verileri doğrulayın. Test sürücü işlevselliği test ve onaylandı. Bu adım yalnızca etkin bir test sürüşüne ile teklifler için geçerlidir.

- **Test sürücüsü (yaklaşık 30 dakika) sağlama**

Bu adımda, dağıtmak ve test sürüşünüz örneklerini çoğaltmak, böylece bunlar müşterinin kullanıma hazır olur.  Bu adım yalnızca etkin bir test sürüşüne ile teklifler için geçerlidir.

- **Yönetim doğrulama ve kayıt sağlama (> 15 dakika)**

Bu adımda, müşteri adayı Yönetimi sisteminizde müşteri adayları teklif kurulumunda sağlanan Ayrıntılar temel alabilir onaylayın. Bu adım yalnızca sağlama yönetimi etkin teklifler için geçerlidir.

- **Son yayımlama (> 30 dakika)**

Bu adımda, teklifinizi Market'te genel olarak kullanılabilir duruma emin olun.

## <a name="update-existing-marketplace-offers"></a>Var olan Market teklifleri güncelleştir

Zaten yayımlanan bir teklifi için değişiklik yapmak istiyorsanız, önce mevcut teklifte güncelleştirin ve sonra tekrar yayımlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bir mevcut teklifi güncelleştirme](./update-existing-offer.md)
