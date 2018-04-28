---
title: Ağ Performansı İzleyicisi Azure ExpressRoute bağlantı hatları için yapılandırma | Microsoft Docs
description: Azure ExpressRoute bağlantı hatları için bulut tabanlı ağ izlemeyi yapılandırın.
documentationcenter: na
services: expressroute
author: ajaycode
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: agummadi
ms.openlocfilehash: 0d8bee936717a5668e16fbd66d416fcc4e738814
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-network-performance-monitor-for-expressroute"></a>Ağ Performansı İzleyicisi ExpressRoute için yapılandırma

Ağ Performans İzleyicisi'ni (NPM) izleme Azure bulut dağıtımları ve şirket içi konumlara (şube ofisleri, vb.) arasında bağlantı izler çözümü bir bulut tabanlı bir ağdır. NPM günlük analizi bir parçasıdır. NPM, özel eşleme kullanmak üzere yapılandırılmış ExpressRoute bağlantı hatları ağ performansını izlemenize izin verir ExpressRoute için artık bir uzantı sunar. ExpressRoute için NPM yapılandırma ağ sorunları belirlemek ve gidermek için algılayabilir.

Şunları yapabilirsiniz:

* Çeşitli Vnet'lerde kaybı ve gecikme izlemek ve uyarıları ayarlama

* Ağdaki tüm yolları (yedekli yollar dahil) izleme

* Çoğaltma zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme

* İçin performans sorumlu olduğu ağ üzerindeki belirli bir segmentle belirlemenize yardımcı olması

