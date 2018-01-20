---
title: "Fiyatlandırma ve faturalama - Azure Logic Apps | Microsoft Docs"
description: "Fiyatlandırma ve faturalama Azure mantıksal uygulamaları için nasıl çalıştığını öğrenin."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: LADocs; klam
ms.openlocfilehash: 49a66c826a98f7160b97b516e6fd541795ae3b0e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="logic-apps-pricing-model"></a>Logic Apps fiyatlandırma modeli
Oluşturun ve Azure Logic Apps ile bulutta otomatik ölçeklenebilir tümleştirme iş akışlarını çalıştırmak. Burada, faturalama ve fiyatlandırma Logic Apps için nasıl çalıştığı hakkında ayrıntılar verilmiştir.
## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli
Yeni oluşturulan logic apps ile yalnızca ne kullanmanız için ücret ödersiniz. Yeni mantıksal uygulamalar mantığı uygulama örneği tarafından gerçekleştirilen tüm yürütmeleri ölçülen, yani bir tüketim planı ve fiyatlandırma modelini kullanır.
### <a name="what-are-action-executions"></a>Eylem yürütmeleri nelerdir?
Her bir mantıksal uygulama tanımını tetikleyiciler, denetim akışı adımlarını, yerleşik Eylemler çağrıları ve bağlayıcılar çağrıları içeren bir eylem adımdır.
### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler belirli bir olay gerçekleştiğinde bir mantıksal uygulama örneğini oluşturduğunuz özel eylemlerdir. Tetikleyiciler nasıl mantıksal uygulama ölçülen etkileyen birkaç farklı davranışlar vardır.
* **Yoklama tetikleyici** – Bu tetikleyici sürekli olarak, bir ileti alır kadar bir uç nokta iş akışını başlatmak için bir mantıksal uygulama örneği oluşturma ölçütlerini karşılayan denetler. Tetikleyicinin mantığını Uygulama Tasarımcısı aracılığıyla bulunan yoklama aralığını ayarlayabilirsiniz. Her yoklama isteği bile hiçbir mantığı uygulama örneği oluşturulduğunda bir yürütme olarak sayılır.
* **Web kancası tetikleyici** – belirli bir uç noktası için bir istek göndermek bir istemci Bu tetikleyici bekler. Web kancası uç noktasına gönderilen her istek bir eylem yürütme olarak sayılır. Örneğin, istek ve HTTP Web kancası tetikleyici, her iki Web kancası Tetikleyicileri vardır.
* **Yineleme tetikleyici** – Bu tetikleyici tetikleyicinin ayarladığınız yinelenme aralığını temel bir mantıksal uygulama örneği oluşturur. Örneğin, üç günde bir çalışır bir yineleme tetikleyici yukarı veya daha karmaşık bir zamanlamaya göre ayarlayabilirsiniz.

Tetikleyici yürütmeleri mantığı uygulamanızın genel bakış bölmesinde tetikleyici Geçmiş bölümü altında bulabilirsiniz.

### <a name="actions"></a>Eylemler
Yerleşik Eylemler, örneğin, HTTP, Azure işlevleri veya API Management yanı sıra denetim akışı adımlarını çağrı Eylemler yerel eylemler olarak ölçülen ve bunların karşılık gelen türlerine sahip. Çağrı Eylemler [Bağlayıcılar](https://docs.microsoft.com/connectors) "ApiConnection" türüne sahip. Bağlayıcılar standard veya enterprise bağlayıcılar sınıflandırılır ve bunların ilgili ölçülen [fiyatlandırma][pricing].
Tüm başarılı ve başarısız çalışma eylemleri sayılan ve işlem yürütmeleri ölçülen. Ancak, mantıksal uygulama tamamlanmadan önce sonlandırıldığından unmet koşullar veya çalışmıyor, Eylemler nedeniyle atlanır eylemleri eylem yürütmeleri sayılmaz. Bunlar devre dışı durumdayken ücretlendirilmezsiniz şekilde devre dışı logic apps yeni örnekleri örneği oluşturulamıyor.

> [!NOTE]
> Bir mantıksal uygulama devre dışı bıraktıktan sonra şu anda çalışan örneklerini tamamen durdurmadan önce biraz zaman alabilir.

Döngüler içinde çalışan eylemlerin her döngü döngüsünde başına sayılır. Örneğin, 10-öğesi listesini işleyen "için her" döngü tek bir eylemle liste öğelerinin (10) sayısı (1) Döngüdeki eylemlerin sayısını döngüsü başlatmak için bir artı çarparak sayılır. Bu nedenle, bu örnekte, hesaplama (10 * 1), + 11 eylem yürütmeleri sonuçları 1.

### <a name="integration-account-usage"></a>Tümleştirme hesabı kullanımı
Tüketim tabanlı kullanımını içeren bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) geçirebileceğiniz keşfetmek, geliştirmek ve test [B2B/EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) Logic Apps özelliklerini Hayır adresindeki ek bir maliyet. Bu tümleştirme hesapları bölgeye ve 10 sözleşme ve 25 eşlemeleri kadar deposu biri olabilir. Varsa ve sınırsız iş ortakları, şemaları ve sertifikaları karşıya yükleyin.

Logic Apps, temel ve standart tümleştirme hesapları desteklenen Logic Apps SLA ile de sunar. Temel tümleştirme hesapları, yalnızca ileti işleme kullanmak istediğiniz ya da daha büyük bir iş varlığı ile ticari ortak ilişkisi olan küçük iş ortağı olarak hareket kullanabilirsiniz. Standart tümleştirme hesapları daha karmaşık B2B ilişkileri destekler ve yönetebileceğiniz varlıkların sayısı artar. Daha fazla bilgi için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="pricing"></a>Fiyatlandırma
Daha fazla bilgi için bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Sonraki adımlar
* [Logic Apps genel bakış][whatis]
* [İlk mantıksal uygulamanızı oluşturma][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

