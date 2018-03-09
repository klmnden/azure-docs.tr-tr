---
title: Azure trafik analizi | Microsoft Docs
description: "Azure ağ güvenlik grubu akış günlükleri trafiği Analytics ile çözümlemeyi öğrenin."
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: jdial
ms.openlocfilehash: c113bbe646a54813a2885b3a9087a0171220f271
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="traffic-analytics"></a>Trafik analizi

Trafik Analytics bulut ağları kullanıcı ve uygulama etkinliğinde görünürlük sağlayan bir bulut tabanlı bir çözümdür. Trafik Analytics Öngörüler Azure bulut trafik akışını sağlamak için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlükleri analiz eder. Trafik Analytics ile şunları yapabilirsiniz:

- Ağ etkinliği, Azure aboneliklerinize arasında görselleştirmek ve etkin tanımlayın.
- Güvenlik tehditlerine karşı belirleyin ve Aç-bağlantı noktaları, internet erişimi ve sanal ağları standart dışı bağlanma makineler (VM) çalışan uygulamalar gibi bilgilerle ağınızın güvenliğini sağlamak.
- Azure bölgeleri ve ağ dağıtımınız için performans ve kapasite en iyi duruma getirmek için internet üzerinden trafik akışı modelleri anlayın.
- Ağ yapılandırma hataları başarısız bağlantılara ağınızda baştaki sabitleme.

## <a name="why-traffic-analytics"></a>Neden analizi trafiği?

İzleme, yönetme ve kendi ağ güvenliği aşılmamış güvenlik, uyumluluk ve performans için bilmeniz önemlidir. Kendi ortamınızdaki bilerek korumak ve onu en iyi duruma getirmek için en önemli çok önemlidir. Genellikle, hangi bağlantı noktalarının internet'e açık olduğu geçerli durumunu bağlanan ağ ağ davranışı, düzensiz ağ davranışı, beklenen ve ani trafiği miktarı artar bilmeniz gerekir.

Bulut ağları Netflow veya eşdeğer Protokolü özellikli yönlendiriciler ve anahtarlar girdiğinde veya bir ağ arabirimi çıktığında gibi IP ağ trafiğini toplamaya özelliği sağlar sahip olduğu şirket içi Kurumsal ağlarda farklı. Trafik akışı verileri çözümleyerek, ağ trafiği akışını ve birim analizini oluşturabilirsiniz.

Giriş hakkında bilgi sağlayan NSG akış günlükleri, Azure sanal ağı varsa ve tek tek ağ arabirimleri, VM'ler veya alt ağlar için çıkış IP trafiği bir ağ güvenlik grubu ile ilişkili. Akış günlükleri ham NSG çözümleyerek ve güvenlik, topoloji ve Coğrafya Intelligence ekleme, trafik analizi, trafik akışını, ortamınızdaki Öngörüler sağlayabilirsiniz. Trafik Analytics en iletişim ana, en iletişim kuran uygulama protokolleri, en konuşmaya konak çiftleri, izin verilen ve engellenen trafik, gelen/giden trafik, açık internet bağlantı noktaları, en engelleme kuralları, trafik gibi bilgiler sağlar Azure veri merkezi, sanal ağ, alt ağlar, başına dağıtım veya standart dışı ağlar.

## <a name="key-components"></a>Başlıca bileşenler 

