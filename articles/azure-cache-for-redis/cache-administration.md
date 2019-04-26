---
title: Azure önbelleği için Redis yönetme | Microsoft Docs
description: Azure önbelleği için Redis için yeniden başlatma ve zamanlama güncelleştirmeleri gibi yönetim görevlerini gerçekleştirmeyi öğreneceksiniz
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: 81ef669b62c822e10d8bf5c45e58dd769c5dbeb9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60233001"
---
# <a name="how-to-administer-azure-cache-for-redis"></a>Azure önbelleği için Redis yönetme
Bu konuda gibi yönetim görevlerini gerçekleştirmek kullanılan nasıl açıklanmaktadır [yeniden](#reboot) ve [güncelleştirmelerini zamanlama](#schedule-updates) , Azure önbelleği için Redis örneği için.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reboot"></a>Yeniden başlatma
**Yeniden** dikey önbelleğinizin bir veya daha fazla düğüm yeniden başlatmanızı sağlar. Bu yeniden başlatma özelliği, bir önbellek düğümü bir hata varsa, dayanıklılık için uygulamanızı test olanak tanır.

![Yeniden başlatma](./media/cache-administration/redis-cache-administration-reboot.png)

Yeniden başlatma ve düğümleri seçmek **yeniden**.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot.png)

Kümeleme özelliği etkinleştirilmiş bir premium önbellek varsa, hangi parçalar önbelleğin yeniden başlatmayı seçebilirsiniz.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot-cluster.png)

Önbelleğinizin bir veya daha fazla düğümlerini yeniden başlatmanız, istediğiniz düğümleri seçin ve tıklayın **yeniden**. Kümeleme özelliği etkinleştirilmiş bir premium önbellek varsa, yeniden başlatın ve ardından istenen parçalar seçin **yeniden**. Birkaç dakika sonra çalıştırdığınız ve seçili düğümlerin yeniden başlatılması birkaç dakika sonra yeniden çevrimiçi.

İstemci uygulamaları üzerindeki etkisi, yeniden hangi düğüme bağlı olarak değişir.

* **Ana** - ana düğüm yeniden başlatıldığında, Azure önbelleği için Redis çoğaltma düğüme yük devreder ve ana yükseltir. Bu yük devretme sırasında bağlantıları önbelleğe başlatılamayabilir kısa bir aralık olabilir.
* **Bağımlı** - ikincil düğüm yeniden başlatıldığında önbellek istemcileri için herhangi bir etkisi yoktur.
* **Hem ana hem de ikincil** -ne zaman hem önbellek düğümleri yeniden, tüm verileri önbelleğe kaybolur ve bağlantıları önbelleğe kadar birincil düğüm başarısız söz konusu yeniden çevrimiçi. Yapılandırdıysanız [veri kalıcılığı](cache-how-to-premium-persistence.md), en son yedekleme önbelleği olarak yeniden çevrimiçine döner, ancak en son yedeklemeden sonra gerçekleşen herhangi bir önbellek yazma kaybolur geri.
* **Bir premium düğümlerinin önbelleğe kümeleme özellikli** - kümeleme özelliği etkinleştirildiğinde, bir premium önbelleğin bir veya daha fazla düğümlerini yeniden başlatmanız seçili düğümlerin davranışı, karşılık gelen veya kümelenmemiş bir önbellek düğümü yeniden başlatmanız aynıdır.

> [!IMPORTANT]
> Yeniden başlatma için tüm fiyatlandırma katmanında kullanıma sunulmuştur.
> 
> 

