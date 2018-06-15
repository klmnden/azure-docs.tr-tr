---
title: Uyarı Yönetimi'nde Microsoft ürünleri izleme | Microsoft Docs
description: Bir uyarı yöneticinin dikkat gerektiren bazı sorun olduğunu gösterir.  Bu makalede uyarıları nasıl oluşturulduğunu ve System Center Operations Manager (SCOM) ve günlük analizi yönetilen farklar açıklar ve karma uyarı Yönetimi stratejisi iki ürünün yararlanarak, en iyi yöntemler sağlar.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 7df2fd7ef838465a60e3b0ce2f889127b7487684
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23865774"
---
# <a name="managing-alerts-with-microsoft-monitoring"></a>Microsoft izleme uyarıları yönetme
Bir uyarı yöneticinin dikkat gerektiren bazı sorun olduğunu gösterir.  Ayrı arasındaki farklar System Center Operations Manager (SCOM) ve günlük analizi Operations Management Suite (OMS) uyarıları nasıl oluşturulduğunu, nasıl yönetilen analiz ve ve kritik bir sorun olduğunu nasıl bildirilir açısından vardır algıladı.

## <a name="alerts-in-operations-manager"></a>Operations Manager içindeki uyarıları
Scom'deki uyarıları, bireysel kuralları ya da belirli bir sorunun belirtmek için İzleyici tarafından üretilir.  Bir kural yönetilen bir nesnenin sistem durumu için doğrudan ilgili olmayan bazı kritik bir sorunu belirten bir uyarı oluşturabilir sırasında bir hata durumuna girdiğinde, bir izleyici bir uyarı oluşturabilir.  Yönetim paketleri çeşitli uygulama veya yönettikleri hizmet için uyarı oluşturma iş akışlarını içerir.  Yeni bir Yönetim Paketi yapılandırma işleminin bir parçası, kritik düşünün olmayan sorunlar için aşırı uyarıları almadığınız emin olmak için ayarlar.

