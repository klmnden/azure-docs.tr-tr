---
title: Yüklemek için - öğretici Cihazınızı kutusundan çıkarma, rafa, kablo Azure veri kutusu Edge fiziksel cihaz | Microsoft Docs
description: Azure veri kutusu Edge yükleme hakkında daha fazla ikinci öğreticide nasıl Cihazınızı kutusundan çıkarma, rafa ve fiziksel cihaz bağlama gerektirir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/31/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 0a9939155d92897019dc1ad5651d249cda11b993
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476949"
---
# <a name="tutorial-install-azure-data-box-edge"></a>Öğretici: Azure veri kutusu Edge yükleyin

Bu öğreticide Data Box Edge fiziksel cihazı kurulum adımları anlatılmaktadır. Yükleme yordamını açmak, rafa bağlama ve kablo cihazı içerir. 

Yüklemeyi tamamlamak için yaklaşık iki saat sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa takma
> * Cihazın kablolarını bağlama

## <a name="prerequisites"></a>Önkoşullar

Fiziksel bir cihaz şu şekilde yükleme önkoşulları:

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [Azure veri kutusu Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).
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

- Veri kutusu Edge dağıtmak için ağ gereksinimlerini gözden geçirin ve gereksinimleri başına veri merkezi ağı yapılandırın. Daha fazla bilgi için bkz. [Data Box Edge ağ gereksinimleri](data-box-edge-system-requirements.md#networking-port-requirements).

- En düşük Internet bant genişliği için en iyi cihazda çalışan 20 MB/sn olduğundan emin olun.


## <a name="unpack-the-device"></a>Cihazı kutusundan çıkarma

Cihaz tek bir kutuda gönderilir. Cihazınızı kutusundan çıkarmak için aşağıdaki adımları gerçekleştirin. 

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.
2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Paketleme ve kutusu ciddi zarar, açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.
3. Kutuyu açın. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
    - Tek tek kutu veri kutusu Edge cihazı
    - İki güç kablosu
    - Bir parmaklık Seti derleme
    - Güvenliği, çevre ve yasal bilgiler Kitapçığı

Burada listelenen öğelerin tümünü almadıysanız, veri kutusu Edge desteğe başvurun. Sonraki adım etmektir Cihazınızı bağlayın.


## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Bir standart 19 inç rafa cihaza yüklenmesi gerekir. Raf üstü için aşağıdaki yordamı kullanın, standart bir 19 inç rafa Cihazınızı bağlayın.

> [!IMPORTANT]
> Data Box Edge cihazlarının düzgün çalışması için rafa yerleştirilmeleri gerekir.


### <a name="prerequisites"></a>Önkoşullar

- Başlamadan önce güvenliği, çevre ve yasal bilgiler Kitapçığı güvenlik yönergeleri okuyun. Bu Kitapçığı cihazla sevk.
- Rails raf muhafaza alt kısmına yakın ayrılan alana yükleme başlar.
- Tooled parmaklık bağlama yapılandırması için:
    -  Sekiz Vida sağlamanız gereken: #10-32, #12-24, #M5 veya #M6. Baş Vida çapı 10 mm'lik küçüktür (0,4") olmalıdır.
    -  Düz Eğimli tornavida ihtiyacınız vardır.

### <a name="identify-the-rail-kit-contents"></a>Parmaklık Seti içeriği tanımlayın

Parmaklık Seti bütünleştirilmiş kod yükleme için bileşenleri bulun:
1. İki A7 Dell ReadyRails parmaklık derlemeleri kayan II
2. İki kanca ve döngü straps

    ![Parmaklık Seti içeriği tanımlayın](./media/data-box-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Yükleme ve kaldırma alet rails (kare delik veya yuvarlak delik raflar)

> [!TIP]
> Bu seçenek alet çünkü yüklemek ve rails akıtılmamış kare kaldırın veya raflar açıklarına yuvarlamak için araçları gerektirmez.

1. Etiketli sol ve sağ parmaklık son parçaları konumlandırma **ön** içe bakan ve boşluklar dikey raf çıkıntıları ön tarafındaki bilgisayar lisansı için her uç parça yönlendirmek.
2. Her alt ve üst boşluklar son parçası, istenen U alanları hizalayın.
3. Parmaklık arka ucunu kadar tam lisans dikey raf Flanş ve Mandal tıklama yerine etkileşim kurun. Getirin ve ön uç parça dikey raf Flanş üzerinde bilgisayar lisansı için bu adımları yineleyin.
4. Rails kaldırmak için Mandal yayın düğmesini üzerinde uç parça Orta çekme ve her parmaklık unseat.

    ![Yükleme ve alet rails kaldırma](./media/data-box-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Yükleme ve kaldırma tooled rails (zincir delik raflar)

> [!TIP]
> Bu seçenek, bir aracı gerektirdiğinden tooled (_düz Eğimli tornavida_) yükleme ve sunucu rafları iş parçacıklı yuvarlak açıklarına içine rails kaldırın.

1. PIN'ler ön ve arka düz Eğimli tornavida kullanarak montaj kaldırın.
2. Çekme ve bağlama noktalarından kaldırılacak parmaklık Mandal yarı mamullerden döndürün.
3. Sol ve sağda iki Vida çiftlerini kullanarak ön dikey raf çıkıntıları için rails bağlama ekleyin.
4. Köşeli ayraçlar karşı arka dikey raf çıkıntıları iletmek ve iki Vida çiftlerini kullanarak bunları eklemek sol ve sağ arka kaydırın.

    ![Yükleme ve tooled rails kaldırma](./media/data-box-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Sistemin bir raf yükleyin

1. Bunlar kilitlemek kadar iç slayt rails rafa dışında çekin.
2. Sistemin her bir tarafta arka parmaklık standoff bulun ve bunları arka J yuvaları slayt derlemeler halinde düşürün. Parmaklık standoffs J yuvalarda yerleştirildiğinden kadar sistem Aşağı Döndür.
3. Kilit levers yere tıklayana kadar sistem içe gönderin.
4. Slayt yayın kilit düğmeleri rails hem de rafa sistem slayta tuşuna basın.

    ![Bir rafa sistemini yükleyin](./media/data-box-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Sistem rafa kaldırın

1. Kilit levers iç rails tarafında bulun.
2. Her seviyesini yayın konumuna kadar döndürerek kilidini açın.
3. Sistem tarafının metz'in kavrayın ve parmaklık standoffs J yuvaları önündeki kadar ileriye doğru çekin. Sistem yedekleme ve raf uzağa lift- and -düzeyi yüzeyinde yerleştirin.

    ![Sistem rafa kaldırın](./media/data-box-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Etkileşim kurun ve slam Mandal serbest bırakın

> [!NOTE]
> Slam tutma ile donatılmış olmayan sistemler için bu yordamın 3. adımında açıklandığı gibi Vida, kullanarak sistem güvenliğini sağlayın.

1. Öne bakan, sistem her iki tarafında slam Mandal bulun.
2. Mandal otomatik olarak sistemi rafa gönderilir ve tutma üzerinde çekerek serbest olarak etkileşim kurun.
3. Rafa sevkiyat için veya diğer kararsız ortamları sistem güvenliğini sağlamak için her Mandal altında bağlamayı sabit Sarmal bulun ve her Sarmal # 2 sıkılaştıran Phillips tornavida.

    ![Etkileşim kurun ve slam Mandal serbest bırakın](./media/data-box-edge-deploy-install/engaging-releasing-slam-latch.png)

### <a name="route-the-cables"></a>Kabloları yönlendirme

> [!NOTE]
>  İsteğe bağlı kablo yönetim Arm (CMA) sipariş değil, iki kanca kullanın ve sisteminizi arkasına kabloları yönlendirmek için parmaklık Seti sağlanan straps döngü.

1. Dış CMA köşeli ayraçlar iç hem de raf çıkıntıları tarafında bulun.
2. Kabloları yavaşça, bunları, alınan paket sistem bağlayıcıları için sağ ve sol temizleyin.
3. Dış CMA köşeli ayraçlar sistem kablo paketleri güvenliğini sağlamak için her iki tarafında üzerinde yuvaları üzerinden kanca ve döngü straps iş parçacığı.


    ![Kabloları yönlendirme](./media/data-box-edge-deploy-install/routing-cables.png)

## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

Aşağıdaki yordamlarda, güç ve ağ için veri kutusu Edge cihazınızın kablolarını bağlama işlemleri açıklanmaktadır.

Cihazınızı kablo başlamadan önce aşağıdakiler gerekir:

- Açılmış, veri kutusu Edge fiziksel cihazı ve raf bağlanır.
- İki güç kablosu.
- Yönetim arabirimine bağlamak için en az bir 1-GbE RJ-45 ağ kablosu. Cihazda biri yönetim ve diğeri veri olmak üzere iki 1-GbE ağ arabirimi vardır.
- Yapılandırılacak her veri ağı arabirimi için bir 25-GbE SFP+ bakır kablo. Bağlantı noktası 2, 3 bağlantı noktası, bağlantı noktası 4, 5 bağlantı noktası veya bağlantı noktası 6 arasından en az bir veri ağ arabirimi (Azure bağlantısı ile) Internet'e bağlı gerekir.  
- İki güç dağıtım birimleri (önerilen) erişim.

> [!NOTE]
> - Yalnızca bir veri ağ arabirimi bağlanıyorsanız verileri Azure'a göndermek için bağlantı noktası 3, 4 bağlantı noktası, bağlantı noktası 5 veya bağlantı noktası 6 gibi 25/10 GbE ağ arabirimi kullanmanızı öneririz. 
> - En iyi performansı elde etmek ve büyük miktarda veriyi işlemek için tüm veri bağlantı noktalarını bağlamak isteyebilirsiniz.
> - Böylece veri kaynağı sunuculardan veri alabilen veri kutusu sınır cihazı veri merkezi ağa bağlanması gerekir.

Veri kutusu Edge Cihazınızda:

- Ön panelini disk sürücüleri ve güç düğmesi vardır.

    - Cihazınızı kuyruğun 10 disk yuvaları vardır.
    - Yuva 0 240 GB, işletim sistemi diski kullanılan bir SATA sürücüsünü sahiptir. Yuva 1 boş ve NVMe SSD veri diski kullanılan Yuva 2'dan 9 olmasıdır.
- Arka düzlem yedek güç kaynağı birimleri (PSUs) içerir.
- Arka düzlem altı ağ arabirimi bulunur:

    - İki 1 GB/sn arabirim.
    - Ayrıca 10 GB/sn arabirim hizmet verebilen dört 25 GB/sn arabirim.
    - Bir temel kart yönetim denetleyicisine (BMC).

- İki ağ kartları 6 bağlantı noktalarına karşılık gelen arka düzlem sahiptir:

    - QLogic FastLinQ 41264
    - QLogic FastLinQ 41262

Desteklenen kablo, anahtarlar ve bu ağ kartları için vericilerinin tam listesi için Git [Cavium FastlinQ 41000 serisi birlikte çalışabilirlik matris](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Güç ve ağ için cihazınızın kablolarını bağlama için aşağıdaki adımları uygulayın.

1. Cihazınızın arka düzlem çeşitli bağlantı noktaları belirleyin.

    ![Kablolu bir cihazın arka düzlem](./media/data-box-edge-deploy-install/backplane-cabled.png)

2. Disk yuvaları ve cihazın ön güç düğmesini bulun.

    ![Bir cihazın ön düzlemi](./media/data-box-edge-deploy-install/device-front-plane-labeled-1.png)

3. Güç kablolarını kasadaki PSU'lara bağlayın. Yüksek kullanılabilirlik için iki PSU'yu da takın ve ayrı güç kaynaklarına bağlayın.
4. Güç kablolarını raf güç dağıtım birimlerine (PDU) takın. İki PSU'nun ayrı güç kaynaklarını kullandığından emin olun.
5. Cihazda etkinleştirmek için güç düğmesine basın.
6. Bağlantı noktası 1 1 GbE ağ arabirimi, fiziksel cihaz yapılandırmak için kullanılan bilgisayara bağlanın. PORT 1, yönetim için ayrılmış arabirimdir.
7. PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6 bağlantı noktalarından birini veya birkaçını veri merkezi ağına/İnternete bağlayın.

    - PORT 2’yi bağlıyorsanız, RJ-45 ağ kablosunu kullanın.
    - 25/10-GbE ağ arabirimleri, SFP + siyah Bakır kablo kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl gibi veri kutusu Edge konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa yerleştirme
> * Cihazın kablolarını bağlama

Cihazınıza bağlanma, kurulumunu yapma ve etkinleştirme adımları için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bağlanmak ve veri kutusu Edge ayarlayın](./data-box-edge-deploy-connect-setup-activate.md)
