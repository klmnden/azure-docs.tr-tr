---
title: "StorSimple cihaz yapılandırmasını değiştirme | Microsoft Docs"
description: "StorSimple Yöneticisi hizmeti zaten dağıtılmış bir StorSimple cihazı yeniden yapılandırmak için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 5025dc2643f9e57c0d8fa919b7d601f184d74431
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple cihaz yapılandırmasını değiştirmek için StorSimple Yöneticisi hizmetini kullanma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [StorSimple cihaz yapılandırmasını değiştirmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-8000-modify-device-config.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Klasik Azure portalı **yapılandırma** sayfası bir StorSimple Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazda yapılandırabilirsiniz tüm aygıt parametreleri içerir. Bu öğretici nasıl kullanabileceğinizi açıklar **yapılandırma** sayfasını aşağıdaki cihaz düzeyinde görevleri gerçekleştirmek için:

* Cihaz ayarlarını değiştirme 
* Saat ayarlarını değiştirme 
* DNS ayarlarını değiştirme 
* Ağ arabirimleri değiştirme
* Değiştirme veya IP'leri yeniden atama

## <a name="modify-device-settings"></a>Cihaz ayarlarını değiştirme
Cihaz ayarları cihaz ve aygıt açıklaması kolay adını içerir.

> [!NOTE] 
> Klasik Azure portalındaki aygıt adı değiştirilemiyor. Cihaz yeniden adlandırılması desteklenmez.

StorSimple Yöneticisi hizmetine bağlı bir StorSimple cihazına bir varsayılan ad atanır. Varsayılan adı cihazın seri numarası genellikle yansıtır. Örneğin, 15 karakter uzunluğunda 8600-SHX0991003G44HT gibi bir varsayılan cihaz adı aşağıdaki gösterir:

* **8600** – cihaz modeli gösterir.
* **SHX** – üretim sitesini gösterir.
* **0991003** -belirli bir ürün gösterir.
* **G44HT**-benzersiz seri numaraları oluşturmak için en son 5 rakamlı artırılır. Bu ardışık olmayabilir.

Aygıt açıklaması belirtebilirsiniz. Aygıt açıklaması genellikle sahibi ve cihazın fiziksel konumunu belirlemenize yardımcı olur. Açıklama alanı, 256'dan az karakter içermelidir.

## <a name="modify-time-settings"></a>Saat ayarlarını değiştirme
Cihazınızı bulut depolama hizmeti sağlayıcısı ile kimlik doğrulamak için zaman eşitlemeniz gerekir. Aşağı açılan listeden saat dilimini seçin ve en fazla iki ağ zaman Protokolü (NTP) sunucuları belirtin. Birincil NTP sunucusu gereklidir ve Cihazınızı yapılandırmak için StorSimple için Windows PowerShell'i kullandığınızda belirtilir. Windows Server varsayılan belirtebilirsiniz **time.windows.com** NTP sunucusu olarak. Klasik Azure Portalı aracılığıyla birincil NTP sunucusu yapılandırma görüntüleyebilirsiniz, ancak bunu değiştirmek için Windows PowerShell arabirimini kullanmanız gerekir.

İkincil NTP sunucusu yapılandırması isteğe bağlıdır. İkincil bir NTP sunucusunu yapılandırmak için Klasik Portalı'nı kullanabilirsiniz. 

NTP sunucusu yapılandırırken, ağınızı merkeziniz Internet'e geçirmek NTP trafiğini verdiğinden emin olun. Bir ortak NTP sunucusu belirtirken, ağ güvenlik duvarları ve diğer güvenlik aygıtlarını Ağ dışından gelen ve giden NTP trafiğin izin verecek şekilde yapılandırıldığından emin olmalısınız. Çift yönlü NTP trafiğini izin verilmiyor (Bu işlev bir Windows etki alanı denetleyicisi sunar) bir iç NTP sunucusu kullanmanız gerekir. Cihazınızı zaman eşitleyemez, bulut depolama sağlayıcısı ile iletişim kurabildiğinden olmayabilir.

Ortak NTP sunucularının bir listesini görmek için Git [NTP sunucuları Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Aygıtı farklı bir saat diliminde dağıtılırsa ne olur?
Aygıtı farklı bir saat diliminde dağıtılırsa, aygıt saat dilimini değiştirir. O yedekleme ilkelerini aygıt saat dilimini kullanır, yedekleme ilkelerini otomatik olarak yeni saat dilimine göre ayarlanır. Kullanıcı müdahalesi gerekli değildir.

## <a name="modify-dns-settings"></a>DNS ayarlarını değiştirme
Cihazınızı bulut depolama hizmeti sağlayıcısı ile iletişim kurmayı denediğinde bir DNS sunucusu kullanılır. Yüksek kullanılabilirlik için hem birincil hem de ikincil DNS sunucuları ilk cihaz dağıtımı sırasında yapılandırmak için gereklidir. Birincil DNS sunucusunu yeniden yapılandırmak için Windows PowerShell arabirimi StorSimple Cihazınızda kullanmanız gerekecektir.

İkincil DNS sunucusu değiştirmek için Klasik Azure portalını kullanabilirsiniz.

## <a name="modify-network-interfaces"></a>Ağ arabirimleri değiştirme
Cihazınızı dört biri 1 GbE ve ikisi de 10 GbE altı aygıt, ağ arabirimleri vardır. Bu arabirimleri 0 – veri 5 verileri olarak etiketlenir. Veri 2 ve veri 3 10 GbE ağ arabirimleri ise 0, 1 DATA, veri 4 ve verileri 5 1 GbE verilerdir.

Yapılandırma **ağ arabirimi ayarları** her kullanılacak arabirimler. Yüksek kullanılabilirlik sağlamak için Cihazınızda en az iki iSCSI arabirimleri ve iki bulut etkin arabirimlere sahip öneririz. Size önerilir ancak kullanılmayan arabirimleri devre dışı bırakılmasını gerektirmez.

Ağ arabirimlerinin yapılandırdığınızda, sanal IP (VIP) yapılandırmanız gerekir.

Veri 0 bulut varsayılan olarak etkindir. Veri 0 yapılandırırken, iki sabit IP adresleri, her denetleyici için bir tane yapılandırmak için gereklidir. Bu sabit IP adreslerini cihaz denetleyicilerinin doğrudan erişmek için kullanılan ve cihazda veya sorun giderme amacıyla denetleyicileri eriştiğinizde güncelleştirmeleri yüklediğinizde yararlıdır.

StorSimple 8000 serisi güncelleştirme 1'de veri 0 Yönlendirme ölçüsünü ayarlamak için en düşük; Bu nedenle, Cihazınızı StorSimple 8000 serisi güncelleştirme 1 çalışıyorsa, tüm bulut trafiği veri 0 yönlendirilir. StorSimple Cihazınızda birden fazla bulut etkin ağ arabirimi varsa, bu not edin.

> [!NOTE]
> Denetleyiciye yönelik sabit IP adresleri, cihaza güncelleştirmelerin uygulanması için kullanılır. Bu nedenle, sabit IP’lerin yönlendirilebilir ve İnternet’e bağlanabilir olması gerekir.
> 
> 

Her ağ arabirimi için aşağıdaki parametreleri görüntülenir:

* **Hızı** – kullanıcı tarafından yapılandırılabilir bir parametre değil. Veri 2 ve veri 3 10 GbE arabirimleri ise 0, 1 DATA, veri 4 ve verileri 5 her zaman 1 GbE verilerdir.
  
  > [!NOTE]
  > Hız ve çift yönlü her zaman otomatik-belirlenir. Jumbo çerçeveler desteklenmez.
  > 
  > 
* **Arabirim durumu** – bir arabirim etkin veya devre dışı. Etkinleştirilirse, aygıt arabirimi kullanmayı dener. Ağa bağlı ve kullanılan arabirimleri etkinleştirilmesi önerilir. Kullanmadığınız arabirimleri devre dışı bırakın.
* **Arabirim türü** – Bu parametre, bulut depolama trafiğinden iSCSI trafiğini yalıtmak sağlar. Bu parametre, aşağıdakilerden biri olabilir:
  
  * **Bulut etkin** – etkin olduğunda, cihaz bu arabirim Bulutu ile iletişim kurmak için kullanır.
  * **iSCSI etkin** – etkin olduğunda, cihaz bu arabirim iSCSI ana bilgisayarı ile iletişim kurmak için kullanır.
    
    Bulut depolama trafiğinden iSCSI trafiğini yalıtmak öneririz. Ayrıca, ana bilgisayarınız Cihazınızı aynı alt ağda içindeyse, bir ağ geçidi atamak gerekmez unutmayın; Ancak, ana bilgisayarınız Cihazınızı daha farklı bir alt ağda ise, bir ağ geçidi atamanız gerekir.
* **IP adresi** – bu IPv4 veya IPv6 veya her ikisi de olabilir. IPv4 ve IPv6 adres ailesi için cihaz ağ arabirimleri desteklenir. IPv4 kullanırken, bir 32 bit IP adresi belirtin (*xxx.xxx.xxx.xxx*) noktalı ondalık gösterimde. IPv6 kullanırken, yalnızca 4 basamaklı önek sağlayın ve 128-bit adres bu ön ekini temel alarak, aygıt ağ arabirimi için otomatik olarak oluşturulur.
* **Alt ağ** – bu alt ağ maskesi başvuruyor ve Windows PowerShell arabirimi yapılandırılır.
* **Ağ geçidi** – (alt ağ) aynı IP adresi alanı içinde olmayan düğüm ile iletişim kurmayı denediğinde, bu arabirim tarafından kullanılan varsayılan ağ geçidi budur. Varsayılan Ağ Geçidi Arabirimi aynı adres alanında (alt ağ) IP adresi, alt ağ maskesi kullanılarak belirlenen olması gerekir.
* **IP adresi sabit** – yalnızca veri 0 yapılandırması sırasında bu alan kullanılabilir arabirimi. Güncelleştirmeleri veya cihaz sorunlarını giderme gibi işlemleri için doğrudan aygıt denetleyicisine bağlanmak gerekebilir. Sabit IP adresi, etkin ve aygıtınızda pasif denetleyiciyi erişmek için kullanılabilir.

Klasik Azure portalı üzerinden denetleyici 0 ve denetleyici 1 yeniden yapılandırabilirsiniz.

> [!NOTE]
> * Düzgün çalışmasını sağlamak için arabirim hızı ve çift yönlü her aygıt arabiriminin bağlı olduğu anahtar üzerindeki doğrulayın. Anahtar arabirimleri ile ya da anlaşması gereken veya Gigabit Ethernet için yapılandırılması (1000 MB/sn) ve tam çift yönlü. İşletim yavaş hızlarda veya yarı çift yönlü arabirimleri performans sorunlarına neden olur.
> * Kesinti ve kapalı kalma süresini en aza indirmek için her Cihazınızı iSCSI ağ arabiriminin bağlanma anahtar bağlantı noktalarını portfast etkinleştirmenizi öneririz. Bu, ağ bağlantısı hızlı bir şekilde yük devretmesi durumunda kurulması güvence altına alır.
> 
> 

## <a name="swap-or-reassign-ips"></a>Değiştirme veya IP'leri yeniden atama
Herhangi bir ağ denetleyicisi arabirimde kullanımda olan bir VIP (aynı cihaz veya ağdaki başka bir aygıt) atanır. şu anda, ardından denetleyici üzerinden başarısız olur. Bu nedenle, aygıt ağ arabirimi için VIP'ler takas yinelenen bir IP durum oluşturacağından uygun yordamı izleyin gerekir.

Değiştirme veya herhangi bir ağ arabirimleri için VIP'ler yeniden atamak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-reassign-ips"></a>IP'leri yeniden atamak için
1. Her iki arabirimleri için IP adresi temizleyin.
2. IP adreslerini temizlendikten sonra ilgili arabirimlerine yeni IP adresleri atayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple cihazınız için MPIO yapılandırma](storsimple-configure-mpio-windows-server.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

