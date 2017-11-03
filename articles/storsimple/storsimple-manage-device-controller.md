---
title: "StorSimple cihaz Denetleyicilerini Yönet | Microsoft Docs"
description: "Durdurmak, yeniden başlatın, kapatmak veya StorSimple cihaz denetleyicilerinin sıfırlama öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2016
ms.author: alkohli
ms.openlocfilehash: 67dbb0c4066002256efbab6061157c641527e441
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>StorSimple cihaz Denetleyicilerini Yönet
## <a name="overview"></a>Genel Bakış
Bu öğretici, StorSimple cihaz denetleyicileri üzerinde gerçekleştirilen farklı işlemler açıklanmaktadır. StorSimple Cihazınızı denetleyicileri yedek (eş) denetleyicileri bir Aktif-Pasif yapılandırmasında etkilenir. Belirli bir zamanda yalnızca bir denetleyici etkindir ve tüm disk ve ağ işlemleri işliyor. Diğer bir edilgen modda denetleyicisidir. Etkin denetleyicisi başarısız olursa, pasif denetleyiciyi otomatik olarak etkinleşir.

Bu öğretici aygıt denetleyicileri kullanarak yönetmek için adım adım yönergeleri içerir:

* **Denetleyicileri** bölümünü **Bakım** StorSimple Yöneticisi hizmet sayfası
* StorSimple için Windows PowerShell.

StorSimple Yöneticisi hizmeti aracılığıyla cihaz Denetleyicilerini Yönet öneririz. Bir eylem yalnızca StorSimple için Windows PowerShell kullanarak gerçekleştirilebilir, öğreticiyi Not yapar.

Bu öğretici okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Yeniden başlatma veya kapatma StorSimple cihaz denetleyicisi
* Bir StorSimple cihazı kapatmak
* StorSimple Cihazınızı fabrika ayarlarına sıfırlama

## <a name="restart-or-shut-down-a-single-controller"></a>Yeniden başlatma veya kapatma tek bir denetleyici
Bir denetleyici yeniden başlatma veya kapatma normal sistem işleminin bir parçası olarak gerekli değildir. Kapatma işlemleri tek aygıt denetleyicisi için yalnızca bir başarısız aygıt donanım bileşeni değiştirme gerektiren durumlarda yaygındır. Denetleyici yeniden başlatma, performans aşırı bellek kullanımı veya düzgün çalışmayan bir denetleyici tarafından etkilenir durumda da gerekebilir. Ve değiştirilen denetleyicisi test etkinleştirmek isterseniz, bir denetleyici başarılı denetleyicisi değiştirme sonra yeniden başlatmanız gerekebilir.

Bir aygıt yeniden başlatma pasif denetleyiciyi kullanılabilir olduğunu varsayarak bağlı başlatıcıları kesintiye uğratan değil. Bir pasif denetleyiciyi değilse kullanılabilir veya kapalı kapalı sonra etkin denetleyiciyi yeniden başlatma hizmeti ve kapalı kalma süresi kesilme neden olabilir.

