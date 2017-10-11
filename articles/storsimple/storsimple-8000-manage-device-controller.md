---
title: "StorSimple 8000 serisi cihaz Denetleyicilerini Yönet | Microsoft Docs"
description: "Durdurmak, yeniden başlatın, kapatmak veya StorSimple cihaz denetleyicilerinin sıfırlama öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 75c1bdb570967b6d1902697597f0b5bf3f4ffb7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>StorSimple cihaz Denetleyicilerini Yönet

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple cihaz denetleyicileri üzerinde gerçekleştirilen farklı işlemler açıklanmaktadır. StorSimple Cihazınızı denetleyicileri yedek (eş) denetleyicileri bir Aktif-Pasif yapılandırmasında etkilenir. Belirli bir zamanda yalnızca bir denetleyici etkindir ve tüm disk ve ağ işlemleri işliyor. Diğer bir edilgen modda denetleyicisidir. Etkin denetleyicisi başarısız olursa, pasif denetleyiciyi otomatik olarak etkinleşir.

Bu öğretici aygıt denetleyicileri kullanarak yönetmek için adım adım yönergeleri içerir:

* **Denetleyicileri** dikey aygıtınızın StorSimple cihaz Yöneticisi hizmeti.
* StorSimple için Windows PowerShell.

StorSimple cihaz Yöneticisi hizmeti aracılığıyla cihaz Denetleyicilerini Yönet öneririz. Bir eylem yalnızca StorSimple için Windows PowerShell kullanarak gerçekleştirilebilir, öğreticiyi Not yapar.

Bu öğretici okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Yeniden başlatma veya kapatma StorSimple cihaz denetleyicisi
* Bir StorSimple cihazı kapatmak
* StorSimple Cihazınızı fabrika ayarlarına sıfırlama

## <a name="restart-or-shut-down-a-single-controller"></a>Yeniden başlatma veya kapatma tek bir denetleyici
Bir denetleyici yeniden başlatma veya kapatma normal sistem işleminin bir parçası olarak gerekli değildir. Kapatma işlemleri tek aygıt denetleyicisi için yalnızca bir başarısız aygıt donanım bileşeni değiştirme gerektiren durumlarda yaygındır. Denetleyici yeniden başlatma, performans aşırı bellek kullanımı veya düzgün çalışmayan bir denetleyici tarafından etkilenir durumda da gerekebilir. Ve değiştirilen denetleyicisi test etkinleştirmek isterseniz, bir denetleyici başarılı denetleyicisi değiştirme sonra yeniden başlatmanız gerekebilir.

Bir aygıt yeniden başlatma pasif denetleyiciyi kullanılabilir olduğunu varsayarak bağlı başlatıcıları kesintiye uğratan değil. Bir pasif denetleyiciyi değilse kullanılabilir veya kapalı kapalı sonra etkin denetleyiciyi yeniden başlatma hizmeti ve kapalı kalma süresi kesilme neden olabilir.

