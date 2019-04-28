---
title: SQL veritabanı durumunu izlemek için Azure kaynak durumu kullanın | Microsoft Docs
description: Azure kaynak durumu, SQL veritabanı sistem durumu, tanılamanıza ve bir Azure sorunu SQL kaynaklarınızı etkilediğinde, destek almanıza yardımcı izlemek için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: c3b9fecd3ad404385732e55a9cf3aa65a6e388b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483063"
---
# <a name="use-resource-health-to-troubleshoot-connectivity-for-azure-sql-database"></a>Azure SQL veritabanı için bağlantı sorunlarını gidermek için kaynak durumu kullanın

## <a name="overview"></a>Genel Bakış

[Kaynak durumu](../service-health/resource-health-overview.md#getting-started) için SQL veritabanı, tanılamanıza ve bir Azure sorunu SQL kaynaklarınızı etkilediğinde destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

![Genel Bakış](./media/sql-database-resource-health/sql-resource-health-overview.jpg)

## <a name="health-checks"></a>Sistem durumu denetimleri

Kaynak durumu, SQL kaynak durumu, başarı ve başarısızlık kaynağa oturumlarının inceleyerek belirler. Şu anda, SQL veritabanı kaynağınızın kaynak durumu, yalnızca sistem hatası ve kullanıcı hatası nedeniyle oturum açma hataları inceler. Kaynak sistem durumu, 1-2 dakikada bir güncelleştirilir.

## <a name="health-states"></a>Sistem sağlığı durumları

### <a name="available"></a>Kullanılabilir

Durumu **kullanılabilir** kaynak durumu SQL kaynağınıza sistem hataları nedeniyle oturum açma hataları algılamadı anlamına gelir.

![Kullanılabilir](./media/sql-database-resource-health/sql-resource-health-available.jpg)

### <a name="degraded"></a>Düşürüldü

Durumun **Düşürüldü** olması, Kaynak Durumu'nun oturum açma işlemlerinin çoğunun başarılı olduğunu ancak arada başarısız girişler de olduğunu gösterir. Bunlar büyük olasılıkla geçici bir oturum açma hatalardır. Geçici oturum açma hataları nedeniyle bağlantı sorunları etkisini azaltmak için lütfen uygulama [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) kodunuzda.

![Düşürüldü](./media/sql-database-resource-health/sql-resource-health-degraded.jpg)

### <a name="unavailable"></a>Kullanılamaz

Durumu **kullanılamıyor** kaynak durumu, tutarlı bir oturum açma hataları SQL kaynağınıza algıladı anlamına gelir. Kaynağınızı uzun bir süre bu durumda kalırsa, lütfen desteğe başvurun.

![Kullanılamaz](./media/sql-database-resource-health/sql-resource-health-unavailable.jpg)

### <a name="unknown"></a>Bilinmeyen

Sistem durumunu **bilinmeyen** bu kaynak hakkında bilgi için 10 dakikadan daha fazla kaynak durumu henüz aldığını gösterir. Bu durum kaynak durumunun eksiksiz bir gösterimi olmasa da, bir sorun giderme işlemi önemli veri noktası var. Kaynak beklenen şekilde çalışıyorsa, kaynak durumu için kullanılabilen birkaç dakika sonra değiştirin. Kaynağı ile ilgili sorunlar yaşıyorsanız, bilinmeyen durumunu platform içindeki bir olay kaynağı etkilediğini önerebilir.

![Bilinmeyen](./media/sql-database-resource-health/sql-resource-health-unknown.jpg)

## <a name="historical-information"></a>Geçmiş bilgileri

Kaynak durumu sistem durumu geçmişi bölümünde sistem durumu geçmiş 14 güne kadar erişebilirsiniz. Bölüm için kaynak durumu tarafından bildirilen kapalı kalma süreleri (varsa) kapalı kalma süresi nedeni de içerecektir. Şu anda Azure SQL veritabanı kaynağınızın kalma süresini iki dakikalık ayrıntı düzeyinde göstermektedir. Gerçek kapalı kalma süresi bir dakikadan az olasıdır-8s ortalama.

### <a name="downtime-reasons"></a>Kapalı kalma nedenleri

SQL veritabanınız kesinti yaşandığında analiz bir nedenini belirlemek için gerçekleştirilir. Kullanılabilir olduğunda, kapalı kalma süresi nedeni kaynak durumu sistem durumu geçmişi bölümünde raporlanır. Kapalı kalma süresinin nedenleri genellikle bir olaydan 30 dakika sonra yayımlanır.

#### <a name="planned-maintenance"></a>Planlı bakım

Azure altyapısı, düzenli olarak planlanan bakım – veri merkezindeki donanım veya yazılım bileşenlerini yükseltme yapar. Bakım veritabanı uygulanır, ancak SQL var olan bazı bağlantılar sonlandırmak ve yenilerini reddet. Planlanan bakım sırasında karşılaşılan oturum açma hataları genellikle geçicidir ve [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) yardımcı etkisini azaltır. Oturum açma hataları yaşamaya devam ederseniz Lütfen desteğe başvurun.

#### <a name="reconfiguration"></a>Yeniden yapılandırma

Yeniden yapılandırmalar, geçici koşullar kabul edilir ve zaman zaman beklenir. Bu olaylar, Yük Dengeleme veya yazılım/donanım arızaları tetiklenebilir. Bir bulut veritabanına bağlanan herhangi bir istemci üretim uygulama, sağlam bir bağlantı uygulamalıdır [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors), bu gibi durumlarda azaltılmasına yardımcı olur ve genellikle hatalarını saydam son kullanıcıya olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [geçici hatalar için yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors)
- [SQL bağlantı hatalarını giderme, tanılama ve önleme](./sql-database-connectivity-issues.md)
- Daha fazla bilgi edinin [kaynak sistem durumu uyarılarını yapılandırma](../service-health/resource-health-alert-arm-template-guide.md)
- Genel Bakış [kaynak durumu](../service-health/resource-health-overview.md)
- [Kaynak durumu hakkında SSS](../service-health/resource-health-faq.md)
