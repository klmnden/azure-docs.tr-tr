---
title: Güncelleştirme 0.5 StorSimple sanal dizisi yükleme | Microsoft Docs
description: StorSimple sanal dizisi web kullanıcı Arabirimi Azure portalı ve düzeltme yöntemi kullanarak güncelleştirmeleri uygulamak için kullanmayı açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: e09ff4bcbc141b1a1f80bc278918a291639c1885
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61445434"
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>StorSimple sanal dizisi üzerinde güncelleştirme 0.5 yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, yerel web UI aracılığıyla ve Azure Portalı aracılığıyla StorSimple Virtual Array'iniz 0,5 güncelleştirmeyi yüklemek için gereken adımlar açıklanmaktadır. Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple Virtual Array'iniz güncel tutmak için uygulama gerekir.

Bir güncelleştirmeyi uygulamadan önce çevrimdışı paylaşımları ve birimleri konakta ilk almanızı öneririz ve ardından cihaz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra ayrıca el ile yedekleme aygıtı almalıdır.

> [!IMPORTANT]
> - Güncelleştirme 0.5 karşılık gelen **10.0.10290.0** Cihazınızda yazılım sürümü. Bu güncelleştirmede yenilikler hakkında daha fazla bilgi için Git [sürüm notları için güncelleştirme 0.5](storsimple-virtual-array-update-05-release-notes.md).
>
> - Güncelleştirme 0.2 çalıştırdığınızı veya öneririz daha sonra Azure Portalı aracılığıyla güncelleştirmeleri yükleyin. Güncelleştirme 0.1 veya GA yazılım sürümleri çalıştırıyorsanız, güncelleştirme 0.5 yüklemek için yerel web UI aracılığıyla düzeltme yöntemi kullanmanız gerekir.
>
> - Bir güncelleştirme veya düzeltme yükleme Cihazınızı yeniden göz önünde bulundurun. StorSimple sanal dizi tek düğümlü bir cihaz olduğunu devam eden herhangi bir g/ç kesintiye ve kapalı kalma süresi Cihazınızı deneyimleri.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 ve üzeri çalıştırılıyorsa, Azure Portalı aracılığıyla güncelleştirmeleri yüklemeniz önerilir. Tarama, indirin ve ardından güncelleştirmeleri yüklemek Kullanıcı Portalı yordamı gerektirir. Bu yordamı tamamlamak için yaklaşık 7 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve güncelleştirdiğiniz cihaza tıklayın. Git **ayarlar > Yönet > cihaz güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10290.0**.

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı Arabirimi kullanılırken iki adımı vardır:

* Güncelleştirme veya düzeltme indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve gidin [ https://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Update Kataloğu arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Girin **4021576** 0,5 güncelleştirin ve ardından **arama**.
   
    Düzeltme listesi görüntülenir, örneğin, **StorSimple Virtual Array güncelleştirme 0.5**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-05/download1.png)

4. **İndir**'e tıklayın. 

5. İndirmek için iki dosya görmeniz gerekir bir *.msu* ve *.cab* dosya. Bu dosyaların her biri, bir klasöre indirin. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

6. Dosyalarının bulunduğu klasörü açın.
    ![Paket dosyaları](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    Şununla karşılaşırsınız:
    -  Microsoft Update tek başına paketin dosya `WindowsTH-KB3011067-x64`. Bu dosya, cihaz yazılımı güncelleştirmek için kullanılır.
    - Geneva İzleme Aracısı paket dosyası `GenevaMonitoringAgentPackageInstaller`. Bu dosya, izleme ve Tanılama Hizmeti (MDS) aracısını güncelleştirmek için kullanılır. Cab dosyasına çift tıklayın. Bir .msi görüntülenir. Dosyasını seçin, sağ tıklatın ve ardından **ayıklamak** dosya. Kullanacağınız _.msi_ aracısını güncelleştirmek için dosya.

        ![MDS aracı güncelleştirme dosyasını ayıklayın](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yüklemeden önce bir ağ paylaşımı üzerinden erişilebilir veya sahip olduğunuz güncelleştirme veya düzeltme konağınızdaki yerel olarak ya da indirilen emin olun.

GA çalıştıran bir cihaza güncelleştirmeleri yükleme veya güncelleştirme 0.1 yazılım sürümleri için bu yöntemi kullanın. Bu yordamı tamamlamak için değerinden 2 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme dosyasının adını girin. Ayrıca, bir ağ paylaşımında yerleştirdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Bir uyarı görüntülenir. Tek düğümlü bir cihaz güncelleştirme uygulanır, cihaz yeniden başlatıldıktan sonra kapalı kalma süresi budur. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel kullanıcı Arabirimi, bu süre içinde erişilebilir değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra yönlendirilirsiniz **oturum** sayfası. Cihaz yazılımı, yerel web kullanıcı Arabiriminde, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10290.0** 0,5 güncelleştirmesi.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10290** ve Azure portal raporların **10.0.10290.0** aynı sürümü için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. Sonraki adım MDS Aracısı güncelleştirmektir. İçinde **yazılım güncelleştirmesi** sayfasında, Git **güncelleştirme dosya yolu** ve `GenevaMonitoringAgentPackageInstaller.msi` dosya. 2-4 arası adımları yineleyin. Sanal diziyi yeniden başlatıldıktan sonra yerel web kullanıcı Arabirimi ile oturum açın.

Güncelleştirme tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Virtual Array'iniz yönetme](storsimple-ova-web-ui-admin.md).

