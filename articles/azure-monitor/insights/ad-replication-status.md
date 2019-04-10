---
title: Active Directory çoğaltma durumu Azure İzleyici ile izleme | Microsoft Docs
description: Active Directory çoğaltma durumu çözüm paketi, Active Directory ortamınızı çoğaltma hataları için düzenli olarak izler.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.openlocfilehash: f7bbde98c6ef35021cc03b2646193d3601ca1cff
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425857"
---
# <a name="monitor-active-directory-replication-status-with-azure-monitor"></a>Azure İzleyici ile Active Directory çoğaltma durumunu izleme

![AD çoğaltma durumu simgesi](./media/ad-replication-status/ad-replication-status-symbol.png)

Active Directory kuruluş BT ortamında önemli bir bileşenidir. Yüksek kullanılabilirlik ve yüksek performans sağlamak için kendi Active Directory veritabanının kopyasını her etki alanı denetleyicisi vardır. Etki alanı denetleyicileri birbirlerini değişiklikleri kuruluş genelinde yaymak için çoğaltma. Bu çoğaltma işlemi hataları kuruluş genelinde çeşitli sorunlara neden olabilir.

AD çoğaltma durumu çözüm paketi, Active Directory ortamınızı çoğaltma hataları için düzenli olarak izler.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand-solution.md)]

## <a name="installing-and-configuring-the-solution"></a>Çözümünü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

### <a name="install-agents-on-domain-controllers"></a>Etki alanı denetleyicilerinde aracıları yükleyin
Değerlendirilecek etki alanının üyesi olan etki alanı denetleyicilerinde aracıları yüklemeniz gerekir. Veya üye sunuculara aracıları yükleyin ve aracılar için Azure İzleyici AD çoğaltma verileri göndermek için yapılandırmanız gerekir. Windows bilgisayarları Azure İzleyicisi'ne bağlanma hakkında bilgi almak için bkz. [Azure İzleyici bağlanmak Windows bilgisayarlara](../../azure-monitor/platform/agent-windows.md). Etki alanı denetleyicisine bağlanmak için Azure İzleyici, görmek istediğiniz mevcut bir System Center Operations Manager ortamın parçası ise [Azure İzleyici için Operations Manager'ı bağlama](../../azure-monitor/platform/om-agents.md).

### <a name="enable-non-domain-controller"></a>Olmayan etki alanı denetleyicisi etkinleştir
Herhangi bir etki alanı denetleyicilerinizin doğrudan Azure İzleyici bağlanmak istemiyorsanız, herhangi bir bilgisayarda etki alanınızı Azure İzleyicisi'ne bağlı olarak AD çoğaltma durumu çözüm paketi için veri toplamak ve bu verileri göndermek için kullanabilirsiniz.

