---
title: StorSimple sanal dizinin web kullanıcı Arabirimi Yönetim | Microsoft Docs
description: StorSimple sanal dizinin web kullanıcı Arabirimi aracılığıyla temel aygıt yönetim görevlerinin nasıl gerçekleştirileceğini açıklar.
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
ms.openlocfilehash: 989e7b697f9b527df549fb32be18edd1d3c8d224
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875889"
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>StorSimple sanal dizinizi yönetmek için Web kullanıcı arabirimini kullanın
![Kurulum işlem akışı](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makalede öğreticileri Microsoft Azure StorSimple sanal Mart 2016 genel kullanılabilirlik (GA) sürüm çalıştıran diziye (olarak da bilinen StorSimple şirket içi sanal cihazı) geçerlidir. Bu makalede karmaşık iş akışları ve StorSimple sanal dizi gerçekleştirilebilir yönetim görevlerini bazıları açıklanmaktadır. StorSimple sanal StorSimple Yöneticisi'ni kullanarak dizi yönetebilmeniz için kullanıcı Arabirimi (UI portalı olarak adlandırılır) ve cihaz için yerel web kullanıcı Arabirimi hizmet. Bu makalede, web kullanıcı arabirimini kullanarak gerçekleştirebileceğiniz görevler odaklanır.

Bu makalede aşağıdaki öğreticiler içerir:

* Hizmet verileri şifreleme anahtarı alma
* Web kullanıcı Arabirimi Kurulum hatalarında sorun giderme
* Bir günlük paketi oluştur
* Kapatıldı veya aygıtınızı yeniden başlatın

## <a name="get-the-service-data-encryption-key"></a>Hizmet verileri şifreleme anahtarı alma
StorSimple Yöneticisi hizmetiyle ilk Cihazınızı kaydederken bir hizmet verileri şifreleme anahtarı oluşturulur. Bu anahtarı ise StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarı ile gerekli.

Hizmet verileri şifreleme anahtarı ve bunu almak için gereksinim yanlış olmadığını aşağıdakileri gerçekleştirmek yerel web kullanıcı Arabirimi cihazın adımlarda hizmetiniz ile kayıtlı.

#### <a name="to-get-the-service-data-encryption-key"></a>Hizmet verileri şifreleme anahtarı alma
1. Yerel web kullanıcı Arabirimi bağlayın. Git **yapılandırma** > **bulut ayarları**.
2. Sayfanın alt kısmındaki tıklatın **Get hizmet verileri şifreleme anahtarı**. Bir anahtarı görüntülenir. Kopyalayın ve bu anahtar kaydedin.
   
    ![Hizmet verileri şifreleme anahtarı 1 alma](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Web kullanıcı Arabirimi Kurulum hatalarında sorun giderme
Bazı durumlarda, yerel web kullanıcı Arabirimi üzerinden Cihazınızı yapılandırırken hatalarla karşılaşırsanız çalıştırabilirsiniz. Tanılamak ve bu tür hatalarında sorun giderme için tanılama testleri çalıştırabilirsiniz.

#### <a name="to-run-the-diagnostic-tests"></a>Tanılama testleri çalıştırmak için
1. Yerel web kullanıcı Arabirimi, Git **sorun giderme** > **tanılama testleri**.
   
    ![Tanılama'yı 1 çalıştırın](./media/storsimple-ova-web-ui-admin/image29.png)
2. Sayfanın alt kısmındaki tıklatın **tanılama Testleri Çalıştır**. Bu ağ, aygıt, web proxy ile ilgili tüm olası sorunları tanılamak için testleri başlatır zaman ya da bulut ayarları. Cihaz testleri çalıştığını bildirilir.
3. Sınamalar tamamladıktan sonra sonuçları görüntülenir. Aşağıdaki örnek tanılama testleri sonuçlarını gösterir. Bu aygıtta web proxy ayarları yapılandırılmadı ve bu nedenle, web proxy testi değil çalıştırıldığı unutmayın. Ağ ayarlarını, DNS sunucusu ve saat ayarları için diğer tüm sınamalar başarılı oldu.
   
    ![Tanılama 2 çalıştırma](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Bir günlük paketi oluştur
Bir günlük paketi Microsoft Support cihaz sorunları gidermenize yardımcı olabilecek ilgili günlükleri oluşur. Bu sürümde, bir günlük paketi yerel web kullanıcı Arabirimi oluşturulabilir.

#### <a name="to-generate-the-log-package"></a>Günlük paketi oluşturmak için
1. Yerel web kullanıcı Arabirimi, Git **sorun giderme** > **sistem günlüklerini**.
   
    ![Günlük Paketi 1 oluştur](./media/storsimple-ova-web-ui-admin/image31.png)
2. Sayfanın alt kısmındaki tıklatın **günlük Paket Oluştur**. Sistem günlüklerini paketi oluşturulacak. Bu işlem birkaç dakika sürebilir.
   
    ![2 günlük paketi oluştur](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Paket başarıyla oluşturuldu ve sayfa paketinin oluşturulduğu tarih ve saatini belirtmek için güncelleştirilmiş sonra size bildirilecek.
   
    ![3 günlük paketi oluştur](./media/storsimple-ova-web-ui-admin/image33.png)
3. Tıklatın **yükleme günlük paketini**. Sıkıştırılmış paketi sisteminizde indirilir.
   
    ![Günlük Paketi 4 oluştur](./media/storsimple-ova-web-ui-admin/image34.png)
4. İndirilen günlük paketin sıkıştırmasını açın ve sistem günlük dosyalarını görüntüleyin.

## <a name="shut-down-and-restart-your-device"></a>Kapatılır ve aygıtınızı yeniden başlatın
Kapatıldı veya yerel web kullanıcı arabirimini kullanarak sanal Cihazınızı yeniden başlatın. Biz yeniden başlatmadan önce birimleri veya ana bilgisayar ve aygıt çevrimdışı paylaşımları alın önerilir. Bu veri bozulması olasılığını en aza indirgenecektir. 

#### <a name="to-shut-down-your-virtual-device"></a>Sanal cihazı kapatmak için
1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **güç ayarları**.
2. Sayfanın alt kısmındaki tıklatın **kapatma**.
   
    ![cihaz kapatma 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Kapalı kalma süresi ile elde edilen etmekte olan tüm g/ç aygıtı bir kapatma kesintiye uğrar bildiren bir uyarı görüntülenir. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![cihaz kapatma uyarı](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Kapatma başlatıldı bildirilir.
   
    ![cihaz kapatma başlatıldı](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Cihaz şimdi kapanacak. Cihazınızı başlatmak istiyorsanız, Hyper-V Yöneticisi'yle yapmak gerekir.

#### <a name="to-restart-your-virtual-device"></a>Sanal cihazınız yeniden başlatmak için
1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **güç ayarları**.
2. Sayfanın alt kısmındaki tıklatın **yeniden**.
   
    ![Aygıt yeniden başlatma](./media/storsimple-ova-web-ui-admin/image36.png)
3. Cihaz yeniden kapalı kalma süresi ile elde edilen etmekte olan tüm IOs kesecektir bildiren bir uyarı görüntülenir. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Uyarı yeniden başlatın](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Yeniden başlatma başlatıldı bildirilir.
   
    ![başlatılan yeniden başlatma](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Yeniden başlatma işlemi devam ederken, kullanıcı Arabiriminde bağlantıyı kaybedeceksiniz. Kullanıcı arabirimini düzenli aralıklarla yenileyerek yeniden izleyebilirsiniz. Alternatif olarak, Hyper-V Yöneticisi aracılığıyla aygıt yeniden başlatma durumunu izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md).

