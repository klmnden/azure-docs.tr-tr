---
title: StorSimple 8000 serisi cihaz yapılandırmasını değiştirme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti zaten dağıtılmış bir StorSimple cihazı yeniden yapılandırmak için kullanmayı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 774f5a73a5fc30352698c0af0c279fbbe488c480
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60632231"
---
# <a name="use-the-storsimple-device-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple cihaz Yöneticisi hizmeti, StorSimple cihaz yapılandırmasını değiştirmek için kullanın

## <a name="overview"></a>Genel Bakış

Azure portalında **cihaz ayarları** konusundaki **ayarları** dikey penceresinde, StorSimple cihaz Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazında yeniden tüm cihaz parametreleri içerir . Bu öğreticide nasıl kullanabileceğinizi açıklar **ayarları** dikey penceresinde aşağıdaki cihaz düzeyinde görevleri gerçekleştirmek için:

* Cihaz kolay adı değiştirin
* Cihaz saat ayarlarını değiştirme
* İkincil DNS atayın
* Ağ arabirimleri değiştirme
* Takas veya IP'ler yeniden atama

## <a name="modify-device-friendly-name"></a>Cihaz kolay adı değiştirin

Cihaz adını değiştirin ve tercih ettiğiniz benzersiz bir kolay ad atamak için Azure portalını kullanabilirsiniz. Kullanım **genel ayarlar** dikey penceresinde cihaz kolay adı değiştirmek için cihaz. Kolay ad, tüm karakterleri içerebilir ve en fazla 64 karakter uzunluğunda olabilir.

> [!NOTE] 
> Cihaz Kurulumu tamamlamadan önce yalnızca Azure portalında cihaz adını değiştirebilirsiniz. En düşük cihaz kurulumu tamamlandıktan sonra cihaz adını değiştiremezsiniz.

