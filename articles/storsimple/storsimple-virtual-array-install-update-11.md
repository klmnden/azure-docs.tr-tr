---
title: StorSimple sanal dizisi güncelleştirme 1.1 yükleyin | Microsoft Docs
description: StorSimple Virtual Array için Azure portalı ve yerel web kullanıcı arabirimini kullanarak güncelleştirmeleri uygulamak açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/18/2018
ms.author: alkohli
ms.openlocfilehash: 88b903d68e4398b4e30b0b7435279c29bee6cd6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61408769"
---
# <a name="install-update-11-on-your-storsimple-virtual-array"></a>StorSimple sanal dizisi üzerinde güncelleştirme 1.1 yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple sanal dizisi yerel web UI aracılığıyla ve Azure Portalı aracılığıyla güncelleştirme 1.1 yüklemek için gereken adımlar açıklanır.

Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple Virtual Array'iniz güncel tutmak için geçerlidir. Bir güncelleştirmeyi uygulamadan önce çevrimdışı paylaşımları ve birimleri konakta ilk almanızı öneririz ve ardından cihaz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra ayrıca el ile yedekleme aygıtı almalıdır.

> [!IMPORTANT]
> - Güncelleştirme 1.1 karşılık gelen **10.0.10307.0** Cihazınızda yazılım sürümü. Bu güncelleştirmede yenilikler hakkında daha fazla bilgi için Git [1.1 güncelleştirmek için sürüm notları](storsimple-virtual-array-update-11-release-notes.md).
>
> - Bir güncelleştirme veya düzeltme yükleme Cihazınızı yeniden göz önünde bulundurun. StorSimple sanal dizi tek düğümlü bir cihaz olduğunu devam eden herhangi bir g/ç kesintiye ve kapalı kalma süresi Cihazınızı deneyimleri.
>
> - Yalnızca sanal diziyi güncelleştirme 1 çalışıyorsa güncelleştirme 1.1 Azure portalında kullanılabilir. Güncelleştirme 0.6 sürümlerini çalıştıran sanal diziler için Güncelleştirme 1.0 yüklemeniz ve ardından güncelleştirme 1.1 uygulayın.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 ve üzeri çalıştırılıyorsa, Azure Portalı aracılığıyla güncelleştirmeleri yüklemeniz önerilir. Tarama, indirin ve ardından güncelleştirmeleri yüklemek Kullanıcı Portalı yordamı gerektirir. Sanal diziniz çalışan yazılım sürümüne bağlı olarak, Azure portalından bir güncelleştirmeyi uygulamadan farklıdır.

 - Sanal diziniz güncelleştirme 1 çalışıyorsa, bu Azure portalında doğrudan Cihazınızda güncelleştirme 1.1 (10.0.10307.0) yükler. Bu yordamı tamamlamak için yaklaşık 10 dakika sürer.
 - Sanal diziniz güncelleştirme 0.6 çalışıyorsa, bu güncelleştirme iki aşamada gerçekleştirilir. Azure portalı, cihazınızın ilk güncelleştirme 1.0 (10.0.10296.0) yükler. Sanal diziyi yeniden başlatır ve portal Cihazınızda güncelleştirme 1.1 (10.0.10307.0) yükler. Bu yordamı tamamlamak için yaklaşık 15 dakika sürer.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-11.md)]

Yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve güncelleştirdiğiniz cihaza tıklayın. Git **ayarlar > Yönet > cihaz güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10307.0**.

![Yazılım sürümü güncelleştirme](./media/storsimple-virtual-array-install-update-11/azupdate17m2.png)

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı Arabirimi kullanılırken iki adımı vardır:

* Güncelleştirme veya düzeltme indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

> [!IMPORTANT] 
> **Güncelleştirme 1 (10.0.10296.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Güncelleştirme 0.6, çalıştırıyorsanız [güncelleştirme 1'i yükleme](storsimple-virtual-array-install-update-1.md) Cihazınızda ilk ve güncelleştirme 1.1 uygulayın.**

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme indirin

Microsoft Update Kataloğu'ndan güncelleştirme 1.1 indirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve gidin [ https://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).

2. Microsoft Update Kataloğu'nu bu bilgisayarda ilk kez kullanıyorsanız, **yükleme** Microsoft Update Kataloğu eklentisini yüklemek isteyip istemediğiniz sorulduğunda.

3. Microsoft Update Kataloğu arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Girin **4337628** için 1.1 güncelleştirin ve ardından **arama**.
   
    Düzeltme listesi görüntülenir, örneğin, **StorSimple sanal dizisi güncelleştirme 1.1**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-11/download1.png)

4. **İndir**'e tıklayın.

5. İki dosyayı bir klasöre indirin. Klasör, CİHAZDAN erişilebilen bir ağ paylaşımına da kopyalayabilirsiniz.

6. Dosyalarının bulunduğu klasörü açın.

    ![Paket dosyaları](./media/storsimple-virtual-array-install-update-11/update01folder.png)

    İki dosya görürsünüz:
    -  Microsoft Update tek başına paketin dosya `WindowsTH-KB3011067-x64`. Bu dosya, cihaz yazılımı güncelleştirmek için kullanılır.
    - Haziran için toplu güncelleştirmeleri içeren bir dosyayı `Windows8.1-KB4284815-x64`. Bu paketi içinde yer aldığı daha fazla bilgi için Git [Haziran aylık güvenlik dökümü](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815).

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yüklemeden önce emin olun:

 - Güncelleştirme veya düzeltme konağınızdaki yerel olarak veya bir ağ paylaşımı üzerinden erişilebilir indirilen var.
 - Sanal diziniz güncelleştirme 1 (10.0.10296.0) çalışıyor. Güncelleştirme 0.6, çalıştırıyorsanız [güncelleştirme 1'i yükleme](storsimple-virtual-array-install-update-1.md) ilk ve ardından güncelleştirme 1.1 yükleyin.

Bu yordamı tamamlamak için yaklaşık 4 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**. Çalıştırmakta olduğunuz yazılım sürümü not edin. **Güncelleştirme 1 (10.0.10296.0) çalıştırıyorsanız bu güncelleştirme ile devam edin. Güncelleştirme 0.6, çalıştırıyorsanız [güncelleştirme 1'i yükleme](storsimple-virtual-array-install-update-1.md) Cihazınızda ilk ve güncelleştirme 1.1 uygulayın.**
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme dosyasının adını girin. Ayrıca, bir ağ paylaşımında yerleştirdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update2m.png)

3. Bir uyarı görüntülenir. Verilen sanal dizi tek düğümlü bir cihaz güncelleştirme uygulanır, cihaz yeniden başlatıldıktan sonra kapalı kalma süresi olduğunu. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel kullanıcı Arabirimi, bu süre içinde erişilebilir değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra yönlendirilirsiniz **oturum** sayfası. Cihaz yazılımı, yerel web kullanıcı Arabiriminde, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10307** güncelleştirme 1.1.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10307** ve Azure portal raporların **10.0.10307.0** aynı sürümü için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update6m.png)

6. Windows güvenlik düzeltme ekini dosyasını kullanarak yüklemek için 2-4 arası adımları yineleyin `Windows8.1-KB4284815-x64`. Yüklemeyi tamamladıktan sonra sanal diziyi yeniden başlatır ve yerel web kullanıcı Arabirimi açmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Virtual Array'iniz yönetme](storsimple-ova-web-ui-admin.md).
