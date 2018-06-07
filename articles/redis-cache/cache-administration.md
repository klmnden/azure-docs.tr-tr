---
title: Azure Redis önbelleği yönetme | Microsoft Docs
description: Azure Redis önbelleği için yeniden başlatma ve zamanlama güncelleştirmeleri gibi yönetim görevlerini gerçekleştirmek öğrenin
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 3b62b41fb7b9d7ff6f40191c48d00c1f0a941e48
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639461"
---
# <a name="how-to-administer-azure-redis-cache"></a>Azure Redis önbelleği yönetme
Bu konu, aşağıdaki gibi yönetim görevlerini gerçekleştirmek açıklar [yeniden](#reboot) ve [güncelleştirmeleri zamanlama](#schedule-updates) Azure Redis önbelleği örnekleri için.

## <a name="reboot"></a>Yeniden başlatma
**Yeniden** dikey önbelleğiniz bir veya daha fazla düğümlerinin yeniden başlatılmasını sağlar. Bu yeniden başlatma özelliği, bir önbellek düğümü ise dayanıklılık için uygulamanızı test etmek sağlar.

![Yeniden başlatma](./media/cache-administration/redis-cache-administration-reboot.png)

Yeniden başlatma ve'ı tıklatın düğümleri seçin **yeniden**.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot.png)

Kümeleme özelliği etkinleştirilmiş bir premium önbelleği varsa, hangi parça önbelleğin yeniden başlatmayı seçebilirsiniz.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot-cluster.png)

Bir veya daha fazla düğüm, önbelleğin yeniden, istediğiniz düğümleri seçin ve ' **yeniden**. Kümeleme özelliği etkinleştirilmiş bir premium önbelleği varsa, yeniden başlatın ve ardından istenen parça seçin **yeniden**. Birkaç dakika sonra seçili düğümler yeniden başlatma ve bu birkaç dakika sonra yeniden çevrimiçi.

İstemci uygulamaları üzerindeki etkisini, yeniden hangi düğümlerin bağlı olarak değişir.

* **Ana** - ana düğüm üzerinde çoğaltma düğüme yeniden başlatılan, Azure Redis önbelleği başarısız olduğunda ve ana yükseltir. Bu yük devretme sırasında önbelleğine bağlantı başarısız olabileceği kısa bir aralık olabilir.
* **Bağımlı** - ikincil düğüm yeniden başlatıldığında, önbellek istemcilerine üzerinde etkisi yoktur.
* **Hem ana hem de ikincil** -zaman hem önbellek düğümleri yeniden, tüm verileri önbellekte kaybolur ve önbellek bağlantı kadar birincil düğüm başarısız gelir tekrar çevrimiçi. Yapılandırdıysanız [veri kalıcılığını](cache-how-to-premium-persistence.md), en son yedeklemeden geri önbellek yeniden çevrimiçi olur, ancak en son yedeklemeden sonra gerçekleşen tüm önbellek yazma kaybolur.
* **Premium düğümlerinin önbelleğe kümeleme özelliği etkinleştirilmiş** - kümeleme özelliği etkinleştirilmiş bir veya daha fazla düğüm premium önbelleğin yeniden başlattığınızda seçili düğümler için karşılık gelen veya kümelenmemiş bir önbellek düğümü yeniden zaman aynı davranıştır.

> [!IMPORTANT]
> Yeniden başlatma için tüm fiyatlandırma katmanlarına kullanıma sunulmuştur.
> 
> 

