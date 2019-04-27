---
title: StorSimple sanal dizisi web kullanıcı Arabirimi Yönetim | Microsoft Docs
description: StorSimple sanal dizisi web UI aracılığıyla temel cihaz yönetim görevlerinin nasıl gerçekleştirileceğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 92671206a4171ca838423f55b526191ef30e5c35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630518"
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>StorSimple Virtual Array'iniz yönetmek için Web kullanıcı arabirimini kullanın
![Kurulum işlem akışı](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Genel Bakış
Öğreticiler, bu makalede, Microsoft Azure StorSimple sanal Mart 2016 genel kullanılabilirlik (GA) sürümünü çalıştıran dizisi için (diğer adıyla StorSimple şirket içi sanal cihazı) uygulanır. Bu makalede, StorSimple sanal dizisi üzerinde gerçekleştirilebilen yönetim görevleri ve karmaşık iş akışları bazılarını açıklar. StorSimple Virtual Array'ı StorSimple Yöneticisi'ni kullanarak yönetebilmeniz için kullanıcı Arabirimi (UI portalı adlandırılır) ve cihazın yerel web kullanıcı Arabirimi hizmet. Bu makalede, web kullanıcı arabirimini kullanarak gerçekleştirebileceğiniz görevler üzerinde odaklanır.

Bu makale aşağıdaki öğreticileri içerir:

* Hizmet veri şifreleme anahtarı alma
* Web kullanıcı Arabirimi Kurulum hatalarını giderme
* Bir günlük paketi oluşturun
* Cihazınızı kapatma ya da yeniden başlatma

## <a name="get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarı alma
Hizmet veri şifreleme anahtarı StorSimple Yöneticisi hizmetine ilk Cihazınızı kaydettiğinizde oluşturulur. Bu durumda bu anahtar StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla gereklidir.

Hizmet veri şifreleme anahtarı ve bunu alma gerek kaybedilmesi, aşağıdaki işlemi gerçekleştirin adımları yerel Web kullanıcı Arabiriminde cihazın kayıtlı hizmetinizle.

#### <a name="to-get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarı almak için
1. Yerel web kullanıcı Arabirimine bağlanın. Git **yapılandırma** > **Cloud ayarları**.
2. Sayfanın en altında tıklayın **hizmet veri şifreleme anahtarını Al**. Bir anahtarı görüntülenir. Kopyalayın ve bu anahtarı kaydedin.
   
    ![Hizmet veri şifreleme anahtarı 1 alma](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Web kullanıcı Arabirimi Kurulum hatalarını giderme
Bazı durumlarda, yerel web kullanıcı Arabirimi üzerinden Cihazınızı yapılandırırken hatalarla karşılaşırsanız çalıştırmanız gerekebilir. Tanılama ve bu tür hataları gidermek için tanılama testlerini çalıştırabilirsiniz.

#### <a name="to-run-the-diagnostic-tests"></a>Tanılama testleri çalıştırmak için
1. Yerel web kullanıcı Arabirimi, Git **sorun giderme** > **tanılama testleri**.
   
    ![1. tanılama Çalıştır](./media/storsimple-ova-web-ui-admin/image29.png)
2. Sayfanın en altında tıklayın **tanılama Testleri Çalıştır**. Bu ağ, cihaz, web Ara sunucusu, olası sorunları tanılamak için testleri başlatır zaman ya da bulut ayarları. Cihaz testleri çalıştığını size bildirilir.
3. Testleri tamamladıktan sonra sonuçları görüntülenir. Aşağıdaki örnek, tanılama testlerin sonuçlarını gösterir. Web proxy ayarlarını bu cihaz üzerinde yapılandırılmamış ve bu nedenle, web proxy testi değil çalıştırıldığı unutmayın. Ağ ayarlarını, DNS sunucusu ve saat ayarlarını diğer tüm testler başarılı.
   
    ![2 tanılama Çalıştır](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Bir günlük paketi oluşturun
Microsoft Support cihaz sorunları gidermeye yardımcı olabilecek ilgili günlükler bir günlük paketi içerir. Bu sürümde, bir günlük paketi yerel web UI oluşturulabilir.

#### <a name="to-generate-the-log-package"></a>Günlük paketi oluşturmak için
1. Yerel web kullanıcı Arabirimi, Git **sorun giderme** > **sistem günlüklerini**.
   
    ![1 günlük paketini oluşturma](./media/storsimple-ova-web-ui-admin/image31.png)
2. Sayfanın en altında tıklayın **günlük paketi oluştur**. Sistem günlüklerinin bir paket oluşturulur. Bu işlem birkaç dakika sürebilir.
   
    ![2. günlük paketi oluştur](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Paket başarıyla oluşturulduktan sonra sayfa, paketin oluşturulduğu tarih ve saatini göstermek için güncelleştirilir bildirim alırsınız.
   
    ![3 günlük paketi oluştur](./media/storsimple-ova-web-ui-admin/image33.png)
3. Tıklayın **indirme günlük paketi**. Sıkıştırılmış bir paket, sisteminize yüklenir.
   
    ![4 günlük paketini oluşturma](./media/storsimple-ova-web-ui-admin/image34.png)
4. İndirilen günlük paketin sıkıştırmasını açın ve sistem günlük dosyalarını görüntüleyin.

## <a name="shut-down-and-restart-your-device"></a>Cihazınızı yeniden başlatın ve kapatın
Kapatabilir veya sanal cihazınızın yerel web UI aracılığıyla yeniden başlatın. Biz yeniden başlatmadan önce birimleri veya ana bilgisayar ve cihaz çevrimdışı paylaşımları olması önerilir. Bu, veri bozulması olasılığını en aza indirirsiniz. 

#### <a name="to-shut-down-your-virtual-device"></a>Sanal cihazı kapatmak için
1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **güç ayarları**.
2. Sayfanın en altında tıklayın **kapatma**.
   
    ![1 cihaz kapatma](./media/storsimple-ova-web-ui-admin/image36.png)
3. Cihazın bir kapatma bir çalışmama süresi sürmekte olan tüm GÇ kesintiye uğrar belirten bir uyarı görüntülenir. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![cihaz kapatma Uyarısı](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Kapatma başlatıldığını bildirilir.
   
    ![cihaz kapatma başlatıldı](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Cihaz artık kapanır. Cihazınızı başlatmak istiyorsanız, Hyper-V Yöneticisi'yle yapmak gerekir.

#### <a name="to-restart-your-virtual-device"></a>Sanal Cihazınızı yeniden başlatmak için
1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **güç ayarları**.
2. Sayfanın en altında tıklayın **yeniden**.
   
    ![cihaz yeniden başlatma](./media/storsimple-ova-web-ui-admin/image36.png)
3. Cihaz yeniden başlatma bir çalışmama süresi sürmekte olan tüm IOs kesme bildiren bir uyarı görüntülenir. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Uyarı yeniden başlatın](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Yeniden başlatma başlatıldı bildirilir.
   
    ![yeniden başlatma başlatıldı](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Yeniden başlatma işlemi devam ederken, kullanıcı Arabirimi için bağlantıyı kaybedeceksiniz. Kullanıcı arabirimini düzenli aralıklarla yenileyerek yeniden izleyebilirsiniz. Alternatif olarak, cihaz yeniden başlatma durumu Hyper-V Yöneticisi aracılığıyla izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md).