![Cihaz adı genel ayarları](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

StorSimple cihaz Yöneticisi hizmetine bağlı bir StorSimple cihazı varsayılan bir ad atanır. Varsayılan adı, cihazın seri numarası genellikle yansıtır. Örneğin, 15 karakter uzunluğunda 8600 SHX0991003G44HT gibi bir varsayılan cihaz adı aşağıdaki gösterir:

* **8600** – cihaz modeli gösterir.
* **SHX** – üretim sitesini belirtir.
* **0991003** -belirli bir ürün gösterir.
* **G44HT**-benzersiz seri numaraları oluşturmak için son 5 rakamlı artırılır. Bu, bir sıralı küme olmayabilir.

## <a name="modify-device-description"></a>Cihaz açıklaması değiştirme

Kullanım **genel ayarlar** dikey penceresinde cihaz açıklaması değiştirmek için cihaz.

![Cihaz açıklaması genel ayarları](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

Cihaz açıklaması genellikle sahibi ve cihazın fiziksel konumunu belirlemek de yardımcı olur. Açıklama alanı en fazla 256 karakter içermelidir.

## <a name="modify-time-settings"></a>Saat ayarlarını değiştirme

Bulut depolama hizmeti sağlayıcısı ile kimlik doğrulaması için Cihazınızı saati eşitlemesi gerekir. Kullanım **genel ayarlar** dikey penceresinde cihaz saat ayarlarını değiştirmek için cihaz.

![Cihaz açıklaması genel ayarları](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Saat diliminizi aşağı açılan listeden seçin. En fazla iki Ağ Saati Protokolü (NTP) sunucuları belirtebilirsiniz:

 - **Birincil NTP sunucusu** -yapılandırma gereklidir ve Cihazınızı yapılandırmak için StorSimple için Windows PowerShell kullanırken belirtilir. Windows Server varsayılan belirtebilirsiniz **time.windows.com** NTP sunucunuzla. Birincil NTP sunucusu yapılandırması Azure Portalı aracılığıyla görüntüleyebilirsiniz, ancak bunu değiştirmek için Windows PowerShell arabirimini kullanmanız gerekir. Kullanım `Set-HcsNTPClientServerAddress` cihazınızın birincil NTP sunucusunu değiştirmek için cmdlet'i. Daha fazla bilgi için söz dizimi için Git [kümesi HcsNTPClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet'i.

- **İkincil NTP sunucusu** -isteğe bağlı bir yapılandırmadır. İkincil NTP sunucusu yapılandırmak için bu portalı kullanabilirsiniz.

NTP sunucusunu yapılandırırken, ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin verdiğini emin olun. Genel bir NTP sunucusu belirtirken, ağ güvenlik duvarları ve diğer güvenlik cihazları Ağ dışından gelen ve giden her fırsatta seyahat etmeye NTP trafiğine izin verecek şekilde yapılandırıldığından emin emin olmanız gerekir. Çift yönlü NTP trafiğini izin verilmiyor (Bu işlev bir Windows etki alanı denetleyicisi sağlar), dahili bir NTP sunucusu kullanmanız gerekir. Cihazınızı saati eşitlemesi olamaz, bulut depolama sağlayıcısı ile iletişim kurmak mümkün olmayabilir.

Genel NTP sunucuları listesini görmek için Git [NTP sunucuları Web](https://support.ntp.org/bin/view/Servers/WebHome).

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Farklı bir saat diliminde cihaza dağıtılırsa ne olur?

Farklı bir saat diliminde cihaza dağıtılırsa, cihazın saat dilimini değiştirir. Yedekleme ilkelerini cihazın saat dilimini kullanmasını düşünüldüğünde yedekleme ilkelerini otomatik olarak yeni saat dilimine uygun olarak ayarlayın. Kullanıcı müdahalesi gerekli değildir.

## <a name="modify-dns-settings"></a>DNS ayarlarını değiştirme

Cihazınız, bulut depolama hizmeti sağlayıcısı ile iletişim kurmayı denediğinde bir DNS sunucusu kullanılır. Kullanım **ağ ayarları** dikey penceresinde görüntülemek ve yapılandırılan DNS ayarları değiştirmek için cihaz. 

![Ağ ayarlarını, DNS ayarları](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

Yüksek kullanılabilirlik için ilk cihaz dağıtımı sırasında hem birincil hem de ikincil DNS sunucularını yapılandırmak için gereklidir.

**Birincil DNS sunucusu** -ilk ilk kurulum sırasında birincil DNS sunucusunu belirtmek için StorSimple için Windows PowerShell kullanın. Windows PowerShell arabirimi üzerinden yalnızca birincil DNS sunucusunu yeniden yapılandırabilirsiniz. Kullanım `Set-HcsDNSClientServerAddress` cihazınızın birincil DNS sunucusunu değiştirmek için. Daha fazla bilgi için söz dizimi için Git [kümesi HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet'i.

**İkincil DNS sunucusu** - ikincil DNS sunucusunu değiştirmek için kullanın `Set-HcsDNSClientServerAddress` cihazın Windows PowerShell arabiriminde cmdlet'ini veya **ağ ayarları** Azure portalında StorSimple cihazınızın dikey penceresi.

Azure Portalı'nda ikincil DNS sunucusu değiştirmek için aşağıdaki adımları gerçekleştirin.

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Cihazlar listesinden seçin ve Cihazınızı tıklayın.

2. İçinde **ayarları** dikey penceresinde, Git **cihaz Ayarları > Ağ**. Bu açılır **ağ ayarları** dikey penceresi. Tıklayın **DNS ayarlarını** Döşe. İkincil DNS sunucusu IP adresini değiştirin.

    ![İkincil DNS sunucusu IP adresini değiştirme](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Komut çubuğundaki **Kaydet** onaylamanız istendiğinde tıklayın **Tamam**.

    ![Kaydet ve değişiklikleri onaylayın](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Ağ arabirimleri değiştirme

Dört biri 1 GbE olan ve biri iki 10 GbE altı cihaz ağ arabirimleri, cihazınız vardır. Bu arabirimler DATA 0 – veri 5 etiketlenir. DATA 2 ve DATA 3 10 GbE ağ arabirimleri ise 0, verileri 1, veri 4 ve 5 veri 1 GbE verilerdir.

Kullanım **ağ ayarları** her arabirim kullanılmak üzere yapılandırmak için dikey pencere.

![Ağ arabirimleri aracılığıyla ağ ayarlarını yapılandırma](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

Yüksek kullanılabilirlik sağlamak için Cihazınızda en az iki iSCSI arabirimleri ve bulut özellikli iki arabirim olmasını öneririz. Öneririz ancak bu kullanılmayan arabirimleri devre dışı bırakılmasını gerektirmez.

Her ağ arabirimi için aşağıdaki parametreler görüntülenir:

* **Hızı** – kullanıcı tarafından yapılandırılabilen bir parametre değil. DATA 2 ve DATA 3 10 GbE arabirimleri ise 0, verileri 1, veri 4 ve 5 veri her zaman 1 GbE verilerdir.
  
  > [!NOTE]
  > Hız ve çift yönlü her zaman otomatik-belirlenir. Jumbo çerçeveler desteklenmez.
  
* **Arabirim durumu** – bir arabirim etkin veya devre dışı. Etkinleştirilirse, cihazın arabirimini kullanma dener. Ağa bağlı ve kullanılan arabirimleri etkin olmasını öneririz. Kullanmadığınız tüm arabirimleri devre dışı bırakın.
* **Arabirim türü** – iSCSI trafiği bulut depolama trafiğinden yalıtmak bu parametreyi sağlar. Bu parametre, aşağıdakilerden biri olabilir:
  
  * **Etkin bulut** – etkin olduğunda, cihazın bu arabirimi bulutla iletişim kurmak için kullanır.
  * **iSCSI etkin** – etkin olduğunda, cihazın bu arabirimi iSCSI konaklarla iletişim kurmak için kullanır.
    
    İSCSI trafiği bulut depolama trafiğinden yalıtmak öneririz. Ayrıca, ana Cihazınızı aynı alt ağda içindeyse, bir ağ geçidi atayabilirsiniz gerekmez unutmayın; ancak konağınız Cihazınızı daha farklı bir alt ağ içinde ise, bir ağ geçidi atamanız gerekir.
* **IP adresi** – ağ arabirimlerinin yapılandırdığınızda sanal IP (VIP) yapılandırmanız gerekir. Bu, IPv4 veya IPv6 veya her ikisi de olabilir. IPv4 ve IPv6 adres ailelerinin cihaz ağ arabirimleri için desteklenir. Bir 32-bit IP adresi IPv4 kullanırken belirtin (*.xxx.xxx*), nokta-ondalık gösteriminde. IPv6 kullanırken, yalnızca 4 basamaklı ön eki girin ve 128-bit adresi bu ön ekini temel alarak, cihaz ağ arabirimi için otomatik olarak oluşturulur.
* **Alt ağ** – bu alt ağ maskesi için ifade eder ve Windows PowerShell arabirimi üzerinden yapılandırılır.
* **Ağ geçidi** – (alt ağ) aynı IP adresi alanı içinde olmayan düğümleriyle iletişim kurmayı denediğinde bu arabirim tarafından kullanılan varsayılan ağ geçidini budur. Varsayılan Ağ Geçidi Arabirimi aynı adres alanını (alt ağ) IP adresi alt ağ maskesi tarafından belirlenen şekilde olmalıdır.
* **IP adresi sabit** – yalnızca DATA 0 yapılandırırken Bu alan kullanılabilir arabirimi. Güncelleştirmeleri veya cihaz sorunlarını giderme gibi işlemler için doğrudan cihaz denetleyiciye bağlanmak gerekebilir. Sabit IP adresini, hem etkin hem de cihaz edilgen denetleyiciye erişmek için kullanılabilir.

> [!NOTE]
> * Düzgün çalışmasını sağlamak için arabirim hızı ve çift yönlü her cihaz arabiriminin bağlı olduğu anahtar üzerindeki doğrulayın. Anahtar arabirimleri ile ya da anlaşması gereken veya Gigabit Ethernet için yapılandırılması (1000 MB/sn) ve tam çift yönlü. Daha yavaş veya yarı çift yönlü işletim arabirimleri, performans sorunlarına neden olur.
> * Kesintileri ve kapalı kalma süresini en aza indirmek için her bir iSCSI ağ arabirimini cihazınızın bağlanma anahtar bağlantı noktalarını portfast etkinleştirmenizi öneririz. Bu işlem, ağ bağlantısı bir yük devretme durumunda hızlı bir şekilde kurulabilir garanti eder.

### <a name="configure-data-0"></a>Veri 0'ı yapılandırma

DATA 0 bulut varsayılan olarak etkindir. DATA 0 yapılandırırken iki sabit IP adreslerinin her denetleyici için bir yapılandırma gerekmez. Bu sabit IP adresleri cihaz denetleyicilerinin doğrudan erişmek için kullanılan ve cihazda düzgün çalışması çöp toplama için güncelleştirmeleri yüklediğinizde veya sorun giderme amacıyla denetleyicileri eriştiğinizde yararlıdır.

Veri 0 ayarları dikey penceresi aracılığıyla sabit IP denetleyicileri yeniden yapılandırabilirsiniz.

![Ağ arabirimi - veri 0'ı yapılandırma](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> Denetleyici sabit IP adresleri ve alan geri kazanma algoritmalar (Çöp toplamada) cihaza güncelleştirmelerin uygulanması için düzgün çalışması için kullanılır. Bu nedenle, sabit IP’lerin yönlendirilebilir ve İnternet’e bağlanabilir olması gerekir.

### <a name="configure-data-1---data-5"></a>VERİLERİ 1 - 5 veri yapılandırma

VERİ için 1 - veri 5 ağ arabirimleri, aşağıdaki ekran görüntüsünde gösterildiği gibi tüm ağ ayarlarını yapılandırabilirsiniz:

![Ağ arabirimleri verileri 1 - 5 veri yapılandırın](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>Takas veya IP'ler yeniden atama

Herhangi bir ağ arabirimi denetleyicisi üzerinde kullanımda olan bir VIP (aynı cihaz veya ağdaki başka bir cihaz) atanır, şu anda, ardından denetleyici üzerinde başarısız olur. Değiştirme ya da cihaz ağ arabirimi için VIP'ler yeniden atama, yinelenen bir IP durum oluşturduğunuzda bir uygun bir yordamı izlemeniz gerekir.

Değiştirme veya herhangi bir ağ arabirimleri için VIP'ler yeniden atamak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-reassign-ips"></a>IP'ler yeniden atamak için

1. Her iki arabirimleri için IP adresi temizleyin.
2. IP adreslerini temizlendikten sonra ilgili arabirimler için yeni IP adresleri atayın.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [StorSimple cihazınız için MPIO yapılandırma](storsimple-8000-configure-mpio-windows-server.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