![SCOM uyarı görünümü](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM tam uyarı Yönetimi sorunu çözmek için çalışırken, yöneticiler tarafından değiştirilebilir bir durumu olan uyarılar sağlar.  Sorunu Çözümlendi, yönetici aynı zamanda, artık görünür kapalı olarak uyarı etkin uyarıları görüntüleme görünümlerde ayarlar.  İzleme sağlıklı duruma dönünce izlemelerden üretilen uyarıları otomatik olarak çözülebilir.

![Uyarı durumu](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Günlük analizi uyarıları
Günlük analizi bir uyarıda, düzenli aralıklarla otomatik olarak çalışacak bir günlük sorgudan oluşturulur.  Tüm günlük sorgudan bir uyarı kuralı oluşturabilirsiniz.  Sorgu belirttiğiniz ölçütlerle eşleşen sonuç döndürüyorsa, bir uyarı oluşturulur.  Bu belirli bir olay algılandığında bir uyarı oluşturur. belirli bir sorgu olabilir veya belirli bir uygulamayla ilgili herhangi bir hata olayı arar daha genel bir sorgu kullanabilirsiniz.

Günlük analizi uyarılar OMS depo olarak bir olay yazılır ve bir günlük sorgu alınabilir.  Sorunu Çözümlendi, belirtmek üzere SCOM olayları gibi bir durum yok.

![OMS Uyarısı](media/operations-management-suite-monitoring-alerts/oms-alert.png)

SCOM için günlük analizi veri kaynağı olarak kullanıldığında, oluşturulan ve değiştirilen SCOM uyarıları OMS depoya yazılır.  

![SCOM Uyarısı](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Uyarı yönetimi çözümü](http://technet.microsoft.com/library/mt484092.aspx) etkin uyarıları ve uyarı farklı kümelerini almak için birkaç genel sorgular özetini sağlar.  Bu, daha etkili bir şekilde analiz uyarılarınızın SCOM raporda daha sağlar.  Üzerinde özetleri ayrıntılı veri ayrıntıya ve Uyarıları farklı kümelerini almak için geçici sorgular oluşturun.

![Uyarı yönetimi çözümü](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Bildirimler
Scom'deki bildirimleri, bir posta veya metin belirli ölçütlere uyan uyarılara yanıt olarak gönderir.  Bildirim izlenmekte olan nesneyi gibi ölçütlere, uyarının önem derecesini, algılanan sorun türüne bağlı olarak farklı kişiler farklı bildirim abonelikleri veya günün saatini oluşturabilirsiniz.

Birkaç Abonelikleri, çok sayıda yönetim paketleri için bir tam bildirim stratejisi uygulamak için kullanılabilir.

![SCOM uyarıları](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Günlük analizi size bildirebilir aracılığıyla posta her bir e-posta bildirim eylemi ayarlayarak bir uyarı oluşturuldu [uyarı kuralı](http://technet.microsoft.com/library/mt614775.aspx).  Tek bir kural ile birden çok uyarı abone ol SCOM özelliği yok.  Ayrıca, OMS herhangi önceden yapılandırılmış sağlamadığından kendi uyarı kuralları oluşturmanız gerekir.

![Log Analytics uyarıları](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Bunları yalnızca Operations konsolunda değiştirebilirsiniz beri tamamen günlük analizi SCOM uyarılar ancak yönetemez.  Günlük analizi yararlı bir uyarı Yönetimi parçası işlem ancak Çözümleme Araçları tek başına bu SCOM sağlama yok.

## <a name="alert-remediation"></a>Uyarı düzeltme
[Düzeltme](http://technet.microsoft.com/library/mt614775.aspx) otomatik olarak bir uyarı tarafından tanımlanan sorunu düzeltmek için girişiminde başvuruyor.

SCOM yanıt sağlıksız bir duruma girerek bir izleyici için tanılama ve kurtarma işlemleri çalıştırmanıza olanak sağlar.  Eş zamanlı uyarı oluşturma İzleyicisi meydana gelir.  Tanılama ve kurtarma işlemleri genellikle aracı üzerinde çalışan bir komut olarak uygulanır.  Sorunu düzeltmek bir kurtarma denemeleri sırasında algılanan sorun hakkında daha fazla bilgi toplamak bir tanılama çalışır.

Günlük analizi başlatmanızı sağlar bir [Azure Otomasyonu runbook](https://azure.microsoft.com/documentation/services/automation/) veya bir Web kancası yanıt günlük analizi uyarı olarak çağırın.  Runbook'ları, PowerShell içinde uygulanan karmaşık mantık içerebilir.  Komut dosyası Azure üzerinde çalışır ve herhangi bir Azure kaynaklarını veya dış kaynaklar buluttan erişebilir.  Azure Otomasyonu runbook'ları bir sunucuda yerel veri merkezinizdeki yürütme yeteneğini sahip, ancak bu özellik günlük analizi uyarılara yanıt olarak runbook başlatma sırasında şu anda kullanılamıyor.

Scom'deki kurtarmaları hem OMS runbook'larda PowerShell Betiği içerebilir, ancak kurtarma işlemleri oluşturmak ve bir yönetim paketi içinde yer almalıdır çünkü yönetmek daha zordur.  Runbook'ları yazma, test ve runbook'ları yönetmek için özellikler sağlayan bir Azure Otomasyonu depolanır.

Günlük analizi için bir veri kaynağı olarak SCOM kullanırsanız, OMS deposunda saklanır SCOM uyarıları almak için bir günlük sorgu kullanarak günlük analizi uyarı oluşturabilirsiniz.  Bu, yanıt SCOM uyarı için bir Azure Otomasyonu runbook'u çalıştırmak olanak tanır.  Runbook, Azure'da çalışır olduğundan, doğal olarak, bu şirket içi sorunların kurtarma işlemleri için uygun bir strateji olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılarını öğrenmek [uyarılar System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).