- **Ağ güvenlik grubu (NSG)**: izin veren veya reddeden bir Azure sanal ağına bağlı kaynaklar için ağ trafiği güvenlik kuralları listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager). Daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ güvenlik grubu (NSG) akış günlükleri**: giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek sağlar. NSG akış günlükleri json biçiminde yazılır ve giden Göster ve gelen akış kuralı başına temelinde akış NIC uygulanır, 5-tanımlama grubu bilgileri (kaynak/hedef IP adresi, kaynak/hedef bağlantı noktası ve protokol) akışı hakkında ve trafiğe izin verildiği veya engellendi. NSG akış günlükleri hakkında daha fazla bilgi için bkz: [NSG akış günlükleri](network-watcher-nsg-flow-logging-overview.md).
- **Günlük analizi**: izleme verilerini toplayan ve merkezi bir depoya veri depolayan bir Azure hizmeti. Bu veriler, olayları, performans verileri ya da Azure API aracılığıyla sağlanan özel veri içerebilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Günlük analizi temel olarak kullanarak ağ Performans İzleyicisi'ni ve trafik Analytics yerleşik olanlar gibi uygulamaları izleme. Daha fazla bilgi için bkz: [oturum analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Günlük analizi çalışma alanı**: günlük analizi, bir Azure hesabı için ilgili verilerin depolandığı örneği. Günlük analizi çalışma alanları hakkında daha fazla bilgi için bkz: [günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Ağ İzleyicisi**: izleme ve Azure ağ senaryo düzeyinde koşullar tanılama sağlar bölgesel bir hizmet. NSG akış günlükleri açma ve kapatma Ağ İzleyicisi'ni kapatabilirsiniz. Daha fazla bilgi için bkz: [Ağ İzleyicisi](network-watcher-monitoring-overview.md#network-watcher).

## <a name="how-traffic-analytics-works"></a>Trafik Analytics nasıl çalışır? 

Trafik Analytics ham NSG akış günlükleri inceler ve aynı kaynak IP adresi, hedef IP adresi, hedef bağlantı noktası ve protokol arasında ortak akışları toplayarak azaltılmış günlükleri yakalar. Örneğin, ana bilgisayar 1 (IP adresi: 10.10.10.10) ana bilgisayar 2'ye iletişim (IP adresi: 10.10.20.10), bağlantı noktası (örneğin, 80) ve Protokolü (örneğin, http) kullanarak 1 saatlik bir süre boyunca 100 kez. Ana bilgisayar 1 & ana bilgisayar 2 100 kez bir süre 1 bağlantı noktası kullanılarak saat iletilen bir giriş, azaltılmış günlüğü var *80* ve Protokolü *HTTP*, 100 girdilerine sahip yerine. Azaltılmış günlükleri günlük analizi çalışma alanında depolanır ve coğrafi konum, güvenlik ve topoloji bilgilerle Gelişmiş. Gelişmiş günlükleri analytics türetmek için daha fazla analiz edilir. Aşağıdaki resimde veri akışı gösterilmektedir:

![Trafik Analytics nasıl çalışır?](media/traffic-analytics/1.png)

## <a name="supported-regions"></a>Desteklenen bölgeler

Trafik Analytics Önizleme'de kullanılabilir. Önizleme sürümü özelliklerinde genel yayın aynı düzeyde kullanılabilirlik ve güvenilirlik özellikleri olarak yok.  Önizleme sürümde karşın, trafiği Analytics Nsg'ler herhangi birinde aşağıdaki bölgeler için kullanabilirsiniz: Batı Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Orta ABD, Orta Güney ABD, Orta ABD, Batı ABD, Batı ABD-2, Batı Avrupa, Kuzey Avrupa, Batı İngiltere, Güney İngiltere, Avustralya Doğu ve Avustralya Güneydoğu. Günlük analizi çalışma alanı, Batı Orta ABD, Doğu ABD, Batı Avrupa, Avustralya Güneydoğu veya Güney UK bölge içinde bulunmalıdır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="enable-network-watcher"></a>Ağ İzleyicisi'ni etkinleştir 

Trafiğini analiz etmek için var olan bir Ağ İzleyicisi olması gerekir veya [bir Azure Ağ İzleyicisini etkinleştirmek](network-watcher-create.md) için analiz etmek istediğiniz Nsg'ler sahip her bölgede trafiği. Trafik analizi, herhangi bir barındırılan Nsg'ler için etkinleştirilebilir [bölgeler desteklenen](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>Ağ kaynak sağlayıcı yeniden kaydedilir 

Önizleme sırasında trafiği Analytics kullanmadan önce ağ kaynak sağlayıcısı yeniden kaydetmeniz gerekir. Tıklatın **deneyin** aşağıdaki kodu kutusunda Azure bulut Kabuğu'nu açın. Bulut Kabuk Azure aboneliğinize, içine otomatik olarak günlüğe kaydeder. Bulut Kabuk açıldıktan sonra ağ kaynak sağlayıcısı yeniden kaydetmek için aşağıdaki komutu girin:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Bir ağ güvenlik grubu seçin 

NSG akış günlüğü etkinleştirmeden önce akışları için oturum için bir ağ güvenlik grubu olması gerekir. Bir ağ güvenlik grubu yoksa bkz [bir ağ güvenlik grubu oluşturun](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) oluşturmak için.

Azure portalının sol tarafta seçin **İzleyici**, ardından **Ağ İzleyicisi**ve ardından **NSG akış günlükleri**. Aşağıdaki resimde gösterildiği gibi bir NSG akış günlüğü, etkinleştirmek istediğiniz ağ güvenlik grubu seçin:

![Bir NSG'yi seçin](media/traffic-analytics/2.png)

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

    Barındırma trafiğini analiz çözümü ve Nsg'ler günlük analizi (OMS) çalışma aynı bölgede olması gerekmez. Örneğin, Nsg'ler Doğu ABD ve Batı ABD olabilir, ancak bir çalışma alanı Batı Avrupa bölgedeki trafiği Analytics olabilir. Birden çok Nsg'ler aynı çalışma alanında yapılandırılabilir.
6. **Kaydet**’i seçin.

    ![Trafik analytics etkinleştir](media/traffic-analytics/3.png)

Trafik analizi için etkinleştirmek istediğiniz diğer Nsg'ler için önceki adımları yineleyin. Akış günlükleri verilerden çalışma alanına gönderilir, böylece yerel yasalarına ve düzenlemelerine ülkenizdeki veri depolama çalışma bulunduğu bölgede izin olun.

## <a name="view-traffic-analytics"></a>Görünüm trafiği analizi

Sol taraftaki Portalı'nın, seçin **tüm hizmetleri**, enter *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** arama sonuçlarında görünür. Trafik analizi ve özelliklerini keşfetmeye başlamak için seçin **Ağ İzleyicisi**, ardından **trafiği Analytics (Önizleme)**.

![Görünüm trafiği analizi](media/traffic-analytics/4.png)

Pano trafiği Analytics önce herhangi bir rapor oluşturmadan önce anlamlı bilgiler çıkarmanıza kendisine için yeterli veri toplama gerekir çünkü ilk kez görünmesi 30 dakika sürebilir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Trafik Analytics tam olarak yapılandırdıktan sonra elde isteyebilirsiniz Öngörüler bazıları şunlardır:

### <a name="find-traffic-hotspots"></a>Trafiği etkin Bul

**Aramak**

- Hangi ana bilgisayarların gönderme veya en çok trafik almasını var mı?
    - Hangi ana bilgisayarların gönderme veya trafiğinin çoğu alma anlama trafiğinin çoğu işleme konakları belirlemenize yardımcı olabilir.
    - Trafik hacmi bir ana bilgisayar için uygun olup olmadığını değerlendirebilirsiniz. Trafik normal davranışın birim ya da daha fazla araştırma belirlenmiştir mu?
- Ne kadar gelen/giden trafik var mı?
    -   Daha fazla gelen trafiği giden daha ya da tam tersini almak için konak beklenen?
- İzin verilen ve engellenen trafik istatistikleri.
    - Neden olan bir konak izin verme veya önemli bir trafik hacmi engelleme?

    Seçin **daha fazla ayrıntı**altında **trafiğinin çoğu konaklarla**, aşağıdaki resimde gösterildiği gibi:

    ![Trafik analytics etkinleştir](media/traffic-analytics/5.png)

- Aşağıdaki resimde, zaman talking beş ana ve akışıyla ilgili ayrıntıları (izin verilen – gelen/giden ve reddedilen - gelen/giden trafik akışları) için eğilimleri belirlemek için bir VM gösterilmiştir:

    ![](media/traffic-analytics/6.png)

**Aramak**

- Hangi en conversing konak çiftleri misiniz?
    - Arka uç Internet trafiğini gibi ön uç arka uç iletişim veya düzensiz davranışı gibi davranış bekleniyordu.
- İzin verilen ve engellenen trafik istatistikleri
    - Neden bir ana bilgisayar izin verme veya engelleme önemli trafik hacmi
- Uygulama protokolü çoğu conversing konak çiftleri arasında en sık kullanılan:
    - Bu uygulamalar, bu ağ üzerinde izin verilir?
    - Uygulamaların düzgün yapılandırılmış? İletişim için uygun Protokolü kullandıkları? Seçin **daha fazla ayrıntı** altında **görüşmeleri'en sık**, aşağıdaki resimde gösterildiği gibi:

    ![](media/traffic-analytics/7.png)

- Aşağıdaki resimde zaman için ilk beş görüşmeleri eğilim gösterir ve akışıyla ilgili ayrıntıları gibi izin verilen gelen ve giden akışlar bir konuşma çifti için engellendi:

    ![](media/traffic-analytics/8.png)

**Aramak**

- Hangi uygulama protokolü ortamınızda en sık kullanılan ve hangi conversing konak çiftleri en uygulama protokolü kullanarak?
    - Bu uygulamalar, bu ağ üzerinde izin verilir?
    - Uygulamaların düzgün yapılandırılmış? İletişim için uygun Protokolü kullandıkları? Ortak bağlantı noktaları 80 ve 443 gibi beklenen davranıştır. Standart iletişim için herhangi bir olağan dışı bağlantı görüntüleniyorsa, bir yapılandırma değişikliği gerektirebilir. Seçin **daha fazla ayrıntı** altında **üst uygulama protokolleri**, aşağıdaki resimde:

    ![](media/traffic-analytics/9.png)

- Aşağıdaki resim Göster ilk beş L7 protokolleri ve (örneğin, izin verilen ve akışlar reddedildi) akışıyla ilgili ayrıntılar için L7 protokolü için eğilim süre:

    ![](media/traffic-analytics/10.png)

    ![](media/traffic-analytics/11.png)

**Aramak**

- Bir VPN ağ geçidi, ortamınızdaki Kapasite Kullanım Eğilimleri.
    - Her VPN SKU belirli miktarda bant genişliği sağlar. VPN ağ geçitleri gereğinden az?
    - Ağ geçitleri kapasite ulaşıyor? Sonraki daha yüksek SKU yükseltmeniz gerekir?
- Hangi bağlantı noktası üzerinden hangi VPN ağ geçidi üzerinden en conversing konakları olduğu?
    - Bu desen normal mi? Seçin **daha fazla ayrıntı** altında **üst etkin VPN bağlantılarını**, aşağıdaki resimde gösterildiği gibi:

    ![](media/traffic-analytics/12.png)

- Aşağıdaki resimde bir Azure VPN ağ geçidi ve akışıyla ilgili ayrıntıları (örneğin, izin verilen akışları ve bağlantı noktaları) kapasite kullanımı için zaman eğilim gösterir:

    ![](media/traffic-analytics/13.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Trafik dağılımı coğrafyaya Görselleştirme

**Aramak**

- Trafik dağılımı üst kaynakları veri merkezi ve uygulama protokolleri konuşmaya üst konuşmaya üst dolandırıcı ağ trafiği bir veri merkezine gibi veri merkezi başına.
    - Bir veri merkezi hakkında daha fazla yük gözlemlerseniz, verimli trafik dağıtım planlayabilirsiniz.
    - Yanlış ağları veri merkezinde konuşmaya varsa, bunları engellemek için NSG kuralları düzeltin.

    Seçin **Canlı geomap görmek için burayı tıklatın** altında **dağıtım Azure veri merkezleri arasında trafiği**, aşağıdaki resimde gösterildiği gibi:

    ![](media/traffic-analytics/14.png)

- Coğrafi harita parametrelerinin seçilmek üst Şerit gibi veri merkezleri gösterir (No/dağıtıldı-dağıtım/etkin/etkin/trafiği Analytics etkin/trafiği Analytics etkin değil) ve etkin Benign/kötü amaçlı trafiği katkıda bulunan ülkelerde Dağıtım:

    ![](media/traffic-analytics/15.png)

- Coğrafi harita trafik dağılımı ülkeler ve kendisine (zararsız trafiği) mavi ve kırmızı (kötü amaçlı trafiği) satırları renkli iletişim Kıtalar bir veri merkezine gösterir:

    ![](media/traffic-analytics/16.png)

    ![](media/traffic-analytics/17.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Trafik dağılımı sanal ağları tarafından Görselleştirme

**Aramak**

- Sanal ağ topolojisi, üst kaynakları sanal ağ ve uygulama protokolleri konuşmaya üst konuşmaya üst dolandırıcı ağlar, sanal ağ trafiğinin başına trafik dağılımı.
    - Hangi sanal ağa, sanal ağ bilerek konuşmaya. Konuşma görülmüyorsa düzeltilebilir.
    - Yanlış ağların bir sanal ağ ile konuşmaya, dolandırıcı ağları engellemek için NSG kuralları düzeltebilirsiniz.
 
    Seçin **VNet trafiği topoloji görmek için burayı tıklatın** altında **sanal ağ dağıtım**, aşağıdaki resimde gösterildiği gibi: 

        ![](media/traffic-analytics/18.png)

- Sanal ağ topolojisi bir sanal ağın (Inter sanal ağ bağlantıları/etkin/etkin değil) gibi parametreleri seçimi, dış bağlantıları, etkin araç ve sanal ağ kötü amaçlı akışları için üst Şerit gösterir.
- Sanal ağ topolojisi akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve ağ güvenlik grupları, göre sanal ağa trafik dağılımı örneğin gösterir:

    ![](media/traffic-analytics/19.png)

    ![](media/traffic-analytics/20.png)

**Aramak**

- Alt ağ, topoloji, üst kaynakları alt ağ ve uygulama protokolleri konuşmaya üst konuşmaya üst rouge ağ trafiği alt ağına başına trafik dağılımı.
    - Hangi alt bilerek hangi alt ağına konuşmaya. Beklenmeyen görüşmeleri görürseniz, yapılandırmanızı düzeltebilirsiniz.
    - Rouge ağları bir alt ağ ile konuşmaya, dolandırıcı ağlar, örneğin engellemek için NSG kuralları yapılandırarak düzeltebilirsiniz.
- Alt ağ topolojisi Active/Inactive alt ağ, dış bağlantıları, etkin araç ve kötü amaçlı akışlar alt ağın gibi parametreleri seçilmek üst Şerit gösterir.
- Alt ağ topolojisi akışlar (izin verilen/engellenen/gelen/giden/Benign/kötü amaçlı), uygulama protokolü ve Nsg'ler, göre sanal ağa trafik dağılımı örneğin gösterir:

    ![](media/traffic-analytics/21.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Bağlantı noktaları ve sanal makineleri trafiğini internet'ten alma görüntüleyin

**Aramak**

- Hangi açık bağlantı noktalarını Internet üzerinden konuşmaya?
    - Beklenmeyen bağlantı noktalarının açık bulunamazsa, yapılandırmanızı düzeltebilirsiniz:

        ![](media/traffic-analytics/22.png)

        ![](media/traffic-analytics/23.png)

**Aramak**

Kötü amaçlı trafiği ortamınızda var mı? Burada, kaynaklanan? Burada için yönlendirilir?

![](media/traffic-analytics/24.png)


### <a name="visualize-the-trends-in-nsg-rule-hits"></a>NSG kuralının isabet eğilimleri Görselleştirme

**Aramak**

- Hangi NSG/kural en isabet var?
- NSG başına üst kaynak ve hedef konuşma çiftleri nelerdir?

    ![](media/traffic-analytics/25.png)

- Aşağıdaki resim Göster NSG kuralları ve kaynak hedef akışı bir ağ güvenlik grubu ayrıntılarını İsabeti için eğilim süre:

    ![](media/traffic-analytics/26.png)

    ![](media/traffic-analytics/27.png)