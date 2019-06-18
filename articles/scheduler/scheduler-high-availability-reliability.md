---
title: Yüksek kullanılabilirlik ve güvenilirlik - Azure Zamanlayıcı
description: Yüksek kullanılabilirlik ve güvenilirlik Azure scheduler'da hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.topic: article
ms.date: 08/16/2016
ms.openlocfilehash: 50ab6cfefe4a7df9d671e7fd1287aa16b803f260
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702914"
---
# <a name="high-availability-and-reliability-for-azure-scheduler"></a>Yüksek kullanılabilirlik ve güvenilirlik için Azure Zamanlayıcı

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Azure Zamanlayıcı sağlar hem de [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/architecture/guide/pillars#availability) ve işleriniz için güvenilirlik. Daha fazla bilgi için [Zamanlayıcı için SLA](https://azure.microsoft.com/support/legal/sla/scheduler).

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure Zamanlayıcı [yüksek] ve coğrafi olarak yedekli hizmet dağıtımı hem iş coğrafi bölge çoğaltma kullanır.

### <a name="geo-redundant-service-deployment"></a>Coğrafi olarak yedekli hizmet dağıtımı

Azure Zamanlayıcı, Azure portalı kullanılabilir neredeyse [bugün Azure tarafından desteklenen her coğrafi bölge](https://azure.microsoft.com/global-infrastructure/regions/#services). Azure veri merkezinde barındırılan bir bölgede kullanılamaz duruma gelirse, hizmetin yük devretme özellikleri Zamanlayıcı başka bir veri merkezinden kullanılabilir hale getirmek için bu nedenle, Azure Scheduler kullanmaya devam edebilirsiniz.

### <a name="geo-regional-job-replication"></a>Coğrafi bölge iş çoğaltma

Azure Scheduler kendi işlerinde Azure bölgeleri arasında çoğaltılır. Bu nedenle bir bölgenin kesinti varsa, Azure Scheduler devreder ve işinizi eşleştirilmiş coğrafi bölgede başka bir veri merkezinden çalışmasını sağlar.

Örneğin, Güney Orta ABD bir proje oluşturursanız, Azure Scheduler, Orta Kuzey ABD işinde otomatik olarak çoğaltır. Güney Orta ABD başarısız olursa, Azure Scheduler, Kuzey Orta ABD işi olarak çalıştırır. 

![Coğrafi bölge iş çoğaltma](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png)

Azure Zamanlayıcı da Azure'da bir hatası oluşursa ihtimale verilerinizi aynı ancak daha geniş bir coğrafi bölge içinde kalmasını sağlar. Bu nedenle, yalnızca yüksek kullanılabilirlik istediğinizde işlerinizi yinelenen gerekmez. Azure Zamanlayıcı, işleriniz için otomatik olarak yüksek kullanılabilirlik sağlar.

## <a name="reliability"></a>Güvenilirlik

Azure Zamanlayıcı, kendi yüksek kullanılabilirliği garanti eder, ancak kullanıcı tarafından oluşturulan işler için farklı bir yaklaşım alır. Örneğin, işiniz kullanılamıyor HTTP uç noktası çağırır varsayalım. Azure Zamanlayıcı yine de iş, başarıyla alternatif yolları hataların işlenmesi için veren tarafından çalıştırmayı dener: 

* Yeniden deneme ilkeleri ayarlayın.
* Alternatif uç noktaları ayarlama ayarlayın.

<a name="retry-policies"></a>

### <a name="retry-policies"></a>İlkeleri yeniden deneme

Azure Zamanlayıcı, yeniden deneme ilkeleri ayarlamanıza olanak tanır. Sonra varsayılan olarak, bir işi, başarısız olursa Zamanlayıcı İş dört kez 30 saniyelik aralıklarla yeniden dener. Bu yeniden deneme ilkesi daha agresif, gibi 10 kez aralıklarla 30 saniye veya daha az agresiftir, gibi iki kez, günlük aralıklarla yapabilirsiniz.

Örneğin, bir HTTP uç noktasına çağrı haftalık bir işi oluşturduğunuzu varsayalım. HTTP uç noktası için birkaç saate işiniz çalıştırıldığında kullanılamaz duruma gelirse, işi yeniden çalıştırmak varsayılan yeniden deneme ilkesi bu durumda çalışmaz gerçekleşen için başka bir hafta bekleyin istemeyebilirsiniz. Bu nedenle, böylece Örneğin, üç saatte yerine her 30 saniyede yeniden denemeler meydana standart yeniden deneme ilkesi değiştirmek isteyebilirsiniz. 

Bir yeniden deneme ilkesi ayarlama konusunda bilgi almak için bkz: [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoints"></a>Alternatif uç noktaları

Azure Zamanlayıcı İş erişilemiyor, yeniden deneme ilkesi izledikten sonra bile bir uç nokta çağırırsa, Zamanlayıcı, bu tür hatalar işleyebilen bir alternatif uç noktasına geri döner. Bu nedenle, bu uç nokta ayarlarsanız, Zamanlayıcı hata gerçekleştiğinde kendi işlerinizi yüksek oranda kullanılabilir olmasını sağlayan Bu uç nokta çağırır.

Örneğin, New York'da bulunan bir web hizmetini çağırırken Zamanlayıcı yeniden deneme ilkesi nasıl izlediği Bu diyagramda gösterilmektedir. Yeniden deneme işlemleri başarısız olursa Zamanlayıcı için alternatif bir uç nokta denetler. Uç nokta varsa, Zamanlayıcı istekleri alternatif uç noktaya gönderme başlatır. Aynı yeniden deneme ilkesi, hem özgün eylemi hem de alternatif eylem için geçerlidir.

![Yeniden deneme ilkesi ve diğer uç nokta ile Zamanlayıcı davranışı](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png)

Diğer eylem için eylem türü özgün eylemden farklı olabilir. Orijinal eylem bir HTTP uç noktasına çağrı olsa da, örneğin, diğer eylem hatalar bir depolama kuyruğu, hizmet veri yolu kuyruğu ya da hizmet veri yolu konusu eylemi kullanarak oturum.

Diğer uç nokta ayarlamayı öğrenmek için bkz. [errorAction](scheduler-concepts-terms.md#error-action).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Kavramlar, terminoloji ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Karmaşık zamanlamalar ve gelişmiş yinelemeler oluşturma](scheduler-advanced-complexity.md)
* [Sınırlar, kotalar, varsayılan değerler ve hata kodları](scheduler-limits-defaults-errors.md)