## <a name="reboot-faq"></a>Yeniden başlatma ile ilgili SSS
* [Uygulamam sınamak için hangi düğümün yeniden?](#which-node-should-i-reboot-to-test-my-application)
* [İstemci bağlantıları temizlemek için kullanılan önbellek yeniden başlatılabilir?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Yeniden başlatma işlemi yaparsanız ı my önbellekten veri kaybedeceksiniz?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [PowerShell'i, CLI veya diğer yönetim araçlarını kullanarak my önbelleği yeniden başlatılabilir?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Hangi fiyatlandırma katmanlarını yeniden başlatma işlevselliği kullanabilir miyim?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Uygulamam sınamak için hangi düğümün yeniden?
Önbelleğinizi birincil düğümün arızasına karşı uygulamanızın dayanıklılık test etmek için yeniden başlatma **ana** düğümü. İkincil düğümünün arızasına karşı uygulamanızın dayanıklılık test etmek için yeniden başlatma **bağımlı** düğümü. Önbelleğin toplam arızasına karşı uygulamanızın dayanıklılık test etmek için yeniden başlatma **her ikisi de** düğümleri.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>İstemci bağlantıları temizlemek için kullanılan önbellek yeniden başlatılabilir?
Evet, önbellek yeniden başlatırsanız tüm istemci bağlantıları temizlenir. Yeniden başlatma tüm istemci bağlantıları bir mantık hatası veya istemci uygulamada bir hata nedeniyle kullanıldığı durumda yararlı olabilir. Her fiyatlandırma katmanının farklı olan [istemci bağlantı sınırlarını](cache-configure.md#default-redis-server-configuration) çeşitli boyutlarda ve sonra bu sınırlar tamamladı için hiçbir daha fazla istemci bağlantılarını kabul edilir. Önbelleğin yeniden tüm istemci bağlantıları temizlemek için bir yol sağlar.

> [!IMPORTANT]
> İstemci bağlantıları temizlemek için önbelleğiniz yeniden başlatırsanız, Redis düğüm tekrar çevrimiçi olduğunda StackExchange.Redis otomatik olarak yeniden bağlanır. Temel sorun giderilmezse istemci bağlantıları kullanılacak devam edebilir.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Yeniden başlatma işlemi yaparsanız ı my önbellekten veri kaybedeceksiniz?
Her ikisini de yeniden başlatırsanız **ana** ve **bağımlı** düğümleri, önbelleği (veya premium önbelleği kümeleme özelliği etkinleştirilmiş kullanıyorsanız bu parça) tüm veriler kaybolur. Yapılandırdıysanız [veri kalıcılığını](cache-how-to-premium-persistence.md), en fazla önbellek yeniden çevrimiçi olur, ancak yedekleme yapıldıktan sonra oluşan önbellek yazma kaybolur son yedeklemeden geri yüklenir.

Yalnızca bir düğümünde yeniden başlatırsanız, veri genellikle kaybı olmadığından, ancak hala olabilir. Örneğin ana düğüm yeniden ve önbellek yazma, önbellek yazma verilerden ediyor kaybolduğu için. Veri kaybı için başka bir senaryo, bir düğümü yeniden başlatma ve aynı anda bir hata nedeniyle Git aşağı için başka bir düğüme olur olacaktır. Veri kaybı olası nedenleri hakkında daha fazla bilgi için bkz: [Redis içinde verilerimi ne oldu?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>PowerShell'i, CLI veya diğer yönetim araçlarını kullanarak my önbelleği yeniden başlatılabilir?
Evet, yönergeler için PowerShell bkz [Redis önbelleği yeniden](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Hangi fiyatlandırma katmanlarını yeniden başlatma işlevselliği kullanabilir miyim?
Yeniden başlatma tüm fiyatlandırma katmanları için kullanılabilir.

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Zamanlama güncelleştirmeleri** dikey Premium katmanı önbelleğiniz için bir bakım penceresi tanımlamanızı sağlar. Bakım penceresi belirtildiğinde, herhangi bir Redis sunucu güncelleştirme sırasında bu pencereyi yapılır. 

> [!NOTE] 
> Bakım penceresi yalnızca sunucu güncelleştirmelerini Redis için geçerlidir ve değil tüm Azure güncelleştirmelerini veya önbellek barındıran sanal makineleri için işletim sistemini güncelleştirir.
> 
> 

![Güncelleştirmeleri zamanlama](./media/cache-administration/redis-schedule-updates.png)

Bir bakım penceresi belirtmek için istenen gün kontrol edin ve her gün için bakım penceresi başlangıç saati belirtin ve tıklatın **Tamam**. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

Güncelleştirmeleri için varsayılan ve en düşük gereksinim, bakım penceresi beş saattir. Bu değer Azure portalından yapılandırılabilir değildir, ancak PowerShell kullanarak yapılandırabilirsiniz `MaintenanceWindow` parametresinin [yeni AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry) cmdlet'i. Daha fazla bilgi için bkz: [PowerShell'i, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeler yönetebilirim?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)


## <a name="schedule-updates-faq"></a>Güncelleştirmeleri zamanla SSS
* [Zamanlama güncelleştirmeleri özelliğini kullanmıyorsanız güncelleştirmeler olduğunda?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Güncelleştirmeleri ne tür zamanlanmış bakım penceresi sırasında hale getirilir?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Zamanlanmış güncelleştirmeler PowerShell'i, CLI veya diğer yönetim araçlarını kullanarak yönetebilir miyim?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Hangi fiyatlandırma katmanlarını zamanlama Güncelleştirmeler işlevini kullanabilir miyim?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Zamanlama güncelleştirmeleri özelliğini kullanmıyorsanız güncelleştirmeler olduğunda?
Bir bakım penceresi belirtmezseniz, güncelleştirmelerinin herhangi bir zamanda yapılabilir.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Güncelleştirmeleri ne tür zamanlanmış bakım penceresi sırasında hale getirilir?
Yalnızca sunucu zamanlanmış bakım penceresi sırasında yapılan güncelleştirmeler Redis. Bakım penceresi uygulanmaz Azure güncelleştirmeleri veya VM işletim sisteminde güncelleştirmeler.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Zamanlanmış güncelleştirmeler PowerShell'i, CLI veya diğer yönetim araçlarını kullanarak yönetilen alabilirim?
Evet, aşağıdaki PowerShell cmdlet'lerini kullanarak, Zamanlanmış güncelleştirmeleri yönetebilirsiniz:

* [Get-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/get-azurermrediscachepatchschedule)
* [New-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/new-azurermrediscachepatchschedule)
* [AzureRmRedisCacheScheduleEntry yeni](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry)
* [Remove-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/remove-azurermrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Hangi fiyatlandırma katmanlarını zamanlama Güncelleştirmeler işlevini kullanabilir miyim?
**Zamanlama güncelleştirmeleri** özelliktir yalnızca premium fiyatlandırma katmanı kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla araştırmak [Azure Redis önbelleği premium katmanına](cache-premium-tier-intro.md) özellikleri.

