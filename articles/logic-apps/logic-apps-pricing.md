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
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: 63784c5e3af360b2f3f8cb330a9df8b27a85d859
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="logic-apps-pricing-model"></a>Logic Apps fiyatlandırma modeli
Azure mantıksal uygulamaları, ölçeklendirme ve bulutta tümleştirme iş akışlarını yürütmek sağlar.  Faturalama ve fiyatlandırma planı için Logic Apps hakkında ayrıntılar verilmiştir.
## <a name="consumption-pricing"></a>Tüketim fiyatlandırma
Yeni oluşturulan Logic Apps tüketim planını kullanın. Logic Apps tüketim fiyatlandırma modeli ile yalnızca, kullanım için ödeme yaparsınız.  Logic Apps tüketim planı kullanırken kısıtlanan değil.
Bir mantıksal uygulama örneğini'nın çalıştırılmasıyla yürütülen tüm eylemler ölçülen.
### <a name="what-are-action-executions"></a>Eylem yürütmeleri nelerdir?
Bir mantıksal uygulama tanımını her adımda Tetikleyicileri içeren bir eylem, denetim akışı koşullar, her döngü için kapsamı döngüler, bağlayıcılar çağrıları kadar gibi adımları ve yerel Eylemler çağırır.
Tetikleyicileri, belirli bir olay gerçekleştiğinde bir mantıksal uygulama yeni bir örneğini oluşturmak için tasarlanmış özel eylemlerdir.  Mantıksal uygulama nasıl ölçülen etkileyebilecek Tetikleyiciler için birkaç farklı davranışlar vardır.
* **Yoklama tetikleyici** – bir mantıksal uygulama örneğini oluşturma ölçütlerini karşılayan bir ileti alıncaya kadar Bu tetikleyici sürekli bir uç nokta yoklar.  Yoklama aralığı tetikleyicinin mantığını Uygulama Tasarımcısı'nda yapılandırılabilir.  Bir mantıksal uygulama örneğini oluşturmaz olsa bile her yoklama isteği bir eylem yürütme olarak sayılır.
* **Web kancası tetikleyici** – belirli bir noktadaki bir istek göndermek bir istemci Bu tetikleyici bekler.  Web kancası uç noktasına gönderilen her istek bir eylem yürütme olarak sayılır. İstek ve HTTP Web kancası tetikleyici her iki Web kancası Tetikleyicileri var.
* **Yineleme tetikleyici** – Bu tetikleyici tetikleyicinin yapılandırılmış yinelenme aralığını temel mantıksal uygulama örneğini oluşturur.  Örneğin, bir yineleme tetikleyici üç günde veya hatta dakikada çalıştırmak için yapılandırılabilir.

Tetikleyici yürütmeleri tetikleyici geçmiş bölümünde Logic Apps kaynak dikey penceresinde görülebilir.

Yürütülen, tüm eylemler bunlar başarılı veya başarısız bir eylem yürütme olarak ölçülen.  Bir koşulu karşılanmadı nedeniyle atlandı eylem veya mantıksal uygulama tamamlanmadan önce sonlandırıldığından yürütme kaydetmedi Eylemler eylem yürütmeleri sayılmaz.

Döngüler içinde yürütülen eylemler döngü sayılır.  Örneğin, tek bir işlemde bir her bir döngü için (10) listedeki öğeleri (1) Döngüdeki Eylemler sayısına göre bu örnekte, (10 * 1) olur döngüsü başlatma için bir tane + 1 çarpılan sayı olarak sayılır 10 öğelerin listesini yineleme 11 eylem yürütmeleri =.
Devre dışı Logic Apps örneği yeni örnek bulunamaz ve bu nedenle, devre dışı sırasında değil ücretlendirilirsiniz.  Bir mantıksal uygulama devre dışı bıraktıktan sonra sessiz moda örneklerine tamamen devre dışı bırakılıyor önce biraz zaman alabileceğini dikkatli olun.
### <a name="integration-account-usage"></a>Tümleştirme hesabı kullanımı
Tüketim tabanlı kullanımı dahil olduğu bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) araştırması, geliştirme ve sınama amacıyla kullanmanıza olanak sağlayan [B2B/EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) Logic Apps özelliklerini hiçbir ek ücret ödemeden. En fazla bölgeye ve 10 sözleşme ve 25 eşlemeleri kadar deposu bir hesap oluşturabilirsiniz. Şemalar, sertifikalar ve iş ortakları sınırsız sahip ve gerektiği kadar karşıya yükleyebilirsiniz.

Tümleştirme hesapları kullanımına sahip dahil edilmesi ek olarak, standart tümleştirme hesapları bu sınırlar olmadan ve bizim standart Logic Apps SLA de oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>App Service planları
Daha önce oluşturduğunuz mantıksal uygulamalar bir uygulama hizmeti planı başvuran devam önceki gibi davranır. Belirtilen günlük yürütmeleri aşıldığında ancak eylem yürütme ölçer kullanarak faturalandırılır sonra seçilen plana bağlı olarak, kısıtlanan.
Bir uygulama hizmeti planı açıkça mantıksal uygulama ile ilişkilendirilmiş olması gerekmez, kendi abonelikte sahip EA müşteriler dahil edilen miktarlar avantajı alın.  EA aboneliğinizi ve bir mantıksal uygulama aynı abonelikte standart bir uygulama hizmeti planı varsa, örneğin, daha sonra her gün (aşağıdaki tablonun bakın) 10.000 eylem yürütmeleri için ücretlendirilmezsiniz. 

App Service planları ve onların günlük izin verilen eylem yürütmeleri:
|  | Serbest / / temel paylaşılan | Standart | Premium |
| --- | --- | --- | --- |
| Gün başına eylem yürütmeleri |200 |10,000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Uygulama hizmeti planı tüketimi fiyatlandırma gelen Dönüştür
Tüketim modeline ilişkili bir uygulama hizmeti planı sahip bir mantıksal uygulama değiştirmek için mantıksal uygulama tanımı'nda uygulama hizmeti planı başvurusunu kaldırın.  Bu değişiklik, bir PowerShell cmdlet çağrısıyla yapılabilir:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Fiyatlandırma
Fiyatlandırma ayrıntıları için bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Sonraki adımlar
* [Logic Apps genel bakış][whatis]
* [İlk mantıksal uygulamanızı oluşturma][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

