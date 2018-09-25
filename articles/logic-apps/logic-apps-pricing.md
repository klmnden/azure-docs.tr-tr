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
ms.date: 09/24/2018
ms.openlocfilehash: b75fba2ba0e9fa922b1252378e0bab326cada7d2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974315"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure Logic Apps fiyatlandırma modeli

Oluşturun ve Azure Logic Apps ile bulutta otomatik ölçeklenebilir tümleştirmesi iş akışlarınızı çalıştırın. Logic Apps için faturalama ve fiyatlandırma nasıl çalıştığı hakkında ayrıntılar aşağıda verilmiştir. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli

Genel veya "Genel" Logic Apps hizmetini kullanarak oluşturduğunuz yeni mantıksal uygulamalar için yalnızca kullandığınız kadarı için ödeme yaparsınız. Bu mantıksal uygulamalar, bir tüketim tabanlı planı ve bir mantıksal uygulama tarafından gerçekleştirilen tüm eylem yürütmeleri ölçülür anlamına gelir fiyatlandırma modeli kullanın. Her bir mantıksal uygulama tanımını tetikleyicileri, denetim akışı adımlarını, yerleşik Eylemler çağrıları ve bağlayıcılar çağrıları içeren bir eylem adımdır. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Sabit fiyatlandırma modeli

> [!NOTE]
> Tümleştirme hizmeti ortamı bulunduğu *özel Önizleme*. Erişim talep etmek [burada katılma isteğiniz oluşturma](https://aka.ms/iseprivatepreview).

İle oluşturduğunuz yeni mantıksal uygulamalar için bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)ayrılmış kaynakları kullanan Logic Apps örneği yalıtılmış bir özel olan, sabit bir aylık fiyat için ödeme yapın Yerleşik Eylemler ve standart ISE etiketli bağlayıcılar. Bir kurumsal tüketim fiyatı temel alınarak Kurumsal bağlayıcı ek Kurumsal bağlayıcılar ücretlendirilirsiniz ancak ücretsiz, temel, işe içerir. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler bir mantıksal uygulama örneği belirli bir olay meydana geldiğinde oluşturduğunuz özel eylemlerdir. Tetikleyiciler, mantıksal uygulamanın nasıl ölçülüp etkiler, farklı şekillerde işlevi görür.

* **Yoklama tetikleyici** – bu tetikleyiciyi bir mantıksal uygulama örneği oluşturmak ve iş akışını başlatmak için ölçütleri karşılayan iletiler için bir uç nokta sürekli olarak denetler. Her yoklama isteği, bir yürütme olarak sayar ve bile hiçbir mantıksal uygulama örneği oluşturulduğunda, ölçülür. Yoklama aralığını belirtmek için mantıksal Uygulama Tasarımcısı aracılığıyla bir tetikleyici ayarlayın.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Web kancası tetikleyici** – belirli bir uç noktaya bir istek göndermek bir istemci bu tetikleyiciyi bekler. Web kancası uç noktasına gönderilen her istek, bir eylem yürütme olarak sayılır. Örneğin, istek ve HTTP Web kancası tetikleyici her iki Web kancası Tetikleyicileri vardır.

* **Yinelenme tetikleyicisini** – Bu tetikleyici, tetikleyici ayarladığınız yinelenme aralığı temel bir mantıksal uygulama örneği oluşturur. Örneğin, üç günde bir çalışır bir yinelenme tetikleyicisi veya daha karmaşık bir zamanlamaya göre ayarlayabilirsiniz.

Tetikleyici yürütmelerinin mantıksal uygulamanızın genel bakış bölmesinde tetikleyici geçmişi bölümü altında bulabilirsiniz.

## <a name="actions"></a>Eylemler

Yerleşik Eylemler, HTTP, Azure işlevleri veya API Management'ı çağırın ve ayrıca akış adımları denetim eylemleri gibi ilgili türleri olan yerel Eylemler ölçülür. Çağrı Eylemler [Bağlayıcılar](https://docs.microsoft.com/connectors) "ApiConnection" türüne sahip. Bu bağlayıcıları üzerinde kendi ilgili temel ölçülür, standart veya Kurumsal bağlayıcılar olarak sınıflandırılan [fiyatlandırma][pricing]. 

Tüm başarılı ve başarısız çalıştırma işlemleri olarak sayılır ve eylem yürütmeleri ölçülür. Ancak, mantıksal uygulama tamamlanmadan önce sonlandırıldığından karşılaşılmamış koşullar veya çalıştırma, Eylemler nedeniyle atlanır eylemleri eylem yürütmeleri sayılmaz. Bunlar devre dışı durumdayken ücret ödemediğiniz şekilde devre dışı logic apps yeni örnekleri örneği oluşturulamıyor.

> [!NOTE]
> Mantıksal uygulama devre dışı bıraktıktan sonra şu anda çalışan örnekleri tamamen durdurmadan önce biraz zaman alabilir.

Döngü içinde çalışacak eylemleri, Döngüdeki her bir döngü başına sayılır. Örneğin, 10 öğe listesini işleme bir "for each" döngüsü tek bir eylemle liste öğelerini (10) sayısı (1) döngüsünde işlemlerinin sayısı artı bir döngü başlatmak için çarpılmasıyla hesaplanır. Bu nedenle, bu örnekte hesaplama (10 * 1), + 1, 11 eylem yürütmeleri içinde sonuçlanır.

## <a name="integration-account-usage"></a>Tümleştirme hesabı kullanımı

Tüketim tabanlı kullanımını içeren bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) nerede keşfedin, geliştirebilir ve test [B2B/EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) olmadan Logic Apps özellikleri ek bir maliyet. Bir tümleştirme hesabı başına bölge ve belirli en fazla depolama olabilir [yapıtları sayıda](../logic-apps/logic-apps-limits-and-config.md)EDI ticari iş ortakları ve anlaşmalar, haritalar, şemalar, derlemeleri, sertifikaları ve toplu iş yapılandırması gibi.

Logic Apps, ayrıca temel ve standart tümleştirme hesapları ile desteklenen Logic Apps SLA'sı sunar. Temel tümleştirme hesapları yalnızca ileti işleme kullanmak istiyorsanız, ya da daha büyük bir iş varlığı bir alım-satım iş ortağı ilişkisi olan küçük bir iş ortağı olarak davranacak kullanabilirsiniz. Standart tümleştirme hesapları daha karmaşık B2B ilişkileri destekler ve yönetebileceğiniz varlık sayısını artırın. Daha fazla bilgi için [Azure fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps hakkında daha fazla bilgi edinin][whatis]
* [İlk mantıksal uygulamanızı oluşturun][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

