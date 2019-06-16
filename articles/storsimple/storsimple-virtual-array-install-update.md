---
title: Bir Microsoft Azure StorSimple sanal dizisi güncelleştirmeleri yükleyin | Microsoft Docs
description: StorSimple sanal dizisi web kullanıcı Arabirimi portalı ve düzeltme yöntemi kullanarak güncelleştirmeleri uygulamak için kullanmayı açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 9997a97b-9382-43ed-b56e-61369335c987
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7bf064ff01693f7a65c756a99c435d7f1a39840e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61409464"
---
# <a name="install-updates-on-your-storsimple-virtual-array---azure-portal"></a>StorSimple Virtual Array'iniz üzerinde - Azure portal güncelleştirmeleri yükle

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple sanal dizisi yerel web UI aracılığıyla ve Azure Portalı aracılığıyla güncelleştirmeleri yüklemek için gereken adımlar açıklanmaktadır. Yazılım güncelleştirmelerini veya düzeltmeleri StorSimple Virtual Array'iniz güncel tutmak için uygulama gerekir. 

Bir güncelleştirme veya düzeltme yükleme Cihazınızı yeniden göz önünde bulundurun. StorSimple sanal dizi tek düğümlü bir cihaz olduğunu devam eden herhangi bir g/ç kesintiye ve kapalı kalma süresi Cihazınızı deneyimleri. 

Bir güncelleştirmeyi uygulamadan önce çevrimdışı paylaşımları ve birimleri konakta ilk almanızı öneririz ve ardından cihaz. Bu, veri bozulması olasılığını en aza indirir.

> [!IMPORTANT]
> Güncelleştirme 0.1 veya GA yazılım sürümleri çalıştırıyorsanız, güncelleştirme 0.3 yüklemek için yerel web UI aracılığıyla düzeltme yöntemi kullanmanız gerekir. Güncelleştirme 0.2 çalıştırıyorsanız, Klasik Azure Portalı aracılığıyla güncelleştirmeleri yüklemeniz önerilir.
 

## <a name="use-the-local-web-ui"></a>Yerel web kullanıcı arabirimini kullanın

Yerel web kullanıcı Arabirimi kullanılırken iki adımı vardır:

* Güncelleştirme veya düzeltme indirin
* Güncelleştirmeyi veya düzeltmeyi yükleyin

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı başlatın ve gidin [ https://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Update Kataloğu arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Girin **3182061** güncelleştirme 0.3 ve ardından **arama**.
   
    Düzeltme listesi görüntülenir, örneğin, **StorSimple Virtual Array güncelleştirme 0.3**.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update/download1.png)

4. **Ekle**'yi tıklatın. Güncelleştirme sepete eklenir.

5. **Sepeti Görüntüle**’ye tıklayın.

6. **İndir**’e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeler belirtilen konuma indirilir ve güncelleştirme ile aynı adı taşıyan alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

7. Kopyalanan klasörünü açın, bir Microsoft Update tek başına paket dosyası görmeniz gerekir `WindowsTH-KB3011067-x64`. Bu dosya, güncelleştirme veya düzeltme yüklemek için kullanılır.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleyin

Güncelleştirme veya düzeltme yüklemeden önce bir ağ paylaşımı üzerinden erişilebilir veya sahip olduğunuz güncelleştirme veya düzeltme konağınızdaki yerel olarak ya da indirilen emin olun. 

GA çalıştıran bir cihaza güncelleştirmeleri yükleme veya güncelleştirme 0.1 yazılım sürümleri için bu yöntemi kullanın. Bu yordamı tamamlamak için değerinden 2 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirme veya düzeltme yüklemek için

1. Yerel web kullanıcı Arabirimi, Git **Bakım** > **yazılım güncelleştirmesi**.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update1m.png)

2. İçinde **güncelleştirme dosya yolu**, güncelleştirme veya düzeltme dosyasının adını girin. Ayrıca, bir ağ paylaşımında yerleştirdiyseniz güncelleştirme veya düzeltme yükleme dosyasına göz atabilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update2m.png)

3. Bir uyarı görüntülenir. Tek düğümlü bir cihaz güncelleştirme uygulanır, cihaz yeniden başlatıldıktan sonra kapalı kalma süresi budur. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update3m.png)

4. Güncelleştirme başlatır. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel kullanıcı Arabirimi, bu süre içinde erişilebilir değil.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra yönlendirilirsiniz **oturum** sayfası. Cihaz yazılımı, yerel web kullanıcı Arabiriminde, güncelleştirilmiş olduğunu doğrulamak için Git **Bakım** > **yazılım güncelleştirmesi**. Görüntülenen yazılım sürümü olmalıdır **10.0.0.0.0.10288.0** güncelleştirme 0.3 için.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web kullanıcı Arabirimi biraz farklı bir şekilde hem de Azure portalında bildirin. Örneğin, yerel web kullanıcı Arabirimi raporları **10.0.0.0.0.10288** ve Azure portal raporların **10.0.10288.0** aynı sürümü için.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0.2 çalıştırılıyorsa, Azure Portalı aracılığıyla güncelleştirmeleri yüklemeniz önerilir. Tarama, indirin ve ardından güncelleştirmeleri yüklemek Kullanıcı Portalı yordamı gerektirir. Bu yordamı tamamlamak için yaklaşık 7 dakika sürer. Güncelleştirme veya düzeltme yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal.md)]

(Olarak iş durumu % 100 tarafından gösterilen) yükleme tamamlandıktan sonra StorSimple cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve bu hizmete bağlı cihazlar listesinden güncelleştirmek istediğiniz cihaza tıklayın. İçinde **ayarları** dikey penceresinde, Git **Yönet** seçin ve bölüm **cihaz güncelleştirmeleri**. Görüntülenen yazılım sürümü olmalıdır **10.0.10288.0**.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Virtual Array'iniz yönetme](storsimple-ova-web-ui-admin.md).

