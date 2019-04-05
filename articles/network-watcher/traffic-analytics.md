---
title: Azure trafik analizi | Microsoft Docs
description: Trafik Analizi ile Azure ağ güvenlik grubu akış günlüklerini analiz etmeyi öğrenin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: yagup;jdial
ms.openlocfilehash: f00c816f34978ee2f14f16ee9882860750d0e658
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051895"
---
# <a name="traffic-analytics"></a>Trafik Analizi

Trafik analizi, bulut ağlarındaki kullanıcı ve uygulama etkinliğiniz görünürlük sağlayan bir bulut tabanlı bir çözümdür. Trafik analizi, Azure bulut trafik akışını Öngörüler sağlamak için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlüklerini analiz eder. Trafik Analizi ile şunları yapabilirsiniz:

- Azure aboneliklerinizde ağ etkinliğini Görselleştirme ve etkin noktaları belirleyin.
- Güvenlik tehditleri belirleyin ve açık bağlantı noktaları, internet erişimi ve sanal ağları standart dışı bağlanmak makineleri (VM) çalışan uygulamalar gibi bilgileri ile ağınızın güvenliğini sağlayın.
- Azure bölgeleri ve performans ve kapasite kendi ağ dağıtımınıza en iyi duruma getirmek için internet üzerinden trafik akış desenlerini anlayın.
- Ağınızda başarısız bağlantılar için önde gelen ağ yanlış yapılandırmalarını saptayın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="why-traffic-analytics"></a>Neden analiz trafiği?

İzleme, yönetme ve kendi ağ güvenliği aşılmamış güvenlik, uyumluluk ve performans için bilmeniz önemlidir. Kendi ortamınızda bilerek korumak ve bunu en iyi duruma getirmek için en önemli çok önemlidir. Genellikle bağlanan ağ geçerli durumunu bilmeniz gerekir, nerede bunlar, bağlantı noktalarını bağlanmakta olduğunuz açık internet'e, beklenen ağ davranışı, düzensiz ağ davranışı ve ani trafik yükseldiğinde.

Bulut ağlarındaki netflow veya eşdeğer Protokolü özellikli yönlendiriciler ve girer veya bir ağ arabirimi çıkar IP ağ trafiğini toplama yeteneği sağlayan anahtarları, sahip olduğu şirket içi Kurumsal ağlara farklı. Trafik akışı verileri analiz ederek, ağ trafiği akışını ve birim analizini oluşturabilirsiniz.

Azure sanal ağları hakkında giriş bilgilerini NSG akış günlüklerini sahip ve tek tek ağ arabirimleri, VM'ler veya alt ağlara mı çıkış IP trafiği bir ağ güvenlik grubu ile ilişkili. Ham NSG akış günlüklerini analiz ve zeka güvenlik, topoloji ve Coğrafya, trafiğin ekleme analytics, trafik akışını, ortamınızdaki Öngörüler sağlayabilirsiniz. Trafik analizi en çok iletişim kuran konaklar, en çok iletişim kuran uygulama protokolleri, konak en görüşme çiftleri, izin verilen/engellenen trafik, gelen/giden trafik, internet bağlantı noktalarını açma, en engelleme kuralları, trafiği gibi bilgiler sağlar Azure veri merkezi, sanal ağ, alt ağlar, her dağıtım veya standart dışı, ağlar.

## <a name="key-components"></a>Başlıca bileşenler

