---
title: "Fiyatlandırma ve faturalama - Azure Logic Apps | Microsoft Docs"
description: "Fiyatlandırma ve faturalama Azure mantıksal uygulamaları için nasıl çalıştığını öğrenin"
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
ms.openlocfilehash: 096fdd5a6604ed8cecc931da2169194b777664d2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="logic-apps-pricing-model"></a>Logic Apps fiyatlandırma modeli

Oluşturun ve Azure Logic Apps ile bulutta otomatik ölçeklenebilir tümleştirme iş akışlarını çalıştırmak. Burada, faturalama ve fiyatlandırma Logic Apps için nasıl çalıştığı hakkında ayrıntılar verilmiştir. 

## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli

Yeni oluşturulan logic apps ile yalnızca ne kullanmanız için ücret ödersiniz. Yeni mantıksal uygulamalar bir tüketim planı ve mantığı uygulama örneği tarafından gerçekleştirilen tüm eylem yürütmeleri ölçülen anlamına gelir fiyatlandırma modelini kullanır. Her bir mantıksal uygulama tanımını tetikleyiciler, denetim akışı adımlarını, yerleşik Eylemler çağrıları ve bağlayıcılar çağrıları içeren bir eylem adımdır. Daha fazla bilgi için bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler belirli bir olay gerçekleştiğinde bir mantıksal uygulama örneğini oluşturduğunuz özel eylemlerdir. Tetikleyiciler nasıl mantıksal uygulama ölçülen etkiler, farklı şekillerde davranır.

* **Yoklama tetikleyici** – Bu tetikleyici sürekli olarak bir mantıksal uygulama örneğini oluşturma ve iş akışı başlatma ölçütlerini karşılayan iletiler için bir uç nokta denetler. Her yoklama isteği bir yürütme olarak sayar ve hatta hiçbir mantığı uygulama örneği oluşturulduğunda, tarifeli. Yoklama aralığı belirtmek için tetikleyicinin mantığını Uygulama Tasarımcısı aracılığıyla ayarlayın.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Web kancası tetikleyici** – belirli bir uç noktası için bir istek göndermek bir istemci Bu tetikleyici bekler. Web kancası uç noktasına gönderilen her istek bir eylem yürütme olarak sayılır. Örneğin, istek ve HTTP Web kancası tetikleyici, her iki Web kancası Tetikleyicileri vardır.

* **Yineleme tetikleyici** – Bu tetikleyici tetikleyicinin ayarladığınız yinelenme aralığını temel bir mantıksal uygulama örneği oluşturur. Örneğin, üç günde bir çalışır bir yineleme tetikleyici yukarı veya daha karmaşık bir zamanlamaya göre ayarlayabilirsiniz.

Tetikleyici yürütmeleri mantığı uygulamanızın genel bakış bölmesinde tetikleyici Geçmiş bölümü altında bulabilirsiniz.

## <a name="actions"></a>Eylemler

Yerleşik Eylemler, HTTP, Azure işlevleri veya API Management çağırın ve ayrıca akışı adımları denetleyen eylemleri gibi ilgili türleri olan yerel eylemler olarak ölçülen. Çağrı Eylemler [Bağlayıcılar](https://docs.microsoft.com/connectors) "ApiConnection" türüne sahip. Bu bağlayıcıların kendi ilgili üzerinde göre hangi ölçülür standard veya enterprise bağlayıcıları, olarak sınıflandırılır [fiyatlandırma][pricing]. 

Tüm başarılı ve başarısız çalışma eylemleri sayılan ve işlem yürütmeleri ölçülen. Ancak, mantıksal uygulama tamamlanmadan önce sonlandırıldığından unmet koşullar veya çalışmıyor, Eylemler nedeniyle atlanır eylemleri eylem yürütmeleri sayılmaz. Bunlar devre dışı durumdayken ücretlendirilmezsiniz şekilde devre dışı logic apps yeni örnekleri örneği oluşturulamıyor.

> [!NOTE]
> Bir mantıksal uygulama devre dışı bıraktıktan sonra şu anda çalışan örnekleri tamamen durdurmadan önce biraz zaman alabilir.

Döngüler içinde çalışan eylemlerin her döngü döngüsünde başına sayılır. Örneğin, 10-öğesi listesini işleyen "için her" döngü tek bir eylemle liste öğelerinin (10) sayısı (1) Döngüdeki eylemlerin sayısını döngüsü başlatmak için bir artı çarparak sayılır. Bu nedenle, bu örnekte, hesaplama (10 * 1), + 11 eylem yürütmeleri sonuçları 1.

## <a name="integration-account-usage"></a>Tümleştirme hesabı kullanımı

Tüketim tabanlı kullanımını içeren bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) geçirebileceğiniz keşfetmek, geliştirmek ve test [B2B/EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) Logic Apps özelliklerini Hayır adresindeki ek bir maliyet. Bu tümleştirme hesapları bölgeye ve 10 sözleşme ve 25 eşlemeleri kadar deposu biri olabilir. Varsa ve sınırsız iş ortakları, şemaları ve sertifikaları karşıya yükleyin.

Logic Apps, temel ve standart tümleştirme hesapları desteklenen Logic Apps SLA ile de sunar. Temel tümleştirme hesapları, yalnızca ileti işleme kullanmak istediğiniz ya da daha büyük bir iş varlığı ile ticari ortak ilişkisi olan küçük iş ortağı olarak hareket kullanabilirsiniz. Standart tümleştirme hesapları daha karmaşık B2B ilişkileri destekler ve yönetebileceğiniz varlıkların sayısı artar. Daha fazla bilgi için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps hakkında daha fazla bilgi edinin][whatis]
* [İlk mantıksal uygulamanızı oluşturma][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