> [!IMPORTANT]
> * **Bu artıklık kaybı ve kalma süresi riskin artmasına neden olur olarak çalışan bir denetleyicisinin hiçbir zaman fiziksel olarak kaldırılması gerekir.**
> * Aşağıdaki yordam yalnızca StorSimple fiziksel cihazı için geçerlidir. Başlatma, durdurma ve sanal cihaz yeniden başlatma konusunda daha fazla bilgi için bkz: [iş sanal cihazla](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Yeniden başlatma veya StorSimple için StorSimple Yöneticisi hizmet ya da Windows PowerShell Klasik Azure portalı kullanarak bir tek aygıt denetleyicisi kapatın

Klasik Azure portalından, aygıt denetleyicileri yönetmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Yeniden başlatmak veya Klasik Portalı'nda bir denetleyici kapatmak için
1. Gidin **cihazlar > Bakım**.
2. Git **donanım durumu** ve durum denetleyicilerinin aygıtınızda olduğundan emin olun **sağlıklı**.
   
    ![StorSimple cihaz denetleyicilerinin sağlıklı olduğunu doğrula](./media/storsimple-manage-device-controller/IC766017.png)
3. Aşağıdan **Bakım** sayfasında, **Denetleyicilerini Yönet**.
   
    ![StorSimple cihaz Denetleyicilerini Yönet](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Göremiyorsanız **Denetleyicilerini Yönet**, sonra da güncelleştirmeleri yüklemeniz gerekir. Daha fazla bilgi için bkz: [StorSimple Cihazınızı güncelleştirin](storsimple-update-device.md).
   > 
   > 
4. İçinde **denetleyicisi ayarlarını değiştir** iletişim kutusunda, aşağıdakileri yapın:
   
   1. Gelen **Select Controller** aşağı açılan listesinde, yönetmek istediğiniz denetleyicisi seçin. , Denetleyici 0 ve denetleyici 1 seçeneklerdir. Bu denetleyicileri da etkin veya pasif olarak tanımlanır.
      
      > [!NOTE]
      > Bu bir denetleyici yönetilemez kullanılamıyor veya kapalı kapalı ve aşağı açılan listede görünmez.
      > 
      > 
   2. Gelen **Eylem Seç** aşağı açılan listesinde, seçin **yeniden denetleyicisi** veya **denetleyicisi Kapat**.
      
       ![StorSimple cihazı pasif denetleyiciyi yeniden Başlat](./media/storsimple-manage-device-controller/IC766020.png)
   3. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-device-controller/IC740895.png).

Yeniden başlatın veya denetleyicisi kapatın. Aşağıdaki tabloda, yaptığınız seçimleri bağlı olarak neler ayrıntıları özetlenmiştir **denetleyicisi ayarlarını değiştir** iletişim kutusu.  

| Seçim # | İsterseniz... | Bu gerçekleşir. |
| --- | --- | --- |
| 1. |Pasif denetleyiciyi yeniden başlatın. |Denetleyici yeniden başlatmak için bir iş oluşturulur ve iş başarıyla oluşturulduktan sonra size bildirilecek. Bu denetleyici yeniden başlatır. Yeniden başlatma işlemi erişerek izleyebilirsiniz **hizmet > Pano > işlem günlükleri görüntülemek** ve parametreleri hizmetinize özgü göre filtreleme. |
| 2. |Etkin denetleyicisini yeniden başlatın. |Aşağıdaki uyarı görürsünüz: "etkin denetleyicisini yeniden başlatın, aygıt üzerinden pasif denetleyiciyi başarısız olur. Devam etmek istiyor musunuz?" </br>Bu işlemle devam etmeyi seçerseniz, sonraki adımlar pasif denetleyiciyi yeniden başlatmak için kullanılan olanlar aynı olacaktır (bkz. Seçimi 1). |
| 3. |Pasif denetleyiciyi kapatın. |Aşağıdaki iletiyi görürsünüz: "kapatma tamamlandıktan sonra açmak için denetleyicinizde güç düğmesine basın gerekir. Bu denetleyici kapatmak istediğinizden emin misiniz?" </br>Bu işlemle devam etmeyi seçerseniz, sonraki adımlar pasif denetleyiciyi yeniden başlatmak için kullanılan olanlar aynı olacaktır (bkz. Seçimi 1). |
| 4. |Etkin denetleyicisi kapatın. |Aşağıdaki iletiyi görürsünüz: "kapatma tamamlandıktan sonra açmak için denetleyicinizde güç düğmesine basın gerekir. Bu denetleyici kapatmak istediğinizden emin misiniz?" </br>Bu işlemle devam etmeyi seçerseniz, sonraki adımlar pasif denetleyiciyi yeniden başlatmak için kullanılan olanlar aynı olacaktır (bkz. Seçimi 1). |

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Yeniden başlatma veya StorSimple için Windows PowerShell'de denetleyicisi kapatmak için
Kapatıldı veya Azure Klasik portalından, StorSimple Cihazınızda tek bir denetleyici yeniden başlatmak için aşağıdaki adımları gerçekleştirin.

1. Cihaz seri konsol veya uzak bir bilgisayardan telnet oturumu kullanarak erişir. İçindeki adımları izleyerek denetleyici 0 veya denetleyici 1 bağlanmak [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**.
3. Başlık iletisi (denetleyici 0 veya denetleyici 1 için) bağlı denetleyicisi Not ve etkin veya Pasif (bekleme) denetleyicisi olup.
   
   * Komut isteminde tek bir denetleyici kapatmak için aşağıdakileri yazın:
     
       `Stop-HcsController`
     
       Bu bağlı denetleyicisi kapatılır. Etkin denetleyicisi durdurursanız, onu kapatılmadan önce sonra üzerinden pasif denetleyiciyi başarısız olur.
   * Komut isteminde bir denetleyicisi yeniden başlatmak için aşağıdakileri yazın:
     
       `Restart-HcsController`
     
       Bu, bağlandığınız denetleyicisi yeniden başlatılır. Etkin denetleyicisi yeniden başlatırsanız, üzerinden yeniden başlatmadan önce pasif denetleyiciye başarısız olur.

## <a name="shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak
Bu bölümde, bir çalışan veya uzak bir bilgisayardan başarısız StorSimple cihazını kapatmak üzere açıklanmaktadır. Bir aygıtın hem aygıt denetleyicileri kapatılıyor sonra kapalıdır. Cihaz fiziksel olarak taşındığı veya hizmet dışı gerçekleştirilecek bir aygıt kapatma yapılır.

> [!IMPORTANT]
> Cihazı kapatmanız önce aygıt bileşenlerinin durumunu kontrol edin. Gidin **cihazlar > bakım > donanım durum** ve tüm bileşenlerini LED durumunun yeşil olduğunu doğrulayın. Sağlıklı bir aygıtı yeşil durumu olacaktır. Cihazınızı aşağı kapatılıyor düzgün çalışmayan bir bileşeni Değiştir, başarısız (kırmızı) veya ilgili bileşenleri için düzeyi düşürülmüş (sarı) durumu görürsünüz.
> 
> 

#### <a name="to-shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak için
1. Kullanım [yeniden başlatma veya kapatma denetleyicisi](#restart-or-shut-down-a-single-controller) yordamı tanımlamak ve aygıtınızda pasif denetleyicisini kapatın. StorSimple için Klasik Azure portalı veya Windows PowerShell Bu işlemi gerçekleştirebilir.
2. Etkin denetleyicisi kapatmak için yukarıdaki adımı yineleyin.
3. Şimdi cihaz geri düzlemi aramak gerekir. İki denetleyicileri tamamen kapatıldığından sonra her iki denetleyicilerinde LED'leri durumu kırmızı yanıp sönen. Aygıtı şu anda tamamen kapatmasına izin gerekiyorsa, güç ve soğutma modülleri (PCMs) güç anahtarları kapalı konuma çevir. Bu aygıtı kapatıp açmanız gerekir.

<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Cihazı fabrika varsayılan ayarlarına sıfırlama
> [!IMPORTANT]
> Cihazınızı fabrika varsayılan ayarlarına sıfırlamak gerekiyorsa, Microsoft Support başvurun. Aşağıda açıklanan yordam, yalnızca, Microsoft Support ile birlikte kullanılmalıdır.
> 
> 

Bu yordam, Microsoft Azure StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak Fabrika varsayılan ayarlarına sıfırlamak açıklar.
Bir cihazı sıfırlamak tüm veriler ve ayarlar varsayılan olarak tüm küme kaldırır.

Microsoft Azure StorSimple Cihazınızı fabrika varsayılan ayarlarına sıfırlamak için aşağıdaki adımları gerçekleştirin:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Cihaz StorSimple için Windows PowerShell'de varsayılan ayarlarına sıfırlamak için
1. Cihaz seri Konsolu aracılığıyla erişim. Etkin denetleyiciye bağlı olduğunuzdan emin olmak için başlık iletisi denetleyin.
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**.
3. İsteminde tüm verileri, meta verilerini ve denetleyici ayarları kaldırılıyor kümenin tamamının sıfırlamak için aşağıdaki komutu yazın:
   
    `Reset-HcsFactoryDefault`
   
    Bunun yerine tek bir denetleyici sıfırlayabilmesi için [sıfırlama HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet'iyle `-scope` parametresi.)
   
    Sistem, birden çok kez yeniden başlatılır. Sıfırla başarıyla tamamlandığında size bildirilecek. Sistem modeline bağlı olarak, 45-60 dakika 8100 bir aygıt için ve bu işlemi tamamlamak bir 8600 60-90 dakika sürebilir.
   
   > [!TIP]
   > * Daha önce kullanın veya güncelleştirme 1.2 kullanıyorsanız `–SkipFirmwareVersionCheck` bellenim sürümü denetimi atlamak için parametre (Aksi halde, bellenim uyuşmazlığı hatası görürsünüz: Fabrika sıfırlaması bellenim sürümleri bir uyuşmazlık nedeniyle devam edemiyor. ).
   > * Fabrika sıfırlaması yordamı güncelleştirme 1 veya 1.1 kamu Portalı'nda çalıştırıyorsanız ve başarılı tek veya çift denetleyicisi değiştirme (denetleyicileriyle güncelleştirme 1 yazılımı sevk edilen değiştirme) gerçekleştirmiş StorSimple cihazlar için başarısız olabilir. Fabrika sıfırladığınızda görüntü için Güncelleştirme 1 yazılımı öncesini yok denetleyicisinde SHA1 dosyasının varlığı için doğrulanmış olur. Hata bu fabrika ayarlarına sıfırlayıp görürseniz, sonraki adımlara yardımcı olmak için Microsoft Support başvurun. Bu sorun, güncelleştirme 1 veya sonraki yazılım Fabrika sevk değiştirme denetleyicileriyle görülmez.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Sorular ve yanıtlar aygıt denetleyicileri yönetme hakkında
Bu bölümde, biz bazı sık sorulan sorular özetlenen sahipseniz yönetme StorSimple cihaz denetleyicilerinin ilgili.

**S.** Her iki denetleyicilerinde cihazımı sağlıklı ve açık olduğunda ne olacağını üzerinde ve yeniden başlatın veya etkin denetleyicisi Kapat?

**C.** İki denetleyiciye aygıtınızda sağlıklı ve açık ise, onaylamanız istenir. Seçebilirsiniz:

* **Etkin denetleyicisi yeniden** – denetleyicisini active yeniden cihazı pasif denetleyiciyi üzerinden vermesine neden bildirilir. Denetleyici yeniden başlatılır.
* **Etkin bir denetleyici Kapat** – denetleyicisini active kapatılıyor kapalı kalma sürelerine neden bildirilir. Denetleyicide etkinleştirmek için cihazda güç düğmesine basın gerekecektir.

**S.** Cihazımı pasif denetleyicisinde kullanılamıyor veya açık ise Kapalı ve yeniden başlatın veya etkin denetleyicisi Kapat de ne olur?

**C.** Pasif aygıtınızda denetleyicisiyse kullanılamıyor veya kapalı kapalı ve aktarmayı seçin:

* **Etkin denetleyicisi yeniden** – size işlemi devam hizmetinin geçici kesilme neden olur ve onaylamanız istenir bildirilecek.
* **Etkin bir denetleyici Kapat** – işlem devam kapalı kalma sürelerine neden ve cihazda etkinleştirmek için bir veya iki denetleyicilerinde güç düğmesine basın gerekecek bilgilendirilirsiniz. Onayınız istenir.

**S.** Ne zaman denetleyicisi yeniden başlatma veya kapatma mu ilerleme başarısız oluyor?

**C.** Başlatma veya kapatma denetleyicisi başarısız olabilir:

* Aygıt güncelleştirmesi devam ediyor.
* Zaten bir denetleyici yeniden başlatma işlemi devam ediyor.
* Bir denetleyici kapatma zaten devam ediyor.

**S.** Nasıl, bir denetleyici veya yeniden kapatma anlayıp?

**C.** Bakım sayfasında denetleyicisi durumunu kontrol edebilirsiniz. Denetleyici durumu bir denetleyicisi yeniden veya bırakıldığı kapatma olup olmadığını belirtir. Ayrıca, denetleyici yeniden veya kapatıldıysa uyarılar sayfasında bir bilgilendirme uyarısı içerir. Denetleyicisi yeniden başlatma ve kapatma işlemleri de işlem günlüklerine kaydedilir. İşlem günlükleri hakkında daha fazla bilgi için Git [işlem günlükleri görüntülemek](storsimple-service-dashboard.md#view-the-operations-logs).

**S.** Denetleyici yük devretmesi sonucunda g/ç için herhangi bir etki var mı?

**C.** TCP bağlantılarını başlatıcıları ve etkin denetleyici arasında sonucunda denetleyici yük devretmesi sıfırlanacak, ancak işlemi pasif denetleyiciyi varsayar olduğunda yeniden. Bu işlem sürecinde g/ç etkinliğini başlatıcıları ve aygıtı arasında bir geçici (30 saniyeden daha az) duraklama olabilir.

**S.** My denetleyicisi kapatılır ve kaldırılmış sonra hizmet dönüş nasıl?

**C.** Bir denetleyici hizmetine döndürmek için bu kasaya açıklandığı gibi eklemeniz gerekir [Denetleyici Modülü, StorSimple Cihazınızda Değiştir](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bu öğreticide, listelenen yordamları kullanarak çözümlenemiyor, StorSimple cihaz denetleyicilerini ile herhangi bir sorunla karşılaştığınızda [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).
* StorSimple Yöneticisi hizmetini kullanma hakkında daha fazla bilgi edinmek için şu adrese gidin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

