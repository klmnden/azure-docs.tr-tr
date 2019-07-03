---
title: Microsoft Azure FXT Edge dosyalayıcı ağ bağlantıları ve güç kaynağı
description: Ağ bağlantı noktalarının kablo ve Azure FXT Edge dosyalayıcı donanım için güç ekleme
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: ae179e8ce2a2ba772a7fb14825660e0fff9e7410
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542923"
---
# <a name="tutorial-make-network-connections-and-supply-power-to-the-azure-fxt-edge-filer-node"></a>Öğretici: Ağ bağlantıları oluşturma ve Azure FXT Edge dosyalayıcı düğümüne güç kaynağı

Bu öğreticide bir Azure FXT Edge dosyalayıcı donanım düğüm ağ bağlantılarında kablo öğretir.

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Ortamınız için ağ kablosu türünü seçme
> * Veri Merkezi ağınızı bir Azure FXT Edge dosyalayıcı düğümüne bağlanma
> * Kablo yönetim arm (CMA) nasıl yönlendireceğini kabloları
> * Güç racked cihaza bağlayın ve açın

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Azure FXT Edge dosyalayıcı standart donanım rafa yüklenmesi gerekir. CMA dosyalayıcı düğümde yüklü olması gerekir. 

## <a name="identify-ports"></a>Bağlantı noktalarını belirleme

Çeşitli bağlantı noktaları, Azure FXT Edge dosyalayıcı arkasında belirleyin. 
 
![Kablolu bir cihaz arkasına](media/fxt-back-annotated.png)

## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