* Sanal ağ başına (her bir Vnet'teki yüklü aracılar varsa) Al

* Zaman içinde önceki bir noktaya ExpressRoute sistem durumundan bakın

## <a name="regions"></a>Desteklenen bölgeler

ExpressRoute bağlantı hatları dünya herhangi bir parçası olarak barındırılan bir çalışma alanı aşağıdaki bölgeler birini kullanarak izleyebilirsiniz:

* Batı Avrupa
* Batı Orta ABD
* Doğu ABD 
* Güneydoğu Asya 
* Güney Doğu Avustralya

>[!NOTE]
>ExpressRoute bağlantı hatları Azure kamu bulutta sanal ağlara bağlı izleme desteği için S2 2018 planlanmaktadır.   
>

## <a name="workflow"></a>İş akışı

İzleme aracıları birden fazla sunucuda, yüklü olduğu her iki şirket içi ve Azure. Aracıları birbirleri ile iletişim kurmak, ancak veri gönderme, TCP el sıkışma paketleri gönderir. Ağ topolojisi ve trafik alabilir yolu eşlemek Azure aracılar arasındaki iletişimi sağlar.

1. Bir NPM çalışma birinde oluşturmak [bölgeler desteklenen](#regions).
2. Yükleme ve yazılım aracıları yapılandırma: 
    * Şirket içi sunucular ve Azure sanal makinelerini izleme aracıları yükleyin.
    * İzleme aracılarıyla iletişim kurmak için izin vermek için izleme Aracısı sunucularında ayarlarını yapılandırın. (Güvenlik Duvarı bağlantı noktaları, vb. açın.)
3. İzleme Aracısı ile şirket içi iletişim kurmak için Azure vm'lerinde yüklü izin vermek için ağ güvenlik grubu (NSG) kurallarını yapılandırma izleme aracıları.
4. İzleme işlevini ayarlama: Otomatik bulma ve hangi ağların NPM içinde görünür yönetin.

Diğer nesneler veya hizmetlerini izlemek için Ağ Performansı İzleyicisi zaten kullanıyorsanız ve çalışma alanı desteklenen bölgelerinden birinde zaten varsa, adım 1 ve 2. adımı atlayın ve yapılandırmanızı 3. adım ile başlayın.

## <a name="configure"></a>1. adım: bir çalışma alanı oluşturma

ExpressRoute circuit(s) sanal ağlara bağlantı sahip Abonelikteki bir çalışma alanı oluşturun.

1. İçinde [Azure portal](https://portal.azure.com), sanal ağlar sahip aboneliği seçin, expressroute bağlantı hattı eşlenen. Hizmet listesini arama **Market** 'Ağ Performans İzleyicisi'. Return açmak için tıklatın **Ağ Performansı İzleyicisi** sayfası.

   >[!NOTE]
   >Yeni bir çalışma alanı oluşturun veya varolan bir çalışma alanını kullanın.  Varolan bir çalışma alanı kullanmak isterseniz, yeni sorgu dili çalışma geçirildiğinden emin olun gerekir. [Daha fazla bilgi...](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-upgrade)
   >

   ![portal](.\media\how-to-npm\3.png)<br><br>
2. Ana sonundaki **Ağ Performansı İzleyicisi** sayfasında, **oluşturma** açmak için **Ağ Performansı İzleyicisi - oluştur yeni çözüm** sayfası. Tıklatın **OMS çalışma alanı - çalışma alanı seçin** çalışma sayfasını açın. Tıklatın **+ oluştur yeni çalışma** çalışma sayfasını açın.
3. Üzerinde **OMS çalışma** sayfasında, **Yeni Oluştur** ve aşağıdaki ayarları yapılandırın:

  * OMS çalışma alanı - çalışma alanınız için bir ad yazın.
  * Abonelik - yeni çalışma alanı ile ilişkilendirmek istediğiniz seçin, birden çok aboneliğiniz varsa.
  * Kaynak grubu - bir kaynak grubu oluşturabilir veya mevcut bir kullanabilirsiniz.
  * Konumu - seçmelisiniz bir [bölge desteklenen](#regions).
  * Fiyatlandırma katmanı - seçin 'ücretsiz'
  
    >[!NOTE]
    >Expressroute bağlantı hattı dünyada herhangi bir yerde olabilir ve çalışma alanı ile aynı bölgede olması gerekmez.
    >
  
    ![Çalışma alanı](.\media\how-to-npm\4.png)<br><br>
4. Tıklatın **Tamam** kaydetmek ve ayarları şablonu dağıtmak için. Şablon doğrular sonra tıklayın **oluşturma** çalışma dağıtmak için.
5. Çalışma alanı dağıtıldıktan sonra gidin **NetworkMonitoring(name)** oluşturduğunuz kaynak. Ayarları doğrulayın ve ardından **çözüm ek yapılandırma gerektirir**.

   ![ek yapılandırma](.\media\how-to-npm\5.png)

## <a name="agents"></a>2. adım: Yükleme ve aracıları yapılandırma

### <a name="download"></a>2.1: Aracısı kurulum dosyasını karşıdan yükleyin

1. Git **ortak ayarları** sekmesinde **ağ Performans İzleyicisi'ni yapılandırma** kaynağınız için sayfa. Sunucunuzun işlemcisine karşılık gelen Aracısı'nı **OMS aracıları Yükle** bölümünde ve kurulum dosyasını karşıdan yükleyin.

 
2. Ardından, kopyalama **çalışma alanı kimliği** ve **birincil anahtar** Defteri'ne.
3. Gelen **TCP protokolü kullanılarak izleme için OMS aracıları yapılandırma** bölümünde, Powershell komut dosyasını karşıdan yükleyin. PowerShell Betiği TCP işlemler için ilgili güvenlik duvarı bağlantı noktasını açın yardımcı olur.

  ![PowerShell betiği](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: İzleme Aracısı her izleme sunucusuna (izlemek istediğiniz her bir VNET'teki) yükleyin

Artıklık için ExpressRoute bağlantısı (yani, şirket içi, Azure sanal ağlar) her tarafında en az iki aracı yüklemenizi öneririz. Aracıları yüklemek için aşağıdaki adımları kullanın:
  
  >[!NOTE]
  >Aracı bir Windows sunucusuna yüklenmesi gerekir (2008 SP1 veya üstü). Windows masaüstü işletim sistemi ve Linux işletim sistemi kullanarak ExpressRoute bağlantı hatları izleme desteklenmiyor. 
  >
  >
  
  >[!NOTE]
  >Aracıları SCOM tarafından gönderilen (içeren [MMA](https://technet.microsoft.com/library/dn465154(v=sc.12).aspx)) Azure üzerinde barındırılıyorsa tutarlı bir şekilde kendi konumunu algılayabilir olmayabilir.  Bu aracıları Azure Vnet'lerde ExpressRoute izlemek için kullanmamanızı öneririz.
  >
  >

1. Çalıştırma **Kurulum** ExpressRoute izleme için kullanmak istediğiniz her sunucuda aracıyı yüklemek için. İzleme için kullanacağınız sunucu bir VM ya da şirket içi ya da olabilir ve Internet erişimi olması gerekir. Azure'da izlemek istediğiniz her ağ kesimindeki en az bir aracı şirket içi ve bir aracı yüklemeniz gerekir.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.
3. Üzerinde **Lisans Koşulları'nı** sayfasında, lisans okuyun ve ardından **ediyorum**.
4. Üzerinde **hedef klasörü** sayfasında, değiştirmek veya varsayılan yükleme klasörünü ve ardından **sonraki**.
5. Üzerinde **aracı Kur Seçenekleri** sayfasında, Azure günlük analizi veya Operations Manager Aracısı bağlanmak seçebilirsiniz. Veya aracıyı daha sonra yapılandırmak istiyorsanız seçimleri boş bırakabilirsiniz. Selection(s) yaptıktan sonra tıklatın **sonraki**.

  * Bağlanmak seçerseniz **Azure günlük analizi**, yapıştırma **çalışma alanı kimliği** ve **çalışma alanı anahtarı** (birincil anahtar, önceki bölümde Defteri'ne kopyaladığınız). Ardından **İleri**'ye tıklayın.

    ![Kimliği ve anahtarı](.\media\how-to-npm\8.png)
  * Bağlanmak seçerseniz **Operations Manager**, **yönetim grubu Yapılandırması** sayfasında, **yönetim grubu adı**, **yönetim sunucusu** ve **yönetim sunucusu bağlantı noktası**. Ardından **İleri**'ye tıklayın.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * Üzerinde **aracı eylem hesabı** sayfasında, ya da **yerel sistem** hesabı veya **etki alanı veya yerel bilgisayar hesabı**. Ardından **İleri**'ye tıklayın.

    ![Hesap](.\media\how-to-npm\10.png)
6. Üzerinde **yüklemeye hazır** sayfasında, seçimlerinizi gözden geçirin ve ardından **yükleme**.
7. **Yapılandırma başarıyla tamamlandı** sayfasında **Son**'a tıklayın.
8. Tamamlandığında, Microsoft Monitoring Agent Denetim Masası'nda görünür. Vardır, yapılandırmanızı gözden geçirin ve aracıyı Azure günlük analizi (OMS) bağlı olduğunu doğrulayın. Bağlandığınızda, aracı belirten bir ileti görüntüler: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı**.

9. Lütfen bu izlenmesi gereken her sanal ağ için yineleyin.

### <a name="proxy"></a>2.3: (isteğe bağlı) proxy ayarlarını yapılandır

Internet'e erişmek için bir web proxy kullanıyorsanız, Microsoft İzleme Aracısı için proxy ayarlarını yapılandırmak için aşağıdaki adımları kullanın. Her sunucu için aşağıdaki adımları gerçekleştirin. Yapılandırmanız gereken birden çok sunucu olması durumunda, bu işlemi otomatikleştirmek için bir betik kullanmak sizin için daha kolay olabilir. Aksi takdirde bkz [Microsoft İzleme Aracısı bir betik kullanarak proxy ayarlarını yapılandırmak için](../log-analytics/log-analytics-windows-agent.md).

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
3. Tıklatın **Azure günlük analizi** sekmesi.
4. İçinde **durum** sütun, aracı için günlük analizi başarıyla bağlandı görmelisiniz.

  ![durum](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: İzleme Aracısı sunucular üzerinde güvenlik duvarı bağlantı noktalarını açın

TCP protokolünü kullanmak için izleme aracıları iletişim kurabildiğinden emin olmak için güvenlik duvarı bağlantı noktalarını açmanız gerekir.

İzleme birbirleri ile TCP bağlantıları oluşturmak için aracıları izin vermek için Windows Güvenlik duvarı kuralları oluşturma yanı sıra ağ performansı İzleyicisi tarafından gerekli kayıt defteri anahtarları oluşturur, bir PowerShell betiğini çalıştırabilirsiniz. Komut dosyası tarafından oluşturulan kayıt defteri anahtarlarını da oturum hata ayıklama günlükleri ve günlükleri dosyasının yolunu belirtin. Ayrıca, iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarları el ile değiştirmemelisiniz şekilde bu anahtarları için değerleri otomatik olarak komut dosyası tarafından ayarlanır.

Bağlantı noktası 8084 varsayılan olarak açılır. Komut dosyası için ' BağlantıNoktasıNumarası' parametresi sağlayarak, özel bir bağlantı noktası kullanabilirsiniz. Ancak, bunu yaparsanız, komut dosyasını çalıştırmak tüm sunucuların aynı bağlantı noktasını belirtmeniz gerekir.

>[!NOTE]
>'EnableRules' PowerShell komut dosyası komut çalıştırıldığı sunucuda Windows Güvenlik duvarı kuralları yapılandırır. Ağ güvenlik duvarı varsa, Ağ Performansı İzleyicisi tarafından kullanılan TCP bağlantı noktası için giden trafiğe izin verdiğinden emin olun.
>
>

Aracı sunucularda yönetici ayrıcalıklarıyla bir PowerShell penceresi açın. Çalıştırma [EnableRules](https://aka.ms/npmpowershellscript) (daha önce indirdiğiniz) PowerShell komut dosyası. Herhangi bir parametre kullanmayın.

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>3. adım: ağ güvenlik grubu kurallarını yapılandırma

Azure'da olan Aracısı sunucularını izlemek için ağ güvenlik grubu (NSG) kuralları tarafından NPM yapay işlemler için kullanılan bağlantı noktası TCP trafiğine izin verecek şekilde yapılandırmanız gerekir. Varsayılan bağlantı noktası 8084 ' dir. Bir şirket içi ile iletişim kurmak için Azure VM yüklü bir izleme aracı böylece İzleme Aracısı.

NSG hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

>[!NOTE]
>(Hem şirket içi sunucu Aracısı hem de Azure sunucu Aracısı) aracıları yüklü ve PowerShell Betiği devam etmeden önce bu adımda çalıştırdığınızdan emin olun.
>
>


## <a name="setupmonitor"></a>Adım 4: ExpressRoute izleme için NPM yapılandırma

Önceki bölümlerde tamamladıktan sonra izleme ayarlayabilirsiniz.

1. Ağ Performansı İzleyicisi'ne genel bakış döşemeye giderek gidin **tüm kaynakları** sayfasında ve Güvenilenler listesine NPM çalışma alanı'nı tıklatın.

  ![npm çalışma](.\media\how-to-npm\npm.png)
2. Tıklatın **Ağ Performansı İzleyicisi** Pano getirmek için genel bakış kutucuğu. Pano ExpressRoute 'yapılandırılmamış duruma' içinde olduğunu gösteren bir ExpressRoute sayfası içerir. Tıklatın **özelliği kurulum** ağ Performans İzleyicisi'ni yapılandırma sayfasını açın.

  ![özellik kurulumu](.\media\how-to-npm\npm2.png)
3. Yapılandırma sayfasında, sol tarafındaki Panoda bulunan 'ExpressRoute eşlemeler' sekmesine gidin. Tıklatın **Şimdi Bul**.

  ![bul](.\media\how-to-npm\13.png)
4. Bulma tamamlandığında benzersiz hattı adı ve sanal ağ adı için kurallar konusuna bakın. Başlangıçta, bu kuralları devre dışı bırakılır. Kuralları etkinleştirin, ardından izleme aracıları ve eşik değerleri seçin.

  ![rules](.\media\how-to-npm\14.png)
5. Kuralları etkinleştirme ve değerleri seçip izlemek istediğiniz aracıları sonra yaklaşık olarak 30-60 dakika doldurma başlamak değerleri için bekleme yoktur ve **ExpressRoute izleme** döşeme kullanılabilir hale gelir. İzleme döşeme gördüğünüzde, ExpressRoute bağlantı hatları ve bağlantı kaynaklarını NPM tarafından izlenen.

  ![Döşeme izleme](.\media\how-to-npm\15.png)

## <a name="explore"></a>5. adım: döşeme izleme görünümü

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

![filtreler](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Bir bağlantı hattının ayrıntılı topoloji görünümü

Bu görünüm, VNet bağlantıları gösterir.
![ayrıntılı topolojisi](.\media\how-to-npm\17.png)
