---
title: StorSimple 8000 serisi cihaz için uyarıları görüntüleyin ve yönetin | Microsoft Docs
description: StorSimple uyarı koşullarına ve önem derecesi, uyarı bildirimleri yapılandırma ve Uyarıları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.
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
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: c3be0cdf2ef33c26dfa9d177e9b34f808b1b862a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320439"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-storsimple-alerts"></a>StorSimple uyarıları görüntülemek ve yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış

**Uyarılar** StorSimple cihaz Yöneticisi hizmet dikey gözden geçirin ve gerçek zamanlı olarak StorSimple cihaz ile ilgili uyarılar temizlemek için bir yol sağlar. Bu dikey pencereden sistem durumu sorunları genel Microsoft Azure StorSimple çözümü, StorSimple cihazlarınızı merkezi olarak izleyebilirsiniz.

Bu öğreticide, sık karşılaşılan uyarı koşulları, uyarı önem düzeyleri ve uyarı bildirimleri yapılandırma açıklanır. Ayrıca, belirli bir uyarı hızla bulup uygun şekilde yanıt sağlayan uyarı hızlı başvuru tabloları içerir.

![Uyarılar sayfası](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Sık karşılaşılan uyarı koşulları

StorSimple Cihazınızı yanıt koşullar çeşitli uyarılar oluşturur. En sık karşılaşılan uyarı koşullar şunlardır:

* **Donanım sorunları** – Bu uyarılar donanım sistem durumu hakkında bilgi verin. Bir ağ arabirimi sorunlar varsa, üretici yazılımını yükseltme gerekli olup olmadığını veya veri sürücüleriniz biri ile ilgili bir sorun varsa size sağlarlar.
* **Bağlantı sorunları** – Bu uyarılar veri aktarımı zorluk olduğunda oluşur. İletişim sorunları, veri aktarımı sırasında ve Azure depolama hesabından veya StorSimple cihaz Yöneticisi hizmeti ile cihazlar arasındaki bağlantısının olmaması nedeniyle oluşabilir. İletişim sorunları uygulamalarınızdaki hata çok sayıda noktası olmadığından düzeltmek bazılarıdır. Her zaman önce ağ bağlantısı ve Internet erişimi için daha gelişmiş sorun giderme açın devam etmeden önce kullanılabilir olduğunu da doğrulamanız gerekir. Sorun giderme konusunda yardım için Git [Test-Connection cmdlet'iyle sorun giderme](storsimple-8000-troubleshoot-deployment.md).
* **Performans sorunlarını** – sisteminizi en uygun şekilde, ağır bir yük altında olduğunda gibi değil gerçekleştirirken bu uyarıya neden.

Ayrıca, güvenlik, güncelleştirmeleri veya işi hataları ile ilgili uyarılar görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarılar, uyarı durumu sahip etki ve uyarıya yanıt gereksinimini bağlı olarak farklı önem düzeyleri vardır. Önem dereceleri aşağıdaki gibidir:

* **Kritik** – yanıt sisteminizin başarılı performansını etkileyen bir koşul olarak bu uyarıdır. Eylem, hizmet kesintiye StorSimple emin olmak için gereklidir.
* **Uyarı** – bu koşula çözümlenmedi, kritik duruma gelebilir. Durum araştırmanıza ve sorunu temizlemek için gerekli herhangi bir eylemde bulunmanız gerekir.
* **Bilgi** – Bu uyarının sisteminizi yönetme ve izleme de yararlı olabilecek bilgiler içerir.

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandır

Uyarı koşulları StorSimple cihazlarınızın her biri için e-postayla bildirim almak isteyip istemediğinizi seçebilirsiniz. Ayrıca, e-posta adreslerini girerek başka uyarı bildirimi alıcıları tanımlayabilirsiniz **diğer e-posta alıcılarını** kutusuna noktalı virgülle ayrılmış.

> [!NOTE]
> En fazla 20 e-posta adresi cihaz başına girebilirsiniz.

Bir cihaz için e-posta bildirimi etkinleştirdikten sonra bildirim listesi üyelerinin kritik uyarı her oluştuğunda bir e-posta iletisi alırsınız. İletilerin gönderildiği *storsimple uyarılar noreply\@mail.windowsazure.com* ve uyarı koşulu anlatmaktadır. Alıcılar tıklayabilirsiniz **Unsubscribe** kendilerini e-posta bildirimi listesinden kaldırmak için.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Bir cihaz için uyarıların e-posta bildirimini etkinleştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Cihazlar listesinde, seçin ve yapılandırmak istediğiniz cihaza tıklayın.
2. Git **ayarları** > **genel** aygıt için.

   ![Uyarıları dikey penceresi](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. İçinde **genel ayarlar** dikey penceresinde, Git **uyarı ayarları** ve aşağıdakileri ayarlayın:
   
   1. İçinde **e-posta bildirimi gönder** alanın, Seç **Evet**.
   2. İçinde **hizmet yöneticilerine e-posta** alanın, Seç **Evet** için hizmet yöneticisinin ve tüm ortak yöneticilerin Uyarı bildirimlerini almasını.
   3. İçinde **diğer e-posta alıcılarını** Uyarı bildirimlerini alması gereken diğer tüm alıcıların e-posta adreslerini girin. Biçiminde adlarını girin *birisi\@somewhere.com*. E-posta adreslerini ayırmak için noktalı virgül kullanın. En fazla cihaz başına 20 e-posta adresi yapılandırabilirsiniz. 
      
3. Bir test e-posta bildirimi göndermek için tıklayın **sınama epostası Gönder**. Test bildirimi iletir olarak StorSimple cihaz Yöneticisi hizmeti durum iletilerini görüntüler.

    ![Uyarı ayarları](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. Test e-posta gönderildiğinde bir bildirim görür. 
   
    ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Test bildirim iletisi gönderilemez, StorSimple cihaz Yöneticisi hizmetine uygun bir hata iletisi görüntüler. Birkaç dakika bekleyin ve sonra test bildirim iletisi yeniden göndermeyi deneyin. 

5. Yapılandırmasını tamamladıktan sonra tıklayın **Kaydet**. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Gönderilen bildirim e-posta uyarıları test](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Uyarıları görüntüleme ve izleme

StorSimple cihaz Yöneticisi hizmeti Özet dikey penceresinde, bir bakışta önem düzeyine göre düzenlenmiş cihazlarınızda uyarıların sayısını sağlar.

![Uyarılar Panosu](./media/storsimple-8000-manage-alerts/device-summary4.png)

Önem düzeyi tıklayarak açılır **uyarılar** dikey penceresi. Sonuçlar bu önem düzeyi eşleşen uyarıları içerir.

Bir uyarı listesinde tıklayarak ek ayrıntılar ile uyarı bildirildi, en son ne zaman dahil olmak üzere Uyarı, uyarının cihaz ve uyarıyı çözmek için önerilen eylemi oluşum sayısını sağlar. Donanım uyarı ise, donanım bileşeni de belirler.

![Donanım uyarı örneği](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Microsoft Support bilgileri göndermek istiyorsanız, bir metin dosyasına uyarı ayrıntılarını kopyalayabilirsiniz. Öneri izleyen ve çözümlenen Uyarı koşulu şirket içi sonra uyarıda seçerek uyarıyı CİHAZDAN temizlemelisiniz **uyarılar** dikey penceresinde ve tıklayarak **Temizle**. Birden çok uyarı temizlemek için her bir uyarı seçin, dışında herhangi bir sütunu **uyarı** sütun ve ardından **Temizle** tüm uyarılar temizlenmiş olması seçtikten sonra. Bazı uyarılar sorun çözüldüğünde veya sistem uyarı yeni bilgilerle güncelleştirir. otomatik olarak temizlenir unutmayın.

Tıkladığınızda **Temizle**, uyarı ve sorunu çözmek için gerçekleştirdiğiniz adımlar hakkında yorum sağlamak için bir fırsat sunulur. Başka bir olay yeni bilgilerle tetikleniyorsa, bazı olaylar sistem tarafından temizlenir. Bu durumda, aşağıdaki iletiyi görürsünüz.

![Temiz uyarı iletisi](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sıralama ve İnceleme uyarıları

Raporları, uyarılar çalıştırın, böylece gözden geçirin ve bunları gruplar halinde temizlemek daha verimli bulabilirsiniz. Ayrıca, **uyarılar** dikey penceresinde, en fazla 250 uyarıları görüntüleyebilirsiniz. Bu uyarı sayısı aşılırsa tüm uyarılar varsayılan görünümü'nde görüntülenir. Hangi uyarıların görüntülenme biçimini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – ya da görüntüleyebilirsiniz **etkin** veya **işaretli** uyarıları. Temizlenmiş uyarıları ya da el ile bir yönetici tarafından temizlenmiş veya sistem uyarı koşulu bilgisiyle güncelleştirilmiştir çünkü program aracılığıyla seçili durumdayken, sisteminizde etkin uyarılar hala tetiklenmekte.
* **Önem derecesi** – tüm önem düzeyleri (kritik, uyarı ve bilgi) veya yalnızca bir belirli önem derecesi, yalnızca kritik uyarıları gibi uyarıları görüntüleyebilirsiniz.
* **Kaynak** – tüm kaynaklardan gelen Uyarıları görüntüle veya hizmet veya bir ya da tüm cihazlar gelenleri uyarılarını sınırlayın.
* **Zaman aralığı** – belirterek **gelen** ve **için** tarih ve zaman damgaları gösterdiğin uyarını ilgilendiğiniz zaman diliminde.

![Uyarılar listesinde](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Uyarılar hızlı başvurusu

Aşağıdaki tablolarda, ek bilgi ve öneriler yanı sıra mümkün olan durumlarda karşılaşabileceğiniz Microsoft Azure StorSimple uyarıların bazıları listelenmektedir. StorSimple cihaz uyarıları aşağıdaki kategorilerden birine ayrılır:

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

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Bağlantıyı <*bulut kimlik bilgisi adı*> kurulamıyor. |Depolama hesabına bağlanılamadı. |Cihazınızda bir bağlantı sorunu olabilir gibi görünüyor. Lütfen çalıştırın `Test-HcsmConnection` belirlemek ve sorunu düzeltmek için cihazınızdaki StorSimple için Windows PowerShell Arabirimi'nden cmdlet'i. Ayarlar doğruysa, sorun uyarının gerçekleştiği zaman depolama hesabı kimlik bilgileriyle olabilir. Bu durumda, `Test-HcsStorageAccountCredential` cmdlet'ini çözümlemek için sorunları olup olmadığını belirleyin.<ul><li>Ağ ayarlarınızı denetleyin.</li><li>Depolama hesabı kimlik bilgilerinizi kontrol edin.</li></ul> |
| Bir sinyal cihazınızdan son aldık değil <*numarası*> dakika. |Cihaz için bağlanamaz. |Cihazınızda bir bağlantı sorunu var gibi görünüyor. Lütfen kullanın `Test-HcsmConnection` cmdlet'ini StorSimple için Windows PowerShell arabiriminden Cihazınızı belirlemek ve sorunu düzeltin veya ağ yöneticinize başvurun. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Bulut bağlantısı başarısız olduğunda StorSimple davranışı

StorSimple Cihazınızı üretim ortamında çalışmasını için bulut bağlantısı başarısız olursa ne olur?

StorSimple üretim Cihazınızda bulut bağlantısı başarısız olursa, cihaz durumuna bağlı olarak aşağıdaki oluşabilir:

* **Cihazınızda yerel verileri**: Bir süre, hiçbir kesinti olur ve okuma alınacağı devam eder. Ancak, bekleyen GÇ'li sayısını artırır ve sınırı aşan şekilde okuma başarısız başlatabilir.

    Veri miktarına bağlı olarak Cihazınızda, yazma işlemleri de ilk birkaç saat sonra kesintisi için bulut bağlantısı devam eder. Yazma ardından yavaşlar ve sonunda bulut bağlantısı birkaç saat boyunca kesintiye uğrarsa başarısız olmaya Başlat. (Var. geçici depolama cihazı buluta itilecek olan veriler için Verileri gönderildiğinde, bu alanı temizlenir. Bağlantı başarısız olursa, bu depolama alanındaki verileri buluta gönderilen değil ve g/ç başarısız olur.)
* **Bulutta veri**: Çoğu bulut bağlantı hataları için hata döndürülür. Bağlantı kurulduktan sonra birimi çevrimiçi duruma getirmek zorunda olmadan kullanıcı IOs sürdürülür. Nadir durumlarda, kullanıcı müdahalesi geri Azure portalından birimi çevrimiçi duruma getirmek için gerekli olabilir.
* **Devam eden bulut anlık görüntüleri için**: İşlemi birkaç kez 4-5 saat içinde yeniden denenen ve bağlantı geri kazanılmazsa, bulut anlık görüntüleri başarısız olur.

### <a name="cluster-alerts"></a>Küme uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Cihaz yük devretti için <*cihaz adı*>. |Cihaz, bakım moduna geçer. |Cihaz girme veya bakım modundan çıkma nedeniyle yük devretti. Bu normaldir ve hiçbir eylem gerekmiyor. Bu uyarıyı onayladıktan sonra uyarılar sayfasından temizleyin. |
| Cihaz yük devretti için <*cihaz adı*>. |Cihaz üretici yazılımını veya yazılım yalnızca güncelleştirildi. |Bir küme yük devretmesi bir güncelleştirme nedeniyle oluştu. Bu normaldir ve hiçbir eylem gerekmiyor. Bu uyarıyı onayladıktan sonra uyarılar sayfasından temizleyin. |
| Cihaz yük devretti için <*cihaz adı*>. |Denetleyicisi kapatıldığı veya yeniden başlatıldı. |Etkin denetleyiciyi kapatıldığı veya bir yönetici tarafından yeniden için cihaz yük devretti. Eylem gerekmiyor. Bu uyarıyı onayladıktan sonra uyarılar sayfasından temizleyin. |
| Cihaz yük devretti için <*cihaz adı*>. |Planlı yük devretme. |Bu planlı yük devretme olup olmadığını doğrulayın. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| Cihaz yük devretti için <*cihaz adı*>. |Planlanmamış yük devretme. |StorSimple, planlanmamış yük devretme işlemleri otomatik olarak kurtarmak için oluşturulmuştur. Bu uyarı çok sayıda görürseniz, Microsoft Support başvurun. |
| Cihaz yük devretti için <*cihaz adı*>. |Diğer/bilinmeyen neden. |Bu uyarı çok sayıda görürseniz, Microsoft Support başvurun. Sorun çözüldükten sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| Kritik başlatma cihazını hizmet durumu başarısız olarak bildirir. |DataPath Hizmeti hatası. |Yardım için Microsoft Support başvurun. |
| Ağ arabirimi için sanal IP adresi <*veri #* > durumu başarısız olarak bildirir. |Diğer/bilinmeyen neden. |Bazen geçici koşullar Bu uyarılar neden olabilir. Bu durumda, bu uyarıyı otomatik olarak bir süre sonra temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Ağ arabirimi için sanal IP adresi <*veri #* > durumu başarısız olarak bildirir. |Arabirim adı: <*veri #* > IP adresi `<IP address>` ağ üzerinde yinelenen bir IP adresi algılandığı için çevrimiçi duruma getirilemiyor. |Yinelenen IP adresi ağdan kaldırıldığından emin olun veya farklı bir IP adresi arabirimiyle yeniden yapılandırın. |

### <a name="disaster-recovery-alerts"></a>Olağanüstü durum kurtarma uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Kurtarma işlemleri bu hizmetin ayarlarının tümünü geri yükleyemedi. Cihaz yapılandırması bazı cihazlar için tutarsız bir durumda verilerdir. |Veri tutarsızlığına olağanüstü durum kurtarma sonrasında algılandı. |Hizmetteki şifrelenmiş veriler ile cihazda eşitlenmez. Cihaz yetkilendirme <*cihaz adı*> StorSimple cihaz eşitleme işlemini başlatmak için Yöneticisi'nden. StorSimple için Windows PowerShell arabirimi çalıştırmak için kullanın `Restore-HcsmEncryptedServiceData` cihazda <*cihaz adı*> cmdlet'i, hizmetin güvenlik profilini geri yüklemek için bu cmdlet'in girdisi olarak eski parola sağlama. Ardından çalıştırın `Invoke-HcsmServiceDataEncryptionKeyChange` hizmet veri şifreleme anahtarını güncelleştirmek için cmdlet. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |

### <a name="hardware-alerts"></a>Donanım uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Donanım bileşeni <*bileşen Kimliğini*> olarak durum raporları <*durumu*>. | |Bazen geçici koşullar Bu uyarılar neden olabilir. Bu durumda, bu uyarı bir süre sonra otomatik olarak temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Edilgen denetleyici arızası. |Edilgen denetleyici (ikincil) çalışmıyor. |Cihazınız çalışıyor, ancak denetleyicilerinizden biri arızalı. Bu denetleyiciyi yeniden başlatmayı deneyin. Sorun çözülmezse Microsoft Support başvurun. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Yedeklemeyi <*kaynak birim Grup Kimliği*> başarısız oldu. |Yedekleme işi başarısız oldu. |Bağlantı sorunları Yedekleme işleminin başarıyla tamamlanmasını engelliyor. Hiçbir bağlantı sorunu varsa, yedekleme maksimum sayısına ulaşmış olabilirsiniz. İşlemi yeniden deneyin ve artık gerekmeyen yedekleri silin. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| Kopyası <*yedekleme öğesi kimlikleri kaynak*> için <*hedef birim seri numaraları*> başarısız oldu. |Kopyalama işi başarısız oldu. |Yedeğin hala geçerli olduğunu doğrulamak için yedek listesini yenileyin. Yedek geçerliyse, kopyalama işleminin başarıyla tamamlanmasını bulut bağlantı sorunları engelliyor. Hiçbir bağlantı sorunu varsa, depolama sınırına ulaşmış olabilirsiniz. İşlemi yeniden deneyin ve artık gerekmeyen yedekleri silin. Bu sorunu çözmek için gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| Geri Yükleme <*yedekleme öğesi kimlikleri kaynak*> başarısız oldu. |Geri yükleme işi başarısız oldu. |Yedeğin hala geçerli olduğunu doğrulamak için yedek listesini yenileyin. Yedek geçerliyse, geri yükleme işleminin başarıyla tamamlanmasını bulut bağlantı sorunları engelliyor. Hiçbir bağlantı sorunu varsa, depolama sınırına ulaşmış olabilirsiniz. İşlemi yeniden deneyin ve artık gerekmeyen yedekleri silin. Bu sorunu çözmek için gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |

### <a name="locally-pinned-volume-alerts"></a>Yerel olarak sabitlenmiş birim uyarılarını

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Yerel biriminin oluşturulması <*birim adı*> başarısız oldu. |Birim oluşturma işi başarısız oldu. <*Başarısız, hata koduna karşılık gelen hata iletisi*>. |Bağlantı sorunları, alan oluşturma işleminin başarıyla tamamlanmasını engelliyor. Yerel olarak sabitlenmiş birimler bol sağlanır ve alan oluşturma işleminin katmanlı birimler buluta taşabilir. Hiçbir bağlantı sorunu varsa, cihazdaki yerel alanı tüketmiş olabilirsiniz. Bu işlemi yeniden denemeden önce boşluk cihazda mevcut olup olmadığını belirler. |
| Yerel Birim genişletme <*birim adı*> başarısız oldu. |Birimi değiştirme işi nedeniyle başarısız oldu <*başarısız hata koduna karşılık gelen hata iletisi*>. |Bağlantı sorunları, Birim genişletme işleminin başarıyla tamamlanmasını engelliyor. Yerel olarak sabitlenmiş birimler bol sağlanır ve mevcut alanı genişletme işlemi katmanlı birimler buluta taşabilir. Hiçbir bağlantı sorunu varsa, cihazdaki yerel alanı tüketmiş olabilirsiniz. Bu işlemi yeniden denemeden önce boşluk cihazda mevcut olup olmadığını belirler. |
| Birim dönüştürme <*birim adı*> başarısız oldu. |Birim türünü yerel olarak dönüştürmek için birim dönüştürme işi katmanlı başarısız sabitlenmiş. |Dönüştürme türü için yerel olarak sabitlenmiş birimin katmanlı yüklenemedi tamamlandı. Hiçbir bağlantı sorunu işleminin başarıyla tamamlanmasını engelleyen olduğundan emin olun. Git sorunları bağlantı sorunlarını giderme için [Test-HcsmConnection cmdlet'iyle sorun giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Dönüştürme sırasında buluta bazı verileri yerel olarak sabitlenmiş birim geçmiş beri özgün yerel olarak sabitlenmiş birim artık katmanlı birim işaretlendi. Sonuç katmanlı birim hala alınamayacağından gelecekteki yerel birimler için cihazda yerel alan kaplamıyordur.<br>Bağlantı sorunları çözün, uyarıyı temizleyin ve bu birimi tüm verileri yerel olarak tekrar kullanılabilir hale getirileceğini emin olmak için geri özgün yerel olarak sabitlenmiş birim türüne dönüştürün. |
| Birim dönüştürme <*birim adı*> başarısız oldu. |Katmanlı birim türünü dönüştürmek için birim dönüştürme işi yerel olarak sabitlenmiş için başarısız oldu. |Türünden dönüştürme birimin katmanlı yerel olarak sabitlenmiş için tamamlanamadı. Hiçbir bağlantı sorunu işleminin başarıyla tamamlanmasını engelleyen olduğundan emin olun. Git sorunları bağlantı sorunlarını giderme için [Test-HcsmConnection cmdlet'iyle sorun giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Bu birimi için cihazda bol sağlanan alan gelecekteki yerel birimler için artık geri alınabilmesini sırada bulutta bulunan veriler dönüştürme işleminin bir parçası olarak özgün katmanlı birim artık yerel olarak sabitlenmiş bir birim işaretlenmiş.<br>Bağlantı sorunları çözün, uyarıyı temizleyin ve bu birimi bol cihazda sağlanan yerel alan geri alınabilmesini sağlamak için geri özgün katmanlı birim türüne dönüştürün. |
| Yerel anlık görüntüler için yerel alan tüketimi yaklaşan <*birim grup adı*> |Yedekleme ilkesinin yerel anlık görüntüler, yakında kalmayabilir ve konak yazma hatalarını önlemek için geçersiz kılınabilir. |Bu yedekleme İlkesi grubuyla ilişkili birimlerde yüksek veri değişim sıklığının yanı sıra sık yerel anlık görüntüler, hızlı bir şekilde kullanılması için cihazda yerel alan neden oluyor. Artık gerekmeyen yerel anlık görüntüleri silin. Ayrıca, daha az sıklıkta yerel anlık görüntülerini alabilir ve bulut anlık görüntüleri düzenli olarak gerçekleştirilen emin olmak Bu yedekleme ilkesinin yerel anlık görüntü zamanlamaları güncelleştirin. Bu eylemler gerçekleştirildikçe değil, bu anlık görüntüler için yerel alan yakında tüketilebilir ve sistem bunlar konak yazma başarıyla işlenen devam etmesini sağlamak için otomatik olarak siler. |
| Yerel anlık görüntüleri <*birim grup adı*> geçersiz kılındı. |Yerel anlık görüntüleri <*birim grup adı*> geçersiz kılındı ve cihazda yerel alan aşan silinemiyor. |Bu gelecekte oluşmaz emin olmak için bu yedekleme ilkesinin yerel anlık görüntü zamanlamalarını gözden geçirin ve artık gerekmeyen yerel anlık görüntüleri silin. Bu yedekleme İlkesi grubuyla ilişkili birimlerde yüksek veri değişim sıklığının yanı sıra sık yerel anlık görüntüler, hızlı bir şekilde kullanılması için cihazda yerel alan neden olabilir. |
| Geri Yükleme <*yedekleme öğesi kimlikleri kaynak*> başarısız oldu. |Geri yükleme işi başarısız oldu. |Yerel olarak sabitlenmiş veya yerel olarak sabitlenmiş hem de katmanlı birimleri bu yedekleme İlkesi'nde bir karışımını doğrulamak için yedek listesini yenileyin yedeğin hala geçerli değildir. Yedek geçerliyse, geri yükleme işleminin başarıyla tamamlanmasını bulut bağlantı sorunları engelliyor. Bu anlık görüntü grubunun bir parçası yüklenen yerel olarak sabitlenmiş birimlerin tüm cihaza indirilip verilerini yoksa ve bu anlık görüntü grubunda katmanlı ve yerel olarak sabitlenmiş birimlerin bir karışımı varsa, birbiriyle eşitlenmiş durumda olmaz. Geri yükleme işlemi başarıyla tamamlamak için bu gruba çevrimdışı konaktaki birimler alın ve geri yükleme işlemini yeniden deneyin. Herhangi bir değişiklik geri yükleme işlemi sırasında gerçekleştirilen toplu veri kaybı olmayacağını unutmayın. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| StorSimple hizmetleri başlayamadı. |DataPath hata |Sorun devam ederse, Microsoft Destek'e başvurun. |
| Yinelenen IP adresi 'Data0 için' algılandı. | |Sistem, '10.0.0.1' IP adresi için bir çakışma algıladı. Cihazdaki ağ kaynağı 'Data0'  *\<cihaz1 ' >* çevrimdışı. Bu IP adresinin bu ağdaki başka bir varlık tarafından kullanılmaz emin olun. Ağ sorunları gidermek için Git [Get-NetAdapter cmdlet'iyle sorun giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Bu sorunu çözmenize yardımcı olması için ağ yöneticinize başvurun. Sorun devam ederse, Microsoft Destek'e başvurun. |
| IPv4 (veya IPv6) adresi için 'Data0' çevrimdışı. | |IP adresi '10.0.0.1' olan ağ kaynağı 'Data0' önek uzunluğu '22' cihazda  *\<cihaz1 ' >* çevrimdışı. Bu arabirimin bağlı olduğu anahtar bağlantı noktalarını çalışır durumda olduğundan emin olun. Ağ sorunları gidermek için Git [Get-NetAdapter cmdlet'iyle sorun giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Kimlik doğrulama hizmetine bağlanamadı. |DataPath hata |URLthat kimliğini doğrulamak için kullanılan erişilebilir değil. Güvenlik Duvarı kurallarınız StorSimple cihazı için belirtilen URL desenleri eklediğinizden emin olun. Azure portalındaki URL desenleri hakkında daha fazla bilgi için https için Git:\//aka.ms/ss-8000-network-reqs. Azure kamu Bulutu kullanıyorsanız URL desenleri için https gidin:\//aka.ms/ss8000-gov-network-reqs.|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler | |
|:--- |:--- |:--- | --- |
| Cihaz yükü aştı <*eşiği*>. |Beklenen yanıt süreleri daha yavaş. |Cihazınız ağır bir giriş/çıkış yükü altında kullanım bildiriyor. Bu, cihazınızın olması gerektiği kadar iyi çalışmamasına neden olabilir. Başka bir cihaza taşınması veya artık gerekli olmayan cihaza bağlı ve varsa belirlemek iş yüklerini gözden geçirin.|
| StorSimple hizmetleri başlayamadı. |DataPath hata |Sorun devam ederse, Microsoft Destek'e başvurun. |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Microsoft Support oturumu başladı. |Üçüncü taraf destek oturumu erişilir. |Bu erişim yetkisi onaylayın. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| Parola <*öğesi*> içinde sona erecek <*sürenin uzunluğunu*>. |Parola süresinin sonu yaklaşıyor. |Parolanızı süresi dolmadan önce değiştirin. |
| Güvenlik yapılandırma bilgileri için eksik <*öğesi kimliği*>. | |Bu birim kapsayıcısı ile ilişkili birimler, StorSimple yapılandırmanızı çoğaltmak için kullanılamaz. Verilerinizin güvenle depolandığından emin olmak için birim kapsayıcısını ve birim kapsayıcısı ile ilişkili tüm birimleri silmenizi öneririz. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin. |
| <*sayı*> için oturum açma denemesi başarısız oldu <*öğesi kimliği*>. |Birden çok başarısız oturum açma çalışır. |Cihazınız saldırı altında veya yetkisiz bir kullanıcı yanlış bir parolayla bağlanmaya çalışıyor.<ul><li>Yetkili Kullanıcılarınızla başvurun ve bu girişimler meşru bir kaynaktan olduğunu doğrulayın. Çok sayıda başarısız oturum açma denemesi görmeye devam ederseniz, uzaktan yönetimi devre dışı bırakıp ağ yöneticinize başvurarak göz önünde bulundurun. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin.</li><li>Snapshot Yöneticisi örneklerinizin doğru parola ile yapılandırıldığını denetleyin. Gerekli işlemleri yaptıktan sonra uyarılar sayfasından bu uyarıyı temizleyin.</li></ul>Daha fazla bilgi için Git [süresi dolmuş cihaz parolasını değiştirme](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Hizmet veri şifreleme anahtarı değiştirilirken bir veya daha fazla hata oluştu. | |Hizmet veri şifreleme anahtarı değiştirilirken hatalarla vardı. Hata koşulları giderdikten sonra çalıştırın `Invoke-HcsmServiceDataEncryptionKeyChange` hizmeti güncelleştirmek için cihazınızdaki StorSimple için Windows PowerShell Arabirimi'nden cmdlet'i. Bu sorun devam ederse Microsoft desteğe başvurun. Sorunu çözdükten sonra uyarılar sayfasından bu uyarıyı temizleyin. |

### <a name="support-package-alerts"></a>Destek Paketi uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / Önerilen Eylemler |
|:--- |:--- |:--- |
| Destek Paketi oluşturulamadı. |StorSimple, paket oluşturamadı. |Bu işlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. Sorunu çözdükten sonra uyarılar sayfasından bu uyarıyı temizleyin. |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple hataları ve cihaz dağıtım sorunlarını giderme](storsimple-8000-troubleshoot-deployment.md).

