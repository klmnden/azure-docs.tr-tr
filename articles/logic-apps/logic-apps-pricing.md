---
title: Fiyatlandırma ve faturalandırma - Azure Logic Apps | Microsoft Docs
description: Fiyatlandırma ve faturalama için Azure Logic Apps nasıl çalıştığını öğrenin
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
manager: carmonm
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 02/26/2019
ms.openlocfilehash: 9b5452f112c6325dafd5edbe693b90ec2a94abc0
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990246"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure Logic Apps fiyatlandırma modeli

Oluşturun ve Azure Logic Apps kullandığınızda, bulutta ölçeklendirilebilir otomatik tümleştirmesi iş akışlarınızı çalıştırın. Logic Apps için faturalama ve fiyatlandırma nasıl çalıştığı hakkında ayrıntılar aşağıda verilmiştir. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli

Genel veya "Genel" Logic Apps hizmetinde çalışan yeni logic apps için yalnızca kullandığınız kadarı için ödeme yaparsınız. Bu mantıksal uygulamalar, bir tüketim tabanlı plan ve fiyatlandırma modeli kullanın. Mantıksal uygulama tanımında her adımı bir eylemdir. Tetikleyici, bir denetim akışı adımlarını, yerleşik Eylemler ve bağlayıcı çağrıları Eylemler içerir. Logic Apps, mantıksal uygulamanızı çalıştıran tüm eylemleri ölçümleri.  
Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Sabit fiyatlandırma modeli

İçinde çalıştırılır ve yeni mantıksal uygulamalar için bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), yerleşik Eylemler ve standart bağlayıcılar için aylık sabit fiyata ödeme yaparsınız. Bir işe bir Azure sanal ağdaki kaynaklara erişebilen yalıtılmış mantıksal uygulama oluşturma ve çalıştırma bir yol sağlar. 

ISE temel birim kapasitesi, sabit daha fazla performans gerekiyorsa, bu nedenle [daha fazla ölçek birimi ekleme](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity), oluşturma sırasında veya daha sonra. İstediğiniz kadar çok bağlantısı içeren bir ücretsiz Kurumsal bağlayıcı, işe içerir. Kullanımı için ek Kurumsal bağlayıcılar ücretlendirilir Kurumsal tüketim fiyatı temel alınarak temel. 

> [!NOTE]
> ISE bulunduğu [ *genel Önizleme*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler bir mantıksal uygulama örneği belirli bir olay meydana geldiğinde oluşturduğunuz özel eylemlerdir. Tetikleyiciler, mantıksal uygulamanın nasıl ölçülüp etkiler, farklı şekillerde işlevi görür.

* **Yoklama tetikleyici** – bu tetikleyiciyi bir mantıksal uygulama örneği oluşturmak ve iş akışını başlatmak için ölçütleri karşılayan iletiler için bir uç nokta sürekli olarak denetler. Logic Apps bile hiçbir mantıksal uygulama örneği oluştururken, bir yürütme olarak her bir yoklama isteği ölçümleri. Yoklama aralığını belirtmek için mantıksal Uygulama Tasarımcısı aracılığıyla bir tetikleyici ayarlayın.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Web kancası tetikleyici** – belirli bir uç noktaya bir istek göndermek bir istemci bu tetikleyiciyi bekler. Web kancası uç noktasına gönderilen her istek, bir eylem yürütme olarak sayılır. Örneğin, istek ve HTTP Web kancası tetikleyici her iki Web kancası Tetikleyicileri vardır.

* **Yinelenme tetikleyicisini** – Bu tetikleyici, tetikleyici ayarladığınız yinelenme aralığı temel bir mantıksal uygulama örneği oluşturur. Örneğin, üç günde bir çalışır bir yinelenme tetikleyicisi veya daha karmaşık bir zamanlamaya göre ayarlayabilirsiniz.

## <a name="actions"></a>Eylemler

Logic Apps, yerleşik Eylemler yerel eylemleri ölçümleri. Örneğin, Azure işlevleri veya API Management ve denetim akışı adımlarını döngüler ve koşullar gibi çağrılarından HTTP üzerinden çağrılarını yerleşik Eylemler içerir 
- her biri kendi eylem türüne sahip. Çağrı Eylemler [Bağlayıcılar](https://docs.microsoft.com/connectors) "ApiConnection" türüne sahip. Bu bağlayıcıları üzerinde kendi ilgili temel ölçülür, standart veya Kurumsal bağlayıcılar olarak sınıflandırılan [fiyatlandırma][pricing]. Kurumsal bağlayıcı *Önizleme* standart bağlayıcılar ücretlendirilir.

Logic Apps ölçümleri tüm başarılı ve başarısız Eylemler eylem yürütmeleri çalışır. Logic Apps bu işlemleri ölçüm değil: 

* Karşılaşılmamış koşullar nedeniyle atlandı eylemleri
* Mantıksal uygulama bitmeden önce durdurulduğundan çalıştırma eylemleri

Devre dışı logic apps, yeni örneklerini oluşturamayacağınızdan sırasında devre dışı ücretlendirilmezsiniz.

> [!NOTE]
> Mantıksal uygulama devre dışı bıraktıktan sonra şu anda çalışan örnekleri tamamen durdurmadan önce biraz zaman alabilir.

Döngü içinde çalışan işlemleri için Logic Apps döngü döngüsünde başına her bir işlem sayılır. Örneğin, bir liste işleyen bir "for each" döngüsü olduğunu varsayalım. Logic Apps ölçümleri bir liste sayısı tarafından bu döngü eylemi döngü eylem sayısı olan öğeler ve döngü başlatan bunu eylem ekler. Hesaplama için 10-öğe listesi olan (10 * 1) + 1, 11 eylem yürütmeleri içinde sonuçlanır.

## <a name="integration-account-usage"></a>Tümleştirme hesabı kullanımı

Tüketim tabanlı kullanım uygulandığı [tümleştirme hesapları](logic-apps-enterprise-integration-create-integration-account.md) nerede keşfedin, geliştirebilir ve test [B2B/EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) olmadan Logic Apps özellikleri ek bir maliyet. Bölge başına bir tümleştirme hesabı olabilir. Her tümleştirme hesabı belirli depolayabilirsiniz [yapıtları sayıda](../logic-apps/logic-apps-limits-and-config.md), iş ortakları, anlaşmalar, haritalar, şemalar, derlemeleri, sertifikaları, toplu iş yapılandırması ve benzeri ticari içerir.

Logic Apps, ayrıca temel ve standart tümleştirme hesapları ile desteklenen Logic Apps SLA'sı sunar. Temel tümleştirme hesapları yalnızca ileti işleme istediğiniz veya daha büyük bir iş varlığı bir alım-satım iş ortağı ilişkisi olan küçük bir iş ortağı olarak davranacak kullanabilirsiniz. Standart tümleştirme hesapları daha karmaşık B2B ilişkileri destekler ve yönetebileceğiniz varlık sayısını artırın. Daha fazla bilgi için [Azure fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps hakkında daha fazla bilgi edinin][whatis]
* [İlk mantıksal uygulamanızı oluşturun][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