- **Ağ güvenlik grubu (NSG)**: İzin veren veya bir Azure sanal ağa bağlı kaynaklara ağ trafiği reddeden güvenlik kurallarının bir listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager). Daha fazla bilgi için [ağ güvenlik grubu genel bakış](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ güvenlik grubu (NSG) akış günlüklerini**: Bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiğini bilgilerini görüntülemenize olanak sağlar. NSG akış günlükleri json biçiminde yazılır ve akış NIC uygulandığı bir kural başına temelinde giden ve gelen akışlar Göster, 5 demet bilgi (kaynak/hedef IP adresi, kaynak/hedef bağlantı noktası ve protokol) akışla ilgili ve trafiğe izin verildi veya reddedildi. NSG akış günlükleri hakkında daha fazla bilgi için bkz: [NSG akış günlüklerini](network-watcher-nsg-flow-logging-overview.md).
- **Log Analytics**: İzleme verilerini toplayan ve merkezi bir depoya veri depolayan bir Azure hizmeti. Bu veriler, olaylar, performans verilerini ve Azure API aracılığıyla sağlanan özel veriler içerebilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Ağ Performans İzleyicisi'ni ve trafik analizi temel olarak Azure İzleyici günlüklerine kullanılarak oluşturulan gibi uygulamalarını izleme. Daha fazla bilgi için [Azure İzleyicisi](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Log Analytics çalışma alanı**: Azure İzleyici günlüklerine, bir Azure hesabıyla ilişkili verilerin depolandığı bir örneği. Log Analytics çalışma alanları hakkında daha fazla bilgi için bkz. [Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ İzleyicisi**: Koşulları azure'da ağ senaryosu düzeyinde izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmet. NSG akış günlüklerini açıp Ağ İzleyicisi ile kapatabilirsiniz. Daha fazla bilgi için [Ağ İzleyicisi](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>Trafik analizi nasıl çalışır?

Trafik analizi, ham NSG akış günlüklerini inceler ve aynı kaynak IP adresi, hedef IP adresi, hedef bağlantı noktası ve protokol arasında yaygın akış toplayarak azaltılmış günlükleri yakalar. Örneğin, ana bilgisayar 1 (IP adresi: 10.10.10.10) ana bilgisayar 2'ye iletişim (IP adresi: 10.10.20.10), 1 saat (örneğin, 80) bağlantı noktası ve protokol (örneğin, http) kullanarak bir süre boyunca 100 kez. Ana bilgisayar 1 & ana bilgisayar 2 100 kez bir zaman bağlantı noktası 1 saat boyunca iletilen bir giriş, sınırlı günlük sahip *80* ve protokol *HTTP*, 100 girdilerine sahip yerine. Azaltılmış günlükleri Coğrafya, güvenlik ve topoloji bilgilerini Gelişmiş ve sonra bir Log Analytics çalışma alanında depolanır. Aşağıdaki resimde veri akışı gösterilmektedir:

![NSG akış günlüklerini işleme için veri akışı](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Desteklenen bölgeler

Trafik analizi, aşağıdaki desteklenen bölgelerden'nde Nsg'ler için kullanabilirsiniz:

* Orta Kanada
* Batı Orta ABD
* Doğu ABD
* Doğu ABD 2
* Orta Kuzey ABD
* Orta Güney ABD
* Orta ABD
* Batı ABD
* Batı ABD 2
* Fransa Orta
* Batı Avrupa
* Kuzey Avrupa
* Güney Brezilya
* Birleşik Krallık Batı
* Birleşik Krallık Güney
* Avustralya Doğu
* Avustralya Güneydoğu
* Doğu Asya
* Güneydoğu Asya
* Kore Orta
* Orta Hindistan
* Güney Hindistan
* Japonya Doğu 
* Japonya Batı
* ABD Devleti Virginia

Log Analytics çalışma alanı şu bölgelerde bulunmalıdır:
* Orta Kanada
* Batı Orta ABD
* Batı ABD 2
* Doğu ABD
* Fransa Orta
* Batı Avrupa
* Birleşik Krallık Güney
* Avustralya Güneydoğu
* Güneydoğu Asya
* Kore Orta
* Orta Hindistan
* Japonya Doğu
* ABD Devleti Virginia

## <a name="prerequisites"></a>Önkoşullar

### <a name="user-access-requirements"></a>Kullanıcı erişimi gereksinimleri

Hesabınızda aşağıdaki Azure birine üye olmalıdır [yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json):

|Dağıtım modeli   | Rol                   |
|---------          |---------               |
|Resource Manager   | Sahip                  |
|                   | Katılımcı            |
|                   | Okuyucu                 |
|                   | Ağ Katılımcısı    |

Hesabınızı yerleşik rollerden biri atanmamışsa, atanmalıdır bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) aşağıdaki eylemler, abonelik düzeyinde atanmış:

- "Microsoft.Network/applicationGateways/read"
- "Microsoft.Network/connections/read"
- "Microsoft.Network/loadBalancers/read"
- "Microsoft.Network/localNetworkGateways/read"
- "Microsoft.Network/networkInterfaces/read"
- "Microsoft.Network/networkSecurityGroups/read"
- "Microsoft.Network/publicIPAddresses/read"
- "Microsoft.Network/routeTables/read"
- "Microsoft.Network/virtualNetworkGateways/read"
- "Microsoft.Network/virtualNetworks/read"

Kullanıcı erişimi izinleri denetleme hakkında daha fazla bilgi için bkz: [trafik Analizi ile ilgili SSS](traffic-analytics-faq.md).

### <a name="enable-network-watcher"></a>Ağ İzleyicisini etkinleştirme

Trafiğini analiz etmek için var olan bir Ağ İzleyicisi olması gerekir veya [Ağ İzleyicisini etkinleştirme](network-watcher-create.md) için analiz etmek istediğiniz Nsg'ler sahip olduğunuz her bir bölgede trafiği. Trafik analizi, herhangi bir barındırılan Nsg'ler için etkinleştirilebilir [desteklenen bölgeler](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>Ağ kaynak Sağlayıcısı'nı yeniden kaydedin

Trafik analizi kullanabilmeniz için ağ kaynak sağlayıcısı yeniden kaydetmeniz gerekir. Tıklayın **deneyin** Azure Cloud Shell'i açmak için aşağıdaki kodu kutusuna. Cloud Shell, Azure aboneliğinizin, içine otomatik olarak günlüğe kaydeder. Cloud Shell açıldıktan sonra ağ kaynağı sağlayıcı yeniden kaydetmek için aşağıdaki komutu girin:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Bir ağ güvenlik grubu seçin

NSG akış günlüğü etkinleştirmeden önce akışlar için oturum açmak için bir ağ güvenlik grubu olması gerekir. Bir ağ güvenlik grubu yoksa bkz [ağ güvenlik grubu oluşturma](../virtual-network/manage-network-security-group.md#create-a-network-security-group) oluşturmak için.

Azure portalının sol taraftan **İzleyici**, ardından **Ağ İzleyicisi**ve ardından **NSG akış günlüklerini**. Aşağıdaki resimde gösterildiği gibi bir NSG akış günlüğü etkinleştirmek istediğiniz ağ güvenlik grubu seçin:

![NSG akış günlüğü etkinleştirme gerekli nsg seçimi](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

Herhangi bir bölgede dışında barındırılan bir NSG için trafik Analizi'ni etkinleştirmek çalışırsanız [desteklenen bölgeler](#supported-regions), bir "Bulunamadı" hatasını alıyorsunuz.

## <a name="enable-flow-log-settings"></a>Akış günlüğü ayarları etkinleştirin

Akış günlüğü ayarları etkinleştirmeden önce aşağıdaki görevleri tamamlamanız gerekir:

Aboneliğiniz için henüz kayıtlı değilse Azure Insights sağlayıcısını kaydedin:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

Henüz yoksa, oturum açtığında NSG akış depolamak için bir Azure depolama hesabı, bir depolama hesabı oluşturmanız gerekir. Aşağıdaki komutla bir depolama hesabı oluşturabilirsiniz. Komutu çalıştırmadan önce değiştirin `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumlarında benzersiz olan bir ada sahip yalnızca sayı ve küçük harfler kullanarak. Kaynak grubu adı, gerekirse de değiştirebilirsiniz.

```azurepowershell-interactive
New-AzStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Aşağıdaki seçenekler, resimde gösterildiği gibi seçin:

1. Seçin *üzerinde* için **durumu**
2. Akış günlüklerini depolamak için mevcut bir depolama hesabını seçin. Verileri sonsuza kadar saklamak istiyorsanız, değer kümesine *0*. Depolama hesabı için Azure depolama ücretleri uygulanır.
3. Ayarlama **bekletme** verilerini saklamak istediğiniz gün sayısı.
4. Seçin *üzerinde* için **trafik analizi durumu**.
5. Mevcut bir Log Analytics çalışma alanı seçin ya da seçin **yeni çalışma alanı oluştur** yeni bir tane de oluşturabilirsiniz. Bir Log Analytics çalışma alanı trafik analizi tarafından analiz oluşturmak için kullanılır toplanmış ve dizinli verileri depolamak için kullanılır. Mevcut bir çalışma öğesini seçerseniz, desteklenen bölgelerden birinde mevcut olmalıdır ve yeni sorgu diline yükseltme yaptı. Mevcut bir çalışma yükseltmek istiyor musunuz veya bir çalışma alanı, desteklenen bir bölgede izniniz yok, yeni bir tane oluşturun. Sorgu dilleri hakkında daha fazla bilgi için bkz. [yeni günlük araması için Azure İzleyici günlükleri yükseltme](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    Trafik analizi çözümü ve Nsg'ler barındırma Log Analytics çalışma alanı aynı bölgede olması gerekmez. Örneğin, Doğu ABD ve Batı ABD içindeki Nsg'ler olabilir, ancak bir çalışma alanında, Batı Avrupa bölgesinde trafik analizi olabilir. Birden çok Nsg'ler aynı çalışma alanında yapılandırılabilir.
6. **Kaydet**’i seçin.

    ![Depolama hesabına, Log Analytics çalışma alanı ve trafik analizi etkinleştirme seçimi](./media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Trafik analizi için etkinleştirmek istediğiniz diğer tüm Nsg'ler için önceki adımı yineleyin. Akış günlükleri verilerini çalışma alanına gönderilir, böylece yerel kanunlarınız ve düzenlemelerinizle ülkenizde veri depolama çalışma alanının bulunduğu bölgede izin emin olun.

Trafik analizi kullanarak da yapılandırabilirsiniz [kümesi AzNetworkWatcherConfigFlowLog](/powershell/module/az.network/set-aznetworkwatcherconfigflowlog) Azure PowerShell'in PowerShell cmdlet'i. Çalıştırma `Get-Module -ListAvailable Az` yüklü sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps).

## <a name="view-traffic-analytics"></a>Trafik analizi görüntüle

Sol taraftaki Portalı'nın, seçin **tüm hizmetleri**, enter *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** arama sonuçlarında görünür. Trafik analizi ve özelliklerini keşfetmek için seçin **Ağ İzleyicisi**, ardından **trafik analizi**.

![Trafik analizi Panosu erişme](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Pano trafik analizi için herhangi bir raporu oluşturmadan önce anlamlı bilgiler çıkarmanıza için yeterli veri toplama ilk gerekir çünkü ilk kez görünmesi 30 dakika kadar sürebilir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Trafik analizi tam olarak yapılandırdıktan sonra elde etmek isteyebileceğiniz ınsights bazıları şu şekildedir:

### <a name="find-traffic-hotspots"></a>Trafiği etkin noktaları Bul

**Aramak**

- Hangi ana bilgisayarlar, alt ağlar ve sanal ağları gönderme veya çoğu trafiğini gönderip almak, en fazla kötü amaçlı trafik geçiş ve önemli akışları engelleyen?
    - Konak, alt ağ ve sanal ağ için karşılaştırma grafiği denetleyin. Hangi konakların anlama, alt ağlar ve sanal ağları gönderiyorsunuz veya en çok trafiği almak en çok trafiği işleyen ana belirlemenize yardımcı ve olup trafik dağılımı düzgün bir şekilde gerçekleştirilir.
    - Gelen trafiğin hacmini bir ana bilgisayar için uygun olup olmadığını değerlendirebilirsiniz. Normal davranış trafik hacminin ya da daha fazla araştırma belirlenmiştir mu?
- Ne kadar gelen/giden trafik var mı?
    -   Konak, gelen daha fazla giden trafiği veya tam tersi alması beklenir?
- Engellenen trafik istatistikleri.
    - Neden bir konak zararsız trafik önemli bir sesini engelliyor? Bu davranışı, daha fazla araştırma ve büyük olasılıkla en iyi duruma getirme yapılandırması gerektirir
- İzin verilen/engellenen kötü amaçlı trafik istatistikleri
  - Neden bir ana bilgisayar kötü amaçlı trafik ve kaynak kötü amaçlı akışlar izin verilen neden alıyor? Bu davranış, daha fazla araştırma ve büyük olasılıkla en iyi duruma getirme yapılandırması gerektirir.

    Seçin **tümünü gör**altında **konak**, aşağıdaki resimde gösterildiği gibi:

    ![Çoğu trafiği ayrıntıları konakla vitrini Panosu](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- Aşağıdaki resimde, zaman talking beş ana ve flow ile ilgili ayrıntıları (izin verilen – gelen/giden ve engellenen - gelen/giden akışlar) için bir ana bilgisayar için popüler gösterilmiştir:

    ![İlk beş çoğu Konuşmayı konak eğilimi](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Aramak**

- Hangi en görüşme konağı çiftleri misiniz?
    - Beklenen davranış iletişim ön uç veya arka uç veya arka uç internet trafiğini gibi düzensiz davranışı gibi.
- İzin verilen/engellenen trafik istatistikleri
    - Neden bir konak izin verme veya engelleme önemli trafik hacmi
- Uygulama protokolü çoğu görüşme konağı çiftleri arasındaki en sık kullanılan:
    - Bu uygulamalar, bu ağ üzerinde izin veriliyor mu?
    - Uygulamaların düzgün bir şekilde yapılandırılır? Bunlar, iletişim için uygun Protokolü kullanıyor musunuz? Seçin **tümünü gör** altında **sık konuşma**, aşağıdaki resimde gösterildiği gibi:

        ![En sık rastlanan konuşma vitrini Panosu](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- Aşağıdaki resimde zaman için en çok beş yapılan görüşmeler popüler gösterir ve flow ile ilgili ayrıntıları gibi izin verilen ve reddedilen gelen ve giden akışlar konuşma çifti için:

    ![İlk beş geveze konuşma ayrıntıları ve eğilim](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Aramak**

- Ortamınızda hangi uygulama protokolü en çok kullanılan ve hangi görüşme konağı çiftleri en iyi uygulama protokolü kullanarak?
    - Bu uygulamalar, bu ağ üzerinde izin veriliyor mu?
    - Uygulamaların düzgün bir şekilde yapılandırılır? Bunlar, iletişim için uygun Protokolü kullanıyor musunuz? 80 ve 443 gibi yaygın bağlantı noktalarını beklenen davranıştır. Standart iletişim için olağan dışı herhangi bir bağlantı noktası görüntüleniyorsa, bir yapılandırma değişikliği gerektirebilir. Seçin **tümünü gör** altında **uygulama bağlantı noktası**, aşağıdaki resimde:

        ![En çok kullanılan uygulama protokolleri vitrini Panosu](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- Aşağıdaki resim ilk beş L7 protokolleri ve (örneğin, izin verilen ve engellenen akışlar) akışı ile ilgili ayrıntılar için bir L7 protokolü için eğilimleri belirleme zamanı göster:

    ![En iyi beş katman 7 protokolleri ayrıntıları ve eğilim](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Ayrıntılar için günlük araması'nda uygulama protokolü akış](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Aramak**

- Ortamınızda bir VPN ağ geçidinin kapasite kullanımı eğilimleri.
    - Her VPN SKU'ya belirli miktarda bant genişliği sağlar. VPN ağ geçitleri potansiyelinden az kullanılmasına neden?
    - Ağ geçitlerinizi, kapasite ulaşıyor? Sonraki daha yüksek SKU için yükseltmeniz gerekir?
- Bu, hangi bağlantı noktası üzerinden hangi VPN ağ geçidi üzerinden en görüşme konakları misiniz?
    - Bu desen, normal mi? Seçin **tümünü gör** altında **VPN ağ geçidi**, aşağıdaki resimde gösterildiği gibi:

        ![Üst etkin VPN bağlantıları vitrini Panosu](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- Aşağıdaki resimde, kapasite kullanımı Azure VPN Gateway ve flow ile ilgili ayrıntıları (örneğin, izin verilen akışlar ve bağlantı noktaları) için saat eğilimler gösterilmiştir:

    ![VPN ağ geçidi kullanımı eğilimi ve akış ayrıntıları](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Trafik dağılımı coğrafyaya göre görselleştirin

**Aramak**

- En çok kullanılan kaynaklar veri merkezi ve uygulama protokolleri konuşmaya üst konuşmaya üst sahte ağ trafiğinin bir veri merkezine gibi veri merkezi başına trafik dağılımı.
  - Bir veri merkezi hakkında daha fazla yük gözlemlerseniz, verimli trafik dağıtımı için planlayabilirsiniz.
  - Sahte ağ veri merkezinde konuşmaya, sonra bunları engellemek için NSG kuralları düzeltin.

    Seçin **haritayı görüntüleme** altında **ortamınızı**, aşağıdaki resimde gösterildiği gibi:

    ![Pano gösterildiği trafik dağılımı](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- Coğrafi harita veri merkezleri gibi parametreleri seçimi için üst Şerit gösterir (Hayır/dağıtıldı-dağıtım/etkin/etkin/trafik analizi Etkin/trafik analizi etkin değil) ve etkin Benign/kötü amaçlı trafik katkısında bulunan ülkeler Dağıtım:

    ![Etkin dağıtım vitrini coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- Coğrafi harita ülkeler ve kıtadaki kendisine mavi (zararsız trafik) ve kırmızı (kötü amaçlı trafiği) satırları renkli iletişim kurulurken bir veri merkezine trafik dağılımı gösterir:

    ![Trafik dağılımı ülkeler ve kıtadaki vitrini coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Ayrıntılar için günlük araması'nda trafik dağılımı akış](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Sanal ağlar ile trafik dağılımı görselleştirin

**Aramak**

- Sanal ağ, topoloji, en çok kullanılan kaynaklar sanal ağ ve uygulama protokolleri konuşmaya üst konuşmaya üst sahte ağ trafiğinin sanal ağ başına trafik dağılımı.
  - Hangi sanal ağa sanal ağı'nı bilerek konuşmaya. Konuşma görülmüyorsa düzeltilebilir.
  - Sahte ağları, sanal ağ ile konuşmaya, dolandırıcı ağları engellemeye yönelik NSG kuralları düzeltebilirsiniz.
 
    Seçin **görünümü Vnet'ler** altında **ortamınızı**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal ağ dağıtım vitrini Panosu](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- Üst Şerit için bir sanal ağın (Inter sanal ağ bağlantıları/Active/Inactive) gibi parametrelerin seçimi, dış bağlantılar, etkin akışlar ve kötü amaçlı akışlar sanal ağın sanal ağ topolojisini gösterir.
- Sanal ağ topolojisini Abonelikleri, çalışma alanları, kaynak grupları ve zaman aralığına göre filtreleyebilirsiniz. Akış yardımcı olan ek filtreler anlayın: Akış türü (sanal ağlar arası, IntraVNET vb.), akış yönünü (gelen, giden), akış durumu (izin verilen, engellenen) sanal ağlar (hedeflenen ve bağlı), bağlantı türü (eşlemesi veya ağ geçidi - P2S ve S2S) ve NSG. Bu filtreler, ayrıntılı olarak incelemek istediğiniz sanal ağlar odaklanmak için kullanın.
- Örneğin, sanal ağ topolojisini bakımından akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve ağ güvenlik grupları, sanal ağa trafik dağılımı gösterir:

    ![Trafik dağıtım ve akış ayrıntıları vitrini sanal ağ topolojisi](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)
    
    ![Üst düzey vitrini sanal ağ topolojisi ve daha fazla filtre](./media/traffic-analytics/virtual-network-filters.png)

    ![Ayrıntılar için günlük araması'nda sanal ağ trafik dağılımı akış](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Aramak**

- Dağıtım başına alt ağ, topoloji, en çok kullanılan kaynaklar, alt ağa trafiğin alt ve üst uygulama protokolleri konuşmaya konuşmaya üst sahte ağ trafiği.
    - Alt ağı için alt ağı bilerek konuşmaya. Beklenmeyen konuşmaları görürseniz, yapılandırmanızı düzeltebilirsiniz.
    - Sahte ağ alt ağı ile konuşmaya, dolandırıcı ağları engellemeye yönelik NSG kuralları yapılandırarak düzeltebilirsiniz.
- Alt ağlar topoloji seçimi Active/Inactive alt ağ, dış bağlantılar, etkin akışlar ve alt ağın kötü amaçlı akışlar gibi parametreleri için üst Şerit gösterir.
- Örneğin, alt ağ topolojisi bakımından akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve Nsg'ler, bir sanal ağa trafik dağılımı gösterir:

    ![Bir sanal ağ alt akışlar bakımından trafik dağılımı vitrini alt ağ topolojisi](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Aramak**

Trafik dağılımı uygulama ağ geçidi ve yük dengeleyici, topoloji başına en çok kullanılan kaynaklar trafik, üst uygulama ağ geçidi & yük dengeleyici ve uygulama protokolleri konuşmaya üst konuşmaya ağları standart dışı. 
    
 - Alt ağı bilerek, hangi uygulama ağ geçidi veya yük dengeleyici konuşmaya. Beklenmeyen konuşmaları gözlemlerseniz, yapılandırmanızı düzeltebilirsiniz.
 - Sahte ağları bir uygulama ağ geçidi veya yük dengeleyici ile konuşmaya, dolandırıcı ağları engellemeye yönelik NSG kuralları yapılandırarak düzeltebilirsiniz. 

    ![subnet-topology-showcasing-traffic-Distribution-to-a-Application-Gateway-subnet-With-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Bağlantı noktaları ve internet'ten trafik alan sanal makineleri görüntüle

**Aramak**

- Hangi bağlantı noktalarını açma, internet üzerinden konuşmaya?
  - Beklenmeyen bir bağlantı noktalarını açık bulunamazsa, yapılandırmanızı düzeltebilirsiniz:

    ![Pano vitrini alma ve internet'e trafik gönderen bağlantı noktaları](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Azure hedef bağlantı noktaları ve ana bilgisayar ayrıntıları](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Aramak**

Ortamınızda kötü amaçlı trafik var mı? Burada, kaynaklanan? Burada için yönlendirilir?

![Günlük araması'nda kötü amaçlı trafik akışı ayrıntıları](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>NSG/NSG kuralları isabet eğilimleri Görselleştirme

**Aramak**

- Hangi NSG/NSG kuralları isabetlerin çoğu akışlar dağıtım ile karşılaştırmalı grafikte mı var?
- Üst kaynak ve hedef görüşme çiftleri başına NSG/NSG kuralları nelerdir?

    ![İstatistikleri Pano NSG vitrini ziyaret sayısı](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- Aşağıdaki resim NSG kuralları ve kaynak-hedef akışın bir ağ güvenlik grubu ayrıntılarını isabetleri için eğilimleri belirleme zamanı göster:

  - Hızlı bir şekilde tespit hangi Nsg'leri ve NSG kuralları kötü amaçlı akışlar geçiş yapma ve olduğu üst kötü amaçlı IP adresleri bulut ortamınıza erişen
  - Hangi NSG/NSG kuralları izin verme/önemli ölçüde ağ trafiği engelliyor tanımlayın
  - Select üst kuralları NSG veya NSG ayrıntılı incelemesi için filtreler

    ![NSG kural İsabetleri ve en iyi NSG kuralları için zaman popüler vitrini](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Günlük araması istatistikleri ayrıntılarında üst NSG kuralları](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sık sorulan sorulara yanıtlar almak için bkz. [trafik Analizi ile ilgili SSS](traffic-analytics-faq.md).
