---
title: StorSimple sanal dizisi güncelleştirme 1.0 yükleyin | Microsoft Docs
description: StorSimple sanal dizisi web kullanıcı Arabirimi Azure portalı ve düzeltme yöntemi kullanarak güncelleştirmeleri uygulamak için kullanmayı açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: fa53213e577028628d48db91704578e23888f2a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61411767"
---
# <a name="install-update-10-on-your-storsimple-virtual-array"></a>StorSimple sanal dizisi üzerinde güncelleştirme 1.0 yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple sanal dizisi yerel web UI aracılığıyla ve Azure Portalı aracılığıyla güncelleştirme 1.0 yüklemek için gereken adımlar açıklanır.

Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple Virtual Array'iniz güncel tutmak için geçerlidir. Bir güncelleştirmeyi uygulamadan önce çevrimdışı paylaşımları ve birimleri konakta ilk almanızı öneririz ve ardından cihaz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra ayrıca el ile yedekleme aygıtı almalıdır.

> [!IMPORTANT]
> - Güncelleştirme 1.0 karşılık gelen **10.0.10296.0** Cihazınızda yazılım sürümü. Bu güncelleştirmede yenilikler hakkında daha fazla bilgi için Git [için Güncelleştirme 1.0 sürüm notlarını](storsimple-virtual-array-update-1-release-notes.md).
>
> - Bir güncelleştirme veya düzeltme yükleme Cihazınızı yeniden göz önünde bulundurun. StorSimple sanal dizi tek düğümlü bir cihaz olduğunu devam eden herhangi bir g/ç kesintiye ve kapalı kalma süresi Cihazınızı deneyimleri.
>
> - Güncelleştirme 1 yalnızca sanal diziyi güncelleştirme 0.6 çalışıyorsa, Azure portalında kullanılabilir. Öncesi güncelleştirme 0.6 sürümlerini çalıştıran sanal diziler için Güncelleştirme 1'i yükledikten ve güncelleştirme 0.6 önce yüklemeniz gerekir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 ve üzeri çalıştırılıyorsa, Azure Portalı aracılığıyla güncelleştirmeleri yüklemeniz önerilir. Tarama, indirin ve ardından güncelleştirmeleri yüklemek Kullanıcı Portalı yordamı gerektirir. Sanal diziniz çalışan yazılım sürümüne bağlı olarak, Azure portalından bir güncelleştirmeyi uygulamadan farklıdır.

 - Sanal diziniz güncelleştirme 0.6 çalışıyorsa, bu Azure portalında doğrudan Cihazınızda güncelleştirme 1 (10.0.10296.0) yükler. Bu yordamı tamamlamak için yaklaşık 7 dakika sürer.
 - Sanal diziniz güncelleştirme 0.6 önceki bir sürümü çalıştırıyorsa, güncelleştirme iki aşamada gerçekleştirilir. Azure portalı, cihazınızın ilk güncelleştirme 0.6 (10.0.10293.0) yükler. Sanal diziyi yeniden başlatır ve portal Cihazınızda güncelleştirme 1 (10.0.10296.0) yükler. Bu yordamı tamamlamak için yaklaşık 15 dakika sürer.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-1.md)]

Yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve güncelleştirdiğiniz cihaza tıklayın. Git **ayarlar > Yönet > cihaz güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10296.0**.

![Yazılım sürümü güncelleştirme](./media/storsimple-virtual-array-install-update-1/azupdate17m1.png)

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı Arabirimi kullanılırken iki adımı vardır:

* Güncelleştirme veya düzeltme indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

> [!IMPORTANT] 
> **Güncelleştirme 0.6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Önceki bir sürümü çalıştırıyorsanız [güncelleştirme 0.6](storsimple-virtual-array-install-update-06.md) Cihazınızda ilk ve güncelleştirme 1 uygulayın.**

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme indirin

Sanal diziniz güncelleştirme 0.6 çalıştırıyorsa, Microsoft Update Kataloğu'ndan güncelleştirme 1'i indirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve gidin [ https://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).

2. Microsoft Update Kataloğu'nu bu bilgisayarda ilk kez kullanıyorsanız, **yükleme** Microsoft Update Kataloğu eklentisini yüklemek isteyip istemediğiniz sorulduğunda.

3. Microsoft Update Kataloğu arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Girin **4047203** için 1.0 güncelleştirin ve ardından **arama**.
   
    Düzeltme listesi görüntülenir, örneğin, **StorSimple Virtual Array güncelleştirme 1.0**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-1/download1.png)

4. **İndir**’e tıklayın.

5. İki dosyayı bir klasöre indirin. Klasör, CİHAZDAN erişilebilen bir ağ paylaşımına da kopyalayabilirsiniz.

6. Dosyalarının bulunduğu klasörü açın.

    ![Paket dosyaları](./media/storsimple-virtual-array-install-update-1/update01folder.png)

    İki dosya görürsünüz:
    -  Microsoft Update tek başına paketin dosya `WindowsTH-KB3011067-x64`. Bu dosya, cihaz yazılımı güncelleştirmek için kullanılır.
    - Ağustos için'toplu güncelleştirmeleri içeren bir dosyayı `windows8.1-kb4034681-x64`. Bu paketi içinde yer aldığı daha fazla bilgi için Git [Ağustos aylık güvenlik dökümü](https://support.microsoft.com/help/4034681/windows-8-1-windows-server-2012-r2-update-kb40346810).

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yüklemeden önce emin olun:

 - Güncelleştirme veya düzeltme konağınızdaki yerel olarak veya bir ağ paylaşımı üzerinden erişilebilir indirilen var.
 - Sanal diziniz güncelleştirme 0.6 (10.0.10293.0) çalışıyor. Güncelleştirme 0.6, önceki bir sürümü çalıştırıyorsanız [güncelleştirme 0.6](storsimple-virtual-array-install-update-06.md) ilk ve güncelleştirme 1'i yükleyin.

Bu yordamı tamamlamak için yaklaşık 4 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**. Çalıştırmakta olduğunuz yazılım sürümü not edin. **Güncelleştirme 0.6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Önceki bir sürümü çalıştırıyorsanız [güncelleştirme 0.6](storsimple-virtual-array-install-update-06.md) Cihazınızda ilk ve güncelleştirme 1 uygulayın.**
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme dosyasının adını girin. Ayrıca, bir ağ paylaşımında yerleştirdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update2m.png)

3. Bir uyarı görüntülenir. Verilen sanal dizi tek düğümlü bir cihaz güncelleştirme uygulanır, cihaz yeniden başlatıldıktan sonra kapalı kalma süresi olduğunu. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel kullanıcı Arabirimi, bu süre içinde erişilebilir değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra yönlendirilirsiniz **oturum** sayfası. Cihaz yazılımı, yerel web kullanıcı Arabiriminde, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10296** güncelleştirme 1.0 için.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10296** ve Azure portal raporların **10.0.10296.0** aynı sürümü için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update6m.png)

6. Windows güvenlik düzeltme ekini dosyasını kullanarak yüklemek için 2-4 arası adımları yineleyin `windows8.1-kb4012213-x64`. Yüklemeyi tamamladıktan sonra sanal diziyi yeniden başlatır ve yerel web kullanıcı Arabirimi açmanız gerekir.

> [!NOTE]
> Güncelleştirme 0.6 önceki bir sürümü çalıştıran bir cihazda doğrudan güncelleştirme 1 uyguladıysanız, bazı güncelleştirmeler eksik. Sonraki adımlar için Microsoft Support temasa geçin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Virtual Array'iniz yönetme](storsimple-ova-web-ui-admin.md).
