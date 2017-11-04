---
title: "StorSimple 8000 serisi aygıt yapılandırmasını değiştirme | Microsoft Docs"
description: "StorSimple cihaz Yöneticisi hizmeti zaten dağıtılmış bir StorSimple cihazı yeniden yapılandırmak için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 13ff24c24a881297775fa5f65821e53ceb83c351
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple cihaz yapılandırmasını değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Azure portalı **aygıt ayarları** bölümüne **ayarları** dikey StorSimple cihaz Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazda yapılandırabilirsiniz tüm aygıt parametreleri içerir . Bu öğretici nasıl kullanabileceğinizi açıklar **ayarları** aşağıdaki cihaz düzeyinde görevleri gerçekleştirmek için dikey penceresinde:

* Cihaz kolay adı değiştir
* Aygıt saat ayarlarını değiştirme
* İkincil DNS atayın
* Ağ arabirimleri değiştirme
* Değiştirme veya IP'leri yeniden atama

## <a name="modify-device-friendly-name"></a>Cihaz kolay adı değiştir

Aygıt adını değiştirin ve tercih ettiğiniz benzersiz bir kolay ad atamak için Azure portalını kullanabilirsiniz. Kullanım **genel ayarları** dikey cihaz kolay adını değiştirmek için Cihazınızda. Kolay ad, herhangi bir karakter içerebilir ve en fazla 64 karakter uzunluğunda olabilir.

> [!NOTE] 
> Cihaz kurulumu tamamlanmadan önce yalnızca Azure portalında aygıt adı değiştirin. Minimum cihaz kurulumu tamamlandıktan sonra cihaz adını değiştiremezsiniz.

