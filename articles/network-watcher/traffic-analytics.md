---
title: Azure trafik analizi | Microsoft Docs
description: Azure ağ güvenlik grubu akış günlükleri trafiği analytics ile çözümlemeyi öğrenin.
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
ms.openlocfilehash: ad26772650cf052926a2534d343f64765f47b78f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333403"
---
# <a name="traffic-analytics"></a>Trafik analizi

Trafik analytics bulut ağları kullanıcı ve uygulama etkinliğinde görünürlük sağlayan bir bulut tabanlı bir çözümdür. Trafik analytics Öngörüler Azure bulut trafik akışını sağlamak için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlükleri analiz eder. Trafik analytics ile şunları yapabilirsiniz:

- Ağ etkinliği, Azure aboneliklerinize arasında görselleştirmek ve etkin noktalar tanımlayın.
- Güvenlik tehditlerine karşı belirleyin ve Aç-bağlantı noktaları, internet erişimi ve sanal ağları standart dışı bağlanma makineler (VM) çalışan uygulamalar gibi bilgilerle ağınızın güvenliğini sağlamak.
- Azure bölgeleri ve ağ dağıtımınız için performans ve kapasite en iyi duruma getirmek için internet üzerinden trafik akışı modelleri anlayın.
- Ağ yapılandırma hataları başarısız bağlantılara ağınızda baştaki sabitleme.

## <a name="why-traffic-analytics"></a>Neden analizi trafiği?

İzleme, yönetme ve kendi ağ güvenliği aşılmamış güvenlik, uyumluluk ve performans için bilmeniz önemlidir. Kendi ortamınızdaki bilerek korumak ve onu en iyi duruma getirmek için en önemli çok önemlidir. Genellikle bağlanan ağ geçerli durumunu bilmeniz gereken, burada bunlar, bağlantı noktalarını bağlantı kurduğunuz internet'e, beklenen ağ davranışı düzensiz ağ davranışı, açık ve ani trafiği miktarı artar.

Bulut ağları netflow veya eşdeğer Protokolü özellikli yönlendiriciler ve anahtarlar girdiğinde veya bir ağ arabirimi çıktığında gibi IP ağ trafiğini toplamaya özelliği sağlar sahip olduğu şirket içi Kurumsal ağlarda farklı. Trafik akışı verileri çözümleyerek, ağ trafiği akışını ve birim analizini oluşturabilirsiniz.

Giriş hakkında bilgi sağlayan NSG akış günlükleri, Azure sanal ağı varsa ve tek tek ağ arabirimleri, VM'ler veya alt ağlar için çıkış IP trafiği bir ağ güvenlik grubu ile ilişkili. Ham NSG akış günlüklerini analiz ve güvenlik, topoloji ve coğrafi konum, trafik Intelligence ekleme analytics, trafik akışını, ortamınızdaki Öngörüler sağlayabilirsiniz. Trafik Analytics en iletişim ana, en iletişim kuran uygulama protokolleri, en konuşmaya konak çiftleri, izin verilen ve engellenen trafik, gelen/giden trafik, açık internet bağlantı noktaları, en engelleme kuralları, trafik gibi bilgiler sağlar Azure veri merkezi, sanal ağ, alt ağlar, başına dağıtım veya standart dışı ağlar.

## <a name="key-components"></a>Başlıca bileşenler

