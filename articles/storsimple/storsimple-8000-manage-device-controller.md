---
title: StorSimple 8000 serisi cihaz Denetleyicilerini Yönet | Microsoft Docs
description: Durdurma, yeniden başlatın, kapatın veya, StorSimple cihaz denetleyicilerinin sıfırlama öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 5e461f340e1c58f64c6d645a1e47cfd811bc4de5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60505993"
---
# <a name="manage-your-storsimple-device-controllers"></a>StorSimple cihaz Denetleyicilerini Yönet

## <a name="overview"></a>Genel Bakış

Bu öğreticide, StorSimple cihaz denetleyicileri üzerinde gerçekleştirilen farklı işlemleri açıklanır. StorSimple cihazınızdaki denetleyicileri yedek (eş) bir Aktif-Pasif yapılandırmada denetleyicileridir. Belirli bir zamanda yalnızca bir denetleyici etkin ve tüm disk ve ağ işlemleri işleniyor. Diğer edilgen modda denetleyicisidir. Edilgen denetleyici etkin denetleyiciyi başarısız olursa, otomatik olarak etkinleşir.

Bu öğretici, cihaz denetleyicileri kullanarak yönetmek için adım adım yönergeler içerir:

* **Denetleyicileri** Cihazınızı StorSimple cihaz Yöneticisi hizmet dikey.
* StorSimple için Windows PowerShell.

StorSimple cihaz Yöneticisi hizmeti aracılığıyla cihaz Denetleyicilerini Yönet öneririz. Bir eylem yalnızca StorSimple için Windows PowerShell kullanarak gerçekleştirilebilir, öğreticiyi Not yapar.

Bu öğreticide okuduktan sonra şunları yapabilir:

* Yeniden başlatma veya bir StorSimple cihazı denetleyiciyi kapatma
* Bir StorSimple cihazı kapatmak
* StorSimple Cihazınızı fabrika ayarlarına sıfırlama

## <a name="restart-or-shut-down-a-single-controller"></a>Yeniden başlatma veya tek bir denetleyiciyi kapatma
Bir denetleyiciyi yeniden başlatma veya kapatma normal sistem işleminin bir parçası olarak gerekli değildir. Tek bir cihaz denetleyicisi için kapatma işlemleri, yalnızca başarısız cihaza bir donanım bileşenini değiştirme gerektiren durumlarda yaygındır. Bir denetleyiciyi yeniden başlatma, performans aşırı bellek kullanımı veya hatalı çalışan bir denetleyici tarafından etkilenir bir durumda da gerekebilir. Ve değiştirilen denetleyici test etkinleştirmek istiyorsanız bir başarılı denetleyicisi değiştirme işleminden denetleyiciyi yeniden başlatma gerekebilir.

Bir cihazı yeniden başlatma edilgen denetleyici kullanılabilir olduğunu varsayarak bağlı başlatıcıları kesintiye uğratan değil. Edilgen denetleyici değilse kullanılabilir veya kapalı kapalı, sonra etkin denetleyiciyi yeniden başlatma, kapalı kalma süresi ve hizmet kesintisi neden olabilir.

> [!IMPORTANT]
> * **Bu yedeklilik kaybı ve kapalı kalma süresi riskin artmasına neden olur olarak çalışan bir denetleyici hiçbir zaman fiziksel olarak kaldırılmalıdır.**
> * Aşağıdaki yordam yalnızca StorSimple fiziksel cihaz için geçerlidir. Başlatma, durdurma ve StorSimple Cloud Appliance'ı yeniden başlatma hakkında daha fazla bilgi için bkz. [iş bulut gereciyle](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

StorSimple için StorSimple cihaz Yöneticisi hizmetini Windows PowerShell veya Azure portalından bir tek cihaz denetleyicisi kapatma ya da yeniden başlatın.

