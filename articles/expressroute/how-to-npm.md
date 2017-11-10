---
title: "Ağ Performans İzleyicisi'ni (Önizleme) Azure ExpressRoute bağlantı hatları için yapılandırma | Microsoft Docs"
description: "NPM Azure ExpressRoute bağlantı hatları için yapılandırın. (Önizleme)"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35dd3c6be2fb2fa5ec4d14eefce1c16005210364
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-network-performance-monitor-for-expressroute-preview"></a>Ağ Performans İzleyicisi'ni (Önizleme) ExpressRoute için yapılandırma

Ağ Performans İzleyicisi'ni (NPM) izleme Azure bulut dağıtımları ve şirket içi konumlara (şube ofisleri, vb.) arasında bağlantı izler çözümü bir bulut tabanlı bir ağdır. NPM Microsoft Operations Management Suite (OMS) bir parçasıdır. NPM, özel eşleme kullanmak üzere yapılandırılmış ExpressRoute bağlantı hatları ağ performansını izlemenize izin verir ExpressRoute için artık bir uzantı sunar. ExpressRoute için NPM yapılandırma ağ sorunları belirlemek ve gidermek için algılayabilir.

Şunları yapabilirsiniz:

* Çeşitli Vnet'lerde kaybı ve gecikme izlemek ve uyarıları ayarlama

* Ağdaki tüm yolları (yedekli yollar dahil) izleme

* Çoğaltma zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme

* İçin performans sorumlu olduğu ağ üzerindeki belirli bir segmentle belirlemenize yardımcı olması

