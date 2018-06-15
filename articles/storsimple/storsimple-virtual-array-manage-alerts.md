---
title: Microsoft Azure StorSimple sanal dizinin uyarıları görüntüleyin ve yönetin | Microsoft Docs
description: StorSimple sanal dizi uyarı koşulları ve önem derecesi ve Uyarıları yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.
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
ms.openlocfilehash: 7d4d680e3460fbeff73c2f334c6461da7967374d
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
ms.locfileid: "27786416"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple sanal dizi için uyarıları yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmet uyarıları özelliğini gözden geçirin ve StorSimple sanal diziler için bir gerçek zamanlı olarak ilgili uyarılar temizlemek bir yol sağlar. Uyarıları kullanabileceğiniz **hizmet özeti** StorSimple sanal dizileriniz ve genel Microsoft Azure StorSimple çözümünüzle sistem durumu sorunları merkezi olarak izlemek için dikey penceresini.

Bu öğretici uyarı bildirimleri, genel uyarı koşullar, uyarı önem düzeyleri yapılandırmak ve görüntülemek ve Uyarıları izlemek açıklar. Ayrıca, hızlı bir şekilde belirli bir uyarıyı bulmak ve uygun şekilde yanıt olanak uyarı hızlı başvuru tabloları içerir.

![Uyarılar sayfası](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandırma

Her bir StorSimple sanal dizileriniz için uyarı koşulları e-postayla bildirilmesini istediğinizi seçebilirsiniz. Ayrıca, e-posta adreslerini girerek diğer uyarı bildirimi alıcılarını tanımlayabilirsiniz **ek e-posta alıcılarını** noktalı virgülle ayırarak kutusu.

> [!NOTE]
> En fazla 20 e-posta adreslerini sanal dizi başına girebilirsiniz.


Bir sanal dizini e-posta bildirimi etkinleştirdikten sonra bildirim listesi üyeleri kritik uyarı her oluştuğunda bir e-posta iletisi alır. İletileri gönderecek  *storsimple-alerts-noreply@mail.windowsazure.com*  ve uyarı koşulu anlatmaktadır. Alıcılar tıklatabilirsiniz **Unsubscribe** kendilerini e-posta bildirim listesinden kaldırmak için.

#### <a name="to-enable-email-notification-for-alerts"></a>Uyarılar için e-posta bildirimleri etkinleştirmek için

1. StorSimple cihaz Yöneticisi hizmetinize ve içinde gidin **Yönetim** bölümünde, seçin ve tıklatın **aygıtları**. Görüntülenen aygıtları listesinden seçin ve aygıtınızı tıklatın.
   
    ![Uyarı ayarlarını](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Bu açılır **ayarları** dikey. İçinde **aygıt ayarları** bölümünde, select **genel**. Bu açılır **genel ayarları** dikey.
   
    ![Uyarı bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. İçinde **genel ayarları** dikey penceresinde, Git **uyarı ayarları** bölümünde ve aşağıdakileri ayarlayın:
   
   1. İçinde **e-posta bildirimi etkinleştir** alan, select **Evet**.
   2. İçinde **hizmet yöneticilerinin e-posta** alan, select **Evet** Hizmet Yöneticisi olmasını istediğiniz ve tüm ortak Yöneticiler uyarı bildirimleri alma.
   3. İçinde **ek e-posta alıcılarını** alan, uyarı bildirimleri alması gereken diğer tüm alıcıların e-posta adreslerini girin. Adları biçiminde girin  *someone@somewhere.com* . E-posta adresleri ayırmak için noktalı virgül kullanın. Maksimum sanal aygıt başına 20 e-posta adresi yapılandırabilirsiniz.
      
       ![Uyarı bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Bir test e-posta bildirimi göndermek için tıklatın **test e-postası gönderme**. Test bildirimi iletir gibi StorSimple cihaz Yöneticisi hizmeti durum iletilerini görüntüler.
      
       ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Sınama bildirim iletisi gönderilemiyor, StorSimple cihaz Yöneticisi hizmeti uygun bir mesaj görüntüler. Tıklatın **Tamam**, birkaç dakika bekleyin ve ardından sınama bildirim iletisini göndermeyi yeniden deneyin.
      > 
      > 
   5. Sayfanın alt kısmındaki tıklatın **kaydetmek** yapılandırmanızı kaydetmek için. Onayınız istendiğinde **Evet**’e tıklayın.
      
      ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Sık karşılaşılan uyarı koşulları

StorSimple sanal dizinizi yanıt koşullar çeşitli uyarılar oluşturur. En sık karşılaşılan uyarı koşullar şunlardır:

* **Bağlantı sorunları** – Bu uyarılar veri aktarma zorluk olduğunda oluşur. İletişim sorunları veri aktarımı sırasında ve Azure depolama hesabından veya sanal cihazlar ve StorSimple cihaz Yöneticisi hizmeti arasında bağlantı eksikliği nedeniyle oluşabilir. İletişim sorunları en zor hatanın çok fazla sayıda noktası olmadığından düzeltmek bazılarıdır. Her zaman ilk ağ bağlantısı ve Internet erişimi için daha gelişmiş sorun giderme açın devam etmeden önce kullanılabilir olduğunu doğrulamanız gerekir. Bağlantı noktaları ve güvenlik duvarı ayarları hakkında daha fazla bilgi için Git [StorSimple sanal dizinin sistem gereksinimleri](storsimple-ova-system-requirements.md). Sorun giderme ile ilgili daha fazla yardım için Git [Bağlantıyı Sına cmdlet ile ilgili sorunları giderme](storsimple-troubleshoot-deployment.md).
* **Performans sorunlarını** – sisteminizi ağır bir yük altında olduğu zaman gibi en iyi şekilde gerçekleştirirken değil, bu uyarılar nedeniyle oluşur.

Ayrıca, güvenlik, güncelleştirmelerinin veya iş hataları ilgili uyarıları görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarıların uyarı durumu olan etkisini ve uyarı yanıt gereksinimini bağlı olarak farklı önem düzeyleri vardır. Önem düzeyleri şunlardır:

* **Kritik** – yanıt sisteminizin başarılı performansını etkileyen bir koşul olarak bu uyarısıdır. Eylem, hizmet kesintiye StorSimple emin olmak için gereklidir.
* **Uyarı** – bu koşul çözümlenen varsa kritik duruma gelebilir. Durum araştırın ve sorunun temizlemek için gerekli herhangi bir eylemde bulunmanız gerekir.
* **Bilgi** – bu uyarı sisteminizi yönetme ve izleme de yararlı olabilecek bilgiler içerir.

## <a name="view-and-track-alerts"></a>Görünüm ve izleme uyarıları

StorSimple cihaz Yöneticisi hizmeti Özet dikey önem düzeyine göre düzenlenmiş cihazlarınızda sanal uyarıların sayısını en hızlı bir bakış sağlar.

![Uyarılar Panosu](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Önem düzeyi açmak için tıklatın **uyarıları** dikey. Sonuçlar yalnızca bu önem düzeyi eşleşen uyarılar içerir.

![Uyarı türüne göre kapsamlandırılan uyarılar raporu](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Uyarı bildirildi, en son ne zaman dahil olmak üzere Uyarı için aygıt ve uyarıyı çözmek için önerilen eylemi uyarının yinelenme sayısını ek ayrıntıları almak için listedeki bir uyarıyı tıklatın.

![Uyarılar listesinde ve ayrıntıları](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Microsoft Support bilgileri göndermek gerekiyorsa, bir metin dosyasına uyarı ayrıntılarını kopyalayabilirsiniz. Öneri takip edip içi uyarı koşulu çözümledikten sonra listeden uyarı temizlemeniz gerekir. Uyarı listesinden seçin ve ardından **Temizle**. Birden çok uyarı temizlemek için her bir uyarı seçin, dışındaki herhangi bir sütun **uyarı** sütun ve ardından **temizleyin** temizlenecek tüm uyarıları seçtikten sonra.

Tıkladığınızda **Temizle**, uyarı ve sorunu çözmek için harcanan gereken adımlar hakkında açıklama sağlamak üzere fırsatınız olur. 

![Uyarı açıklamaları](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Başka bir olay yeni bilgilerle tetikleniyorsa, bazı olaylar sistem tarafından temizlenir. 

## <a name="sort-and-review-alerts"></a>Sıralama ve İnceleme uyarıları

**Uyarıları** dikey penceresinde, en fazla 250 uyarıları görüntüleyebilirsiniz. Bu uyarı sayısı aşılırsa, tüm uyarıları varsayılan görünümünde görüntülenir. Hangi uyarıların görüntülenme biçimini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – ya da görüntüleyebilirsiniz **etkin** veya **temizlenmiş** uyarıları. Temizlenmiş uyarıları da el ile bir yönetici tarafından temizlenmiş veya sistem yeni bilgilerle Uyarı koşulu güncelleştirilmediğinden programlı olarak işaretli sırada etkin uyarılar hala, sisteminizde tetiklenen.
* **Önem derecesi** – tüm önem düzeyleri (kritik, uyarı, bilgi) ya da yalnızca bir belirli önem derecesi, yalnızca kritik uyarıları gibi uyarıları görüntüleyebilirsiniz.
* **Kaynak** – tüm kaynaklardan gelen uyarıları görüntülemek veya bu hizmeti veya bir ya da tüm sanal cihazlar gelen uyarılar sınırlandırın.
* **Zaman aralığı** – belirterek **gelen** ve **için** tarih ve zaman damgaları göz önünde bulundurmanız uyarılara ilgilendiğiniz zaman diliminde.

## <a name="alerts-quick-reference"></a>Uyarıları hızlı başvuru

Aşağıdaki tablolarda, ek bilgi ve öneriler yanı sıra kullanılabiliyorsa karşılaşabileceğiniz StorSimple uyarıları bazıları listelenmektedir. StorSimple sanal dizinin uyarıları aşağıdaki kategoriden ayrılır:

* [Bulut bağlantı uyarıları](#cloud-connectivity-alerts)
* [Yapılandırma uyarılarını](#configuration-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Aygıt  *<device name>*  buluta bağlı değil. |Adlandırılmış aygıt buluta bağlanamıyor. |Buluta bağlanılamadı. Bunu aşağıdakilerden biri nedeniyle olabilir:<ul><li>Cihazınızı ağ ayarları ile ilgili bir sorun olabilir.</li><li>Depolama hesabı kimlik bilgileri ile ilgili bir sorun olabilir.</li></ul>Bağlantı sorunlarını giderme hakkında daha fazla bilgi için Git [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) cihaz. |

### <a name="configuration-alerts"></a>Yapılandırma uyarılarını

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Şirket içi sanal aygıt yapılandırmasını desteklenmiyor. |Yavaş performans. |Geçerli yapılandırma, performans düşüşüne neden olabilir. Sunucunuzu en düşük yapılandırma gereksinimlerini karşıladığından emin olun. Daha fazla bilgi için Git [StorSimple sanal dizinin gereksinimleri](storsimple-ova-system-requirements.md). |
| Sağlanan disk alanı yetersiz kullanmakta olduğunuz <*aygıt adı*>. |Disk alanı uyarısı. |Size sağlanan disk alanı azalıyor. Alan boşaltmak için başka bir birime veya paylaşıma iş yükleri taşıma veya veri silme göz önünde bulundurun. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Yedeklemeyi <*aygıt adı*> işlemi tamamlanamadı. |Yedekleme iş başarısız oldu. |Bir yedekleme oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Bağlantı sorunları, yedekleme işlemi başarıyla tamamlanmasını engelliyor. Bağlantı sorunu olmadığından emin olun. Bağlantı sorunlarını giderme hakkında daha fazla bilgi için Git [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) sanal cihazınız için.</li><li>Kullanılabilir depolama sınırına ulaştınız. Alan boşaltmak için artık gerekli olmayan tüm yedeklemelerinin silme göz önünde bulundurun.</li></ul> Sorunları gidermek, uyarı işaretini kaldırın ve işlemi yeniden deneyin. |
| Kopyalama <*aygıt adı*> işlemi tamamlanamadı. |İş hatası kopyalayın. |Bir kopya oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Yedekleme listenizin geçerli olmayabilir. Hala geçerli olduğunu doğrulamanız için listeyi yenileyin.</li><li>Bağlantı sorunları kopyalama işlemi başarıyla tamamlanmasını engelliyor. Bağlantı sorunu olmadığından emin olun.</li><li>Kullanılabilir depolama sınırına ulaştınız. Alan boşaltmak için artık gerekli olmayan tüm yedeklemelerinin silme göz önünde bulundurun.</li></ul>Sorunları gidermek, uyarı işaretini kaldırın ve işlemi yeniden deneyin. |

### <a name="networking-alerts"></a>Ağ uyarıları
| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Kimlik doğrulama hizmetine bağlanamadı. |DataPath hata |Kimlik doğrulaması için kullanılan URL erişilebilir değil. Güvenlik Duvarı kurallarınız StorSimple cihaz için belirtilen URL desenlerini eklediğinizden emin olun. Azure portalında URL desenlerini hakkında daha fazla bilgi için Git [StorSimple sanal ağ gereksinimleri dizi](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Veri aktarımı beklenmeyen gecikme yaşıyor. |Yavaş veri aktarın. |Bir depolama birimi hizmeti ölçeklenebilirlik hedeflerini aşan azaltma hataları oluşur. Depolama hizmetinin tek bir istemci ya da Kiracı başkalarının ödün verme pahasına hizmeti kullandığınızdan emin olmak için bunu yapar. Azure depolama hesabınızın sorunlarını giderme hakkında daha fazla bilgi için Git [izleme, tanılama ve Microsoft Azure Storage sorun giderme](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Yerel ayırma disk alanı azalıyor <*aygıt adı*>. |Yavaş yanıt süresi. |% 10'için sağlanan toplam boyutu <*aygıt adı*> yerel ayrılmış aygıt ve şimdi çalışıyor düşük ayrılmış alan. İş yüküne göre <*aygıt adı*> yüksek bir üretiyor karmaşası veya oranını yakın zamanda geçirilmiş büyük miktarda veri. Bu performansın düşmesine neden olabilir. Bu sorunu çözmek için aşağıdaki eylemlerden birini göz önünde bulundurun:<ul><li>Bu cihaz bulut bant genişliğini artırır.</li><li>Azaltın veya başka bir birime veya paylaşıma iş yükleri taşıyın.</li></ul> |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Parola <*aygıt adı*> içinde sona erecek <*numarası*> gün. |Parola uyarı. |Parolanızı içinde sona erecek < numarası < gün. Parolanızı değiştirmeyi düşünün. Daha fazla bilgi için Git [StorSimple sanal dizinin cihaz Yöneticisi parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md). |

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple sanal dizinin hakkında bilgi edinin](storsimple-ova-overview.md).