Cihaz denetleyicilerinizi Azure portalından yönetmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-restart-or-shut-down-a-controller-in-azure-portal"></a>Azure portalında bir denetleyici kapatma veya yeniden başlatmak için
1. StorSimple cihaz Yöneticisi hizmetinize gidin **cihazları**. Cihaz listesinden Cihazınızı seçin. 

    ![Cihaz seçin](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Git **Ayarları > denetleyicileri**.
   
    ![StorSimple cihaz denetleyicilerinin sağlıklı olduğunu doğrula](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. İçinde **denetleyicileri** dikey penceresinde, Cihazınızda iki denetleyici durumunu olduğundan emin olun **sağlıklı**. Bir denetleyici seçin, sağ tıklayın ve ardından **yeniden** veya **kapatma**.

    ![Yeniden seçin veya StorSimple cihaz denetleyicilerinin Kapat](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Yeniden başlatma veya kapatma denetleyici için bir iş oluşturulur ve varsa geçerli uyarılarla sunulur. Yeniden başlatma veya kapatma izlemek için Git **hizmet > etkinlik günlüklerini** ve ardından, hizmetinize belirli parametreler olarak filtreleyin. Bir denetleyici kapatma, etkinleştirmek için denetleyici üzerindeki etkinleştirmek için güç düğmesine basın gerekecektir.

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de bir denetleyici kapatma veya yeniden başlatmak için
StorSimple için Windows powershell'den StorSimple Cihazınızda tek bir denetleyiciyi yeniden başlatma veya kapatma için aşağıdaki adımları gerçekleştirin.

1. Cihaz seri konsol veya uzak bir bilgisayardan telnet oturumu aracılığıyla erişin. Denetleyici 0 veya denetleyici 1'e bağlanmak için adımları izleyin. [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**.
3. Başlık iletisi (denetleyici 0 veya denetleyici 1) bağlandığınızdan denetleyicisini not edin ve etkin veya Pasif (Beklemede) denetleyicisi olup.
   
   * Komut isteminde, tek bir denetleyiciyi kapatmak için şunu yazın:
     
       `Stop-HcsController`
     
       Bu, bağlandığınızdan denetleyicisini kapatır. Etkin denetleyiciyi durdurursanız, ardından cihaz edilgen denetleyiciye devreder.

   * Komut isteminde bir denetleyiciyi yeniden başlatma için aşağıdakileri yazın:
     
       `Restart-HcsController`
     
       Bu, bağlandığınızdan denetleyicisini yeniden başlatır. Etkin denetleyiciyi yeniden başlatırsanız yeniden başlatmadan önce edilgen denetleyiciye devreder.

## <a name="shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak

Bu bölümde, bir çalışan veya uzak bir bilgisayardan başarısız StorSimple cihazını kapatma açıklanmaktadır. Her iki cihaz denetleyicisinin kapatma sonra bir cihaz kapalıdır. Bir cihaz kapatma cihaza fiziksel olarak taşındığında veya hizmet dışı alınır gerçekleştirilir.

> [!IMPORTANT]
> Cihazı kapatmak önce cihaz bileşenleri durumunu kontrol edin. Cihazınıza gidin ve ardından **Ayarları > donanım sistem durumu**. İçinde **durum ve donanım sistem durumu** dikey penceresinde tüm bileşenlerin LED durumunun yeşil olduğunu doğrulayın. Yalnızca iyi durumda cihaz, yeşil durumu olur. Cihazınızı aşağı kapatılıyor düzgün çalışmayan bir bileşenini değiştirme, başarısız (kırmızı) veya karşılık gelen bileşenleri için düzeyi düşürülmüş (sarı) durumu görürsünüz.


#### <a name="to-shut-down-a-storsimple-device"></a>Bir StorSimple cihazı kapatmak için

1. Kullanım [yeniden başlatma veya kapatma bir denetleyiciyi](#restart-or-shut-down-a-single-controller) yordamı, cihaz edilgen denetleyiciye kapatmak üzere. StorSimple için Windows PowerShell veya Azure portalında bu işlemi gerçekleştirebilir.
2. Etkin denetleyiciyi kapatmak için yukarıdaki adımı yineleyin.
3. Şimdi, cihazın arka düzlem aramanız gerekir. İki denetleyici tamamen kapatıldığından sonra her iki denetleyicide LED'lerini durumu kırmızı yanıp sönen. Cihazı kapatıp şu anda tamamen kapatmak gerekiyorsa, güç ve soğutma modülleri (PCMs) hem güç anahtarlarda kapalı konuma konusuna geçelim. Bu cihazı kapatıp açmanız gerekir.

## <a name="reset-the-device-to-factory-default-settings"></a>Cihazı varsayılan fabrika ayarlarına sıfırlama

> [!IMPORTANT]
> Cihazınızı fabrika varsayılan ayarlarına sıfırlamak gerekiyorsa, Microsoft Support başvurun. Aşağıda açıklanan yordam yalnızca Microsoft Support ile birlikte kullanılmalıdır.

Bu yordamda, Microsoft Azure StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak Fabrika varsayılan ayarlarına sıfırlamak açıklanmaktadır.
Bir cihazı sıfırlamak varsayılan olarak tüm kümesinden tüm verileri ve ayarları kaldırır.

Microsoft Azure StorSimple Cihazınızı fabrika varsayılan ayarlarına sıfırlamak için aşağıdaki adımları gerçekleştirin:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Cihaz StorSimple için Windows PowerShell'de varsayılan ayarlarına sıfırlamak için
1. Cihaz, seri konsol üzerinden erişin. Başlık iletisi bağlı olduğunuzdan emin olmak için kontrol **etkin** denetleyicisi.
2. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**.
3. İstemde, tüm verileri, meta verilerini ve denetleyici ayarları kaldırılıyor kümenin tamamının sıfırlamak için aşağıdaki komutu yazın:
   
    `Reset-HcsFactoryDefault`
   
    Bunun yerine tek bir denetleyici sıfırlamak için kullanın [sıfırlama HcsFactoryDefault](https://technet.microsoft.com/library/dn688132.aspx) cmdlet'iyle `-scope` parametre.)
   
    Sistem, birden çok kez yeniden başlatılır. Sıfırlama başarıyla tamamlandığında size bildirilir. Sistem modeline bağlı olarak, uygulamanın 45-60 dakika 8100 bir cihaz için ve bu işlemi tamamlamak 8600 ürünü için 60-90 dakika sürebilir.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Sorular ve cevaplar cihaz denetleyicilerini yönetme hakkında
Bu bölümde, biz bazı sık sorulan soruları özetlenen StorSimple cihaz denetleyicilerini yönetme ile ilgili.

**S.** Cihazımı iki denetleyicide sağlıklı ve açık olduğunda ne üzerinde yeniden başlatın veya etkin denetleyiciyi kapatmak?

**C.** Cihazınızda her iki denetleyicilerinin sağlıklı ve açık değilse, onaylamanız istenir. Seçebilirsiniz:

* **Etkin denetleyiciyi yeniden** – etkin bir denetleyici yeniden cihaz edilgen denetleyiciye yük devretmek neden bildirilir. Denetleyici yeniden başlatılır.
* **Bir etkin denetleyiciyi kapatmak** – etkin bir denetleyici kapatma kapalı kalma süresi sonuçları bildirilir. Denetleyicide etkinleştirmek için cihazdaki güç düğmesine itme gerekir.

**S.** Cihazımı edilgen denetleyiciye kullanılamıyor veya kapalı ise Kapalı ve yeniden başlatın veya etkin denetleyiciyi kapatmak ne olacak?

**C.** Cihaz edilgen denetleyiciye ise kullanılamıyor veya açık kapalı ve için seçin:

* **Etkin denetleyiciyi yeniden** – işleminin devam etmesini hizmetinin geçici kesinti neden olur ve onaylamanız istenir bildirilir.
* **Bir etkin denetleyiciyi kapatmak** – size bildirilir kapalı kalma süresi, işlem devam sonuçlanır. Ayrıca, cihaz üzerinde etkinleştirmek için bir veya iki denetleyicilerinde güç düğmesine basın gerekir. Onaylamanız istenir.

**S.** Denetleyiciyi yeniden başlatma veya kapatma zaman mu ilerleme başarısız oluyor?

**C.** Başlatma veya bir denetleyiciyi kapatma durumunda başarısız olabilir:

* Bir cihaz güncelleştirmesi devam ediyor.
* Zaten bir denetleyiciyi yeniden başlatma işlemi devam ediyor.
* Zaten bir denetleyici kapatma işlemi devam ediyor.

**S.** Nasıl, denetleyici veya yeniden kapatma fark edene?

**C.** Denetleyici dikey penceresinde denetleyici durumu kontrol edebilirsiniz. Denetleyici durumu, bir denetleyici yeniden başlatma veya kapatma sürecinde olup olmadığını gösterir. Ayrıca, **uyarılar** dikey bir bilgilendirme uyarısı denetleyicisi yeniden veya kapatıldıysa içerir. Denetleyici yeniden başlatma ve kapatma işlemleri de etkinlik günlüklerine kaydedilir. Etkinlik günlükleri hakkında daha fazla bilgi için Git [etkinlik günlüklerini görüntüleme](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**S.** Denetleyici yük devretmesi nedeniyle g/ç için herhangi bir etki var mı?

**C.** Başlatıcılar ve etkin denetleyiciyi arasındaki TCP bağlantıları sonucunda denetleyici yük devretmesi sıfırlanır, ancak edilgen denetleyici işlemi varsayar zaman yeniden başlatılır. Bu işlemin Kurs sırasında bir geçici (30 saniyeden kısa) duraklama g/ç etkinliğinde başlatıcıları ve cihaz arasında olabilir.

**S.** Kapatmalı ve kaldırılan sonra hizmet denetleyicim nasıl döndürür?

**C.** Bir denetleyici hizmetine döndürmek için bu kasaya açıklandığı eklemeniz gerekir [StorSimple Cihazınızda bir Denetleyici Modülü değiştirin](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bu öğreticide, listelenen yordamları kullanarak çözümlenemiyor, StorSimple cihaz denetleyicilerini ile herhangi bir sorunla karşılaşırsanız [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
* StorSimple cihaz Yöneticisi hizmetini kullanma hakkında daha fazla bilgi edinmek için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

