---
title: Microsoft Azure FXT Edge dosyalayıcı küme yapılandırması - düğüm ekleme
description: Azure FXT Edge dosyalayıcı storage önbelleğine düğümleri ekleme
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 970639eec8c16540d8d7653f1d8bb01e4a397080
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450502"
---
# <a name="tutorial-add-cluster-nodes"></a>Öğretici: Küme Düğümleri Ekle 

Yeni bir Azure FXT Edge dosyalayıcı küme yalnızca tek bir düğüm oluşturulur. En az iki düğüm ekleme ve yüksek kullanılabilirlik başka yapılandırma yapmadan önce etkinleştirmeniz gerekir. 

Bu öğreticide, küme düğümleri eklemek ve yüksek kullanılabilirlik (HA) özelliği etkinleştirmek açıklanmaktadır. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * FXT kümeye düğüm ekleme
> * HA etkinleştirme

Bu öğreticideki adımları tamamlamak için yaklaşık 45 dakika sürebilir.

Bu öğreticiye başlamadan önce eklemek istediğiniz düğümlerde güç ve [ilk parolalarını ayarlama](fxt-node-password.md). 

## <a name="1-load-the-cluster-nodes-page"></a>1. Küme düğümleri sayfa yükleme

Kümenin Denetim Masası'nı bir web tarayıcısında açın ve bir yönetici olarak oturum açın. (Ayrıntılı yönergeler genel bakış makalesinde altındaki olan [ayarları sayfaları](fxt-cluster-create.md#open-the-settings-pages).)

Denetim Masası gösterir **Pano** sekmesinde açıldığında. 

![Denetim Masası Pano (ilk sekme)](media/fxt-cluster-config/dashboard-1-node.png)

Bu görüntü tek bir düğüm ile yeni oluşturulan bir küme panosunu gösterir.

## <a name="2-locate-the-node-to-add"></a>2. Düğümü eklemek için bulun

Düğümleri eklemek için tıklatın **ayarları** sekmesini **FXT düğümleri** sayfasını **küme** bölümü.

![Denetim Masası Ayarları (ikinci sekme) kümesi ile sekmesindeki > FXT yüklenen düğümleri](media/fxt-cluster-config/settings-fxt-nodes.png)