![Aygıt adı genel ayarları](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

StorSimple cihaz Yöneticisi hizmetine bağlı bir StorSimple cihazına bir varsayılan ad atanır. Varsayılan adı cihazın seri numarası genellikle yansıtır. Örneğin, 15 karakter uzunluğunda 8600-SHX0991003G44HT gibi bir varsayılan cihaz adı aşağıdaki gösterir:

* **8600** – cihaz modeli gösterir.
* **SHX** – üretim sitesini gösterir.
* **0991003** -belirli bir ürün gösterir.
* **G44HT**-benzersiz seri numaraları oluşturmak için en son 5 rakamlı artırılır. Bu ardışık olmayabilir.

## <a name="modify-device-description"></a>Aygıt açıklaması değiştirme

Kullanım **genel ayarları** dikey penceresinde Aygıt açıklaması değiştirmek için Cihazınızda.

![Genel Ayarları'nda Aygıt açıklaması](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

Aygıt açıklaması genellikle sahibi ve cihazın fiziksel konumunu belirlemenize yardımcı olur. Açıklama alanı, 256'dan az karakter içermelidir.

## <a name="modify-time-settings"></a>Saat ayarlarını değiştirme

Cihazınızı bulut depolama hizmeti sağlayıcısı ile kimlik doğrulamak için zaman eşitlemeniz gerekir. Kullanım **genel ayarları** dikey aygıt saat ayarlarını değiştirmek için Cihazınızda.

![Genel Ayarları'nda Aygıt açıklaması](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Saat dilimini aşağı açılan listeden seçin. En fazla iki ağ zaman Protokolü (NTP) sunucuları belirtebilirsiniz:

 - **Birincil NTP sunucusu** -yapılandırma gereklidir ve Cihazınızı yapılandırmak için StorSimple için Windows PowerShell'i kullandığınızda belirtilir. Windows Server varsayılan belirtebilirsiniz **time.windows.com** NTP sunucusu olarak. Azure portalı üzerinden birincil NTP sunucusu yapılandırma görüntüleyebilirsiniz, ancak değiştirmek için Windows PowerShell arabirimini kullanmanız gerekir. Kullanım `Set-HcsNTPClientServerAddress` aygıtınızın birincil NTP sunucusunu değiştirmek için cmdlet. Daha fazla bilgi için [Set-HcsNTPClientServerAddress] (https://technet.microsoft.com/library/dn688138.aspx) cmdlet synxtax gidin.

- **İkincil NTP sunucusu** -isteğe bağlı bir yapılandırmadır. Portal, ikincil bir NTP sunucusunu yapılandırmak için kullanabilirsiniz.

NTP sunucusu yapılandırırken, ağınızı merkeziniz Internet'e geçirmek NTP trafiğini verdiğinden emin olun. Bir ortak NTP sunucusu belirtirken, ağ güvenlik duvarları ve diğer güvenlik aygıtlarını Ağ dışından gelen ve giden NTP trafiğin izin verecek şekilde yapılandırıldığından emin olmalısınız. Çift yönlü NTP trafiğini izin verilmiyor (Bu işlev bir Windows etki alanı denetleyicisi sunar) bir iç NTP sunucusu kullanmanız gerekir. Cihazınızı zaman eşitleyemez, bulut depolama sağlayıcısı ile iletişim kurabildiğinden olmayabilir.

Ortak NTP sunucularının bir listesini görmek için Git [NTP sunucuları Web](http://support.ntp.org/bin/view/Servers/WebHome).

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Aygıtı farklı bir saat diliminde dağıtılırsa ne olur?

Aygıtı farklı bir saat diliminde dağıtılırsa, aygıt saat dilimini değiştirir. O yedekleme ilkelerini aygıt saat dilimini kullanır, yedekleme ilkelerini otomatik olarak yeni saat dilimine göre ayarlanır. Kullanıcı müdahalesi gerekli değildir.

## <a name="modify-dns-settings"></a>DNS ayarlarını değiştirme

Cihazınızı bulut depolama hizmeti sağlayıcısı ile iletişim kurmayı denediğinde bir DNS sunucusu kullanılır. Kullanım **ağ ayarlarını** dikey penceresinde görüntülemek ve yapılandırılmış DNS ayarlarını değiştirmek için Cihazınızda. 

![Ağ Ayarları'nda DNS ayarları](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

Yüksek kullanılabilirlik için hem birincil hem de ikincil DNS sunucuları ilk cihaz dağıtımı sırasında yapılandırmak için gereklidir.

**Birincil DNS sunucusu** -ilk birincil DNS sunucusunu ilk kurulum sırasında belirtmek için StorSimple için Windows PowerShell kullanın. Yalnızca Windows PowerShell arabirimi üzerinden birincil DNS sunucusunu yeniden yapılandırabilirsiniz. Kullanım `Set-HcsDNSClientServerAddress` Cihazınızı birincil DNS sunucusunu değiştirmek için cmdlet'ini. Synxtax için daha fazla bilgi için Git [kümesi HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet'i.

**İkincil DNS sunucusu** - ikincil DNS sunucusunu değiştirmek kullanmak için `Set-HcsDNSClientServerAddress` cihazın Windows PowerShell arabiriminde cmdlet'ini veya **ağ ayarlarını** StorSimple Cihazınızı Azure portalında dikey.

Azure portalında ikincil DNS sunucusunu değiştirmek için aşağıdaki adımları gerçekleştirin.

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Aygıtları listesinden seçin ve aygıtınızı tıklatın.

2. İçinde **ayarları** dikey penceresinde, Git **Aygıt Ayarları > Ağ**. Bu açılır **ağ ayarlarını** dikey. Tıklatın **DNS ayarlarını** döşeme. İkincil DNS sunucusu IP adresini değiştirin.

    ![İkincil DNS sunucusu IP adderss değiştirme](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Komut çubuğundaki **kaydetmek** tıklatıp onaylamanız istendiğinde, **Tamam**.

    ![Kaydet ve değişiklikleri onaylayın](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Ağ arabirimleri değiştirme

Cihazınızı dört biri 1 GbE ve ikisi de 10 GbE altı aygıt, ağ arabirimleri vardır. Bu arabirimleri 0 – veri 5 verileri olarak etiketlenir. Veri 2 ve veri 3 10 GbE ağ arabirimleri ise 0, 1 DATA, veri 4 ve verileri 5 1 GbE verilerdir.

Kullanım **ağ ayarlarını** dikey her kullanılacak arabirim yapılandırmak için.

![Ağ arabirimleri aracılığıyla ağ ayarlarını yapılandırma](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

Yüksek kullanılabilirlik sağlamak için Cihazınızda en az iki iSCSI arabirimleri ve iki bulut etkin arabirimlere sahip öneririz. Size önerilir ancak kullanılmayan arabirimleri devre dışı bırakılmasını gerektirmez.

Her ağ arabirimi için aşağıdaki parametreleri görüntülenir:

* **Hızı** – kullanıcı tarafından yapılandırılabilir bir parametre değil. Veri 2 ve veri 3 10 GbE arabirimleri ise 0, 1 DATA, veri 4 ve verileri 5 her zaman 1 GbE verilerdir.
  
  > [!NOTE]
  > Hız ve çift yönlü her zaman otomatik-belirlenir. Jumbo çerçeveler desteklenmez.
  
* **Arabirim durumu** – bir arabirim etkin veya devre dışı. Etkinleştirilirse, aygıt arabirimi kullanmayı dener. Ağa bağlı ve kullanılan arabirimleri etkinleştirilmesi önerilir. Kullanmadığınız arabirimleri devre dışı bırakın.
* **Arabirim türü** – Bu parametre, bulut depolama trafiğinden iSCSI trafiğini yalıtmak sağlar. Bu parametre, aşağıdakilerden biri olabilir:
  
  * **Bulut etkin** – etkin olduğunda, cihaz bu arabirim Bulutu ile iletişim kurmak için kullanır.
  * **iSCSI etkin** – etkin olduğunda, cihaz bu arabirim iSCSI ana bilgisayarı ile iletişim kurmak için kullanır.
    
    Bulut depolama trafiğinden iSCSI trafiğini yalıtmak öneririz. Ayrıca, ana bilgisayarınız Cihazınızı aynı alt ağda içindeyse, bir ağ geçidi atamak gerekmez unutmayın; Ancak, ana bilgisayarınız Cihazınızı daha farklı bir alt ağda ise, bir ağ geçidi atamanız gerekir.
* **IP adresi** – ağ arabirimlerinin yapılandırdığınızda, sanal IP (VIP) yapılandırmanız gerekir. Bu, IPv4 veya IPv6 ya da her ikisini de olabilir. IPv4 ve IPv6 adres ailesi için cihaz ağ arabirimleri desteklenir. IPv4 kullanırken, bir 32 bit IP adresi belirtin (*xxx.xxx.xxx.xxx*) noktalı ondalık gösterimde. IPv6 kullanırken, yalnızca 4 basamaklı önek sağlayın ve 128-bit adres bu ön ekini temel alarak, aygıt ağ arabirimi için otomatik olarak oluşturulur.
* **Alt ağ** – bu alt ağ maskesi başvuruyor ve Windows PowerShell arabirimi yapılandırılır.
* **Ağ geçidi** – (alt ağ) aynı IP adresi alanı içinde olmayan düğüm ile iletişim kurmayı denediğinde, bu arabirim tarafından kullanılan varsayılan ağ geçidi budur. Varsayılan Ağ Geçidi Arabirimi aynı adres alanında (alt ağ) IP adresi, alt ağ maskesi kullanılarak belirlenen olması gerekir.
* **IP adresi sabit** – yalnızca veri 0 yapılandırması sırasında bu alan kullanılabilir arabirimi. Güncelleştirmeleri veya cihaz sorunlarını giderme gibi işlemleri için doğrudan aygıt denetleyicisine bağlanmak gerekebilir. Sabit IP adresi, etkin ve aygıtınızda pasif denetleyiciyi erişmek için kullanılabilir.

> [!NOTE]
> * Düzgün çalışmasını sağlamak için arabirim hızı ve çift yönlü her aygıt arabiriminin bağlı olduğu anahtar üzerindeki doğrulayın. Anahtar arabirimleri ile ya da anlaşması gereken veya Gigabit Ethernet için yapılandırılması (1000 MB/sn) ve tam çift yönlü. İşletim yavaş hızlarda veya yarı çift yönlü arabirimleri performans sorunlarına neden olur.
> * Kesinti ve kapalı kalma süresini en aza indirmek için her Cihazınızı iSCSI ağ arabiriminin bağlanma anahtar bağlantı noktalarını portfast etkinleştirmenizi öneririz. Bu, ağ bağlantısı hızlı bir şekilde yük devretmesi durumunda kurulması güvence altına alır.

### <a name="configure-data-0"></a>Veri 0 yapılandırın

Veri 0 bulut varsayılan olarak etkindir. Veri 0 yapılandırırken, iki sabit IP adresleri, her denetleyici için bir tane yapılandırmak için gereklidir. Bu sabit IP adresleri aygıt denetleyicileri doğrudan erişmek için kullanılan ve cihazda düzgün çalışması atık toplama için güncelleştirmeleri yüklediğinizde veya sorun giderme amacıyla denetleyicileri eriştiğinizde yararlı olur.

Veri 0 ayarlar dikey penceresi aracılığıyla sabit IP denetleyicileri yeniden yapılandırabilirsiniz.

![Ağ arabirimi - DATA 0 için yapılandırın](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> Denetleyicinin sabit IP adresleri, düzgün çalışması için güncelleştirmeleri aygıta Bakım ve alan geri kazanma algoritmalar (çöp toplama) için kullanılır. Bu nedenle, sabit IP’lerin yönlendirilebilir ve İnternet’e bağlanabilir olması gerekir.

### <a name="configure-data-1---data-5"></a>VERİLERİ 1 - 5 veri yapılandırma

VERİ için 1 - veri 5 ağ arabirimleri, aşağıdaki ekran görüntüsünde gösterildiği gibi tüm ağ ayarlarını yapılandırabilirsiniz:

![Ağ arabirimleri veri 1 - veri 5 yapılandırın](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>Değiştirme veya IP'leri yeniden atama

Herhangi bir ağ denetleyicisi arabirimde kullanımda olan bir VIP (aynı cihaz veya ağdaki başka bir aygıt) atanır. şu anda, ardından denetleyici üzerinden başarısız olur. Değiştirme veya bir aygıt ağ arabirimi için VIP'ler yeniden atadığınızda, yinelenen bir IP durum oluşturabilirsiniz gibi uygun bir yordamı izlemeniz gerekir.

Değiştirme veya herhangi bir ağ arabirimleri için VIP'ler yeniden atamak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-reassign-ips"></a>IP'leri yeniden atamak için

1. Her iki arabirimleri için IP adresi temizleyin.
2. IP adreslerini temizlendikten sonra ilgili arabirimlerine yeni IP adresleri atayın.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [StorSimple cihazınız için MPIO yapılandırma](storsimple-8000-configure-mpio-windows-server.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

