---
title: StorSimple 8000 serisi cihaz için uyarıları görüntüleyin ve yönetin | Microsoft Docs
description: StorSimple uyarı koşulları ve önem derecesi, uyarı bildirimleri yapılandırma ve Uyarıları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: e86b6af562208e51e36b4679fd088ea399ce70b8
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
ms.locfileid: "27745787"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-storsimple-alerts"></a>StorSimple uyarıları görüntülemek ve yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

**Uyarıları** dikey penceresinde StorSimple cihaz Yöneticisi hizmeti gözden geçirin ve StorSimple cihaz – ilgili uyarıları gerçek zamanlı olarak temizlemek için bir yol sağlar. Bu dikey pencereden StorSimple aygıtlarınızın ve genel Microsoft Azure StorSimple çözümünüzle sistem durumu sorunları merkezden izleyebilirsiniz.

Bu öğretici, sık karşılaşılan uyarı koşulları, uyarı önem düzeyleri ve uyarı bildirimleri yapılandırmak nasıl açıklar. Ayrıca, hızlı bir şekilde belirli bir uyarıyı bulmak ve uygun şekilde yanıt olanak uyarı hızlı başvuru tabloları içerir.

![Uyarılar sayfası](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Sık karşılaşılan uyarı koşulları

StorSimple Cihazınızı yanıt koşullar çeşitli uyarılar oluşturur. En sık karşılaşılan uyarı koşullar şunlardır:

* **Donanım sorunları** – donanımınız sistem durumu hakkında size bu uyarılar. Bunlar, bir ağ arabirimi sorunlar varsa, bellenim yükseltmeleri, gerekiyorsa ya da veri sürücülerinizin biri ile ilgili bir sorun varsa bilmenizi sağlar.
* **Bağlantı sorunları** – Bu uyarılar veri aktarma zorluk olduğunda oluşur. İletişim sorunları veri aktarımı sırasında ve Azure depolama hesabından veya cihazlar ve StorSimple cihaz Yöneticisi hizmeti arasında bağlantı eksikliği nedeniyle oluşabilir. İletişim sorunları en zor hatanın çok fazla sayıda noktası olmadığından düzeltmek bazılarıdır. Her zaman ilk ağ bağlantısı ve Internet erişimi için daha gelişmiş sorun giderme açın devam etmeden önce kullanılabilir olduğunu doğrulamanız gerekir. Sorun giderme ile ilgili daha fazla yardım için Git [Bağlantıyı Sına cmdlet ile ilgili sorunları giderme](storsimple-8000-troubleshoot-deployment.md).
* **Performans sorunlarını** – sisteminizi ağır bir yük altında olduğu zaman gibi en iyi şekilde gerçekleştirirken değil, bu uyarılar nedeniyle oluşur.

Ayrıca, güvenlik, güncelleştirmelerinin veya iş hataları ilgili uyarıları görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarıların uyarı durumu olan etkisini ve uyarı yanıt gereksinimini bağlı olarak farklı önem düzeyleri vardır. Önem düzeyleri şunlardır:

* **Kritik** – yanıt sisteminizin başarılı performansını etkileyen bir koşul olarak bu uyarısıdır. Eylem, hizmet kesintiye StorSimple emin olmak için gereklidir.
* **Uyarı** – bu koşul çözümlenen varsa kritik duruma gelebilir. Durum araştırın ve sorunun temizlemek için gerekli herhangi bir eylemde bulunmanız gerekir.
* **Bilgi** – bu uyarı sisteminizi yönetme ve izleme de yararlı olabilecek bilgiler içerir.

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandırma

Uyarı koşulu her StorSimple cihazlar için e-postayla bildirilmesini istediğinizi seçebilirsiniz. Ayrıca, e-posta adreslerini girerek diğer uyarı bildirimi alıcılarını tanımlayabilirsiniz **diğer e-posta alıcılarını** noktalı virgülle ayırarak kutusu.

> [!NOTE]
> En fazla cihaz başına 20 e-posta adresleri girebilirsiniz.

E-posta bildirim bir aygıt için etkinleştirdikten sonra bildirim listesi üyeleri kritik uyarı her oluştuğunda bir e-posta iletisi alır. İletileri gönderecek  *storsimple-alerts-noreply@mail.windowsazure.com*  ve uyarı koşulu anlatmaktadır. Alıcılar tıklatabilirsiniz **Unsubscribe** kendilerini e-posta bildirim listesinden kaldırmak için.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Bir aygıt için uyarıların e-posta bildirimi etkinleştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Aygıtları listesinden seçin ve yapılandırmak istediğiniz cihaz seçeneğine tıklayın.
2. Git **ayarları** > **genel** cihaz için.

   ![Uyarıları dikey penceresi](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. İçinde **genel ayarları** dikey penceresinde, Git **uyarı ayarları** ve aşağıdakileri ayarlayın:
   
   1. İçinde **gönderme e-posta bildirimi** alan, select **Evet**.
   2. İçinde **hizmet yöneticilerinin e-posta** alan, select **Evet** için Hizmet Yöneticisi olması ve tüm ortak Yöneticiler uyarı bildirimleri alma.
   3. İçinde **diğer e-posta alıcılarını** alan, uyarı bildirimleri alması gereken diğer tüm alıcıların e-posta adreslerini girin. Adları biçiminde girin  *someone@somewhere.com* . E-posta adresleri ayırmak için noktalı virgül kullanın. En fazla cihaz başına 20 e-posta adresleri yapılandırabilirsiniz. 
      
3. Bir test e-posta bildirimi göndermek için tıklatın **test e-postası gönderme**. Test bildirimi iletir gibi StorSimple cihaz Yöneticisi hizmeti durum iletilerini görüntüler.

    ![Uyarı ayarlarını](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. Test e-posta gönderilirken bir bildirim görür. 
   
    ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Sınama bildirim iletisi gönderilemiyor, StorSimple cihaz Yöneticisi hizmeti uygun bir hata iletisi görüntüler. Birkaç dakika bekleyin ve ardından sınama bildirim iletisini göndermeyi yeniden deneyin. 

5. Yapılandırmayı tamamladıktan sonra tıklatın **kaydetmek**. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Görünüm ve izleme uyarıları

StorSimple cihaz Yöneticisi hizmeti Özet dikey önem düzeyine göre düzenlenmiş cihazlarınızda uyarıların sayısını en hızlı bir bakış sağlar.

![Uyarılar Panosu](./media/storsimple-8000-manage-alerts/device-summary4.png)

Önem düzeyi tıklatıldığında açılır **uyarıları** dikey. Sonuçlar yalnızca bu önem düzeyi eşleşen uyarılar içerir.

Bir uyarı listesinde tıklatarak ek ayrıntılarla uyarı bildirildi, en son ne zaman dahil olmak üzere Uyarı için aygıt ve uyarıyı çözmek için önerilen eylemi uyarının yinelenme sayısını sağlar. Donanım uyarı ise, bu da donanım bileşeni tanımlar.

![Donanım uyarı örneği](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Microsoft Support bilgileri göndermek gerekiyorsa, bir metin dosyasına uyarı ayrıntılarını kopyalayabilirsiniz. Öneri takip edip içi uyarı koşulu çözümledikten sonra uyarı seçerek cihaz uyarıdan temizlemelidir **uyarıları** dikey ve tıklayarak **temizleyin**. Birden çok uyarı temizlemek için her bir uyarı seçin, dışındaki herhangi bir sütun **uyarı** sütun ve ardından **temizleyin** temizlenecek tüm uyarıları seçtikten sonra. Bazı uyarılar otomatik olarak sorun çözüldüğünde veya sistem uyarı yeni bilgilerle güncelleştirir temizlenir unutmayın.

Tıkladığınızda **Temizle**, uyarı ve sorunu çözmek için harcanan gereken adımlar hakkında açıklama sağlamak üzere fırsatınız olur. Başka bir olay yeni bilgilerle tetikleniyorsa, bazı olaylar sistem tarafından temizlenir. Bu durumda, aşağıdaki iletiyi görürsünüz.

![Temiz uyarı iletisi](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sıralama ve İnceleme uyarıları

Böylece gözden geçirin ve bunları gruplara temizleyin uyarılar raporlarını çalıştırmak için daha verimli bulabilirsiniz. Ayrıca, **uyarıları** dikey penceresinde, en fazla 250 uyarıları görüntüleyebilirsiniz. Bu uyarı sayısı aşılırsa, tüm uyarıları varsayılan görünümünde görüntülenir. Hangi uyarıların görüntülenme biçimini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – ya da görüntüleyebilirsiniz **etkin** veya **temizlenmiş** uyarıları. Temizlenmiş uyarıları da el ile bir yönetici tarafından temizlenmiş veya sistem yeni bilgilerle Uyarı koşulu güncelleştirilmediğinden programlı olarak işaretli sırada etkin uyarılar hala, sisteminizde tetiklenen.
* **Önem derecesi** – tüm önem düzeyleri (kritik, uyarı, bilgi) ya da yalnızca bir belirli önem derecesi, yalnızca kritik uyarıları gibi uyarıları görüntüleyebilirsiniz.
* **Kaynak** – tüm kaynaklardan gelen uyarıları görüntülemek veya bu hizmeti veya bir ya da tüm aygıtları gelen uyarılar sınırlandırın.
* **Zaman aralığı** – belirterek **gelen** ve **için** tarih ve zaman damgaları göz önünde bulundurmanız uyarılara ilgilendiğiniz zaman diliminde.

![Uyarılar listesinde](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Uyarıları hızlı başvuru

Aşağıdaki tablolarda, ek bilgi ve öneriler yanı sıra kullanılabiliyorsa karşılaşabileceğiniz Microsoft Azure StorSimple uyarıları bazıları listelenmektedir. StorSimple cihaz uyarıları aşağıdaki kategoriden ayrılır:

* [Bulut bağlantı uyarıları](#cloud-connectivity-alerts)
* [Küme uyarıları](#cluster-alerts)
* [Olağanüstü durum kurtarma uyarıları](#disaster-recovery-alerts)
* [Donanım uyarıları](#hardware-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Yerel olarak sabitlenmiş birim uyarılarını](#locally-pinned-volume-alerts)
* [Ağ uyarıları](#networking-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)
* [Destek Paketi uyarıları](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Bağlantı <*bulut kimlik bilgisi adı*> oluşturulamıyor. |Depolama hesabı bağlantı kurulamıyor. |Cihazınız ile bir bağlantı sorunu olabilir gibi görünüyor. Lütfen çalıştırın `Test-HcsmConnection` StorSimple için Windows PowerShell arabiriminden cmdlet belirlemek ve sorunu düzeltmek için Cihazınızda. Ayarların doğru olduğunu, uyarının gerçekleşmesine neden olan depolama hesabı kimlik bilgileriyle sorunu olabilir. Bu durumda, kullanarak `Test-HcsStorageAccountCredential` çözebilmek için sorunları olup olmadığını belirlemek için cmdlet.<ul><li>Ağ ayarlarınızı denetleyin.</li><li>Depolama hesabı kimlik bilgilerinizi denetleyin.</li></ul> |
| Bir sinyal cihazınızdan son aldık değil <*numarası*> dakika. |Cihaza bağlanamıyor. |Bir bağlantı sorunu aygıtınızda gibi görünüyor. Lütfen kullanın `Test-HcsmConnection` StorSimple için Windows PowerShell arabiriminden cmdlet aygıtınızda belirlemek ve sorunu düzeltin veya ağ yöneticinize başvurun. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Bulut bağlantı başarısız olduğunda StorSimple davranışı

Üretimde çalışan StorSimple Cihazınızı için bulut bağlantı başarısız olursa ne olur?

StorSimple üretim Cihazınızda bulut bağlantı başarısız olursa, cihaz durumuna bağlı olarak aşağıdaki oluşabilir:

* **Yerel veriler için**: süre için olacaktır hiçbir kesintisi ve okuma sunulacak devam edecek. Ancak, bekleyen GÇ sayısını artırır ve bir sınırı aşan gibi okuma başarısız başlayabilirsiniz.

    Veri miktarına bağlı olarak, Cihazınızda, yazmaları ayrıca ilk birkaç saat sonra kesintisi için bulut Bağlantısı'nda kadar devam eder. Yazma sonra yavaşlayabilir ve bulut bağlantı birkaç saat boyunca kesintiye uğrarsa başarısız olmasına sonunda başlatın. (Var. geçici depolama cihazı buluta gönderilen verileri için Bu alan, veri gönderildiğinde temizlenir. Bağlantı başarısız olursa, bu depolama alanındaki verileri buluta gönderilen değil ve g/ç başarısız olur.)
* **Bulut veri**: çoğu bulut bağlantı hataları için hata döndürülür. Bağlantı geri yüklendikten sonra IOs birimini çevrimiçi duruma getirin gerek kalmadan kullanıcı sürdürülür. Nadir durumlarda, kullanıcı müdahalesi geri Azure portalından birimini çevrimiçi duruma getirmek için gerekli olabilir.
* **Devam eden bulut anlık görüntüleri için**: işlemi birkaç kez 4-5 saat içinde yeniden deneme ve bağlantı geri değil, bulut anlık görüntüleri başarısız olur.

### <a name="cluster-alerts"></a>Küme uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Cihaz bakım modunda değil. |Cihaz girmek veya bakım modundan çıktığını nedeniyle devredilir. Bu normaldir ve Eylem gerekmiyor. Bu uyarı onaylanan sonra uyarıları sayfasından temizleyin. |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Cihaz üretici yazılımını veya yazılım yalnızca güncelleştirildi. |Bir küme yük devretmesi bir güncelleştirme nedeniyle oluştu. Bu normaldir ve Eylem gerekmiyor. Bu uyarı onaylanan sonra uyarıları sayfasından temizleyin. |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Denetleyici kapatıldı veya yeniden. |Etkin denetleyicisi kapatıldı veya bir yönetici tarafından yeniden için cihaz üzerinde başarısız oldu. Eylem gerekmiyor. Bu uyarı onaylanan sonra uyarıları sayfasından temizleyin. |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Planlanan yük devretme. |Planlanmış bir yük devretme olduğunu doğrulayın. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin. |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Planlanmamış yük devretme. |StorSimple planlanmamış yük devretme işlemlerini otomatik olarak kurtarmaya yerleşik olarak bulunur. Bu uyarılar çok sayıda görürseniz, Microsoft Support başvurun. |
| Aygıt başarısız üzerinden <*aygıt adı*>. |Diğer/bilinmeyen neden. |Bu uyarılar çok sayıda görürseniz, Microsoft Support başvurun. Sorun çözüldükten sonra bu uyarı uyarılar sayfasından temizleyin. |
| Bir kritik aygıt hizmet durumu başarısız olarak bildirir. |DataPath Hizmeti hatası. |Yardım için Microsoft Support başvurun. |
| Ağ arabirimi için sanal IP adresi <*veri #*> durumu başarısız olarak bildirir. |Diğer/bilinmeyen neden. |Bazen geçici koşullar, bu uyarılar neden olabilir. Bu durumda, bu uyarıyı otomatik olarak bir süre sonra temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Ağ arabirimi için sanal IP adresi <*veri #*> durumu başarısız olarak bildirir. |Arabirim adı: <*veri #*> IP adresi <IP address> ağ üzerinde yinelenen bir IP adresi algılandığından çevrimiçi duruma getirilemiyor. |Yinelenen IP adresi ağdan kaldırıldığından emin olun veya farklı bir IP adresi arabirimiyle yeniden yapılandırın. |

### <a name="disaster-recovery-alerts"></a>Olağanüstü durum kurtarma uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Kurtarma işlemlerinin tüm bu hizmet ayarlarını geri yükleyemedi. Aygıt yapılandırma verilerini bazı cihazlar için tutarsız bir durumda değil. |Veri tutarsızlığı olağanüstü durum kurtarma işleminden sonra algılandı. |Şifrelenmiş veriler Service ile cihazda eşitlenmemiş. Cihaz yetkilendirmek <*aygıt adı*> StorSimple cihaz eşitleme işlemini başlatmak için Yöneticisi'nden. StorSimple için Windows PowerShell arabirimi çalıştırmak için kullanın `Restore-HcsmEncryptedServiceData` aygıtta <*aygıt adı*> cmdlet'i, eski parola güvenlik profili geri yüklemek için bu cmdlet'i bir girdi olarak sağlama. Ardından çalıştırın `Invoke-HcsmServiceDataEncryptionKeyChange` hizmet verileri şifreleme anahtarı güncelleştirmek için cmdlet. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin. |

### <a name="hardware-alerts"></a>Donanım uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Donanım bileşeni <*bileşen kimliği*> olarak durumu raporları <*durum*>. | |Bazen geçici koşullar, bu uyarılar neden olabilir. Öyleyse, bu uyarı bir süre sonra otomatik olarak temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Pasif denetleyiciyi hatalı. |Pasif (ikincil) denetleyicisi çalışmıyor. |Cihazınızı işlevseldir ancak denetleyicilerinizi birini düzgün çalışmıyor. Bu denetleyici yeniden başlatmayı deneyin. Sorun giderilmezse Microsoft Support başvurun. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Yedeklemeyi <*kaynak birim grubu kimliği*> başarısız oldu. |Yedekleme işi başarısız oldu. |Bağlantı sorunları, yedekleme işlemi başarıyla tamamlanmasını engelliyor. Hiçbir bağlantı sorunları varsa, yedekleme maksimum sayısı ulaşmış olabilirsiniz. Artık gerekmeyen ve işlemi yeniden deneyin yedeklemelere silin. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin. |
| Kopyalama <*yedekleme öğesi kimlikleri kaynak*> için <*hedef birim seri numaraları*> başarısız oldu. |Kopyalama işi başarısız oldu. |Yedekleme hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse, bulut bağlantı sorunları kopyalama işlemi başarıyla tamamlanmasını engelliyor mümkündür. Hiçbir bağlantı sorunları varsa, depolama sınırına ulaşmış olabilirsiniz. Artık gerekmeyen ve işlemi yeniden deneyin yedeklemelere silin. Bu sorunu çözmek için uygun eylemi attıktan sonra bu uyarı uyarılar sayfasından temizleyin. |
| Geri Yükleme <*yedekleme öğesi kimlikleri kaynak*> başarısız oldu. |Geri yükleme işi başarısız oldu. |Yedekleme hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse, bulut bağlantı sorunları geri yükleme işleminin başarıyla tamamlanmasını engelliyor mümkündür. Hiçbir bağlantı sorunları varsa, depolama sınırına ulaşmış olabilirsiniz. Artık gerekmeyen ve işlemi yeniden deneyin yedeklemelere silin. Bu sorunu çözmek için uygun eylemi attıktan sonra bu uyarı uyarılar sayfasından temizleyin. |

### <a name="locally-pinned-volume-alerts"></a>Yerel olarak sabitlenmiş birim uyarılarını

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Yerel birim oluşturulmasını <*birim adı*> başarısız oldu. |Birim oluşturma işi başarısız oldu. <*Başarısız, hata koduna karşılık gelen hata iletisi*>. |Bağlantı sorunları alanı oluşturma işlemi başarıyla tamamlanmasını engelliyor. Yerel olarak sabitlenmiş birimlerin sıkı sağlanır ve bulut katmanlı birimlere değişkenlere alanı oluşturma işlemi içerir. Hiçbir bağlantı sorunları varsa, cihazda yerel alan tüketmiş olabilir. Bu işlemi yeniden denemeden önce alanı aygıtta olup olmadığını belirler. |
| Yerel birim genişlemesi <*birim adı*> başarısız oldu. |Birim değişiklik işi nedeniyle başarısız oldu <*başarısız, hata koduna karşılık gelen hata iletisi*>. |Bağlantı sorunları Birim genişletme işlemi başarıyla tamamlanmasını engelliyor. Yerel olarak sabitlenmiş birimlerin sıkı sağlanır ve bulut katmanlı birimlere değişkenlere mevcut alanını genişletme işlemi içerir. Hiçbir bağlantı sorunları varsa, cihazda yerel alan tüketmiş olabilir. Bu işlemi yeniden denemeden önce alanı aygıtta olup olmadığını belirler. |
| Birim dönüştürülmesi <*birim adı*> başarısız oldu. |Birim türünden yerel olarak dönüştürmek için toplu dönüştürme işi katmanlı başarısız sabitlenmiş. |Yerel olarak sabitlenmiş türüne dönüştürme birimin katmanlı çalıştırılamadı tamamlandı. İşlemin başarıyla tamamlanmasını engelleyen bir bağlantı sorunu olmadığından emin olun. Git sorunları bağlantı sorunlarını giderme için [Test HcsmConnection cmdlet ile ilgili sorunları giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Dönüştürme sırasında buluta yerel olarak sabitlenmiş birimin verilerden bazıları geçmiş beri özgün yerel olarak sabitlenmiş birim artık katmanlı birim işaretlendi. Sonuç katmanlı birim hala geri kazanılacak gelecekteki yerel birimleri için cihazda yerel alan bırakmalısınız.<br>Tüm bağlantı sorunlarını giderin, uyarıyı temizleyin ve bu birimi tüm verileri yerel olarak yeniden kullanılabilir hale getirileceğini emin olmak için geri özgün yerel olarak sabitlenmiş birim türüne dönüştürün. |
| Birim dönüştürülmesi <*birim adı*> başarısız oldu. |Birim türünden dönüştürmek için toplu dönüştürme işi katmanlı yerel olarak sabitlenmiş için başarısız oldu. |Birimin türüne dönüştürme katmanlı yerel olarak sabitlenmiş için tamamlanamadı. İşlemin başarıyla tamamlanmasını engelleyen bir bağlantı sorunu olmadığından emin olun. Git sorunları bağlantı sorunlarını giderme için [Test HcsmConnection cmdlet ile ilgili sorunları giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Bu birimi için cihazda sıkı sağlanan alanı gelecekteki yerel birimler için artık alınabilmesini karşın bulutta bulunan veriler dönüştürme işleminin bir parçası olarak özgün katmanlı birim artık yerel olarak sabitlenmiş bir birim işaretlenmiş.<br>Tüm bağlantı sorunlarını giderin, uyarıyı temizleyin ve bu birim sıkı cihazda sağlanan yerel alan geri alınabilmesini sağlamak için geri özgün katmanlı birim türüne dönüştürün. |
| Yerel alan tüketim yerel anlık görüntüleri için yaklaşan <*birim grubu adı*> |Yerel anlık görüntüleri yedekleme ilkesinin alana yakında çalışabilir ve ana bilgisayar yazma hataları önlemek için geçersiz kılındı. |Bu yedekleme İlkesi grupla ilişkili birimler yüksek veri karmaşası yanında sık yerel anlık görüntüleri hızlı bir şekilde tüketilmesi cihazda yerel alan neden oluyor. Artık gerekli olmayan tüm yerel anlık görüntüleri silin. Ayrıca, daha az sıklıkta yerel anlık görüntülerini almak ve bulut anlık görüntüleri düzenli olarak gerçekleştirilecek sağlamak Bu yedekleme ilkesi için yerel anlık görüntü zamanlamaları güncelleştirin. Bu eylemler katılmaz, bu anlık görüntüler için yerel alan yakında tükendi ve sistem onlara konağı yazma başarıyla işlenmeyi devam etmesini sağlamak için otomatik olarak silecektir. |
| Yerel anlık görüntüleri <*birim grubu adı*> geçersiz hale getirildi. |Yerel anlık görüntüler için <*birim grubu adı*> geçersiz kılınan ve cihazda yerel alan aşan silinemiyor. |Bu gelecekte oluşmaz emin olmak için bu yedekleme ilkesi için yerel anlık görüntü zamanlama gözden geçirin ve artık gerekli olmayan tüm yerel anlık görüntüleri silin. Bu yedekleme İlkesi grupla ilişkili birimler yüksek veri karmaşası yanında sık yerel anlık görüntüleri hızlı bir şekilde tüketilmesi cihazda yerel alan neden olabilir. |
| Geri Yükleme <*yedekleme öğesi kimlikleri kaynak*> başarısız oldu. |Geri yükleme işi başarısız oldu. |Yedekleme, yerel olarak sabitlenmiş veya bu yedekleme İlkesi yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını doğrulamak yedekleme listesi yenileme hala geçerli olur. Yedekleme geçerliyse, bulut bağlantı sorunları geri yükleme işleminin başarıyla tamamlanmasını engelliyor mümkündür. Bu anlık görüntü grubunun bir parçası geri yerel olarak sabitlenmiş birimlerin tüm cihaza indirilip verilerini yoktur ve bu anlık görüntü grubundaki bir katmanlı ve yerel olarak sabitlenmiş birimleri karışımı varsa, birbirleri ile eşitlenmiş olmaz. Geri yükleme işlemi başarıyla tamamlamak için bu gruptaki çevrimdışı konaktaki birimlerinin almak ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerini yapılan tüm değişiklikler kaybolacak unutmayın. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| StorSimple hizmetleri başlayamadı. |DataPath hata |Sorun devam ederse, Microsoft Destek'e başvurun. |
| Yinelenen IP adresi 'Data0 için' algılandı. | |Sistem '10.0.0.1' IP adresi için bir çakışma algıladı. Aygıttaki ağ kaynağı 'Data0'  *<device1>*  çevrimdışı. Bu IP adresi bu ağdaki herhangi bir varlık tarafından kullanılmaz emin olun. Ağ sorununu gidermek için şu adrese gidin [sorun giderme için Get-NetAdapter cmdlet'i](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Bu sorunu çözme konusunda yardım almak için ağ yöneticinize başvurun. Sorun devam ederse, Microsoft Destek'e başvurun. |
| IPv4 (veya IPv6) 'Data0' Çevrimdışı adresidir. | |IP adresi '10.0.0.1.' ile ağ kaynak 'Data0' ve önek uzunluğu '22' cihazda  *<device1>*  çevrimdışı. Bu arabirim bağlı olduğu anahtar bağlantı noktalarını çalışır durumda olduğundan emin olun. Ağ sorununu gidermek için şu adrese gidin [sorun giderme için Get-NetAdapter cmdlet'i](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Kimlik doğrulama hizmetine bağlanamadı. |DataPath hata |URLthat kimliğini doğrulamak için kullanılan ulaşılabilir değil. Güvenlik Duvarı kurallarınız StorSimple cihaz için belirtilen URL desenlerini eklediğinizden emin olun. Azure portalında URL desenlerini hakkında daha fazla bilgi için https://aka.ms/ss-8000-network-reqs için gidin. Azure Bulutu kullanıyorsanız, URL desenlerini https://aka.ms/ss8000-gov-network-reqs içinde gidin.|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Cihaz yük aştı <*eşik*>. |Beklenen yanıt süreleri daha yavaş çalışır. |Cihazınızı kullanımı ağır bir giriş/çıkış yük altında bildirir. Bu, Cihazınızı yanı sıra gerektiği çalışmamasına neden olabilir. Başka bir cihaza taşınamıyor veya artık gerekli olan cihaza bağlı ve varsa belirlemek iş yükleri gözden geçirin.| StorSimple hizmetleri başlayamadı. |DataPath hata |Sorun devam ederse, Microsoft Destek'e başvurun. |ve Git geçerli durumunu [Cihazınızı izlemek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-monitor-device.md) |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Microsoft Support oturumu başladı. |Üçüncü taraf destek oturum erişilir. |Lütfen bu erişim yetkisi onaylayın. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin. |
| Parola <*öğesi*> içinde sona erecek <*süre*>. |Parola geçerlilik süresi yaklaşıyor. |Parolanızı süresi dolmadan önce değiştirin. |
| Güvenlik yapılandırma bilgileri için eksik <*öğesi kimliği*>. | |Bu birim kapsayıcısı ile ilişkili birimler, StorSimple yapılandırmanızı çoğaltmak için kullanılamaz. Verilerinizi güvenli bir şekilde saklandığından emin olmak için birim kapsayıcısı ve birim kapsayıcısı ile ilişkili herhangi bir birimi silmenizi öneririz. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin. |
| <*sayı*> başarısız oturum açma denemesi <*öğesi kimliği*>. |Birden çok başarısız oturum açma girişimi. |Cihazınız saldırı altında veya yetkisiz bir kullanıcı yanlış bir parolayla bağlanmaya çalışıyor.<ul><li>Yetkili kullanıcılar başvurun ve bu girişimler meşru bir kaynaktan olduğunu doğrulayın. Çok sayıda başarısız oturum açma denemesi görmeye devam ederseniz, uzaktan yönetimi devre dışı bırakma ve ağ yöneticinize başvurarak göz önünde bulundurun. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin.</li><li>Anlık Görüntü Yöneticisi'ni örneklerinizi doğru parola ile yapılandırılıp yapılandırılmadığını denetleyin. Uygun eylemi attıktan sonra bu uyarıyı uyarıları sayfasından temizleyin.</li></ul>Daha fazla bilgi için Git [süresi dolmuş cihaz parolasını değiştirme](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Hizmet verileri şifreleme anahtarı değiştirilirken bir veya daha fazla hata oluştu. | |Hizmet verileri şifreleme anahtarı değiştirilirken oluşan hatalar vardı. Hata koşulları ele sonra çalıştırın `Invoke-HcsmServiceDataEncryptionKeyChange` StorSimple için Windows PowerShell arabiriminden cmdlet hizmeti güncelleştirmek için Cihazınızda. Bu sorun devam ederse Microsoft desteğe başvurun. Sorunu çözdükten sonra bu uyarıyı uyarıları sayfasından temizleyin. |

### <a name="support-package-alerts"></a>Destek Paketi uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemleri |
|:--- |:--- |:--- |
| Destek paketi oluşturma başarısız oldu. |StorSimple paketi oluşturamadı. |Bu işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. Sorunu çözdükten sonra bu uyarıyı uyarıları sayfasından temizleyin. |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple hataları ve cihaz dağıtım sorunlarını giderme](storsimple-8000-troubleshoot-deployment.md).