> [!IMPORTANT]
> * **Bu artıklık kaybı ve kalma süresi riskin artmasına neden olur olarak çalışan bir denetleyicisinin hiçbir zaman fiziksel olarak kaldırılması gerekir.**
> * Aşağıdaki yordam yalnızca StorSimple fiziksel cihazı için geçerlidir. Başlatma, durdurma ve yeniden StorSimple bulut uygulaması hakkında daha fazla bilgi için bkz: [iş ile bulut uygulaması](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

Yeniden başlatın veya StorSimple için StorSimple Aygıt Yöneticisi'ni hizmet ya da Windows PowerShell Azure portalı üzerinden bir tek aygıt denetleyicisini kapatın.

Cihaz denetleyicilerinizi Azure portalından yönetmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-restart-or-shut-down-a-controller-in-azure-portal"></a>Yeniden başlatmak veya Azure portalında bir denetleyici kapatmak için
1. StorSimple cihaz Yöneticisi hizmetinize Git **aygıtları**. Cihazınızı aygıtları listesinden seçin. 

    ![Bir cihaz seçin](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Git **ayarlar > denetleyicileri**.
   
    ![StorSimple cihaz denetleyicilerinin sağlıklı olduğunu doğrula](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. İçinde **denetleyicileri** dikey penceresinde durum denetleyicilerinin aygıtınızda olduğundan emin olun **sağlıklı**. Bir denetleyici seçin, sağ tıklayın ve ardından **yeniden** veya **kapatma**.

    ![StorSimple cihaz denetleyicilerinin kapatın veya yeniden başlatma seçin](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Yeniden başlatma veya kapatma denetleyici için bir iş oluşturulur ve varsa geçerli uyarılarla sunulur. Yeniden başlatma veya kapatma izlemek için Git **hizmet > etkinlik günlükleri** ve parametreleri hizmetinize özgü göre filtreleyebilirsiniz. Bir denetleyici kapatıldı, açmak için denetleyici üzerindeki açmak için güç düğmesine basın gerekecektir.

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Yeniden başlatma veya StorSimple için Windows PowerShell'de denetleyicisi kapatmak için
Kapatıldı veya StorSimple için Windows powershell'den, StorSimple Cihazınızda tek bir denetleyici yeniden başlatmak için aşağıdaki adımları gerçekleştirin.

1. Cihaz seri konsol veya uzak bir bilgisayardan telnet oturumu aracılığıyla erişim. Denetleyici 0 veya denetleyici 1 bağlanmak için adımları [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**.
3. Başlık iletisi (denetleyici 0 veya denetleyici 1 için) bağlı denetleyicisi Not ve etkin veya Pasif (bekleme) denetleyicisi olup.
   
   * Komut isteminde tek bir denetleyici kapatmak için aşağıdakileri yazın:
     
       `Stop-HcsController`
     
       Bu, bağlandığınız denetleyicisi kapatır. Etkin denetleyicisi Durdur sonra aygıt üzerinden pasif denetleyiciyi başarısız olur.

   * Komut isteminde bir denetleyicisi yeniden başlatmak için aşağıdakileri yazın:
     
       `Restart-HcsController`
     
       Bu, bağlandığınız denetleyicisi yeniden başlatır. Etkin denetleyicisi yeniden başlatırsanız, üzerinden yeniden başlatmadan önce pasif denetleyiciye başarısız olur.

## <a name="shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak

Bu bölümde, bir çalışan veya uzak bir bilgisayardan başarısız StorSimple cihazını kapatmak üzere açıklanmaktadır. Aygıt denetleyicileri kapatma sonra bir aygıtı devre dışı bırakılır. Aygıt fiziksel olarak taşındığında veya hizmet dışı gerçekleştirilecek bir aygıt kapatma yapılır.

> [!IMPORTANT]
> Cihazı kapatmanız önce aygıt bileşenlerinin durumunu kontrol edin. Cihazınıza gidin ve ardından **ayarlar > donanım durumu**. İçinde **durumu ve donanım durumunu** dikey penceresinde tüm bileşenlerin LED durumunun yeşil olduğunu doğrulayın. Sağlıklı bir aygıtı yeşil durum vardır. Cihazınızı aşağı kapatılıyor düzgün çalışmayan bir bileşeni Değiştir, başarısız (kırmızı) veya ilgili bileşenleri için düzeyi düşürülmüş (sarı) durumu görürsünüz.


#### <a name="to-shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak için

1. Kullanım [yeniden başlatma veya kapatma denetleyicisi](#restart-or-shut-down-a-single-controller) yordamı tanımlamak ve aygıtınızda pasif denetleyicisini kapatın. StorSimple için Azure Portalı'nı veya Windows PowerShell Bu işlemi gerçekleştirebilir.
2. Etkin denetleyicisi kapatmak için yukarıdaki adımı yineleyin.
3. Şimdi cihaz geri düzlemi aramanız gerekir. İki denetleyicileri tamamen kapatıldığından sonra her iki denetleyicilerinde LED'leri durumu kırmızı yanıp sönen. Aygıtı şu anda tamamen kapatmasına izin gerekiyorsa, güç ve soğutma modülleri (PCMs) güç anahtarları kapalı konuma çevir. Bu aygıtı kapatıp açmanız gerekir.

## <a name="reset-the-device-to-factory-default-settings"></a>Cihazı fabrika varsayılan ayarlarına sıfırlama

> [!IMPORTANT]
> Cihazınızı fabrika varsayılan ayarlarına sıfırlamak gerekiyorsa, Microsoft Support başvurun. Aşağıda açıklanan yordam, yalnızca, Microsoft Support ile birlikte kullanılmalıdır.

Bu yordam, Microsoft Azure StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak Fabrika varsayılan ayarlarına sıfırlamak açıklar.
Bir cihazı sıfırlamak tüm veriler ve ayarlar varsayılan olarak tüm küme kaldırır.

Microsoft Azure StorSimple Cihazınızı fabrika varsayılan ayarlarına sıfırlamak için aşağıdaki adımları gerçekleştirin:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Cihaz StorSimple için Windows PowerShell'de varsayılan ayarlarına sıfırlamak için
1. Cihaz seri Konsolu aracılığıyla erişim. Başlık iletisi bağlı olduğunuzdan emin olmak için kontrol **etkin** denetleyicisi.
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**.
3. İsteminde tüm verileri, meta verilerini ve denetleyici ayarları kaldırılıyor kümenin tamamının sıfırlamak için aşağıdaki komutu yazın:
   
    `Reset-HcsFactoryDefault`
   
    Bunun yerine tek bir denetleyici sıfırlayabilmesi için [sıfırlama HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet'iyle `-scope` parametresi.)
   
    Sistem, birden çok kez yeniden başlatılır. Sıfırla başarıyla tamamlandığında size bildirilecek. Sistem modeline bağlı olarak, 45-60 dakika 8100 bir aygıt için ve bu işlemi tamamlamak bir 8600 60-90 dakika sürebilir.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Sorular ve yanıtlar aygıt denetleyicileri yönetme hakkında
Bu bölümde, biz bazı sık sorulan sorular özetlenen sahipseniz yönetme StorSimple cihaz denetleyicilerinin ilgili.

**S.** Her iki denetleyicilerinde cihazımı sağlıklı ve açık olduğunda ne olacağını üzerinde ve yeniden başlatın veya etkin denetleyicisi Kapat?

**C.** İki denetleyiciye aygıtınızda sağlıklı ve açık ise, onaylamanız istenir. Seçebilirsiniz:

* **Etkin denetleyicisi yeniden** – denetleyicisini active yeniden cihazı pasif denetleyiciyi üzerinden vermesine neden bildirilir. Denetleyici yeniden başlatır.
* **Etkin bir denetleyici Kapat** – denetleyicisini active kapatılıyor kapalı kalma süresi ile sonuçları bildirilir. Denetleyicide etkinleştirmek için cihazda güç düğmesine basın gerekir.

**S.** Cihazımı pasif denetleyicisinde kullanılamıyor veya açık ise Kapalı ve yeniden başlatın veya etkin denetleyicisi Kapat de ne olur?

**C.** Pasif aygıtınızda denetleyicisiyse kullanılamıyor veya kapalı kapalı ve aktarmayı seçin:

* **Etkin denetleyicisi yeniden** – işlem devam hizmetinin geçici kesilme neden olur ve onaylamanız istenir bildirilir.
* **Denetleyicisini active Kapat** – size bildirilir, işleme devam ederseniz kapalı kalma süresi ile sonuçlanır. Ayrıca aygıtta açmak için bir veya iki denetleyicilerinde güç düğmesine basın gerekir. Onayınız istenir.

**S.** Ne zaman denetleyicisi yeniden başlatma veya kapatma mu ilerleme başarısız oluyor?

**C.** Başlatma veya kapatma denetleyicisi başarısız olabilir:

* Aygıt güncelleştirmesi devam ediyor.
* Zaten bir denetleyici yeniden başlatma işlemi devam ediyor.
* Bir denetleyici kapatma zaten devam ediyor.

**S.** Nasıl, bir denetleyici veya yeniden kapatma anlayıp?

**C.** Denetleyici dikey denetleyicisi durumunu kontrol edebilirsiniz. Denetleyici durumu yeniden başlatma veya kapatma işleminde bir denetleyicisi olup olmadığını belirtir. Ayrıca, **uyarıları** dikey denetleyicisi yeniden veya kapatıldıysa bir bilgilendirme uyarısı içerir. Denetleyicisi yeniden başlatma ve kapatma işlemleri de acitivity günlüklerine kaydedilir. Acitivity günlükleri hakkında daha fazla bilgi için Git [etkinlik günlükleri görüntülemek](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**S.** Denetleyici yük devretmesi sonucunda g/ç için herhangi bir etki var mı?

**C.** TCP bağlantılarını başlatıcıları ve etkin denetleyici arasında sonucunda denetleyici yük devretmesi sıfırlanacak, ancak işlemi pasif denetleyiciyi varsayar olduğunda yeniden. Bu işlem sürecinde g/ç etkinliğini başlatıcıları ve aygıtı arasında bir geçici (30 saniyeden daha az) duraklama olabilir.

**S.** My denetleyicisi kapatılır ve kaldırılmış sonra hizmet dönüş nasıl?

**C.** Bir denetleyici hizmetine döndürmek için bu kasaya açıklandığı gibi eklemeniz gerekir [Denetleyici Modülü, StorSimple Cihazınızda Değiştir](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bu öğreticide, listelenen yordamları kullanarak çözümlenemiyor, StorSimple cihaz denetleyicilerini ile herhangi bir sorunla karşılaştığınızda [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi edinmek için şu adrese gidin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