**FXT düğümleri - çıkarılamaz** listesinde gösterir tüm atanmamış FXT düğümleri (birçok veri merkezi, yalnızca birkaç sahiptir. Kümeye eklemek istediğiniz FXT düğümleri bulur.

> [!Tip] 
> Düğüm bulamazsa, istediğiniz **Unjoined** listesinde, bu gereksinimleri karşıladığından emin olun:
> 
> * Açılana ve oluşmuş bir [kök parola ayarlama](fxt-node-password.md).
> * Erişebileceğiniz bir ağa bağlı. VLAN kullanıyorsanız, bunun olması gerekir kümeyle aynı VLAN yer alır.
> * Bonjour protokolüyle algılanabilir. 
>
>   Bazı güvenlik duvarı ayarları, düğümlerin otomatik olarak algılamasını FXT işletim sistemi önleyen Bonjour tarafından kullanılan TCP/UDP bağlantı noktaları engelleyin.
> 
> Eklemek istediğiniz düğümü listede değilse, bu çözümleri deneyin: 
> 
> * Tıklayın **el ile bulmak** IP adresine göre bulmak için düğme.
> 
> * El ile geçici IP adresleri atayın. Bu, nadir olarak rastlanıyor ancak etiketli VLAN'ları kullanın ve düğümleri doğru ağda olmayan ya da kendi kendine atanan IP adresleri ağınızda izin vermiyor çözümüyse gerekli. Eski sürüm için bu belgedeki yönergeleri [el ile statik bir IP adresi ayarlamak](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html).

Düğüm adı, IP adresi, yazılım sürümü ve uygunluk durumu listesinde görüntülenir. Genellikle, **durumu** ya da yazılı sütun "katılmak istiyor" ya da düğümü kümeye katılmak uygun hale getirir sistemini veya donanımı bir sorunu anlatır.

**Eylemleri** sütununun kümeye düğüm eklemek veya kendi yazılım güncelleştirme olanak tanıyan bir düğme. Güncelleştir düğmesine kümesindeki düğümlere eşleşen yazılım sürümünü otomatik olarak yükler.

Tüm küme düğümlerinin işletim sistemini aynı sürümünü kullanmanız gerekir, ancak bir düğüm eklemeden önce yazılım güncelleştirme gerekmez. Tıkladıktan sonra **birleştirme izin** düğmesi küme birleştirme işlemi otomatik olarak denetler ve kümesinde sürümle eşleşen işletim sistemi yazılım yükler.

Bu sayfadaki seçenekler hakkında daha fazla bilgi edinmek için [ **küme** > **FXT düğümleri** ](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html) küme yapılandırma kılavuzu.

## <a name="3-click-the-allow-to-join-button"></a>3. "İzin vermek için Join" düğmesine tıklayın 

Tıklayın **birleştirmek izin*** düğmesine **eylemleri** sütun eklemek istediğiniz düğüm için.

Düğmeye tıkladığınızda, kümeye eklemek için hazırlık yazılımını güncelleştirildikçe düğümün durumu değişebilir. 

Aşağıdaki görüntüde (önce eklenen bir işletim sistemi güncelleştirme almaktır büyük olasılıkla) Kümeye katılan sürecinde olan bir düğüm gösterir. Hiç düğme görünür **eylemleri** kümeye eklenen sürecinde olan düğümleri için sütun.

![bir düğümün adını, IP adresi, yazılım sürümü, "katılmak için izin verilen" iletisi ve boş bir son sütunu gösteren düğüm tablosundaki bir satır](media/fxt-cluster-config/node-join-in-process.png)

Birkaç dakika sonra yeni düğüm küme düğümleri en üstündeki listesinde görünmelidir **FXT düğümleri** Ayarları sayfası. 

Kümenize diğer düğümler eklemek için bu işlemi yineleyin. Başlamadan önce başka bir kümeye katılan tamamlamak bir düğüm için beklemenize gerek yoktur.

## <a name="enable-high-availability"></a>Yüksek kullanılabilirliği etkinleştir

Kümeniz için ikinci bir düğüm ekledikten sonra yüksek kullanılabilirlik özelliği Pano yapılandırılmış Denetim Masası'ndaki bir uyarı iletisi görebilirsiniz. 

Yüksek kullanılabilirlik (HA) bir arıza durumunda diğer için dengelemek için küme düğümlerinin sağlar. HA varsayılan olarak etkin değildir.

!["Küme birden fazla düğüme sahip, ancak HA etkin değil..." iletisiyle birlikte Pano sekmesi Koşullar tabloda](media/fxt-cluster-config/no-ha-2-nodes.png)

> [!Note] 
> Kümede en az üç düğüm elde edene kadar HA etkinleştirmeyin.

HA üzerinde etkinleştirmek için bu yordamı izleyin: 

1. Yük **yüksek kullanılabilirlik** sayfasını **küme** bölümünü **ayarları** sekmesi.

   ![HA yapılandırma sayfası (küme > yüksek kullanılabilirlik). "Etkinleştirme HA" onay kutusunu üstünde ve Gönder düğmesinin altındaki.](media/fxt-cluster-config/enable-ha.png)

2. Etiketli kutunun seçili olduğunu tıklayın **etkinleştirme HA** tıklatıp **Gönder** düğmesi. 

Bir uyarı görünür **Pano** HA etkin olduğunu onaylamak için.

![İletiyi gösteren panoyu tablo "HA artık tam olarak yapılandırılmış"](media/fxt-cluster-config/ha-configured-alert.png)


## <a name="next-steps"></a>Sonraki adımlar

Tüm düğümler, kümenizin ekledikten sonra Kurulum kümenizin uzun vadeli depolaması yapılandırıp devam.

> [!div class="nextstepaction"]
> [Arka uç depolama ekleyebilir ve sanal ad alanı ayarlama](fxt-add-storage.md)