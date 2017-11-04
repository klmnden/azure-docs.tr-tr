---
title: "Yüksek Scheduler kullanılabilirliği ve güvenilirliği"
description: "Yüksek Scheduler kullanılabilirliği ve güvenilirliği"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 7e7fe49de7814b6058468d630f8638720e5864f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>Yüksek Scheduler kullanılabilirliği ve güvenilirliği
## <a name="azure-scheduler-high-availability"></a>Yüksek Azure Scheduler kullanılabilirliği
Çekirdek Azure platformu hizmet olarak Azure Scheduler yüksek oranda kullanılabilir ve coğrafi olarak yedekli hizmet dağıtımı ve coğrafi bölge iş çoğaltma özellikleri.

### <a name="geo-redundant-service-deployment"></a>Coğrafi olarak yedekli hizmet dağıtımı
Azure Scheduler, Azure'da bugün olduğu neredeyse her coğrafi bölgede kullanıcı Arabirimi aracılığıyla kullanılabilir. Azure Scheduler kullanılabilir bölgelerin listesi [burada listelenen](https://azure.microsoft.com/regions/#services). Bir veri merkezi barındırılan bir bölgede kullanılamıyor işlenir, hizmeti başka bir veri merkezinden kullanılabilir olacak şekilde, Azure Scheduler yük devretme yeteneklerini demektir.

### <a name="geo-regional-job-replication"></a>Coğrafi bölge iş çoğaltma
Yalnızca kendi iş ancak yönetim istekleri için de coğrafi olarak çoğaltılmış kullanılabilir ön uç Azure Scheduler ' dir. Bir bölgede bir kesinti olduğunda Azure Scheduler yöneltilir ve eşleştirilmiş coğrafi bölgede başka bir veri merkezi iş çalışmasını sağlar.

Örneğin, bir iş Orta Güney ABD içinde oluşturduysanız, Azure Scheduler Kuzey Orta ABD o işe otomatik olarak çoğaltır. Olduğunda hata Orta Güney ABD içinde Azure Scheduler işi Kuzey merkezi Bizden çalıştırıldığını sağlar. 

![][1]

Sonuç olarak, Azure Scheduler, verilerinizi Azure hatası durumunda aynı daha geniş coğrafi bölge içinde kalmasını sağlar. Sonuç olarak, işinizi eklemeniz yeterlidir yüksek kullanılabilirlik için yinelenen olmayan – Azure Scheduler otomatik olarak işleriniz için yüksek kullanılabilirlik yetenekleri sağlar.

## <a name="azure-scheduler-reliability"></a>Azure Zamanlayıcı güvenilirlik
Azure Zamanlayıcı kendi yüksek oranda kullanılabilirlik garanti eder ve kullanıcı tarafından oluşturulan işleri için farklı bir yaklaşım alır. Örneğin, işinizi kullanılamaz bir HTTP uç noktası çağırabilir. Azure Zamanlayıcı hata ile mücadele etmek için diğer seçenekleri vererek işinizi başarıyla yürütmek yine de çalışır. Azure Scheduler bunu iki şekilde yapar:

### <a name="configurable-retry-policy-via-retrypolicy"></a>"RetryPolicy" aracılığıyla yapılandırılabilir yeniden deneme ilkesi
Azure Zamanlayıcı, bir yeniden deneme ilkesi yapılandırmanıza olanak sağlar. Bir işi başarısız olursa, varsayılan olarak 30 saniyelik aralıklarda yeniden dört kez daha, iş Zamanlayıcı dener. Bu yeniden deneme ilkesi (örneğin, on kez 30 saniyelik aralıklarda) daha katı olması için yeniden yapılandırabilir veya ok (örneğin, iki kez günlük aralıklarla.)

Ne zaman bu yardımcı olabilecek örnek olarak, haftada bir kez çalıştırır ve bir HTTP uç noktası çağıran bir iş oluşturabilirsiniz. HTTP uç noktası için birkaç saat işiniz çalıştırıldığında çalışmıyorsa, bile varsayılan yeniden deneme ilkesi başarısız olur beri yeniden çalıştırmak işi için daha fazla bir hafta bekleyin istemeyebilirsiniz. Böyle durumlarda, her 30 saniyede yerine üç saatte (örneğin) yeniden denemek için standart yeniden deneme ilkesi yapılandırabilir.

Bir yeniden deneme ilkesi yapılandırma konusunda bilgi için bkz [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>"ErrorAction" yoluyla diğer uç nokta yapılandırılabilirlik
Azure Scheduler işi için hedef uç noktaya erişilemiyor kalırsa, Azure Scheduler, yeniden deneme ilkesi izledikten sonra alternatif hata işleme uç noktasına geri döner. Alternatif bir hata işleme uç noktasını yapılandırdıysanız, Azure Scheduler bunu çağırır. Başka bir uç nokta ile kendi işleri karşısında hatası yüksek oranda kullanılabilir.

Örneğin, aşağıdaki çizimde, New York web hizmeti isabet kendi yeniden deneme ilkesi Azure Scheduler izler. Yeniden deneme başarısız olduktan sonra alternatif olup olmadığını denetler. Sonra ilerler ve aynı yeniden deneme ilkesi ile diğer istekleri yapan başlatır.

![][2]

Aynı yeniden deneme ilkesi orijinal eylem ve diğer hata eylemi için geçerli olduğunu unutmayın. Diğer hata eylemin eylem türü ana eylemin eylem türünden farklı bir değer olması mümkündür. Örneğin, ana eylemi bir HTTP uç noktası çağırma, ancak hata eylemi bunun yerine bir depolama kuyruğu, hizmet veri yolu kuyruğu ya da hata günlüğünü olmadığından hizmet veri yolu konusu eylemi olabilir.

Başka bir uç nokta yapılandıracağınızı öğrenmek için bkz [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