* Sanal ağ başına (her bir Vnet'teki yüklü aracılar varsa) Al

* Zaman içinde önceki bir noktaya ExpressRoute sistem durumundan bakın

**Nasıl çalışır?**

İzleme aracıları birden fazla sunucuda, yüklü olduğu her iki şirket içi ve Azure. Aracıları birbirleri ile iletişim kurmak, ancak veri gönderme, TCP el sıkışma paketleri gönderir. Ağ topolojisi ve trafik alabilir yolu eşlemek Azure aracılar arasındaki iletişimi sağlar.

**İş akışı**

1. Batı Orta ABD bölgesinde bir NPM çalışma alanı oluşturun. Şu anda bu Önizleme burada desteklenen tek bölge budur.
2. Yükleme ve yazılım aracıları yapılandırma: 
    * Şirket içi sunucular ve Azure sanal makinelerini izleme aracıları yükleyin.
    * İzleme aracılarıyla iletişim kurmak için izin vermek için izleme Aracısı sunucularında ayarlarını yapılandırın. (Güvenlik Duvarı bağlantı noktaları, vb. açın.)
3. İzleme Aracısı ile şirket içi iletişim kurmak için Azure vm'lerinde yüklü izin vermek için ağ güvenlik grubu (NSG) kurallarını yapılandırma izleme aracıları.
4. Beyaz liste ile NPM çalışma alanınızı isteği
5. İzleme işlevini ayarlama: Otomatik bulma ve hangi ağların NPM içinde görünür yönetin.

Diğer nesneler veya hizmetlerini izlemek için Ağ Performansı İzleyicisi zaten kullanıyorsanız ve Batı Orta ABD Workspace'te zaten varsa, adım 1 ve 2. adımı atlayın ve yapılandırmanızı 3. adım ile başlayın.

## <a name="configure"></a>1. adım: bir çalışma alanı oluşturma

1. İçinde [Azure portal](https://portal.azure.com), hizmet listesini arama **Market** 'Ağ Performans İzleyicisi'. Return açmak için tıklatın **Ağ Performansı İzleyicisi** sayfası.

  ![portal](.\media\how-to-npm\3.png)<br><br>
2. Ana sonundaki **Ağ Performansı İzleyicisi** sayfasında, **oluşturma** açmak için **Ağ Performansı İzleyicisi - oluştur yeni çözüm** sayfası. Tıklatın **OMS çalışma alanı - çalışma alanı seçin** çalışma sayfasını açın. Tıklatın **+ oluştur yeni çalışma** çalışma sayfasını açın.
3. Üzerinde **OMS çalışma** sayfasında, **Yeni Oluştur** ve aşağıdaki ayarları yapılandırın:

  * OMS çalışma alanı - çalışma alanınız için bir ad yazın.
  * Abonelik - yeni çalışma alanı ile ilişkilendirmek istediğiniz seçin, birden çok aboneliğiniz varsa.
  * Kaynak grubu - bir kaynak grubu oluşturabilir veya mevcut bir kullanabilirsiniz.
  * Konumu - Batı Orta ABD Bu önizleme için seçmeniz gerekir
  * Fiyatlandırma katmanı - seçin 'ücretsiz'

  ![Çalışma alanı](.\media\how-to-npm\4.png)<br><br>
4. Tıklatın **Tamam** kaydetmek ve ayarları şablonu dağıtmak için. Şablon doğrular sonra tıklayın **oluşturma** çalışma dağıtmak için.
5. Çalışma alanı dağıtıldıktan sonra gidin **NetworkMonitoring(name)** oluşturduğunuz kaynak. Ayarları doğrulayın ve ardından **çözüm ek yapılandırma gerektirir**.

  ![ek yapılandırma](.\media\how-to-npm\5.png)
6. Üzerinde **ağ Performans İzleyicisi'na Hoş** sayfasında, **yapay işlemler için kullanım TCP**, ardından **gönderme**. TCP işlemler yapmak ve bağlantıyı kesmek için kullanılır. Hiçbir veri bu TCP bağlantıları üzerinden gönderilir.

  ![Yapay işlemler için TCP](.\media\how-to-npm\6.png)

## <a name="agents"></a>2. adım: Yükleme ve aracıları yapılandırma

### <a name="download"></a>2.1: Aracısı kurulum dosyasını karşıdan yükleyin

1. Üzerinde **ağ Performans İzleyicisi'ni yapılandırma - TCP Kurulum sayfasında** , kaynak için de **OMS aracıları Yükle** bölümünde, sunucunuzun işlemci ve indirme karşılık gelen Aracısı'nı tıklatın Kurulum dosyasını çalıştırın.

  >[!NOTE]
  >ExpressRoute için Linux Aracısı şu anda desteklenmiyor izleme.
  >
  >
2. Ardından, kopyalama **çalışma alanı kimliği** ve **birincil anahtar** Defteri'ne.
3. İçinde **yapılandırma aracıları** bölümünde, Powershell komut dosyasını karşıdan yükleyin. PowerShell Betiği TCP işlemler için ilgili güvenlik duvarı bağlantı noktasını açın yardımcı olur.

  ![PowerShell betiği](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: İzleme Aracısı her izleme sunucusuna yükleyin

1. Çalıştırma **Kurulum** ExpressRoute izleme için kullanmak istediğiniz her sunucuda aracıyı yüklemek için. İzleme için kullanacağınız sunucu bir VM ya da şirket içi ya da olabilir ve Internet erişimi olması gerekir. Azure'da izlemek istediğiniz her ağ kesimindeki en az bir aracı şirket içi ve bir aracı yüklemeniz gerekir.
2. Üzerinde **Hoş Geldiniz** sayfasında, **sonraki**.
3. Üzerinde **Lisans Koşulları'nı** sayfasında, lisans okuyun ve ardından **ediyorum**.
4. Üzerinde **hedef klasörü** sayfasında, değiştirmek veya varsayılan yükleme klasörünü ve ardından **sonraki**.
5. Üzerinde **aracı Kur Seçenekleri** sayfasında, Azure günlük analizi (OMS) veya Operations Manager Aracısı bağlanmak seçebilirsiniz. Veya aracıyı daha sonra yapılandırmak istiyorsanız seçimleri boş bırakabilirsiniz. Selection(s) yaptıktan sonra tıklatın **sonraki**.

  * Bağlanmak seçerseniz **Azure günlük analizi (OMS)**, yapıştırma **çalışma alanı kimliği** ve **çalışma alanı anahtarı** (birincil anahtar, önceki bölümde Defteri'ne kopyaladığınız). Ardından **İleri**'ye tıklayın.

    ![Kimliği ve anahtarı](.\media\how-to-npm\8.png)
  * Bağlanmak seçerseniz **Operations Manager**, **yönetim grubu Yapılandırması** sayfasında, **yönetim grubu adı**, **yönetim sunucusu** ve **yönetim sunucusu bağlantı noktası**. Ardından **İleri**'ye tıklayın.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * Üzerinde **aracı eylem hesabı** sayfasında, ya da **yerel sistem** hesabı veya **etki alanı veya yerel bilgisayar hesabı**. Ardından **İleri**'ye tıklayın.

    ![Hesap](.\media\how-to-npm\10.png)
6. Üzerinde **yüklemeye hazır** sayfasında, seçimlerinizi gözden geçirin ve ardından **yükleme**.
7. Üzerinde **yapılandırması başarıyla tamamlandı** sayfasında, **son**.
8. Tamamlandığında, Microsoft Monitoring Agent Denetim Masası'nda görünür. Vardır, yapılandırmanızı gözden geçirin ve aracı Operational Insights (OMS için) bağlı olduğunu doğrulayın. Bildiren bir ileti aracısı için OMS bağlıyken görüntüler: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı**.

### <a name="proxy"></a>2.3: (isteğe bağlı) proxy ayarlarını yapılandır

Internet'e erişmek için bir web proxy kullanıyorsanız, Microsoft İzleme Aracısı için proxy ayarlarını yapılandırmak için aşağıdaki adımları kullanın. Her sunucu için aşağıdaki adımları gerçekleştirin. Yapılandırmanız gereken birden çok sunucu olması durumunda, bu işlemi otomatikleştirmek için bir betik kullanmak sizin için daha kolay olabilir. Aksi takdirde bkz [Microsoft İzleme Aracısı bir betik kullanarak proxy ayarlarını yapılandırmak için](../log-analytics/log-analytics-windows-agents.md#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

Microsoft Monitoring Agent Denetim Masası'nı kullanarak proxy ayarlarını yapılandırmak için:

1. Açık **Denetim Masası**.
2. **Microsoft İzleme Aracısı**'nı açın.
3. **Ara Sunucu Ayarları** sekmesine tıklayın.
4. Seçin **bir proxy sunucu kullanacak** URL'sini yazın ve bağlantı noktası numarası, gerektiğinde. Ara sunucunuz kimlik doğrulaması gerektiriyorsa ara sunucuya erişmek için kullanıcı adını ve parolayı yazın.

  ![Proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4: Aracısı bağlanabildiğini doğrulayın

Kolayca aracılarınızı iletişim kuran olup olmadığını doğrulayın.

1. İzleme Aracısı ile bir sunucu üzerinde açın **Denetim Masası**.
2. Açık **Microsoft Monitoring Agent**.
3. Tıklatın **Azure günlük analizi (OMS)** sekmesi.
4. İçinde **durum** sütun, aracı Operations Management Suite hizmetine başarıyla bağlandı görmelisiniz.

  ![durum](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: İzleme Aracısı sunucular üzerinde güvenlik duvarı bağlantı noktalarını açın

TCP protokolünü kullanmak için izleme aracıları iletişim kurabildiğinden emin olmak için güvenlik duvarı bağlantı noktalarını açmanız gerekir.

İzleme birbirleri ile TCP bağlantıları oluşturmak için aracıları izin vermek için Windows Güvenlik duvarı kuralları oluşturma yanı sıra ağ performansı İzleyicisi tarafından gerekli kayıt defteri anahtarları oluşturur, bir PowerShell betiğini çalıştırabilirsiniz. Komut dosyası tarafından oluşturulan kayıt defteri anahtarlarını da oturum hata ayıklama günlükleri ve günlükleri dosyasının yolunu belirtin. Ayrıca, iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarları el ile değiştirmemelisiniz şekilde bu anahtarları için değerleri otomatik olarak komut dosyası tarafından ayarlanır.

Bağlantı noktası 8084 varsayılan olarak açılır. Komut dosyası için ' BağlantıNoktasıNumarası' parametresi sağlayarak, özel bir bağlantı noktası kullanabilirsiniz. Ancak, bunu yaparsanız, komut dosyasını çalıştırmak tüm sunucuların aynı bağlantı noktasını belirtmeniz gerekir.

>[!NOTE]
>'EnableRules' PowerShell komut dosyası komut çalıştırıldığı sunucuda Windows Güvenlik duvarı kuralları yapılandırır. Ağ güvenlik duvarı varsa, Ağ Performansı İzleyicisi tarafından kullanılan TCP bağlantı noktası için giden trafiğe izin verdiğinden emin olun.
>
>

Aracı sunucularda yönetici ayrıcalıklarıyla bir PowerShell penceresi açın. Çalıştırma [EnableRules](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) (daha önce indirdiğiniz) PowerShell komut dosyası. Herhangi bir parametre kullanmayın.

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>3. adım: ağ güvenlik grubu kurallarını yapılandırma

Azure'da olan Aracısı sunucularını izlemek için ağ güvenlik grubu (NSG) kuralları tarafından NPM yapay işlemler için kullanılan bağlantı noktası TCP trafiğine izin verecek şekilde yapılandırmanız gerekir. Varsayılan bağlantı noktası 8084 ' dir. Bir şirket içi ile iletişim kurmak için Azure VM yüklü bir izleme aracı böylece İzleme Aracısı.

NSG hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

## <a name="whitelist"></a>4. adım: İsteği güvenilir listeye çalışma

>[!NOTE]
>(Hem şirket içi sunucu Aracısı hem de Azure sunucu Aracısı) aracıları yüklü ve PowerShell Betiği devam etmeden önce bu adımda çalıştırdığınızdan emin olun.
>
>

NPM ExpressRoute izleme özelliğini kullanmaya başlamadan önce çalışma alanı Güvenilenler listesine sahip istemeniz gerekir. [Sayfasına gidin ve istek formu doldurmak için burayı tıklatın](https://go.microsoft.com/fwlink/?linkid=862263). (İpucu: bir yeni penceresinde veya sekmesinde bu bağlantıyı açmak istediğiniz). Uygulamaları güvenilir listeye almayı işlemi, bir iş günü veya daha fazla sürebilir. Uygulamaları güvenilir listeye almayı tamamlandıktan sonra bir e-posta alacaksınız.

## <a name="setupmonitor"></a>Adım 5: ExpressRoute izleme için NPM yapılandırma

>[!WARNING]
>Daha fazla çalışma alanınızı Güvenilenler listesine bırakıldı ve bir onay e-postası kadar devam etmeyin.
>
>

Önceki bölümlerde tamamlamak ve Güvenilenler listesine silinmiş doğruladıktan sonra izleme ayarlayabilirsiniz.

1. Ağ Performansı İzleyicisi'ne genel bakış döşemeye giderek gidin **tüm kaynakları** sayfasında ve Güvenilenler listesine NPM çalışma alanı'nı tıklatın.

  ![npm çalışma](.\media\how-to-npm\npm.png)
2. Tıklatın **Ağ Performansı İzleyicisi** Pano getirmek için genel bakış kutucuğu. Pano ExpressRoute 'yapılandırılmamış duruma' içinde olduğunu gösteren bir ExpressRoute sayfası içerir. Tıklatın **özelliği kurulum** ağ Performans İzleyicisi'ni yapılandırma sayfasını açın.

  ![özellik kurulumu](.\media\how-to-npm\npm2.png)
3. Yapılandırma sayfasında, sol tarafındaki Panoda bulunan 'ExpressRoute eşlemeler' sekmesine gidin. Tıklatın **Şimdi Bul**.

  ![Bul](.\media\how-to-npm\13.png)
4. Bulma tamamlandığında benzersiz hattı adı ve sanal ağ adı için kurallar konusuna bakın. Başlangıçta, bu kuralları devre dışı bırakılır. Kuralları etkinleştirin, ardından izleme aracıları ve eşik değerleri seçin.

  ![rules](.\media\how-to-npm\14.png)
5. Kuralları etkinleştirme ve değerleri seçip izlemek istediğiniz aracıları sonra yaklaşık olarak 30-60 dakika doldurma başlamak değerleri için bekleme yoktur ve **ExpressRoute izleme** döşeme kullanılabilir hale gelir. İzleme döşeme gördüğünüzde, ExpressRoute bağlantı hatları ve bağlantı kaynaklarını NPM tarafından izlenen.

  ![Döşeme izleme](.\media\how-to-npm\15.png)

## <a name="explore"></a>6. adım: döşeme izleme görünümü

### <a name="dashboard"></a>Ağ Performans İzleyicisi sayfası

NPM sayfa ExpressRoute bağlantı hatları ve eşlemeler sistem durumu özetini gösterir ExpressRoute için bir sayfa içerir.

  ![Pano](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>Bağlantı hatları listesi

Tüm izlenen ExpressRoute bağlantı hatları listesini görmek için tıklayın **ExpressRoute bağlantı hatları** döşeme. Bir bağlantı hattı seçin ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin. Grafikler etkileşimlidir. Grafikler çizdirmek için bir özel zaman penceresi seçebilirsiniz. Yakınlaştırma ve hassas veri noktaları görmek için grafikte bir alanın üzerinde fare sürükleyebilirsiniz.

  ![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>Kaybı, gecikme ve verimlilik eğilimi

Bant genişliği, gecikme ve kaybı grafikleri etkileşimlidir. Fare denetimlerini kullanarak bu grafikler herhangi bir bölüme yakınlaştırabilirsiniz. Tıklayarak diğer aralıkları için bant genişliği, gecikme ve kaybı verileri görebiliyor **tarih**, sol üst Eylemler düğmeyi aşağıda bulunan.

![Eğilim](.\media\how-to-npm\16.png)

### <a name="peerings"></a>Eşlemeleri listesi

Tıklayarak **özel eşlemeler** panosundaki bölümünden getirir sanal ağlar için tüm bağlantıların listesi özel eşleme üzerinden. Burada, bir sanal seçebilirsiniz ağ bağlantısı ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin.

  ![bağlantı hattı listesi](.\media\how-to-npm\peerings.png)

### <a name="topology"></a>Bağlantı hattı topolojisi

Bağlantı hattı topolojisini görüntülemek için tıklatın **topoloji** döşeme. Bu seçili topoloji görünümü sürer hattı veya eşliği. Topoloji diyagramı ağdaki her segment için gecikme süresini sağlar ve her katman 3 atlama diyagramın bir düğümle temsil edilir. Atlama tıklatarak atlama hakkında daha fazla ayrıntı ortaya çıkarır.
Şirket içi atlama çubuğu aşağıdaki taşıyarak dahil etmek için görünürlük düzeyini artırabilirsiniz **filtreleri**. Kaydırıcı çubuğun sola veya sağa hareket, artar/topoloji grafikte durak sayısını azaltır. Her segment arasında gecikme yüksek gecikme kesimleri ağınızdaki daha hızlı yalıtım sağlayan, görülebilir.

![filtreleri](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Bir bağlantı hattının ayrıntılı topoloji görünümü

Bu görünüm, VNet bağlantıları gösterir.
![ayrıntılı topolojisi](.\media\how-to-npm\17.png)
