---
title: Azure Data Box Edge fiziksel cihazını kurma öğreticisi | Microsoft Docs
description: Azure Data Box Edge cihazını kurma öğreticilerinin ikincisinde fiziksel cihazı kutusundan çıkarma, rafa yerleştirme ve kablolarını bağlama adımları gösterilmektedir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/01/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 6bd3c1b2cdbd83673a181ee7e088adb39749036e
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963855"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Öğretici: Azure Data Box Edge kurulumu (Önizleme)

Bu öğreticide Data Box Edge fiziksel cihazı kurulum adımları anlatılmaktadır. Kurma süreci cihazı kutusundan çıkarma, rafa yerleştirme ve kablolarını bağlama adımlarını kapsar. 

Kurulum işleminin tamamlanması yaklaşık 2 saat sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa yerleştirme
> * Cihazın kablolarını bağlama

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

Fiziksel cihaz kurulumu ön koşulları aşağıda listelenmiştir.

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* [Portalı Data Box Edge için hazırlama](data-box-edge-deploy-prep.md) adımlarını tamamladınız.
    * Cihazınızı dağıtmak için Data Box Edge kaynağını oluşturdunuz.
    * Cihazınızı Data Box Edge kaynağıyla etkinleştirmek için etkinleştirme anahtarını oluşturdunuz.

 
### <a name="for-the-data-box-edge-physical-device"></a>Data Box Edge fiziksel cihazı için

Cihazı dağıtmadan önce:

- Cihazın düz, sabit ve dengeli bir çalışma yüzeyi (veya benzeri) üzerinde güvenli bir şekilde durduğundan emin olun.
- Kurulumu gerçekleştirmeyi planladığınız yerde şunların bulunduğundan emin olun:
    - Bağımsız bir kaynaktan gelen standart AC güç beslemesi veya
    - Kesintisiz güç kaynağına (UPS) sahip bir raf güç dağıtım birimi (PDU).
- Cihazı yerleştirmek istediğiniz rafta 1U ölçüsünde bir yuva olduğundan emin olun.

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için

Başlamadan önce:

- Data Box Edge dağıtma ağ gereksinimlerini gözden geçirin ve veri merkezi ağını gereksinimlere göre yapılandırın. Daha fazla bilgi için bkz. [Data Box Edge ağ gereksinimleri](data-box-gateway-system-requirements.md#networking-requirements).
- Cihazın en iyi şekilde çalışması için Internet bant genişliğinin en az 20 Mb/sn olduğundan emin olun.


## <a name="unpack-the-device"></a>Cihazı kutusundan çıkarma

Cihaz tek bir kutuda gönderilir. Cihazınızı kutusundan çıkarmak için aşağıdaki adımları gerçekleştirin. 

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.
2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalajda ciddi hasar varsa kutuyu açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.
3. Kutuyu açın. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
    - Tek parçalı bir kasadan oluşan Edge cihazı
    - İki güç kablosu
    - Bir aletsiz kızaklı raf montaj seti (iki yan ray ve montaj parçaları dahil)
4. Yukarıdaki parçalardan herhangi biri eksikse Data Box Edge Desteği ile iletişim kurun. Bir sonraki adım cihazınızı rafa yerleştirmektir.


## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Cihazın 19 inçlik standart bir rafa yerleştirilmesi gerekir. Cihazınızı ön ve arka direkleri bulunan 19 inçlik standart bir rafa yerleştirmek için aşağıdaki yordamı izleyin.

> [!IMPORTANT]
> Data Box Edge cihazlarının düzgün çalışması için rafa yerleştirilmeleri gerekir.


1. Ön serbest bırakma mandalını çekerek kızak tertibatının iç rayının kilidini açın. Emniyet mandalını açın ve ortadaki rayı içeri doğru iterek rayı geri çekin. İç ve dış raylar ayrılmış olmalıdır.

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Şimdi dış rayları kabinin dikey parçalarına monte edin. Doğru şekilde hizalamak için ray kızakları Front (Ön) ifadesiyle işaretlenmiştir ve bu tarafın kasanın ön tarafına doğru takılması gerekir. 
    
    1. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Rayı iki raf direğinin arasına yerleştirmek için uzatın. Dış rayı önce rafın arka tarafına takın. Arka montaj braketini arka raf montaj deliklerinin içine girecek şekilde ayarlayın.   
    2. Arka braket üzerindeki tetiği itip sabit tutarak metal kancaları açığa çıkarın. Montaj deliklerine hizalayıp geçirin ve tetiği bırakın.
    3. Ön braketi montaj deliğiyle hizalayın.
    4. Ön braketin rafa sabitlenmiş olması gerekir. İsterseniz M5 X 10L vida kullanarak rayları direklere sabitleyebilirsiniz. 

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. İç rayı kasaya takmak için iç rayın üzerindeki deliklerin kasanın yanındaki tespit pimleriyle aynı hizada olduğundan emin olun. Kasa tespit pimlerinin iç raydaki delik açıklıklarından dışarı çıktığından emin olun. Ray duyulabilir bir ses eşliğinde yerine oturana kadar rayı kasanın önüne doğru çekin. Aynı işlemleri diğer iç ray için tekrarlayın. Raf montajını tamamlamak için iç rayın takılı olduğu kasayı kızağa yerleştirin.

    ![Raf raylarını takma](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

Aşağıdaki yordamlarda Edge cihazınızın güç ve ağ kablolarını bağlama adımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Cihazınızın kablolarını bağlamaya başlamadan önce şunlara ihtiyacınız olacaktır:

- Kutusu açılmış, ambalajından çıkarılmış ve rafa monte edilmiş Edge fiziksel cihazınız.
- İki güç kablosu. 
- Yönetim arabirimine bağlamak için en az bir 1-GbE RJ-45 ağ kablosu. Cihazda biri yönetim ve diğeri veri olmak üzere iki 1-GbE ağ arabirimi vardır.
- Yapılandırılacak her veri ağı arabirimi için bir 25-GbE SFP+ bakır kablo. En az bir veri ağı arabiriminin (PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6) İnternete bağlı olması gerekir (Azure bağlantısı için).
- İki Güç Dağıtım Birimine erişim (önerilir).

> [!NOTE]
> - Tek bir veri ağı arabirimini bağlıyorsanız Azure'a veri göndermek için 25 GbE ağ arabirimlerinden birini (PORT 3, PORT 4, PORT 5 veya PORT 6 gibi) kullanmanızı öneririz. 
> - En iyi performansı elde etmek ve büyük miktarda veriyi işlemek için tüm veri bağlantı noktalarını bağlamak isteyebilirsiniz.
> - Edge cihazının veri kaynağı sunucularından veri alabilmesi için veri merkezi ağına bağlı olması gerekir. 

Edge cihazınızda 8 adet NVMe SSD vardır. Ayrıca ön panelde durum LED'i ve güç düğmeleri bulunur. Cihazın arkasında yedekli Güç Kaynağı Birimleri (PSU) vardır. Cihazınızda altı ağ arabirimi bulunur: iki 1 Gbps arabirim ve dört 25 Gbps arabirim. Cihazınızda temel kart yönetim denetleyicisi (BMC) bulunur. Cihazınızın arka yüzündeki bağlantı noktalarını inceleyin.
 
  ![Kabloları takılmış bir cihazın arka yüzü](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Cihazınızın güç ve ağ kablolarını bağlamak için aşağıdaki adımları uygulayın.

1. Güç kablolarını kasadaki PSU'lara bağlayın. Yüksek kullanılabilirlik için iki PSU'yu da takın ve ayrı güç kaynaklarına bağlayın.
2. Güç kablolarını raf güç dağıtım birimlerine (PDU) takın. İki PSU'nun ayrı güç kaynaklarını kullandığından emin olun.
3. PORT 1 ile gösterilen 1 GbE ağ arabirimi fiziksel cihazı yapılandırmak için kullanılan bilgisayara bağlayın. PORT 1, yönetim için ayrılmış arabirimdir.
4. PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6 bağlantı noktalarından birini veya birkaçını veri merkezi ağına/İnternete bağlayın. PORT 2’yi bağlıyorsanız, RJ-45 ağ kablosunu kullanın. 25-GbE ağ arabirimleri için SFP+ bakır kablolarını kullanın.  


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konularını öğrendiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa yerleştirme
> * Cihazın kablolarını bağlama

Cihazınıza bağlanma, kurulumunu yapma ve etkinleştirme adımları için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Edge cihazınıza bağlanma ve kurulumunu yapma](./data-box-edge-deploy-connect-setup-activate.md)


