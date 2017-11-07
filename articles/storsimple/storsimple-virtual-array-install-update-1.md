---
title: "StorSimple sanal dizisinde güncelleştirme 1.0 yükleme | Microsoft Docs"
description: "StorSimple sanal dizinin web kullanıcı Arabirimi Azure portal ve düzeltme yöntemini kullanarak güncelleştirmeleri uygulamak için nasıl kullanılacağını açıklar"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: a85290f3f56eb1e1bf57524c43c6d4fea36129f7
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="install-update-10-on-your-storsimple-virtual-array"></a>Güncelleştirme 1.0, StorSimple sanal dizisinde yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple sanal dizinin yerel web kullanıcı Arabirimi aracılığıyla ve Azure Portalı aracılığıyla güncelleştirme 1.0 yüklemek için gereken adımlar açıklanır.

Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple sanal dizinizi güncel tutmak için geçerlidir. Bir güncelleştirmeyi uygulamadan önce birimler veya paylaşımlar çevrimdışı konakta ilk almanızı öneririz ve ardından cihaz. Bu veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra aynı zamanda el ile yedekleme aygıtı almalıdır.

> [!IMPORTANT]
> - Güncelleştirme 1.0 karşılık gelen **10.0.10296.0** Cihazınızda yazılımı sürümü. Bu güncelleştirmede yenilikler hakkında daha fazla bilgi için Git [güncelleştirme 1.0 için sürüm notları](storsimple-virtual-array-update-1-release-notes.md).
>
> - Bir güncelleştirme veya düzeltme yükleme, aygıt yeniden unutmayın. Bir tek düğümlü cihaz StorSimple sanal dizinin olması koşuluyla, devam eden tüm g/ç kesintiye ve kapalı kalma süresi Cihazınızı karşılaştığında.
>
> - Sanal dizinin güncelleştirme 0,6 yalnızca çalışıyorsa, güncelleştirme 1 Azure portalında kullanılabilir. Güncelleştirme öncesi 0,6 sürümlerini çalıştıran sanal diziler için güncelleştirme 0,6 ilk yükleyin ve ardından güncelleştirme 1'i yükleyin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 ve sonraki sürümleri çalışıyorsa, Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Portal yordamı tarama, indirme ve güncelleştirmeleri yüklemek kullanıcının gerektirir. Sanal dizinizi çalışan yazılım sürümüne bağlı olarak, Azure Portalı aracılığıyla güncelleştirmeyi uygulamadan farklıdır.

 - Sanal dizinizi güncelleştirme 0,6 çalışıyorsa, Azure portalında aygıtınızda doğrudan güncelleştirme 1 (10.0.10296.0) yükler. Bu yordamı tamamlamak için yaklaşık 7 dakika sürer.
 - Sanal dizinizi güncelleştirmesinden 0,6 önceki bir sürümünü çalıştırıyorsa, güncelleştirme iki aşamada yapılır. Azure portalını ilk aygıtınızda güncelleştirme 0,6 (10.0.10293.0) yükler. Sanal dizinin yeniden başlatır ve portal aygıtınızda güncelleştirme 1 (10.0.10296.0) yükler. Bu yordamı tamamlamak için yaklaşık 15 dakika sürer.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-1.md)]

Yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **aygıtları** seçin ve güncelleştirdiğiniz cihaz seçeneğine tıklayın. Git **ayarlar > Yönet > aygıt güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10296.0**.

![Güncelleştirmeden sonra yazılım sürümü](./media/storsimple-virtual-array-install-update-1/azupdate17m1.png)

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı arabirimini kullanırken iki adımı vardır:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

> [!IMPORTANT] 
> **Yalnızca güncelleştirme 0,6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Önceki bir sürümünü çalıştırıyorsanız, [güncelleştirme 0,6](storsimple-virtual-array-install-update-06.md) aygıtınızda ilk ve güncelleştirme 1'i uygulayın.**

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Sanal dizinizi güncelleştirme 0,6 çalışıyorsa, Microsoft Update Kataloğu'ndan güncelleştirme 1'i indirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) adresine gidin.

