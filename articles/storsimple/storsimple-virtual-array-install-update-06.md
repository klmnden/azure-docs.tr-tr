---
title: 0,6 StorSimple sanal dizisinde güncelleştirmesini | Microsoft Docs
description: StorSimple sanal dizinin web kullanıcı Arabirimi Azure portal ve düzeltme yöntemini kullanarak güncelleştirmeleri uygulamak için nasıl kullanılacağını açıklar
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 111976cd684561f5bc63b92f09a20470fe3036d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875910"
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>StorSimple sanal dizinizi üzerinde güncelleştirme 0,6 yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, yerel web kullanıcı Arabirimi aracılığıyla ve Azure portalı üzerinden, StorSimple sanal dizisindeki 0,6 Güncelleştirmeyi yüklemek için gereken adımlar açıklanır. Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple sanal dizinizi güncel tutmak için geçerlidir.

Bir güncelleştirmeyi uygulamadan önce birimler veya paylaşımlar çevrimdışı konakta ilk almanızı öneririz ve ardından cihaz. Bu veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra aynı zamanda el ile yedekleme aygıtı almalıdır.

> [!IMPORTANT]
> - Güncelleştirme 0,6 karşılık gelen **10.0.10293.0** Cihazınızda yazılımı sürümü. Bu güncelleştirmede yenilikler hakkında daha fazla bilgi için Git [0,6 güncelleştirmesi sürüm notları](storsimple-virtual-array-update-06-release-notes.md).
>
> - Daha sonra öneririz veya güncelleştirme 0.2 çalıştırıyorsanız Azure Portalı aracılığıyla güncelleştirmeleri yükleyin. Güncelleştirme 0.1 veya GA yazılım sürümleri çalıştırıyorsanız, güncelleştirmeyi 0,6 yüklemek için yerel web kullanıcı Arabirimi aracılığıyla düzeltme yöntemini kullanmanız gerekir.
>
> - Bir güncelleştirme veya düzeltme yükleme, aygıt yeniden unutmayın. Bir tek düğümlü cihaz StorSimple sanal dizinin olması koşuluyla, devam eden tüm g/ç kesintiye ve kapalı kalma süresi Cihazınızı karşılaştığında.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 ve sonraki sürümleri çalışıyorsa, Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Portal yordamı tarama, indirme ve güncelleştirmeleri yüklemek kullanıcının gerektirir. Bu yordamı tamamlamak için yaklaşık 7 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **aygıtları** seçin ve güncelleştirdiğiniz cihaz seçeneğine tıklayın. Git **ayarlar > Yönet > aygıt güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10293.0**.

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı arabirimini kullanırken iki adımı vardır:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) adresine gidin.

2. Microsoft Update Kataloğu bu bilgisayarda ilk kez kullanıyorsanız, **yükleme** Microsoft Update Kataloğu eklentiyi yüklemek isteyip istemediğiniz sorulduğunda.

3. Microsoft Update Kataloğu arama kutusuna, yüklemek istediğiniz düzeltme Bilgi Bankası (KB) sayısını girin. Girin **4023268** 0,6 güncelleştirmesi için ve ardından **arama**.
   
    Düzeltme listesi göründüğünde, örneğin, **StorSimple sanal dizinin güncelleştirme 0,6**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-06/download1.png)

4. **İndir**’e tıklayın.

5. İndirilecek beş dosyaların görmelisiniz. Bu dosyaların her biri bir klasöre yükleyin. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

6. Dosyalarının bulunduğu klasörü açın.
    ![Paket dosyaları](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    Görürsünüz:
    -  Microsoft Update tek başına paket dosyası `WindowsTH-KB3011067-x64`. Bu dosya, cihaz yazılımını güncelleştirmek için kullanılır.
    - Geneva İzleme Aracısı paket dosyası `GenevaMonitoringAgentPackageInstaller`. Bu dosya, izleme ve Tanılama Hizmeti (MDS) aracısını güncelleştirmek için kullanılır. Cab dosyasını çift tıklatın. A _.msi_ görüntülenir. Dosyasını seçin, sağ tıklatın ve ardından **ayıklamak** dosya. Kullandığınız _.msi_ aracısını güncelleştirmek için dosya.

        ![MDS aracı güncelleştirme dosyasını ayıklayın](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > StorSimple güncelleştirme 0,5 (0.0.10293.0) çalıştırıyorsanız, MDS aracısını güncelleştirmek gerekmez.

    - Kritik Windows Güvenlik güncelleştirmeleri içeren üç dosyaları `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, ve `windows8.1-kb4019213-x64`.


### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yükleme önce bir ağ paylaşımı üzerinden erişilebilir veya güncelleştirmeye sahip veya ana bilgisayarınız yerel olarak ya da düzeltmeyi indirdiğiniz olduğundan emin olun.

GA çalıştıran bir cihazda güncelleştirmeleri yüklemek veya 0,1 yazılım sürümleri güncelleştirmek için bu yöntemi kullanın. Bu yordamı tamamlamak için yaklaşık olarak 12 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**. Çalıştırmakta olduğunuz yazılım sürümü not edin. Çalıştırıyorsanız, **10.0.10290.0**, adım 6 MDS aracısını güncelleştirmek gerekmez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme için dosya adı girin. Ayrıca, bir ağ paylaşımında girdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Bir uyarı görüntülenir. Verilen sanal dizi bir tek düğümlü, güncelleştirme uygulanana, aygıt yeniden başladıktan sonra kapalı kalma süresi aygıttır. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatır. Yerel kullanıcı arabirimini bu süre içinde erişilebilir durumda değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra gittiğiniz **oturum** sayfası. Aygıt yazılımı, yerel web kullanıcı Arabirimi, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10293** 0,6 güncelleştirmesi.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10293** ve Azure portal raporları **10.0.10293.0** aynı sürüm için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. StorSimple sanal dizinin güncelleştirme 0,5 çalışıyormuş bu adımı atla (**10.0.10290.0**) bu güncelleştirmeyi uygulamadan önce. Güncelleştirmeye başlamadan önce yazılım sürümü Not 1. adımda yaptığınız. Güncelleştirme 0,5 çalışıyormuş MDS aracı zaten güncel.

    Bir yazılım sürümü güncelleştirme 0,5 önce çalıştırıyorsanız, MDS Aracısı, sonraki adımda güncelleştirmektir. İçinde **yazılım güncelleştirmesi** sayfasında, Git **güncelleştirme dosya yolu** ve `GenevaMonitoringAgentPackageInstaller.msi` dosya. 2-4 arası adımları yineleyin. Sanal dizinin yeniden başlatıldıktan sonra kullanıcı Arabirimi yerel web oturum açın.

7. Windows güvenliği yüklemek için yineleme adım 2-4 giderir dosyalarını kullanarak `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, ve `windows8.1-kb4019213-x64`. Sanal dizinin her yüklendikten sonra yeniden başlatılır ve kullanıcı Arabirimi yerel web oturum açmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md).

