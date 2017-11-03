---
title: "Azure günlük analizi ile Active Directory çoğaltma durumunu izleme | Microsoft Docs"
description: "Active Directory çoğaltma durumunu çözüm paketi düzenli olarak çoğaltma hatalar için Active Directory ortamınızı izler ve OMS Panonuzda sonuçları raporlar."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfe52ef5d9d09ffe179faaf6ffbd90ef964fbda9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Günlük analizi ile Active Directory çoğaltma durumunu izleme

![AD çoğaltma durumu simgesi](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Active Directory kuruluş BT ortamında anahtar bir bileşenidir. Yüksek kullanılabilirlik ve yüksek performans sağlamak için Active Directory veritabanının kopyasını her etki alanı denetleyicisi vardır. Etki alanı denetleyicileri birbirleri ile kuruluş genelinde değişiklikleri yaymak için çoğaltılır. Bu çoğaltma işlemi hatalarına sorunları, çeşitli kuruluş genelinde neden olabilir.

AD çoğaltma durumunu çözüm paketi düzenli olarak çoğaltma hatalar için Active Directory ortamınızı izler ve OMS Panonuzda sonuçları raporlar.

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

* Değerlendirilecek etki alanının üyesi olan etki alanı denetleyicilerinde aracıları yüklemeniz gerekir. Veya üye sunuculara aracıları yükleyene ve aracıları için OMS AD çoğaltma veri göndermek için yapılandırmanız gerekir. Windows bilgisayarları için OMS bağlanmak nasıl anlamak için bkz: [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agents.md). Etki alanı denetleyicisi için OMS bağlanma, görmek istediğiniz mevcut bir System Center Operations Manager ortamının bir parçası ise [Operations Manager'a günlük analizi](log-analytics-om-agents.md).
* Active Directory çoğaltma durumunu çözümü açıklanan işlemi kullanarak OMS çalışma alanınıza ekleyin [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).  Başka bir yapılandırma işlemi gerekmez.

## <a name="ad-replication-status-data-collection-details"></a>AD çoğaltma durumu verileri toplama ayrıntıları
Aşağıdaki tabloda, veri toplama yöntemleri ve veri AD çoğaltma durumunun nasıl toplandığını ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |beş günde |

## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>İsteğe bağlı olarak, olmayan etki alanı denetleyicisi için OMS AD veri göndermesini etkinleştir
Herhangi bir etki alanı denetleyicileriniz doğrudan OMS bağlamak istemiyorsanız, AD çoğaltma durumunu çözüm paketi için veri toplamak ve sahip veri göndermek için etki alanınızdaki diğer OMS bağlı bilgisayar kullanabilirsiniz.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>OMS için AD veri göndermek bir etki alanı dışı denetleyicisi etkinleştirmek için
1. Bilgisayar, AD çoğaltma durumunu çözümünü kullanarak izlemek istediğiniz etki alanının bir üyesi olduğundan emin olun.
2. [Windows bilgisayarına bağlanmak için OMS](log-analytics-windows-agents.md) veya [var olan Operations Manager ortamınıza OMS kullanarak bağlanmak](log-analytics-om-agents.md), zaten bağlı değilse.
3. Bu bilgisayarda, aşağıdaki kayıt defteri anahtarını ayarlayın:

   * Anahtar: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management grupları\<ManagementGroupName > \Solutions\ADReplication**
   * Değer: **IsTarget**
   * Değer verisi: **true**

   > [!NOTE]
   > Bu değişiklikler, yeniden başlatmaya kadar Microsoft İzleme Aracısı hizmeti (HealthService.exe) etkili olmaz.
   >
   >

## <a name="understanding-replication-errors"></a>Çoğaltma hataları anlama
OMS için gönderilen AD çoğaltma durumu verileri bulduktan sonra şu anda kaç çoğaltma hataları belirten OMS panosunda aşağıdaki görüntüye benzer bir kutucuk bakın.  
![AD çoğaltma durumu kutucuğu](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritik çoğaltma hataları** veya üstü %75 hatalarını olan [ömründen](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) , Active Directory ormanı için.

Kutucuğa tıkladığınızda, hatalar hakkında daha fazla bilgi görüntüleyebilirsiniz.
![AD çoğaltma durumu Panosu](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Hedef sunucu durumu ve kaynak sunucu durumu
Bu sütunlar hedef sunucular ve çoğaltma hataları yaşıyor kaynak sunucular durumunu gösterir. Her etki alanı denetleyicisi adı numarasıyla o etki alanı denetleyicisindeki çoğaltma hatalarının sayısını belirtir.

Bazı sorunlar kaynak sunucu Perspektif ve diğerleri hedef sunucu açısından sorun giderme daha kolay olduğundan hataları hedef sunucuları ve kaynak sunucular için gösterilir.

Bu örnekte, birçok hedef sunucusu yaklaşık aynı hata sayısı sahip, ancak diğer tüm daha pek çok daha fazla hata var bir kaynak sunucu (ADDC35) görebilirsiniz. Bazı sorun olduğunu, çoğaltma ortaklarından veri göndermek başarısız olmasına neden olan ADDC35 üzerinde olasıdır. Üzerinde ADDC35 sorunlarını giderme hedef sunucu alanında görünen hataları çoğunu çözebilir.

### <a name="replication-error-types"></a>Çoğaltma hata türleri
Bu alan, kuruluşunuza algılanan hatalar türleri hakkında bilgi sağlar. Her hata benzersiz bir sayısal kod ve hatasının kök nedenini belirlemenize yardımcı olacak bir ileti vardır.

Üst halka hangi hataları daha görünür ve daha az sıklıkta ortamınızdaki bir fikir verir.

Birden çok etki alanı denetleyicileri aynı çoğaltma hatası kullanırken gösterir. Bu durumda, bulma veya bir etki alanı denetleyicisinde bir çözümü belirlemek mümkün sonra aynı hata tarafından etkilenen diğer etki alanı denetleyicilerinde yineleyin.

### <a name="tombstone-lifetime"></a>Silinmiş Öğe işareti yaşam süresi
Kullanım ömrü ilan başvurulan ne kadar süreyle silinmiş bir nesneye belirler, Active Directory veritabanında tutulur. Silinmiş bir nesneyi ömründen geçtiğinde, atık toplama işlemi Active Directory veritabanından otomatik olarak kaldırır.

Varsayılan ömründen 180 gün Windows, en son sürümleri ancak 60 gün eski sürümlerinde olduğu ve açıkça bir Active Directory Yöneticisi tarafından değiştirilebilir.

Kullanım ömrü yaklaştığı ya da çoğaltma hatalarının yaşıyorsanız, bilmeniz önemlidir. İki etki alanı denetleyicisi kullanım ömrü devam ederse çoğaltma hata ile karşılaşırsanız, çoğaltma bir temel alınan çoğaltma hatası sabit olsa bile bu iki etki alanı denetleyicileri arasında devre dışı bırakılmış.

Kullanım ömrü alanı devre dışı çoğaltma oluşmasını tehlike içinde olduğu yerlerde belirlemenize yardımcı olur. Her hata **% 100'den TSL** kategorisi için kaynak ve hedef sunucu arasında en az bir orman için kullanım ömrü çoğaltılmamış bir bölüm temsil eder.

Bu durumda, yalnızca çoğaltma hatanın düzeltilmesi yeterli olmaz. En azından, el ile tanımlamak ve çoğaltma yeniden başlatmadan önce nesneleri kalan yukarı temizlemek için araştırmak gerekir. Bir etki alanı denetleyicisi yetkisini bile gerekebilir.

Kullanım ömrü kalıcı çoğaltma hatalarını tanımlayan ek olarak, ayrıca içine dönmeden hataları dikkat istediğiniz **% 50-75 TSL** veya **75-%100 TSL** kategoriler.

Bu büyük olasılıkla ihtiyaç duydukları çözümlemek için müdahale etmenizi böylece açıkça kalan, değil geçici hatalardır. İyi haber, bunlar henüz ömründen ulaştığınızı değil ' dir. Kısa süre içinde bu sorunları düzeltmek varsa ve *önce* ömründen ulaşmak, çoğaltma ile en az el ile müdahale yeniden başlatın.

AD çoğaltma durumunu çözümü sayısını gösteren için Pano kutucuğuna daha önce belirtildiği gibi *kritik* çoğaltma hataları, ortamınızdaki (üzerinde % 100'tsl hatalarını dahil) kullanım ömrü % 75'den hatalarını tanımlanır. Bu sayı 0'da tutmak çalışmalar yapılmaktadır.

> [!NOTE]
> Bir özel silinmiş öğe işareti yaşam süresi değeri ayarlanmış olsa bile bu yüzdeleri doğru güven şekilde tüm silinmiş öğe işareti yaşam süresi yüzdesini hesaplamalar, Active Directory ormanı için gerçek kullanım ömrü dayanır.
>
>

### <a name="ad-replication-status-details"></a>AD çoğaltma durumu ayrıntıları
Listelerinden birinde herhangi bir öğeyi tıklatın, günlük arama kullanarak hakkında ek ayrıntılar bakın. Sonuçları yalnızca o öğe ile ilgili hataları göstermek için filtrelenir. Tıklayarak, örneğin, ilk etki alanı denetleyicisi altında listelenen **hedef sunucu durumu (ADDC02)**, arama sonuçlarını için filtre göster hataları bu etki alanı denetleyicisi ile hedef sunucu olarak listelenen:

![Arama sonuçlarında AD çoğaltma durumu hataları](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Buradan, daha fazla filtrelemek, arama sorguyu değiştirin ve benzeri. Günlük arama kullanma hakkında daha fazla bilgi için bkz: [oturum aramaları](log-analytics-log-searches.md).

**HelpLink** alan belirli hata hakkında daha ayrıntılı bilgi içeren bir TechNet sayfa URL'sini gösterir. Kopyalayın ve bu bağlantı sorunlarını giderme ve hata düzeltme hakkında bilgi için tarayıcı penceresine yapıştırın.

Tıklatarak **verme** sonuçları Excel'e vermek için. Verileri dışarı aktarma çoğaltma hata verileri istediğiniz herhangi bir şekilde görselleştirmenize yardımcı olabilir.

![Excel'de verilen AD çoğaltma durumu hataları](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD çoğaltma durumu hakkında SSS
**S: ne sıklıkta AD çoğaltma durumu verilerinin güncelleştirildi mi?**
Y: bilgiler beş günde bir güncelleştirilir.

**S: Bu verileri ne sıklıkta güncelleştirileceğini yapılandırmanıza bir yolu var mı?**
Şu anda A:.

**S: my etki alanı denetleyicilerinin çoğaltma durumunu görmek için OMS çalışma alanıma Ekle gerekiyor mu?**
Y: Hayır, yalnızca bir tek etki alanı denetleyicisi eklenmelidir. OMS çalışma alanınızda birden çok etki alanı denetleyicisi varsa, bunların tümünün verileri için OMS gönderilir.

**S: herhangi bir etki alanı denetleyicisine OMS çalışma alanıma Ekle istemiyorum. AD çoğaltma durumunu çözüm kullanabilir miyim?**
Y: Evet. Etkinleştirmek için bir kayıt defteri anahtarının değerini ayarlayabilirsiniz. Bkz: [olmayan etki alanı denetleyicisi için OMS AD veri göndermesini sağlamak için](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**S: adı olmadığından veri toplama işlemi nedir?**
Y: AdvisorAssessment.exe

**S: ne kadar süreyle verilerinin toplanmasını sürer?**
Y: veri toplama zamanı Active Directory ortamında boyutuna bağlıdır, ancak genellikle 15 dakikadan kısa sürer.

**S: hangi türde veri toplanır?**
Y: çoğaltma bilgilerini LDAP toplanır.

**S: verileri toplandığında yapılandırmak için bir yol var mı?**
Şu anda A:.

**S: verileri toplamak hangi izinlerin gerekiyor mu?**
A: Active Directory normal kullanıcı izinlerini yeterlidir.

## <a name="troubleshoot-data-collection-problems"></a>Veri toplama sorunlarını giderme
Verileri toplamak için AD çoğaltma durumunu çözüm paketi, OMS çalışma alanınızla bağlanması için en az bir etki alanı denetleyicisi gerektirir. Bir etki alanı denetleyicisine bağlanmak kadar belirten bir ileti görüntülenir **veri devam toplanır**.

Etki alanı denetleyicilerinden biri bağlama konusunda yardıma ihtiyacınız varsa, belgeleri görüntüleyebilirsiniz [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agents.md). Alternatif olarak, etki alanı denetleyicinizi zaten mevcut bir System Center Operations Manager ortamına bağlı ise, belgeleri görüntüleyebilirsiniz [System Center Operations Manager bağlanmak için günlük analizi](log-analytics-om-agents.md).

Herhangi bir etki alanı denetleyicileriniz doğrudan OMS veya SCOM bağlanmak istemiyorsanız bkz [olmayan etki alanı denetleyicisi için OMS AD veri göndermesini sağlamak için](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı Active Directory çoğaltma Durumu verisini görüntülemek için.