* RJ-45 bağlantı noktaları bölümünde anlatıldığı gibi veri merkezinin ağ kaynağına bağlanmak [ağ bağlantı noktaları](#network-ports).  
* Güvenli bir şekilde bağlanma [iDRAC bağlantı noktası](#idrac-port) ayrı bir ağa güvenli bir DHCP sunucusu. 
* İlk kurulum düğümüne klavye ve monitör bağlamak için USB bağlantı noktaları ve VGA bağlantı noktasını kullanın. Düğüm önyükleme gerekir ve [bir başlangıç parolası ayarlayın](fxt-node-password.md) düğümü etkinleştirmek için diğer bağlantı noktaları kullanıcının. Okuma [ilk parola atama](fxt-node-password.md) Ayrıntılar için. 

Bu makalede ayrıca nasıl [AC gücü bağlanma](#connect-power-cables) düğüm. 

Bu makalede ayrıca düğümün nasıl bağlanılacağı [seri bağlantı noktası](#serial-port-only-when-necessary), özel sorun giderme için gerekiyorsa. 

### <a name="network-ports"></a>Ağ bağlantı noktaları 

Her Azure FXT Edge dosyalayıcı düğümü, aşağıdaki ağ bağlantı noktaları içerir: 

* Altı 25GbE/10GbE çift oranı yüksek hızlı veri bağlantı noktaları: 

  * İki çift bağlantı noktası eklenti ağ bağdaştırıcıları tarafından sağlanan dört bağlantı noktaları
  * Anakart mezzanine ağ bağdaştırıcısı tarafından sağlanan iki bağlantı noktası 

* Anakart mezzanine ağ bağdaştırıcısı tarafından sağlanan iki 1GbE bağlantı noktaları 

Yüksek hızlı 25GbE/10GbE veri bağlantı noktaları, standart SFP28 uyumlu kafesleri bulunur vardır. Optik kablolarını kullanılacak SFP28 optik ileticisi modülleri (sağlanmadı) yüklemeniz gerekir.

1GbE bağlantı noktalarını standart RJ-45 bağlayıcılara sahiptir.

Desteklenen kablolar ve anahtarlar vericilerinin, consult tam listesi için [Cavium FastlinQ 41000 serisi birlikte çalışabilirlik matris](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).

Sisteminiz için kullanılacak bağlantı türü, veri merkezi ortamınıza bağlıdır.

* 25GbE ağa bağlanma, yüksek hızlı veri noktalarının her aşağıdaki kablo türlerinden birini kablo:

  * Optik kablo ve SFP28 optik ileticisi 25GbE veya çift oranı 25GbE/10GbE özelliği
  * SFP28 türü 25GbE özellikli doğrudan twinaxial kablo ekleme

* Her bağlantı aşağıdakilerden biri ile yüksek hızlı veri noktalarının 10GbE ağa bağlanma, kablo: 

  * Optik kablo ve SFP28 optik ileticisi 10GbE veya çift oranı 25GbE/10GbE yeteneği.
  * SFP28 türü 25GbE özellikli doğrudan twinaxial kablo ekleme
  * SFP28 türü adet 10 Gbe özellikli doğrudan twinaxial kablo ekleme

* 1GbE ağ bağlantı noktalarının küme yönetim trafiği için kullanılır. Denetleme **1 Gb mgmt ağ** seçenek kümesi yapılandırması için fiziksel olarak ayrı bir ağ oluşturmak istiyorsanız, kümeyi oluştururken (açıklanan [yönetim ağı yapılandırma](fxt-cluster-create.md#configure-the-management-network)). Standart Cat5 veya desteklenen kabloların listede açıklananlar gibi daha iyi kablosu ile bağlantı noktalarına kablo.

  Tüm trafik için yüksek hızlı bağlantı noktalarını kullanmayı planlıyorsanız uncabled 1GbE bağlantı noktalarını bırakabilirsiniz. Daha yüksek hızlı veri bağlantı noktası varsa, varsayılan olarak, 1GbE ağ bağlantı noktaları kullanılmaz.  

### <a name="idrac-port"></a>iDRAC bağlantı noktası  

İzleme ve donanım yönetimi için kullanılan bir uzaktan erişim denetleyicisi ile iletişim kurmasına olanak tanıyan bir 1 Gb bağlantısı iDRAC etiketli bağlantı noktasıdır. Bu denetleyici ile sorun giderme ve kurtarma için Akıllı Platform Yönetim Arabirimi (IPMI) FXT yazılım kullanır. Yerleşik kullanabileceğiniz [iDRAC arabirimi](https://www.dell.com/support/manuals/idrac9-lifecycle-controller-v3.30.30.30/idrac_3.30.30.30_ug/) donanım Bu bağlantı noktası aracılığıyla izlemek için. iDRAC ve IPMI erişim varsayılan olarak etkindir. 

> [!Note]
> İDRAC bağlantı noktası, işletim sistemi atlamak ve düğümde donanım ile doğrudan etkileşim. 

Bağlanma ve iDRAC bağlantı noktasını yapılandırırken bu güvenlik stratejileri kullanın:

* Yalnızca iDRAC bağlantı kümeye erişmek için kullanılan veri ağdan fiziksel olarak ayrı bir ağa bağlayın.
* Her bir düğümde güvenli iDRAC yönetici parolasını ayarlayın. Donanım etkinleştir - yönergeleri için bu parola ayarlamanız gerekir [donanım parola atama](fxt-node-password.md).
* Varsayılan iDRAC bağlantı noktası yapılandırması, DHCP ve IPv4 IP adresi ataması için kullanır. DHCP ortamınıza iyi korunur ve bağlantıları DHCP istemcilerinin DHCP sunucusu arasında sınırlı olduğundan emin olun. (Küme Denetim Masası'nı küme oluşturduktan sonra düğümlerin adresi yapılandırma yöntemini değiştirmek için ayarlar içerir.)
* İDRAC bağlantı noktası "ayrılmış mod" olarak bırakın (varsayılan), ayrılmış RJ-45 noktasına iDRAC/IPMI ağ trafiğini kısıtlar.

İDRAC bağlantı noktası, yüksek hızlı ağ bağlantısı gerektirmez.
  
### <a name="serial-port-only-when-necessary"></a>Seri bağlantı noktası (yalnızca gerekli olduğunda)

Bazı durumlarda, Microsoft Service ve Destek, bir terminal bir sorunu tanılamak için düğümün seri bağlantı noktasına bağlanmak için söyleyebilir.  

Konsol eklemek için:

1. Seri (COM1) bağlantı noktası FXT Edge dosyalayıcı düğümünün arkada bulun.
1. Seri bağlantı noktası 115200 ANSI 8N1 için yapılandırılmış bir terminal bağlanmak için bir null modem kablosu kullanın.
1. Konsoluna oturum açın ve destek personeli tarafından belirtildiği gibi diğer adımları uygulayın.

## <a name="route-cables-in-the-cable-management-arm-cma"></a>Rota kablolarını kablo yönetim arm (CMA)

Her Azure FXT Edge dosyalayıcı düğümü bir isteğe bağlı kablo yönetim arm ile birlikte gelir. CMA kablo yönlendirme basitleştirir ve kablo bağlantısını kesmek gerek kalmadan kasa arkasına daha kolay erişim sağlar. 

Kabloları CMA üzerinden yönlendirmek için aşağıdaki yönergeleri izleyin: 

1. Girin ve böylece bitişik sistemleriyle (1) karışmaz sepetleri çıkış olarak sağlanan KRAVAT sarmalayan kullanarak, kablolar gruplamak.
1. Hizmet konumda CMA ile iç ve dış sepetleri (2) kablo paket yol.
1. Önceden yüklenmiş kanca ve döngü straps sepetleri ya da sonunda (3) kabloları güvenliğini sağlamak için kullanın.
1. (4) Tepsisi konumunda uygulamasına geri CMA swing.
1. Arkasına sistem durumu göstergesi kablo yükleyin ve kablo CMA yönlendirerek güvenli. Diğer ucundaki kablo köşeye dış CMA sepet (5) ekleyin. 

   > [!CAUTION]
   > Kabloları döşer gelen durumdaki potansiyel hasarı önlemek için bu kablo üzerinden CMA yönlendirme sonra herhangi bir durum göstergesi kablo slack'te güvenli hale getirin. 

![Yüklü kablolarla CMA çizimi](media/fxt-install/cma-cabling-400.png)

> [!NOTE]
>  CMA yüklemediyseniz iki kanca kullanın ve sisteminizin arkasına kabloları yönlendirmek için parmaklık Seti sağlanan straps döngü.
> 
>  1. Dış CMA köşeli ayraçlar iç hem de raf çıkıntıları tarafında bulun.
>  2. Kabloları yavaşça, bunları, alınan paket sistem bağlayıcıları için sağ ve sol temizleyin.
>  3. İş parçacığı üzerinde dış CMA köşeli ayraçlar sistem kablo paketleri güvenliğini sağlamak için her iki tarafında tooled yuvaları üzerinden kanca ve döngü straps.
> 
>     ![Yönlendirilmiş bir CMA kabloları](media/fxt-install/fxt-route-cables-no-cma-400.png)

## <a name="about-ip-address-requirements"></a>IP adresi gereksinimleri hakkında

Donanım düğümler için bir Azure FXT Edge dosyalayıcı karma depolama önbelleği, IP adresleri küme yazılımı tarafından yönetilir.

Her düğüm, en az bir IP adresi gerektirir, ancak düğümleri eklendiğinde veya kümeden kaldırılmış düğüm adresleri atanır. 

Gerekli IP adresleri toplam sayısı, önbelleği oluşturan düğüm sayısını bağlıdır. 

IP adresi aralığı düğümleri yükledikten sonra Denetim Masası yazılımı kullanarak yapılandırın. Daha fazla bilgi edinmek için [küme için bilgi toplamak](fxt-cluster-create.md#gather-information-for-the-cluster).  

## <a name="connect-power-cables"></a>Güç kabloları bağlayın

Her Azure FXT Edge dosyalayıcı düğüm iki güç kaynağı birim (PSUs) kullanır. 

> [!TIP] 
> İki yedekli PSUs yararlanmak için her AC güç kablosunu bağımsız dal devresi bir güç dağıtım birimi (PDU) ekleyin.  
> 
> UPS ek koruma için Pdu'lar güçlendirmek için kullanabilirsiniz. 

1. Dahil edilen güç kablosu kasaya PSUs bağlanın. Yaşamadan ve PSUs tamamen yerleştirildiğinden emin olun. 
1. Güç kablosu ekipman raf güç dağıtım birimleri iliştirin. Mümkünse, iki ayrı güç kaynağı için iki kablosu kullanın. 
 
### <a name="power-on-an-azure-fxt-edge-filer-node"></a>Bir Azure FXT Edge dosyalayıcı düğümde güç

Düğümü desteklemek için sistem ön güç düğmesine basın. Sağ tarafındaki Denetim Masası'ndaki düğmesidir. 

### <a name="power-off-an-azure-fxt-edge-filer-node"></a>Bir Azure FXT Edge dosyalayıcı düğüm kapatma

Güç düğmesi, test sırasında ve bir kümeye eklemeden önce sistemini kapatmak için kullanılabilir. Ancak, bir kümenin parçası olarak Azure FXT Edge dosyalayıcı düğüm olduktan sonra donanım kapatmak için küme Denetim Masası yazılımı kullanmanız gerekir. Okuma [nasıl güvenli bir şekilde Azure FXT Edge dosyalayıcı donanım güç](fxt-power-off.md) Ayrıntılar için. 

## <a name="next-steps"></a>Sonraki adımlar

Son donanım, düğümlerin her birine gücüyle kablo ve sonra kök parolalarının ayarlayarak başlatın. 
> [!div class="nextstepaction"]
> [İlk parolaları ayarlama](fxt-node-password.md)
