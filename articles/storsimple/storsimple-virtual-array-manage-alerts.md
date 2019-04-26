---
title: Microsoft Azure StorSimple Virtual Array uyarıları görüntüleyin ve yönetin | Microsoft Docs
description: StorSimple sanal dizisi uyarı koşullarına ve önem derecesi ve Uyarıları yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/12/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bb6ef5a87c5610d90188471db961ef20dfb18835
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302814"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple sanal dizisi için uyarıları yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti uyarıları özelliği, gözden geçirin ve StorSimple sanal dizilerine gerçek zamanlı olarak ilgili uyarılar temizlemek için bir yol sağlar. Uyarıları kullanabileceğiniz **hizmet özeti** merkezi olarak genel Microsoft Azure StorSimple çözümü, StorSimple sanal dizilerini ve sistem durumu sorunları izlemek için dikey pencere.

Bu öğreticide, uyarı bildirimleri, sık karşılaşılan uyarı koşulları, uyarı önem düzeyleri yapılandırmak nasıl ve Uyarıları görüntüleme ve izleme konusunda açıklanır. Ayrıca, belirli bir uyarı hızla bulup uygun şekilde yanıt sağlayan uyarı hızlı başvuru tabloları içerir.

![Uyarılar sayfası](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandır

StorSimple sanal dizisi, her uyarı koşullarından e-postayla bildirim almak isteyip istemediğinizi seçebilirsiniz. Ayrıca, e-posta adreslerini girerek başka uyarı bildirimi alıcıları tanımlayabilirsiniz **ek e-posta alıcılarını** kutusuna noktalı virgülle ayrılmış.

> [!NOTE]
> En fazla 20 e-posta adresi sanal dizi başına girebilirsiniz.

E-posta bildirimi bir sanal dizin için etkinleştirdikten sonra bildirim listesi üyelerinin kritik uyarı her oluştuğunda bir e-posta iletisi alırsınız. İletilerin gönderildiği *storsimple uyarılar noreply\@mail.windowsazure.com* ve uyarı koşulu anlatmaktadır. Alıcılar tıklayabilirsiniz **Unsubscribe** kendilerini e-posta bildirimi listesinden kaldırmak için.

#### <a name="to-enable-email-notification-for-alerts"></a>Uyarılar için e-posta bildirimlerini etkinleştirmek için

1. StorSimple cihaz Yöneticisi hizmetinize hem de Git **Yönetim** bölümünde seçin ve tıklayın **cihazları**. Listede görüntülenen cihazları seçin ve Cihazınızı tıklayın.
   
    ![Uyarı ayarları](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Bu açılır **ayarları** dikey penceresi. İçinde **cihaz ayarları** bölümünden **genel**. Bu açılır **genel ayarlar** dikey penceresi.
   
    ![uyarı bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. İçinde **genel ayarlar** dikey penceresinde, Git **uyarı ayarları** bölümünde ve aşağıdakileri ayarlayın:
   
   1. İçinde **e-posta bildirimi etkinleştir** alanın, Seç **Evet**.
   2. İçinde **hizmet yöneticilerine e-posta** alanın, Seç **Evet** Hizmet Yöneticisi olmasını istediğiniz ve tüm ortak yöneticilerin Uyarı bildirimlerini almasını.
   3. İçinde **ek e-posta alıcılarını** Uyarı bildirimlerini alması gereken diğer tüm alıcıların e-posta adreslerini girin. Biçiminde adlarını girin *birisi\@somewhere.com*. E-posta adreslerini ayırmak için noktalı virgül kullanın. En fazla sanal cihaz başına 20 e-posta adresi yapılandırabilirsiniz.
      
       ![uyarı bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Bir test e-posta bildirimi göndermek için tıklayın **sınama epostası Gönder**. Test bildirimi iletir olarak StorSimple cihaz Yöneticisi hizmeti durum iletilerini görüntüler.
      
       ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Test bildirim iletisi gönderilemez, StorSimple cihaz Yöneticisi hizmetine uygun bir ileti görüntülenir. Tıklayın **Tamam**, birkaç dakika bekleyin ve sonra test bildirim iletisi yeniden göndermeyi deneyin.
      >
      >
   5. Sayfanın en altında tıklayın **Kaydet** yapılandırmanızı kaydetmek için. Onayınız istendiğinde **Evet**’e tıklayın.
      
      ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Sık karşılaşılan uyarı koşulları

StorSimple Virtual Array'iniz yanıt koşullar çeşitli uyarılar oluşturur. En sık karşılaşılan uyarı koşullar şunlardır:

* **Bağlantı sorunları** – Bu uyarılar veri aktarımı zorluk olduğunda oluşur. İletişim sorunları, veri aktarımı sırasında ve Azure depolama hesabından veya sanal cihazlar ve StorSimple cihaz Yöneticisi hizmeti arasında bağlantı eksikliği nedeniyle oluşabilir. İletişim sorunları uygulamalarınızdaki hata çok sayıda noktası olmadığından düzeltmek bazılarıdır. Her zaman önce ağ bağlantısı ve Internet erişimi için daha gelişmiş sorun giderme açın devam etmeden önce kullanılabilir olduğunu da doğrulamanız gerekir. Bağlantı noktaları ve güvenlik duvarı ayarları hakkında daha fazla bilgi için Git [StorSimple sanal dizini sistem gereksinimleri](storsimple-ova-system-requirements.md). Sorun giderme konusunda yardım için Git [Test-Connection cmdlet'iyle sorun giderme](storsimple-troubleshoot-deployment.md).
* **Performans sorunlarını** – sisteminizi en uygun şekilde, ağır bir yük altında olduğunda gibi değil gerçekleştirirken bu uyarıya neden.

Ayrıca, güvenlik, güncelleştirmeleri veya işi hataları ile ilgili uyarılar görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarılar, uyarı durumu sahip etki ve uyarıya yanıt gereksinimini bağlı olarak farklı önem düzeyleri vardır. Önem dereceleri aşağıdaki gibidir:

* **Kritik** – yanıt sisteminizin başarılı performansını etkileyen bir koşul olarak bu uyarıdır. Eylem, hizmet kesintiye StorSimple emin olmak için gereklidir.
* **Uyarı** – bu koşula çözümlenmedi, kritik duruma gelebilir. Durum araştırmanıza ve sorunu temizlemek için gerekli herhangi bir eylemde bulunmanız gerekir.
* **Bilgi** – Bu uyarının sisteminizi yönetme ve izleme de yararlı olabilecek bilgiler içerir.

## <a name="view-and-track-alerts"></a>Uyarıları görüntüleme ve izleme

StorSimple cihaz Yöneticisi hizmeti Özet dikey penceresinde, bir bakışta önem düzeyine göre düzenlenmiş sanal cihazlarınızın uyarıların sayısını sağlar.

![Uyarılar Panosu](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Önem düzeyi açmak için tıklayın **uyarılar** dikey penceresi. Sonuçlar bu önem düzeyi eşleşen uyarıları içerir.

![Uyarı türüne göre kapsamlandırılan uyarılar raporu](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Uyarı bildirildi, en son ne zaman dahil olmak üzere Uyarı, uyarının cihaz ve uyarıyı çözmek için önerilen eylemi sayısı için ek ayrıntıları almak için listedeki bir uyarıya tıklayın.

![Uyarılar liste ve Ayrıntılar](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Microsoft Support bilgileri göndermek istiyorsanız, bir metin dosyasına uyarı ayrıntılarını kopyalayabilirsiniz. Öneri izleyen ve çözümlenen Uyarı koşulu şirket içi sonra listesinde uyarıyı temizlemeniz gerekir. Listeden uyarıyı seçin ve ardından **Temizle**. Birden çok uyarı temizlemek için her bir uyarı seçin, dışında herhangi bir sütunu **uyarı** sütun ve ardından **Temizle** tüm uyarılar temizlenmiş olması seçtikten sonra.

Tıkladığınızda **Temizle**, uyarı ve sorunu çözmek için gerçekleştirdiğiniz adımlar hakkında yorum sağlamak için bir fırsat sunulur.

![Uyarı açıklamaları](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Başka bir olay yeni bilgilerle tetikleniyorsa, bazı olaylar sistem tarafından temizlenir.

## <a name="sort-and-review-alerts"></a>Sıralama ve İnceleme uyarıları

**Uyarılar** dikey penceresinde, en fazla 250 uyarıları görüntüleyebilirsiniz. Bu uyarı sayısı aşılırsa tüm uyarılar varsayılan görünümü'nde görüntülenir. Hangi uyarıların görüntülenme biçimini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – ya da görüntüleyebilirsiniz **etkin** veya **işaretli** uyarıları. Temizlenmiş uyarıları ya da el ile bir yönetici tarafından temizlenmiş veya sistem uyarı koşulu bilgisiyle güncelleştirilmiştir çünkü program aracılığıyla seçili durumdayken, sisteminizde etkin uyarılar hala tetiklenmekte.
* **Önem derecesi** – tüm önem düzeyleri (kritik, uyarı ve bilgi) veya yalnızca bir belirli önem derecesi, yalnızca kritik uyarıları gibi uyarıları görüntüleyebilirsiniz.
* **Kaynak** – tüm kaynaklardan gelen Uyarıları görüntüle veya hizmet veya bir ya da tüm sanal cihazların gelenleri uyarılarını sınırlayın.
* **Zaman aralığı** – belirterek **gelen** ve **için** tarih ve zaman damgaları gösterdiğin uyarını ilgilendiğiniz zaman diliminde.

## <a name="alerts-quick-reference"></a>Uyarılar hızlı başvurusu

Aşağıdaki tablolarda, ek bilgi ve öneriler yanı sıra mümkün olan durumlarda karşılaşabileceğiniz StorSimple uyarıların bazıları listelenmektedir. StorSimple sanal dizisi uyarılarını aşağıdaki kategorilerden birine ayrılır:

* [Bulut bağlantı uyarıları](#cloud-connectivity-alerts)
* [Yapılandırma uyarılarını](#configuration-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Cihaz <*cihaz adı*> buluta bağlı değil. |Adlandırılmış cihaz buluta bağlanamıyor. |Buluta bağlanılamadı. Bunun nedeni, aşağıdakilerden biri olabilir:<ul><li>Cihazınızdaki ağ ayarları ile ilgili bir sorun olabilir.</li><li>Depolama hesabı kimlik bilgileri ile ilgili bir sorun olabilir.</li></ul>Bağlantı sorunlarını giderme hakkında daha fazla bilgi için Git [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) cihaz. |

### <a name="configuration-alerts"></a>Yapılandırma uyarılarını

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Şirket içi sanal cihaz yapılandırması desteklenmiyor. |Yavaş performans. |Geçerli yapılandırma performans düşüşüne neden olabilir. Sunucunuzun en düşük yapılandırma gereksinimlerini karşıladığından emin olun. Daha fazla bilgi için Git [StorSimple sanal dizi gereksinimleri](storsimple-ova-system-requirements.md). |
| Sağlanan disk alanınız bitiyor çalıştıran <*cihaz adı*\>. |Disk alanı uyarı. |Sağlanan disk alanınız azalıyor. Alan boşaltmak için başka bir birime veya paylaşıma iş yükü taşıyor veya veri silme göz önünde bulundurun. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Yedeklemeyi <*cihaz adı* \> işlemi tamamlanamadı. |Yedekleme işi başarısız. |Bir yedek oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Bağlantı sorunları Yedekleme işleminin başarıyla tamamlanmasını engelliyor. Hiçbir bağlantı sorunu olduğundan emin olun. Bağlantı sorunlarını giderme hakkında daha fazla bilgi için Git [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) sanal cihazınız için.</li><li>Kullanılabilir depolama sınırına ulaştınız. Yer kazanmak için artık gerekmeyen tüm yedeklemeler silme göz önünde bulundurun.</li></ul> Sorunları çözün, uyarıyı temizleyin ve işlemi yeniden deneyin. |
| Kopyası <*cihaz adı* \> işlemi tamamlanamadı. |İş hatası kopyalayın. |Bir kopya oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Yedekleme listenize geçerli olmayabilir. Hala geçerli olduğunu doğrulamak için listeyi yenileyin.</li><li>Bağlantı sorunları kopyalama işleminin başarıyla tamamlanmasını engelliyor. Hiçbir bağlantı sorunu olduğundan emin olun.</li><li>Kullanılabilir depolama sınırına ulaştınız. Yer kazanmak için artık gerekmeyen tüm yedeklemeler silme göz önünde bulundurun.</li></ul>Sorunları çözün, uyarıyı temizleyin ve işlemi yeniden deneyin. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Kimlik doğrulama hizmetine bağlanamadı. |DataPath hata |Kimlik doğrulaması için kullanılan URL erişilebilir değil. Güvenlik Duvarı kurallarınız StorSimple cihazı için belirtilen URL desenleri eklediğinizden emin olun. Azure portalındaki URL desenleri hakkında daha fazla bilgi için Git [gereksinimlerinde StorSimple Virtual Array](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Veri aktarımı beklenmeyen gecikmeler yaşıyorsunuz demektir. |Yavaş veri aktarın. |Depolama hizmeti ölçeklenebilirlik hedefleri aştığında azaltma hataları oluşur. Depolama hizmeti, bu tek istemcide ya da Kiracı hizmetin diğerleri çoğaltamaz kullandığınızdan emin olmak için yapıyorsa. Azure depolama hesabınızın sorun giderme hakkında daha fazla bilgi için Git [izleme, tanılama ve sorun giderme Microsoft Azure depolama](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Yerel ayırma disk alanı azalıyor <*cihaz adı*\>. |Yavaş yanıt süresi. |sağlanan toplam boyutu %10 <*cihaz adı* \> yerel ayrılmıştır ve cihaz şimdi çalışıyor düşük ayrılmış alan. İş yüküne <*cihaz adı* \> daha yüksek bir oluşturuyor veya dalgalanma oranını yakın zamanda geçirilmiş büyük miktarda veri. Bu performansın düşmesine neden olabilir. Bu sorunu çözmek için aşağıdaki eylemlerden birini göz önünde bulundurun:<ul><li>Bu cihazın bulut bant genişliğini artırın.</li><li>Azaltın veya başka bir birime veya paylaşıma iş yüklerinizi taşıyın.</li></ul> |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Parola <*cihaz adı* \> içinde sona erecek <*numarası* \> gün. |Parola uyarısı. |Parolanızı içinde sona erecek <*numarası* \> gün. Parolanızı değiştirmenizde yarar olabilir. Daha fazla bilgi için Git [sanal diziyi StorSimple cihaz Yöneticisi parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md). |

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple sanal dizisi hakkında bilgi edinin](storsimple-ova-overview.md).