- **Ağ güvenlik grubu (NSG)**: izin veren veya reddeden bir Azure sanal ağına bağlı kaynaklar için ağ trafiği güvenlik kuralları listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager). Daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ güvenlik grubu (NSG) akış günlükleri**: giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek sağlar. Günlükleri json biçiminde yazılır ve akış NIC uygulandığı bir kural başına temelinde giden ve gelen akışları Göster NSG akışı, akış (kaynak/hedef IP adresi, kaynak/hedef bağlantı noktası ve Protokolü) beş tanımlama bilgilerini ve trafiğe izin verildiği veya reddedildi. NSG akış günlükleri hakkında daha fazla bilgi için bkz: [NSG akış günlükleri](network-watcher-nsg-flow-logging-overview.md).
- **Günlük analizi**: izleme verilerini toplayan ve merkezi bir depoya veri depolayan bir Azure hizmeti. Bu veriler, olayları, performans verileri ya da Azure API aracılığıyla sağlanan özel veri içerebilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Günlük analizi temel olarak kullanarak ağ Performans İzleyicisi'ni ve trafik analytics yerleşik olanlar gibi uygulamaları izleme. Daha fazla bilgi için bkz: [oturum analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Günlük analizi çalışma alanı**: bir Azure hesabı için ilgili verilerin depolandığı günlük analizi örneği. Günlük analizi çalışma alanları hakkında daha fazla bilgi için bkz: [günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ İzleyicisi**: izleme ve Azure ağ senaryo düzeyinde koşullar tanılama sağlar bölgesel bir hizmet. NSG akış günlükleri açma ve kapatma Ağ İzleyicisi'ni kapatabilirsiniz. Daha fazla bilgi için bkz: [Ağ İzleyicisi](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>Trafik analytics nasıl çalışır?

Trafik analytics ham NSG akış günlükleri inceler ve aynı kaynak IP adresi, hedef IP adresi, hedef bağlantı noktası ve protokol arasında ortak akışları toplayarak azaltılmış günlükleri yakalar. Örneğin, ana bilgisayar 1 (IP adresi: 10.10.10.10) ana bilgisayar 2'ye iletişim (IP adresi: 10.10.20.10), bağlantı noktası (örneğin, 80) ve Protokolü (örneğin, http) kullanarak 1 saatlik bir süre boyunca 100 kez. Ana bilgisayar 1 & ana bilgisayar 2 100 kez bir süre 1 bağlantı noktası kullanılarak saat iletilen bir giriş, azaltılmış günlüğü var *80* ve Protokolü *HTTP*, 100 girdilerine sahip yerine. Azaltılmış günlükleri Gelişmiş Coğrafya, güvenlik ve topoloji bilgilerini ve daha sonra günlük analizi çalışma alanındaki depolanır. Aşağıdaki resimde veri akışı gösterilmektedir:

![NSG akış günlükleri işleme için veri akışı](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Desteklenen bölgeler

Nsg'ler aşağıdaki bölgeler hiçbirinde trafiği analytics kullanabilirsiniz: Batı Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Orta ABD, Orta Güney ABD, Orta ABD, Batı ABD, Batı ABD-2, Batı Avrupa, Kuzey Avrupa, Batı Birleşik Krallık, Güney İngiltere, Avustralya Doğu, Avustralya Güneydoğu ve Güneydoğu Asya. Günlük analizi çalışma alanı, Batı Orta ABD, Doğu ABD, Batı Avrupa, Güney İngiltere, Avustralya Güneydoğu veya Güneydoğu Asya bölge içinde bulunmalıdır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="user-access-requirements"></a>Kullanıcı erişimi gereksinimleri

Hesabınızı aşağıdaki Azure birinin üyesi olmanız gerekir [yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json):

|Dağıtım modeli   | Rol                   |
|---------          |---------               |
|Resource Manager   | Sahip                  |
|                   | Katılımcı            |
|                   | Okuyucu                 |
|                   | Ağ Katılımcısı    |
|Klasik            | Hesap yöneticisi  |
|                   | Hizmet yöneticisi  |
|                   | Ortak yönetici       |

Hesabınızı yerleşik roller birine atanmamışsa, atanmalıdır bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) aşağıdaki eylemler, abonelik düzeyinde atanır:

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

Kullanıcı erişimi izinleri denetleme hakkında daha fazla bilgi için bkz: [trafiği analytics SSS](traffic-analytics-faq.md).

### <a name="enable-network-watcher"></a>Ağ İzleyicisini etkinleştirme

Trafiğini analiz etmek için var olan bir Ağ İzleyicisi olması gerekir veya [bir Ağ İzleyicisi'ni etkinleştirmek](network-watcher-create.md) için analiz etmek istediğiniz Nsg'ler sahip her bölgede trafiği. Trafik analizi, herhangi bir barındırılan Nsg'ler için etkinleştirilebilir [bölgeler desteklenen](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>Ağ kaynak sağlayıcı yeniden kaydedilir

Trafik analytics kullanmadan önce ağ kaynak sağlayıcısı yeniden kaydetmeniz gerekir. Tıklatın **deneyin** aşağıdaki kodu kutusunda Azure bulut Kabuğu'nu açın. Bulut Kabuk Azure aboneliğinize, içine otomatik olarak günlüğe kaydeder. Bulut Kabuk açıldıktan sonra ağ kaynak sağlayıcısı yeniden kaydetmek için aşağıdaki komutu girin:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Bir ağ güvenlik grubu seçin

NSG akış günlüğü etkinleştirmeden önce akışları için oturum için bir ağ güvenlik grubu olması gerekir. Bir ağ güvenlik grubu yoksa bkz [bir ağ güvenlik grubu oluşturun](../virtual-network/manage-network-security-group.md#create-a-network-security-group) oluşturmak için.

Azure portalının sol tarafta seçin **İzleyici**, ardından **Ağ İzleyicisi**ve ardından **NSG akış günlükleri**. Aşağıdaki resimde gösterildiği gibi bir NSG akış günlüğü, etkinleştirmek istediğiniz ağ güvenlik grubu seçin:

![NSG akış günlüğü etkinleştirme gerektiren Nsg'ler seçimi](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

Trafiğini analiz için herhangi bir bölgede dışında barındırılan bir NSG etkinleştirmeye çalışırsanız [bölgeler desteklenen](#supported-regions), "Bulunamadı" hatasını alıyorsunuz.

## <a name="enable-flow-log-settings"></a>Akış günlüğü ayarlarını etkinleştir

Akış günlüğü ayarlarını etkinleştirmeden önce aşağıdaki görevleri tamamlamanız gerekir:

Aboneliğiniz için henüz kayıtlı değilse Azure Öngörüler sağlayıcısını Kaydet:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

Henüz yoksa NSG akış depolamak için bir Azure depolama hesabınızın oturum açtığında, bir depolama hesabı oluşturmanız gerekir. Aşağıdaki komutla bir depolama hesabı oluşturabilirsiniz. Komutu çalıştırmadan önce yerine `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumları arasında benzersiz bir ad yalnızca sayılar ve küçük harfler kullanarak. Kaynak grubu adı, gerekirse de değiştirebilirsiniz.

```azurepowershell-interactive
New-AzureRmStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Aşağıdaki seçenekler, aşağıdaki resimde gösterildiği gibi seçin:

1. Seçin *üzerinde* için **durumu**
2. Akış günlükleri depolamak için mevcut bir depolama hesabını seçin. Verileri sonsuza kadar depolamak istiyorsanız, değer kümesine *0*. Depolama hesabı için Azure depolama ücretleri doğurur.
3. Ayarlama **bekletme** istediğiniz verilerini depolamak için gün sayısı.
4. Seçin *üzerinde* için **trafiği Analytics durum**.
5. Varolan bir günlük analizi (OMS) çalışma alanını seçin ya da seçin **yeni çalışma alanı oluştur** yeni bir tane oluşturmak için. Günlük analizi çalışma alanı trafiğini analizi tarafından sonra analytics oluşturmak için kullanılan toplanmış ve dizinli verilerini depolamak için kullanılır. Var olan bir çalışma öğesini seçerseniz, birinde bulunmalıdır [bölgeler desteklenen](#traffic-analytics-supported-regions) ve yeni sorgu dili yükseltildi. Var olan bir çalışma yükseltmek istiyor musunuz ya da bir çalışma alanı desteklenen bir bölgede olmayan varsa, yeni bir tane oluşturun. Sorgu dilleri hakkında daha fazla bilgi için bkz: [Azure günlük analizi yükseltmek için yeni günlük arama](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    Barındırma trafiğini analiz çözümü ve Nsg'ler günlük analizi çalışma alanı aynı bölgede olması gerekmez. Örneğin, Nsg'ler Doğu ABD ve Batı ABD olabilir, ancak bir çalışma alanı Batı Avrupa bölgedeki trafiği analytics olabilir. Birden çok Nsg'ler aynı çalışma alanında yapılandırılabilir.
6. **Kaydet**’i seçin.

    ![Depolama hesabı, günlük analizi çalışma alanı ve trafik Analytics etkinleştirme](./media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Trafik analizi için etkinleştirmek istediğiniz diğer Nsg'ler için önceki adımları yineleyin. Akış günlükleri verilerden çalışma alanına gönderilir, böylece yerel yasalarına ve düzenlemelerine ülkenizdeki veri depolama çalışma bulunduğu bölgede izin olun.

Trafik analizi kullanarak da yapılandırabilirsiniz [kümesi AzureRmNetworkWatcherConfigFlowLog](/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog) AzureRm PowerShell modülü sürümü 6.2.1 PowerShell cmdlet'ini veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="view-traffic-analytics"></a>Görünüm trafiği analizi

Sol taraftaki Portalı'nın, seçin **tüm hizmetleri**, enter *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** arama sonuçlarında görünür. Trafik analizi ve özelliklerini keşfetmeye başlamak için seçin **Ağ İzleyicisi**, ardından **trafiği Analytics**.

![Trafik Analytics Pano erişme](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Pano trafiği Analytics önce herhangi bir rapor oluşturmadan önce anlamlı bilgiler çıkarmanıza kendisine için yeterli veri toplama gerekir çünkü ilk kez görünmesi 30 dakika sürebilir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Trafik Analytics tam olarak yapılandırdıktan sonra elde isteyebilirsiniz Öngörüler bazıları şunlardır:

### <a name="find-traffic-hotspots"></a>Trafiği etkin Bul

**Aramak**

- Hangi ana bilgisayarlar, alt ağlar ve sanal ağları gönderme veya çoğu trafik, en fazla kötü amaçlı trafiği çapraz geçiş yapma ve önemli akışları engelleme?
    - Karşılaştırma grafiği ana bilgisayar, alt ağ ve sanal ağ için denetleyin. Hangi ana bilgisayarların anlama, alt ağları ve sanal ağlar gönderiyorsunuz ya da en trafiğini alma yardımcı en trafiği, işleme konakları tanımlamanıza ve trafik dağılımı doğru olup olmadığını yapılır.
    - Trafik hacmi bir ana bilgisayar için uygun olup olmadığını değerlendirebilirsiniz. Trafik normal davranışın birim ya da daha fazla araştırma belirlenmiştir mu?
- Ne kadar gelen/giden trafik var mı?
    -   Daha fazla gelen trafiği giden daha ya da tam tersini almak için konak beklenen?
- Engellenen trafiği istatistikleri.
    - Neden bir ana bilgisayar önemli bir zararsız trafik hacmi engelliyor? Bu davranış, daha fazla araştırma ve büyük olasılıkla yapılandırma en iyi duruma getirilmesi gerektirir
- İzin verilen ve engellenen kötü amaçlı trafiği istatistikleri
    - Neden bir ana bilgisayar kötü amaçlı trafiği ve kötü amaçlı kaynağından akışları izin neden alıyor? Bu davranış, daha fazla araştırma ve büyük olasılıkla yapılandırma en iyi duruma getirilmesi gerektirir.

    Seçin **tümünü görmek**altında **konak**, aşağıdaki resimde gösterildiği gibi:

    ![Ana bilgisayar çoğu trafiği ayrıntılarla birtakım sergileyen Panosu](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- Aşağıdaki resimde, zaman talking beş ana ve akışıyla ilgili ayrıntıları (izin verilen – gelen/giden ve reddedilen - gelen/giden trafik akışları) için bir konak için eğilim gösterir:

    ![İlk beş çoğu Konuşmayı konak eğilimi](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Aramak**

- Hangi en conversing konak çiftleri misiniz?
    - Ön uç veya arka uç iletişimini veya arka uç Internet trafiğini gibi düzensiz davranışı gibi beklenen bir davranış.
- İzin verilen ve engellenen trafik istatistikleri
    - Neden bir ana bilgisayar izin verme veya engelleme önemli trafik hacmi
- Uygulama protokolü çoğu conversing konak çiftleri arasında en sık kullanılan:
    - Bu uygulamalar, bu ağ üzerinde izin verilir?
    - Uygulamaların düzgün yapılandırılmış? İletişim için uygun Protokolü kullandıkları? Seçin **tümünü görmek** altında **sık konuşma**, aşağıdaki resimde gösterildiği gibi:

        ![En sık rastlanan konuşma birtakım sergileyen Panosu](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- Aşağıdaki resimde zaman için ilk beş görüşmeleri eğilim gösterir ve akışıyla ilgili ayrıntıları gibi izin verilen gelen ve giden akışlar bir konuşma çifti için engellendi:

    ![İlk beş chatty konuşma ayrıntılarını ve eğilim](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Aramak**

- Hangi uygulama protokolü ortamınızda en sık kullanılan ve hangi conversing konak çiftleri en uygulama protokolü kullanarak?
    - Bu uygulamalar, bu ağ üzerinde izin verilir?
    - Uygulamaların düzgün yapılandırılmış? İletişim için uygun Protokolü kullandıkları? Ortak bağlantı noktaları 80 ve 443 gibi beklenen davranıştır. Standart iletişim için herhangi bir olağan dışı bağlantı görüntüleniyorsa, bir yapılandırma değişikliği gerektirebilir. Seçin **tümünü görmek** altında **uygulaması bağlantı noktası**, aşağıdaki resimde:

        ![Üst uygulama protokolleri birtakım sergileyen Panosu](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- Aşağıdaki resim Göster ilk beş L7 protokolleri ve (örneğin, izin verilen ve akışlar reddedildi) akışıyla ilgili ayrıntılar için L7 protokolü için eğilim süre:

    ![En iyi beş katman 7 protokolleri ayrıntıları ve eğilim](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Ayrıntılar için günlük arama uygulama protokolü akış](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Aramak**

- Bir VPN ağ geçidi, ortamınızdaki Kapasite Kullanım Eğilimleri.
    - Her VPN SKU belirli miktarda bant genişliği sağlar. VPN ağ geçitleri gereğinden az?
    - Ağ geçitleri kapasite ulaşıyor? Sonraki daha yüksek SKU yükseltmeniz gerekir?
- Hangi bağlantı noktası üzerinden hangi VPN ağ geçidi üzerinden en conversing konakları olduğu?
    - Bu desen normal mi? Seçin **tümünü görmek** altında **VPN ağ geçidi**, aşağıdaki resimde gösterildiği gibi:

        ![Üst etkin VPN bağlantılarını birtakım sergileyen Panosu](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- Aşağıdaki resimde bir Azure VPN ağ geçidi ve akışıyla ilgili ayrıntıları (örneğin, izin verilen akışları ve bağlantı noktaları) kapasite kullanımı için zaman eğilim gösterir:

    ![VPN ağ geçidi kullanım eğilim ve akış ayrıntıları](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Trafik dağılımı coğrafyaya Görselleştirme

**Aramak**

- Trafik dağılımı üst kaynakları veri merkezi ve uygulama protokolleri konuşmaya üst konuşmaya üst dolandırıcı ağ trafiği bir veri merkezine gibi veri merkezi başına.
    - Bir veri merkezi hakkında daha fazla yük gözlemlerseniz, verimli trafik dağıtım planlayabilirsiniz.
    - Yanlış ağları veri merkezinde konuşmaya varsa, bunları engellemek için NSG kuralları düzeltin.

    Seçin **haritayı görüntüle** altında **ortamınızı**, aşağıdaki resimde gösterildiği gibi:

    ![Pano birtakım sergileyen trafik dağılımı](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- Coğrafi harita parametrelerinin seçilmek üst Şerit gibi veri merkezleri gösterir (No/dağıtıldı-dağıtım/etkin/etkin/trafiği Analytics etkin/trafiği Analytics etkin değil) ve etkin Benign/kötü amaçlı trafiği katkıda bulunan ülkelerde Dağıtım:

    ![Etkin dağıtım birtakım sergileyen coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- Coğrafi harita trafik dağılımı ülkeler ve kendisine (zararsız trafiği) mavi ve kırmızı (kötü amaçlı trafiği) satırları renkli iletişim Kıtalar bir veri merkezine gösterir:

    ![Trafik dağılımı ülkeler ve Kıtalar birtakım sergileyen coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Ayrıntılar için günlük arama trafiği dağıtımlarında akış](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Trafik dağılımı sanal ağları tarafından Görselleştirme

**Aramak**

- Sanal ağ topolojisi, üst kaynakları sanal ağ ve uygulama protokolleri konuşmaya üst konuşmaya üst dolandırıcı ağlar, sanal ağ trafiğinin başına trafik dağılımı.
    - Hangi sanal ağa, sanal ağ bilerek konuşmaya. Konuşma görülmüyorsa düzeltilebilir.
    - Yanlış ağların bir sanal ağ ile konuşmaya, dolandırıcı ağları engellemek için NSG kuralları düzeltebilirsiniz.
 
    Seçin **görünüm Vnet'ler** altında **ortamınızı**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal ağ dağıtım birtakım sergileyen Panosu](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- Sanal ağ topolojisi bir sanal ağın (Inter sanal ağ bağlantıları/etkin/etkin değil) gibi parametreleri seçimi, dış bağlantıları, etkin araç ve sanal ağ kötü amaçlı akışları için üst Şerit gösterir.
- Sanal ağ topolojisi akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve ağ güvenlik grupları, göre sanal ağa trafik dağılımı örneğin gösterir:

    ![Trafik dağıtım ve akış ayrıntıları birtakım sergileyen sanal ağ topolojisi](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)

    ![Sanal ağ trafiğini günlük arama dağıtımlarında ayrıntılarını akış](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Aramak**

- Alt ağ, topoloji, trafik alt ağına üst kaynakları başına dağıtım alt ağ ve uygulama protokolleri konuşmaya üst konuşmaya üst dolandırıcı ağ trafiği.
    - Hangi alt bilerek hangi alt ağına konuşmaya. Beklenmeyen görüşmeleri görürseniz, yapılandırmanızı düzeltebilirsiniz.
    - Yanlış ağları bir alt ağ ile konuşmaya, dolandırıcı ağları engellemek için NSG kuralları yapılandırarak düzeltmek imkanınız olur.
- Alt ağ topolojisi Active/Inactive alt ağ, dış bağlantıları, etkin araç ve kötü amaçlı akışlar alt ağın gibi parametreleri seçilmek üst Şerit gösterir.
- Alt ağ topolojisi akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve Nsg'ler, göre sanal ağa trafik dağılımı örneğin gösterir:

    ![Trafik dağılımı akışları göre bir sanal ağ alt birtakım sergileyen alt ağ topolojisi](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Aramak**

Trafik dağılımı uygulama ağ geçidi ve yük dengeleyici, topoloji başına trafik, en üst kaynakları uygulama ağ geçidi & yük dengeleyici ve uygulama protokolleri konuşmaya üst konuşmaya ağları standart dışı. 
    
 - Hangi alt bilerek hangi uygulama ağ geçidi veya yük dengeleyici konuşmaya. Beklenmeyen görüşmeleri gözlemlerseniz, yapılandırmanızı düzeltebilirsiniz.
 - Yanlış ağları bir uygulama ağ geçidi veya yük dengeleyici ile konuşmaya, dolandırıcı ağları engellemek için NSG kuralları yapılandırarak düzeltmek imkanınız olur. 

    ![subnet-topology-showcasing-traffic-Distribution-to-a-Application-Gateway-subnet-With-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Bağlantı noktaları ve sanal makineleri trafiğini internet'ten alma görüntüleyin

**Aramak**

- Hangi açık bağlantı noktalarını Internet üzerinden konuşmaya?
    - Beklenmeyen bağlantı noktalarının açık bulunamazsa, yapılandırmanızı düzeltebilirsiniz:

    ![Pano birtakım sergileyen alırken ve trafik Internet'e gönderen bağlantı noktaları](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Azure hedef bağlantı noktaları ve ana bilgisayar ayrıntıları](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Aramak**

Kötü amaçlı trafiği ortamınızda var mı? Burada, kaynaklanan? Burada için yönlendirilir?

![Kötü amaçlı trafiği akışı ayrıntılı günlük arama](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>NSG/NSG kuralları isabet eğilimleri Görselleştirme

**Aramak**

- Hangi NSG/NSG kuralları akışları dağıtım ile karşılaştırmalı grafik en çok ziyaret var?
- NSG/NSG kuralları her üst kaynak ve hedef konuşma çiftleri nelerdir?

    ![NSG birtakım sergileyen Pano istatistikleri isabetler](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- Aşağıdaki resim Göster NSG kuralları ve kaynak hedef akışı bir ağ güvenlik grubu ayrıntılarını İsabeti için eğilim süre:

    - Hızlı bir şekilde tespit hangi Nsg'ler ve NSG kuralları kötü amaçlı akışları geçiş yapma ve hangi olan üst kötü amaçlı IP adresleri bulut ortamınızı erişme
    - Hangi NSG/NSG kuralları izin verme/önemli ölçüde ağ trafiği engelleyen tanımlayın
    - Select üst için ayrıntılı denetleme, bir NSG veya NSG kuralları filtreler.

    ![NSG kuralının isabetler ve üst NSG kuralları için zaman eğilim birtakım sergileyen](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Günlük arama İstatistikler ayrıntıları üst NSG kuralları](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sık sorulan soruların yanıtları almak için bkz: [trafiği analytics SSS](traffic-analytics-faq.md).