2. Microsoft Update Kataloğu bu bilgisayarda ilk kez kullanıyorsanız, **yükleme** Microsoft Update Kataloğu eklentiyi yüklemek isteyip istemediğiniz sorulduğunda.

3. Microsoft Update Kataloğu arama kutusuna, yüklemek istediğiniz düzeltme Bilgi Bankası (KB) sayısını girin. Girin **4047203** için 1.0 güncelleştirin ve ardından **arama**.
   
    Düzeltme listesi göründüğünde, örneğin, **StorSimple sanal dizinin Update 1.0**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-1/download1.png)

4. **İndir**’e tıklayın.

5. İki dosya bir klasöre yükleyin. CİHAZDAN erişilebilir bir ağ paylaşımına klasöre de kopyalayabilirsiniz.

6. Dosyalarının bulunduğu klasörü açın.

    ![Paket dosyaları](./media/storsimple-virtual-array-install-update-1/update01folder.png)

    İki dosyalarına bakın:
    -  Microsoft Update tek başına paket dosyası `WindowsTH-KB3011067-x64`. Bu dosya, cihaz yazılımını güncelleştirmek için kullanılır.
    - Ağustos için toplu güncelleştirmeleri içeren bir dosyayı `windows8.1-kb4034681-x64`. Bu paketi içinde yer aldığı daha fazla bilgi için Git [Ağustos aylık güvenlik dökümü](https://support.microsoft.com/help/4034681/windows-8-1-windows-server-2012-r2-update-kb40346810).

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yükleme önce olduğundan emin olun:

 - Güncelleştirme veya ana bilgisayarınız yerel olarak ya da bir ağ paylaşımı üzerinden erişilebilir indirilen düzeltme var.
 - Sanal dizinizi güncelleştirme 0,6 (10.0.10293.0) çalışıyor. Güncelleştirmesinden 0,6, önceki bir sürümünü çalıştırıyorsanız, [güncelleştirme 0,6](storsimple-virtual-array-install-update-06.md) ilk ve güncelleştirme 1'i yükleyin.

Bu yordamı tamamlamak için yaklaşık 4 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**. Çalıştırmakta olduğunuz yazılım sürümü not edin. **Yalnızca güncelleştirme 0,6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Önceki bir sürümünü çalıştırıyorsanız, [güncelleştirme 0,6](storsimple-virtual-array-install-update-06.md) aygıtınızda ilk ve güncelleştirme 1'i uygulayın.**
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme için dosya adı girin. Ayrıca, bir ağ paylaşımında girdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update2m.png)

3. Bir uyarı görüntülenir. Verilen sanal dizi bir tek düğümlü, güncelleştirme uygulanana, aygıt yeniden başladıktan sonra kapalı kalma süresi aygıttır. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatır. Yerel kullanıcı arabirimini bu süre içinde erişilebilir durumda değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra gittiğiniz **oturum** sayfası. Aygıt yazılımı, yerel web kullanıcı Arabirimi, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10296** güncelleştirme 1.0 için.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10296** ve Azure portal raporları **10.0.10296.0** aynı sürüm için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update6m.png)

6. Windows güvenlik düzeltme ekini dosyasını kullanarak yüklemek için 2-4 arası adımları yineleyin `windows8.1-kb4012213-x64`. Sanal dizinin yüklendikten sonra yeniden başlatılır ve kullanıcı Arabirimi yerel web oturum açmanız gerekir.

> [!NOTE]
> Güncelleştirmesinden 0,6 önceki bir sürümünü çalıştıran bir cihazda güncelleştirme 1 doğrudan uyguladıysanız, bazı güncelleştirmeler eksik. Sonraki adımlar için Microsoft Support temasa geçin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md).
