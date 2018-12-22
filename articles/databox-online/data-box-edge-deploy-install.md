---
title: Bir Azure veri kutusu Edge fiziksel cihaz yükleme Öğreticisi | Microsoft Docs
description: Azure veri kutusu Edge yükleme hakkında daha fazla ikinci öğreticide nasıl Cihazınızı kutusundan çıkarma, rafa ve fiziksel cihaz bağlama gerektirir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/01/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 6776eeb3cfdef98084c36a9441acafb8de1ab5b2
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720331"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Öğretici: Azure veri kutusu Edge (Önizleme) yükleyin

Bu öğreticide Data Box Edge fiziksel cihazı kurulum adımları anlatılmaktadır. Yükleme yordamını açmak, rafa bağlama ve kablo cihazı içerir. 

Yüklemeyi tamamlamak için yaklaşık iki saat sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa takma
> * Cihazın kablolarını bağlama

> [!IMPORTANT]
> Veri kutusu uç çözümü önizlemededir. Sipariş ve bu çözümü dağıtın önce gözden [Azure Önizleme için hizmet koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) .

## <a name="prerequisites"></a>Önkoşullar

Fiziksel bir cihaz şu şekilde yükleme önkoşulları:

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [Azure veri kutusu Edge (Önizleme) dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).
    * Cihazınızı dağıtmak için bir veri kutusu kenar kaynağı oluşturdunuz.
    * Cihazınızı Data Box Edge kaynağıyla etkinleştirmek için etkinleştirme anahtarını oluşturdunuz.

 
### <a name="for-the-data-box-edge-physical-device"></a>Data Box Edge fiziksel cihazı için

Cihazı dağıtmadan önce:

- Cihaz güvenli bir şekilde düz, kararlı ve düzeyi çalışma yüzeyinde üzerine emin olun.
- Kurulumu gerçekleştirmeyi planladığınız yerde şunların bulunduğundan emin olun:
    - Bağımsız bir kaynaktan standart AC gücü

        -VEYA-
    - Bir raf güç dağıtım birimi (PDU) ile bir kesintisiz güç kaynağı (UPS)
    - Bir cihaz bağlamak istediğiniz raf kullanılabilir 1U yuvada

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için

Başlamadan önce:

- Veri kutusu Edge dağıtmak için ağ gereksinimlerini gözden geçirin ve gereksinimleri başına veri merkezi ağı yapılandırın. Daha fazla bilgi için bkz. [Data Box Edge ağ gereksinimleri](data-box-gateway-system-requirements.md#networking-requirements).

- En düşük Internet bant genişliği için en iyi cihazda çalışan 20 MB/sn olduğundan emin olun.


## <a name="unpack-the-device"></a>Cihazı kutusundan çıkarma

Cihaz tek bir kutuda gönderilir. Cihazınızı kutusundan çıkarmak için aşağıdaki adımları gerçekleştirin. 

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.
2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Paketleme ve kutusu ciddi zarar, açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.
3. Kutuyu açın. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
    - Tek parçalı bir kasadan oluşan Edge cihazı
    - İki güç kablosu
    - Bir alet slayt rafa monte Seti (iki tarafı rails ve bağlama donanım dahildir)

Burada listelenen öğelerin tümünü almadıysanız, veri kutusu Edge desteğe başvurun. Sonraki adım etmektir Cihazınızı bağlayın.


## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Bir standart 19 inç rafa cihaza yüklenmesi gerekir. Raf üstü için aşağıdaki yordamı kullanın, ön ve arka gönderiler ile standart 19 inç raf üzerinde Cihazınızı bağlayın.

> [!IMPORTANT]
> Data Box Edge cihazlarının düzgün çalışması için rafa yerleştirilmeleri gerekir.


1. Ön serbest bırakma mandalını çekerek kızak tertibatının iç rayının kilidini açın. Emniyet mandalını açın ve ortadaki rayı içeri doğru iterek rayı geri çekin.  
    İç ve dış raylar ayrılmış olmalıdır.

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Dış rails raf dolap dikey üyeler yükleyin. Parmaklık slaytları yönle yardımcı olmak için işaretlenmiş **ön**, ve bu amaçla muhafaza önüne doğru yapıştırılır.    
    1. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Rayı iki raf direğinin arasına yerleştirmek için uzatın. Dış rayı önce rafın arka tarafına takın. Köşeli ayraç arka raf montaj boşluklarını konumlandırmak için bağlama arka ayarlayın.   

    2. Arka braket üzerindeki tetiği itip sabit tutarak metal kancaları açığa çıkarın. Hizalama ve geri köşeli ayraç bağlama boşluklarını eklemek ve tetikleyici bırakın.

    3. Ön braketi montaj deliğiyle hizalayın.

    4. Ön köşeli ayraç artık rafa düzeltilmelidir. İsteğe bağlı olarak 10 M Vida X M5 gerekirse rails gönderiler ile güvenli hale getirmek için kullanılabilir. 

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Kasa üzerinde iç parmaklık iliştirmek için iç parmaklık üzerinde anahtar deliği kesintileri kasa kenarındaki bulmayla PIN ile hizalanır emin olun. Kasa tespit pimlerinin iç raydaki delik açıklıklarından dışarı çıktığından emin olun. Ray duyulabilir bir ses eşliğinde yerine oturana kadar rayı kasanın önüne doğru çekin. Aynı işlemleri diğer iç ray için tekrarlayın. Raf montajını tamamlamak için iç rayın takılı olduğu kasayı kızağa yerleştirin.

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

Aşağıdaki yordamlarda Edge cihazınızın güç ve ağ kablolarını bağlama adımları açıklanmaktadır.

Cihazınızı kablo başlamadan önce aşağıdakiler gerekir:

- Kutusu açılmış, ambalajından çıkarılmış ve rafa monte edilmiş Edge fiziksel cihazınız.
- İki güç kablosu. 
- Yönetim arabirimine bağlamak için en az bir 1-GbE RJ-45 ağ kablosu. Cihazda biri yönetim ve diğeri veri olmak üzere iki 1-GbE ağ arabirimi vardır.
- Yapılandırılacak her veri ağı arabirimi için bir 25-GbE SFP+ bakır kablo. Bağlantı noktası 2, 3 bağlantı noktası, bağlantı noktası 4, 5 bağlantı noktası veya bağlantı noktası 6 arasından en az bir veri ağ arabirimi (Azure bağlantısı ile) Internet'e bağlı gerekir.  
- İki güç dağıtım birimleri (önerilen) erişim.

> [!NOTE]
> - Tek bir veri ağı arabirimini bağlıyorsanız Azure'a veri göndermek için 25 GbE ağ arabirimlerinden birini (PORT 3, PORT 4, PORT 5 veya PORT 6 gibi) kullanmanızı öneririz. 
> - En iyi performansı elde etmek ve büyük miktarda veriyi işlemek için tüm veri bağlantı noktalarını bağlamak isteyebilirsiniz.
> - Edge cihazının veri kaynağı sunucularından veri alabilmesi için veri merkezi ağına bağlı olması gerekir. 

Edge cihazınızda 8 adet NVMe SSD vardır. Ayrıca ön panelde durum LED'i ve güç düğmeleri bulunur. Cihaz arkasına yedek güç kaynağı birimleri (PSUs) içerir. Cihazınızda altı ağ arabirimi bulunur: iki 1 Gbps arabirim ve dört 25 Gbps arabirim. Cihazınızda temel kart yönetim denetleyicisi (BMC) bulunur. Cihazınızın arka yüzündeki bağlantı noktalarını inceleyin.
 
  ![Kabloları takılmış bir cihazın arka yüzü](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Güç ve ağ için cihazınızın kablolarını bağlama için aşağıdaki adımları uygulayın.

1. Güç kablolarını kasadaki PSU'lara bağlayın. Yüksek kullanılabilirlik için iki PSU'yu da takın ve ayrı güç kaynaklarına bağlayın.

2. Güç kablolarını raf güç dağıtım birimlerine (PDU) takın. İki PSU'nun ayrı güç kaynaklarını kullandığından emin olun.

3. Bağlantı noktası 1 1 GbE ağ arabirimi, fiziksel cihaz yapılandırmak için kullanılan bilgisayara bağlanın. PORT 1, yönetim için ayrılmış arabirimdir.

4. PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6 bağlantı noktalarından birini veya birkaçını veri merkezi ağına/İnternete bağlayın. PORT 2’yi bağlıyorsanız, RJ-45 ağ kablosunu kullanın. 25-GbE ağ arabirimleri için SFP+ bakır kablolarını kullanın.  


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl gibi veri kutusu Edge konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa yerleştirme
> * Cihazın kablolarını bağlama

Cihazınıza bağlanma, kurulumunu yapma ve etkinleştirme adımları için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bağlanmak ve veri kutusu Edge ayarlayın](./data-box-edge-deploy-connect-setup-activate.md)


