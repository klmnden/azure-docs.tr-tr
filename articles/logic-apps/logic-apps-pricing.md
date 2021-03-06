---
title: Fiyatlandırma ve faturalandırma - Azure Logic Apps | Microsoft Docs
description: Fiyatlandırma ve faturalama için Azure Logic Apps nasıl çalıştığını öğrenin
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 05/22/2019
ms.openlocfilehash: 04b1d0eda85972517155f80488ad590fb56619ab
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190675"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure Logic Apps fiyatlandırma modeli

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) oluşturun ve otomatik tümleştirmesi iş akışlarınızı çalışan yardımcı olur, bulutta ölçeklendirilebilir. Bu makalede, Azure Logic Apps için faturalama ve fiyatlandırma nasıl çalıştığı açıklanır. NET fiyatlandırma bilgileri için bkz. [Azure Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli

Genel veya "Genel" Azure Logic Apps hizmetinde çalışan yeni logic apps için yalnızca kullandığınız kadarı için ödeme yaparsınız. Bu mantıksal uygulamalar, bir tüketim tabanlı plan ve fiyatlandırma modeli kullanın. Mantıksal uygulama tanımında her adımı bir eylemdir. Örneğin, eylemler şunlardır:

* Özel eylem tetikler. Tüm mantıksal uygulamalar, ilk adım olarak bir tetikleyici gerektirir.
* "Yerleşik" ya da yerel gibi eylemler HTTP, Azure işlevleri ve API Management ve benzeri çağrıları
* Outlook 365, Dropbox vb. gibi bağlayıcılarda çağrıları
* Döngüler, koşullu ifadeleri gibi akış adımları denetim vb.

Azure Logic Apps, mantıksal uygulamanızı çalıştıran tüm eylemleri ölçümleri. Faturalandırma nasıl çalıştığı hakkında daha fazla bilgi [Tetikleyicileri](#triggers) ve [eylemleri](#actions).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Sabit fiyatlandırma modeli

Bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturmak ve bir Azure sanal ağdaki kaynaklara erişebilen mantık uygulamaları çalıştırmak, özel, yalıtılmış ve adanmış bir yol sağlar. İçinde bir işe çalışan yeni logic apps için ödeme yaptığınız bir [aylık sabit fiyat](https://azure.microsoft.com/pricing/details/logic-apps) yerleşik Eylemler, tetikleyiciler ve ayrıca standart bağlayıcılar.

İŞE kadar içeren bir ücretsiz Kurumsal bağlayıcı ayrıca içerir *bağlantıları* istediğiniz. Ek Kurumsal bağlayıcılar kullanımı temel alınarak ücretlendirilir [Kurumsal tüketim fiyatı](https://azure.microsoft.com/pricing/details/logic-apps). Yalnızca genel olarak kullanılabilir Kurumsal bağlayıcılar Kurumsal tüketim fiyatı Ücret ödersiniz. Genel Önizleme Kurumsal bağlayıcılar ücretlendirilir [standart bağlayıcı oranı](https://azure.microsoft.com/pricing/details/logic-apps).

> [!NOTE]
> Görüntü bir işe, yerleşik tetikleyiciler ve Eylemler içinde **çekirdek** etiket ve logic apps ile aynı ıse'de çalıştırın. Standart ve görüntüleme Kurumsal Bağlayıcılar **ISE** etiket logic apps ile aynı ıse'de çalıştırın. ISE etiketini gösterme bağlayıcılar genel Logic Apps hizmetinde çalıştırın.

ISE temel birim kapasitesi, sabit daha fazla performans gerekiyorsa, bu nedenle [daha fazla ölçek birimi ekleme](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity), oluşturma sırasında veya daha sonra. Veri koruma maliyetlerini bir ISE'de çalışan mantıksal uygulamalar uygulanmaz.

NET fiyatlandırma bilgileri için bkz. [Azure Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="connectors"></a>

## <a name="connectors"></a>Bağlayıcılar

Azure Logic Apps bağlayıcıları yardımcı, mantıksal uygulama erişim uygulamalar, hizmetlerinize ve sistemlerinize bulutta veya şirket içi sağlayarak [Tetikleyicileri](#triggers), [eylemleri](#actions), veya her ikisini de. Bağlayıcılar, standart veya Kurumsal sınıflandırılır. Bu bağlayıcılar hakkında genel bir bakış için bkz. [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md). Önceden oluşturulmuş bağlayıcılar varsa logic apps içinde kullanmak istediğiniz REST API'leri için oluşturabileceğiniz [özel Bağlayıcılar](https://docs.microsoft.com/connectors/custom-connectors), yalnızca bu sarmalayıcılar REST apı'lerdir. Özel bağlayıcılar, standart bir bağlayıcı faturalandırılır. Aşağıdaki bölümler için faturalandırma nasıl Tetikleyiciler hakkında daha fazla bilgi sağlar ve Eylemler çalışabilirsiniz.

<a name="triggers"></a>

## <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler bir mantıksal uygulama örneği belirli bir olay meydana geldiğinde oluşturduğunuz özel eylemlerdir. Tetikleyiciler, mantıksal uygulamanın nasıl ölçülüp etkiler, farklı şekillerde işlevi görür. Azure Logic Apps'te mevcut Tetikleyicileri çeşitli türleri şunlardır:

* **Yoklama tetikleyici**: Bu tetikleyiciyi bir mantıksal uygulama örneği oluşturmak ve iş akışını başlatmak için ölçütleri karşılayan iletiler için bir uç nokta sürekli olarak denetler. Logic Apps bile hiçbir mantıksal uygulama örneği oluştururken, bir yürütme olarak her bir yoklama isteği ölçümleri. Yoklama aralığını belirtmek için mantıksal Uygulama Tasarımcısı aracılığıyla bir tetikleyici ayarlayın.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Web kancası tetikleyici**: Bu tetikleyici, belirli bir uç noktaya bir istek göndermek bir istemci için bekler. Web kancası uç noktasına gönderilen her istek, bir eylem yürütme olarak sayılır. Örneğin, istek ve HTTP Web kancası tetikleyici her iki Web kancası Tetikleyicileri vardır.

* **Yinelenme tetikleyicisini**: Bu tetikleyici, tetikleyici ayarladığınız yinelenme aralığı temel bir mantıksal uygulama örneği oluşturur. Örneğin, üç günde bir çalışır bir yinelenme tetikleyicisi veya daha karmaşık bir zamanlamaya göre ayarlayabilirsiniz.

<a name="actions"></a>

## <a name="actions"></a>Eylemler

Azure Logic Apps gibi HTTP "yerleşik" eylemleri yerel eylemleri ölçümleri. Örneğin, yerleşik Eylemler HTTP aramaları, Azure işlevleri veya API yönetimi de dahil ve akış adımları gibi koşullar, döngüler, denetlemek ve switch deyimlerini korur. Her eylem kendi eylem türüne sahip. Örneğin, çağrı Eylemler [Bağlayıcılar](https://docs.microsoft.com/connectors) "ApiConnection" türüne sahip. Bu bağlayıcıları standart olarak sınıflandırılan veya ölçülür Kurumsal bağlayıcılar, kendi ilgili üzerinde temel [fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps). Kurumsal bağlayıcı *Önizleme* standart bağlayıcılar ücretlendirilir.

Azure Logic Apps başarılı ve başarısız tüm eylem yürütmeleri ölçümleri. Ancak, Logic Apps bu işlemleri ölçüm değil:

* Karşılaşılmamış koşullar nedeniyle atlandı eylemleri
* Mantıksal uygulama bitmeden önce durdurulduğundan çalıştırma eylemleri

Döngü içinde çalışan işlemleri için Azure Logic Apps, her eylem Döngüdeki her bir döngü için sayar. Örneğin, bir liste işleyen bir "for each" döngüsü olduğunu varsayalım. Logic Apps ölçümleri bir liste sayısı tarafından bu döngü eylemi döngü eylem sayısı olan öğeler ve döngü başlatan bunu eylem ekler. Bu nedenle, 10 öğe listesi için (10 * 1) hesaplamadır + 1, 11 eylem yürütmeleri içinde sonuçlanır.

## <a name="disabled-logic-apps"></a>Devre dışı bırakılmış mantıksal uygulamalar

Logic apps devre dışı yeni örnekleri oluşturulamıyor çünkü ücretlendirilmezsiniz devre dışı.
Mantıksal uygulama devre dışı bıraktıktan sonra şu anda çalışan örnekleri tamamen durdurmadan önce biraz zaman alabilir.

## <a name="integration-accounts"></a>Tümleştirme hesapları

Sabit fiyatlandırma modeli uygulanır [tümleştirme hesapları](logic-apps-enterprise-integration-create-integration-account.md) nerede keşfedin, geliştirebilir ve test [B2B ve EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) olmadan Azure Logic Apps özellikleri ek bir maliyet.
Her bir Azure bölgesinde bir tümleştirme hesabı olabilir. Her tümleştirme hesabı belirli depolayabilirsiniz [yapıtları sayıda](../logic-apps/logic-apps-limits-and-config.md), iş ortakları, anlaşmalar, haritalar, şemalar, derlemeleri, sertifikaları, toplu iş yapılandırması ve benzeri ticari içerir.

Azure Logic Apps, ücretsiz, temel ve standart tümleştirme hesapları sunar. Ücretsiz katmanda SLA tarafından desteklenmiyor ve bir üst sınırı temel ve standart Katmanlar Logic Apps hizmet düzeyi sözleşmesi (SLA), aktarım hızı ve kullanımı desteklenir.

Ücretsiz, temel veya standart tümleştirme hesabı arasında seçmek için:

* **Ücretsiz**: Keşif senaryoları, üretim senaryolarında denemek istediğinizde için.

* **Temel**: Yalnızca ileti işleme istediğinizde veya küçük bir iş ortağı olarak davranacak şekilde, daha büyük bir iş varlığı ile ticari bir iş ortağı ilişkisine sahiptir.

* **Standart**: Daha karmaşık B2B ilişkileri ve artan sayıda gereken varlıkları olduğunda için yönetin.

NET fiyatlandırma bilgileri için bkz. [Azure Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="data-retention"></a>

## <a name="data-retention"></a>Veri saklama

Logic apps dışında bir tümleştirme hizmeti ortamı (ISE) Çalıştır'ün tüm giriş ve mantıksal uygulamanızın çalıştırma geçmişinde depolanan çıkışları faturalandırılırsınız bir mantıksal uygulamanın üzerinde temel [saklama süresi çalıştırma](logic-apps-limits-and-config.md#run-duration-retention-limits). Veri koruma maliyetlerini bir ISE'de çalışan mantıksal uygulamalar uygulanmaz. NET fiyatlandırma bilgileri için bkz. [Azure Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

Mantıksal uygulamanızın depolama tüketimini izlemek yardımcı olmak için şunları yapabilirsiniz:

* Mantıksal uygulamanızı aylık kullanan GB depolama birim sayısını görüntüleyin.
* Mantıksal uygulamanızın çalıştırma geçmişinde boyutları için belirli bir eylemin giriş ve çıkışları görüntüleyin.

<a name="storage-consumption"></a>

### <a name="view-logic-app-storage-consumption"></a>Mantıksal uygulama depolama kullanımını görüntüleme

1. Azure portalında bulun ve mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızın menüsünden altında **izleme**seçin **ölçümleri**.

1. Sağ bölmede altında **Grafik başlığı**, gelen **ölçüm** listesinden **depolama tüketimi yürütmeler için fatura kullanım**.

   Bu ölçüm, faturalandırılan depolama tüketimi birimi sayısı GB cinsinden aylık sunar.

<a name="input-output-sizes"></a>

### <a name="view-action-input-and-output-sizes"></a>Eylem girişi görüntülemek ve boyutları çıkış

1. Azure portalında bulun ve mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde **genel bakış**.

1. Sağ bölmede altında **çalıştırma geçmişi**, girişleri ve çıkışları kontrol etmek istediğiniz Çalıştır'ı seçin.

1. Altında **mantıksal uygulama çalıştırması**, seçin **çalıştırma ayrıntıları**.

1. İçinde **mantıksal uygulama çalıştırma ayrıntıları** bölmesinde, her eylemin durumunu ve süresini listeler, Eylemler tablosundan görüntülemek istediğiniz eylemi seçin.

1. İçinde **mantıksal uygulama eylemi** bölmesinde, bu eylemin girişler ve çıkışlar boyutları görünür sırasıyla altında Bul **giriş bağlantısı** ve **çıkış bağlantısı**.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps hakkında daha fazla bilgi edinin](logic-apps-overview.md)
* [İlk mantıksal uygulamanızı oluşturma](quickstart-create-first-logic-app-workflow.md)