## <a name="reboot-faq"></a>Yeniden başlatma ile ilgili SSS
* [Uygulamamın test etmek için hangi düğümün yeniden?](#which-node-should-i-reboot-to-test-my-application)
* [İstemci bağlantıları temizlemek için kullanılan önbellek yeniden başlatma?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Yeniden başlatma yapmam miyim my önbellekten veri kaybedersiniz?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [PowerShell, CLI veya diğer yönetim araçlarını kullanarak önbelleğimin yeniden başlatma?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Fiyatlandırma katmanları, yeniden başlatma işlevlerini kullanabilir miyim?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Uygulamamın test etmek için hangi düğümün yeniden?
Uygulamanızı hataya karşı önbelleğinizin birincil düğüm esnekliğini test etmek için yeniden başlatma **ana** düğümü. Uygulamanızı hataya karşı ikincil düğüme esnekliğini test etmek için yeniden başlatma **bağımlı** düğümü. Toplam hataya karşı uygulamanızı önbellek esnekliğini test etmek için yeniden başlatma **hem** düğümleri.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>İstemci bağlantıları temizlemek için kullanılan önbellek yeniden başlatma?
Önbellek yeniden başlatırsanız, Evet, tüm istemci bağlantıları temizlenir. Yeniden başlatma burada tüm istemci bağlantıları bir mantık hatası veya istemci uygulamasındaki bir hata nedeniyle tüketildikten durumda yararlı olabilir. Her fiyatlandırma katmanının farklı olan [istemci bağlantı sınırları](cache-configure.md#default-redis-server-configuration) çeşitli boyutlarda ve sonra bu sınırlar ulaşıldı için daha fazla istemci bağlantı kabul edilir. Önbellek yeniden başlatılıyor, tüm istemci bağlantıları temizlemek için bir yol sağlar.

> [!IMPORTANT]
> İstemci bağlantıları temizlemek için önbelleğinizi yeniden başlatırsanız, StackExchange.Redis Redis düğümü yeniden çevrimiçi olduğunda otomatik olarak bağlanır. Temel sorun çözülemiyorsa, istemci bağlantılarını kullanılan devam edebilir.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Yeniden başlatma yapmam miyim my önbellekten veri kaybedersiniz?
Her ikisi de yeniden başlatırsanız **ana** ve **bağımlı** düğümler, önbellek (veya bir premium önbellek kümeleme özellikli kullanıyorsanız bu parça) tüm veriler kaybolmuş olabilir, ancak bu ya da garanti edilmez. Yapılandırdıysanız [veri kalıcılığı](cache-how-to-premium-persistence.md), en fazla önbellek olarak yeniden çevrimiçine döner, ancak yedekleme yapıldıktan sonra ortaya çıkan herhangi bir önbellek yazma kaybolur son yedeklemeden geri yüklenir.

Düğümler yalnızca birini yeniden başlatırsanız, veriler genellikle kaybolmaz, ancak yine de olabilir. Örneğin bir önbellek yazma, verileri önbellek yazma devam ediyor ve ana düğüm yeniden kaybolur için. Veri kaybı için başka bir senaryo, bir düğümü yeniden başlatma ve diğer düğümü aynı anda bir hata nedeniyle aşağı git olacağını olacaktır. Veri kaybı için olası nedenler hakkında daha fazla bilgi için bkz: [Redis verilerimi ne?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>PowerShell, CLI veya diğer yönetim araçlarını kullanarak önbelleğimin yeniden başlatma?
Evet, PowerShell için yönergeleri görmek [bir Azure önbelleği için Redis yeniden](cache-howto-manage-redis-cache-powershell.md#to-reboot-an-azure-cache-for-redis).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Fiyatlandırma katmanları, yeniden başlatma işlevlerini kullanabilir miyim?
Yeniden başlatma, tüm fiyatlandırma katmanları için kullanılabilir.

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Güncelleştirmeleri zamanla** dikey Premium katmanı önbellek hesabınız için bir bakım penceresi belirtmek olanak tanır. Bakım penceresi belirtildiğinde, herhangi bir Redis sunucu güncelleştirme sırasında bu pencereyi yapılır. 

> [!NOTE] 
> Bakım penceresi server güncelleştirmeleri yalnızca Redis için geçerlidir ve değil herhangi bir Azure güncelleştirmeleri veya işletim sistemine sanal makinelerinin ana bilgisayar önbelleği güncelleştirir.
> 
> 

![Güncelleştirmeleri zamanlama](./media/cache-administration/redis-schedule-updates.png)

Bir bakım penceresi belirtmek için istenen gün işaretleyin ve her gün için bakım penceresi başlangıç saati belirleyin ve tıklayın **Tamam**. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

Güncelleştirmeler için varsayılan ve en düşük bakım penceresi beş saattir. Bu değer Azure Portalı'ndan yapılandırılabilir değildir, ancak PowerShell kullanarak yapılandırabilirsiniz `MaintenanceWindow` parametresinin [yeni AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry) cmdlet'i. Daha fazla bilgi için bkz: PowerShell, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeler yönetebilirim?


## <a name="schedule-updates-faq"></a>SSS güncelleştirmelerini zamanlama
* [Zamanlamayı güncelleştirme özelliğini kullanmıyorsanız güncelleştirmeler olduğunda?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Zamanlanan bakım penceresi sırasında yapılan güncelleştirmeleri hangi türde?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [PowerShell, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeler yönetebilirim?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Fiyatlandırma katmanları zamanlama Güncelleştirmeler işlevini kullanabilir miyim?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Zamanlamayı güncelleştirme özelliğini kullanmıyorsanız güncelleştirmeler olduğunda?
Bir bakım penceresi belirtmezseniz, herhangi bir zamanda güncelleştirmeleri yapılabilir.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Zamanlanan bakım penceresi sırasında yapılan güncelleştirmeleri hangi türde?
Yalnızca sunucu zamanlanmış bakım penceresi sırasında yapılan güncelleştirmeler Redis. Bakım penceresi uygulanamaz Azure güncelleştirmeleri ya da sanal makine işletim sistemi güncelleştirmeleri.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Ben zamanlanmış güncelleştirmeler PowerShell, CLI veya diğer yönetim araçlarını kullanarak yönetilen alabilirim?
Evet, aşağıdaki PowerShell cmdlet'lerini kullanarak, Zamanlanmış güncelleştirmeleri yönetebilirsiniz:

* [Get-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/get-azrediscachepatchschedule)
* [Yeni AzRedisCachePatchSchedule](/powershell/module/az.rediscache/new-azrediscachepatchschedule)
* [Yeni AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry)
* [Remove-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/remove-azrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Fiyatlandırma katmanları zamanlama Güncelleştirmeler işlevini kullanabilir miyim?
**Güncelleştirmeleri zamanla** özelliği, yalnızca premium fiyatlandırma katmanı kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla keşif [Azure önbelleği için Redis premium katmanı](cache-premium-tier-intro.md) özellikleri.