1. Bilgisayarı AD çoğaltma durumu çözümünü kullanarak izlemek istediğiniz etki alanının bir üyesi olduğunu doğrulayın.
2. [Azure İzleyicisi ile Windows bilgisayarı bağlantı](../../azure-monitor/platform/om-agents.md) veya [mevcut Operations Manager ortamınızı Azure İzleyicisi'ni kullanarak bağlanma](../../azure-monitor/platform/om-agents.md), zaten bağlı değil.
3. Bu bilgisayarda, aşağıdaki kayıt defteri anahtarını ayarlayın:<br>Anahtar: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management Groups\<ManagementGroupName>\Solutions\ADReplication**<br>Değer: **IsTarget**<br>Değer verisi: **true**

   > [!NOTE]
   > Bu değişiklikler (HealthService.exe) Microsoft Monitoring Agent hizmeti yeniden başlatılana kadar etkili olmaz.
   > ### <a name="install-solution"></a>Çözüm yükleme
   > Açıklanan işlemi izleyin [bir izleme çözümü yükleme](solutions.md#install-a-monitoring-solution) eklemek için **Active Directory çoğaltma durumu** Log Analytics çalışma alanınıza çözüm. Başka bir yapılandırma işlemi gerekmez.


## <a name="ad-replication-status-data-collection-details"></a>AD çoğaltma durumu verileri toplama ayrıntıları
Veri toplama metotlarını ve verileri için AD çoğaltma durumu nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan Aracı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |beş günde bir |



## <a name="understanding-replication-errors"></a>Çoğaltma hatalarını anlama

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

AD çoğaltma durumu kutucuğu şu anda kaç çoğaltma hataları görüntüler. **Kritik çoğaltma hataları** veya % 75'ini üzerindeki hatalar [ömründen](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) , Active Directory ormanı için.

![AD çoğaltma durumu kutucuğu](./media/ad-replication-status/oms-ad-replication-tile.png)

Kutucuğa tıkladığınızda, hatalar hakkında daha fazla bilgi görüntüleyebilirsiniz.
![AD çoğaltma durumu Panosu](./media/ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Hedef sunucu durumu ve kaynak sunucu durumu
Bu sütunlar, hedef sunucu ve çoğaltma hataları yaşayan kaynak sunucular durumunu gösterir. Her etki alanı denetleyicisi adından sonra sayı, bu etki alanı denetleyicisi çoğaltma hatalarını sayısını gösterir.

Bazı sorunlar kaynak sunucu Perspektif ve diğer kaynaklardan hedef sunucu açısından gidermeyi kolaylaştırması açısından çünkü hedef sunucuları hem de kaynak sunucular için hataları gösterilmektedir.

Bu örnekte, birden çok hedef sunucu aynı sayıda hata kabaca olması, ancak tüm diğerlerinden çok daha fazla hata sahip bir kaynak sunucu (ADDC35) görebilirsiniz. Bazı sorun olduğunu, çoğaltma ortaklarından veri göndermek başarısız olmasına neden olan ADDC35 üzerinde olasıdır. Üzerinde ADDC35 sorunlarını giderme hedef sunucu alanında görünen hatalarda birçoğu çözebilir.

### <a name="replication-error-types"></a>Çoğaltma hatası türleri
Bu alan, kuruluşunuza algılanan hatalar türleri hakkında bilgi sağlar. Her bir benzersiz sayısal kodu ve hatanın nedenini belirlemenize yardımcı olabilecek bir ileti hatalı.

Üst halka hangi hataları daha fazla görünür ve daha az sıklıkta ortamınızda fikir verir.

Birden çok etki alanı denetleyicileri aynı çoğaltma hatası kullanırken gösterilir. Bu durumda, keşfedin veya bir etki alanı denetleyicisinde bir çözüm sonra aynı hatayı tarafından etkilenen diğer etki alanı denetleyicilerinde yineleyin.

### <a name="tombstone-lifetime"></a>Silinmiş Öğe işareti yaşam süresi
Silinmiş Öğe işareti yaşam bir kaldırma anılan ne kadar silinmiş bir nesneye belirler, Active Directory veritabanında tutulur. Silinmiş bir nesneyi silinmiş öğe işareti yaşam süresi geçtiğinde, çöp toplama işlemi Active Directory veritabanından otomatik olarak kaldırır.

Varsayılan silinmiş öğe işareti yaşam 180 gün Windows, en son sürümleri için ancak daha eski sürümlerindeki 60 gün olduğu ve açıkça bir Active Directory Yöneticisi tarafından değiştirilebilir.

Kullanım ömrü yaklaştığı ya da çoğaltma hatalarının yaşıyorsanız, bilmek önemlidir. İki etki alanı denetleyicisi kullanım ömrü devam eden bir çoğaltma hatası karşılaşırsanız, temel çoğaltma hata düzeltilene bile çoğaltma bu iki etki alanı denetleyicileri arasında devre dışı bırakıldı.

Silinmiş Öğe işareti yaşam süresi alan devre dışı çoğaltma gerçekleşmesini tehlikesi olduğu yerde belirlemenize yardımcı olur. Her bir hata **% 100'den TSL** kategorisi için kaynak ve hedef sunucu arasında en az bir orman için silinmiş öğe işareti yaşam çoğaltılmamış bir bölümünü temsil eder.

Bu durumda, yalnızca çoğaltma hatası düzeltme yeterli olmaz. En az çoğaltma yeniden başlatmadan önce kalan nesneleri yukarı temizlemek üzere el ile araştırma gerekir. Hatta bir etki alanı denetleyicisi yetkisini gerekebilir.

Kullanım ömrü kalıcı çoğaltma hataları belirlemeye ek olarak, ayrıca içine denk gelen tüm hataları dikkat edilmesi gereken istediğiniz **50-%75 TSL** veya **75-%100 TSL** kategorileri.

Bunlar büyük olasılıkla ihtiyaç duydukları müdahale çözümlenecek şekilde açıkça kalan, geçici olmayan hatalar var. Güzel bir haberimiz var, bunlar henüz ömründen ulaştığınıza değil ' dir. Varsa, bu sorunları en kısa sürede düzeltmek ve *önce* ömründen ulaşmadan, çoğaltmayı el ile müdahale en az yeniden başlatın.

AD çoğaltma durumu çözümü sayısını gösterir. Pano kutucuğu daha önce belirtildiği gibi *kritik* %75 (hatalar dahil olmak üzere kullanım ömrü'nın üzerinde olan hata olarak tanımlanır, ortamınızda çoğaltma hataları üzerinde TSL %100 olan). Bu sayı 0'da korumak ister.

> [!NOTE]
> Bir özel silinmiş öğe işareti yaşam süresi değeri ayarlanmış olsa bile bu yüzdeleri doğru güvenebileceğiniz şekilde tüm silinmiş öğe işareti yaşam süresi yüzdesi hesaplamalar, Active Directory ormanı için gerçek ömründen temel alır.
>
>

### <a name="ad-replication-status-details"></a>AD çoğaltma durumu ayrıntıları
Herhangi bir öğeyi, listelerden birine tıkladığınızda, bir günlük sorgusu kullanarak hakkındaki ek ayrıntıları bakın. Sonuçları yalnızca bu öğe ile ilgili hataları gösterecek şekilde filtrelenir. Tıklayarak etki alanı denetleyicisine altında listelendiğinden **hedef sunucu durumu (ADDC02)**, sorgu sonuçları için filtre gördüğünüz show hata etki alanı denetleyicisi ile hedef sunucu olarak listelenen:

![Sorgu sonuçlarında AD çoğaltma durumu hataları](./media/ad-replication-status/oms-ad-replication-search-details.png)

Buradan daha fazla filtrelemek, günlük sorguyu değiştirin ve benzeri. Azure İzleyici'de günlük sorguları kullanma hakkında daha fazla bilgi için bkz. [analiz günlük verilerini Azure İzleyici'de](../../azure-monitor/log-query/log-query-overview.md).

**HelpLink** alan belirli hata hakkında ek ayrıntılar ile TechNet sayfanın URL'sini gösterir. Kopyalayın ve bu bağlantı sorunlarını giderme ve hata düzeltme hakkında bilgi için tarayıcı pencerenizi yapıştırın.

Ayrıca **dışarı** sonuçları Excel'e. Verileri dışarı aktarma, çoğaltma hatası görselleştirmek istediğiniz herhangi bir şekilde yardımcı olabilir.

![Excel dışarı aktarılan AD çoğaltma durumu hataları](./media/ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD çoğaltma durumu ile ilgili SSS
**S: Ne sıklıkta AD çoğaltma durumu veriler güncelleştirildi mi?**
Y: Bilgiler, beş günde bir güncelleştirilir.

**S: Bu veriler sıklıkla güncelleştirilir yapılandırmak için bir yol var mı?**
Y: Şu anda değil.

**S: Tüm etki alanı denetleyicilerine my çoğaltma durumunu görmek için Log Analytics çalışma alanıma Ekle gerekiyor mu?**
Y: Hayır, yalnızca bir tek etki alanı denetleyicisi eklenmelidir. Log Analytics çalışma alanınızda birden çok etki alanı denetleyiciniz varsa, bunları tüm veriler Azure İzleyicisi'ne gönderilir.

**S: Herhangi bir etki alanı denetleyicileri Log Analytics çalışma alanıma Ekle istemiyorum. AD çoğaltma durumu çözümü kullanabilir miyim?**

Y: Evet. Etkinleştirmek için bir kayıt defteri anahtarının değerini ayarlayabilirsiniz. Bkz: [etkin olmayan etki alanı denetleyicisi](#enable-non-domain-controller).

**S: Veri koleksiyonu yapan işlemin adı nedir?**
Y: AdvisorAssessment.exe

**S: Ne kadar toplanacak veri için sürer?**
Y: Veri Toplama süresi, Active Directory ortamında boyutuna bağlıdır, ancak genellikle 15 dakikadan kısa sürer.

**S: Ne tür verilere toplanır?**
Y: Çoğaltma bilgileri LDAP toplanır.

**S: Verileri toplandığında yapılandırmak için bir yol var mı?**
Y: Şu anda değil.

**S: Verileri toplamak hangi izinlerin gerekiyor?**
Y: Active Directory normal kullanıcı izinlerini yeterlidir.

## <a name="troubleshoot-data-collection-problems"></a>Veri toplama sorunlarını giderme
Veri toplamak için Log Analytics çalışma alanınıza bağlı için en az bir etki alanı denetleyicisi AD çoğaltma durumu çözüm paketi gerektirir. Bir etki alanı denetleyicisine bağlanmak kadar belirten bir ileti görünür **veri hala toplanmakta olan**.

Etki alanı denetleyicilerinizden biri bağlama konusunda yardıma ihtiyacınız varsa, belgeleri görüntüleyebilirsiniz [Azure İzleyici bağlanmak Windows bilgisayarlara](../../azure-monitor/platform/om-agents.md). Alternatif olarak, etki alanı denetleyicinize bir System Center Operations Manager ortamı için zaten bağlıysa, belgeleri görüntüleyebilirsiniz [System Center Operations Manager bağlanmak için Azure İzleyici](../../azure-monitor/platform/om-agents.md).

Herhangi bir etki alanı denetleyicilerinizin doğrudan Azure İzleyici veya System Center Operations Manager bağlanmak istemiyorsanız bkz [etkin olmayan etki alanı denetleyicisi](#enable-non-domain-controller).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [sorgular Azure İzleyici'de oturum](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı Active Directory çoğaltma Durumu verisini görüntülemek için.
