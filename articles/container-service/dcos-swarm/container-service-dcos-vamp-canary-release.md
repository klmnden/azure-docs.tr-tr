---
title: "Azure DC/OS kümesinde yalancı sürüm Vamp ile | Microsoft Docs"
description: "Nasıl kullanılacağını Vamp yalancı yayın Hizmetleri ve akıllı trafik bir Azure kapsayıcı hizmeti DC/OS kümesinde filtreleme Uygula"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 4a20091b59f2643ea71cce99c159a5075706e35d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Bir Azure kapsayıcı hizmeti DC/OS kümesinde yalancı yayın mikro Vamp ile

Bu kılavuzda, biz Vamp Azure kapsayıcı hizmeti DC/OS kümesi ile ayarlayın. Biz yalancı Vamp demo hizmeti "sava" yayın ve akıllı trafik filtreleme uygulayarak bir uyumsuzluk Firefox ile hizmetinin çözün. 

> [!TIP] 
> Bu kılavuzda Vamp bir DC/OS kümesinde çalışır, ancak ayrıca Vamp Kubernetes ile orchestrator kullanabilirsiniz.
>

## <a name="about-canary-releases-and-vamp"></a>Yalancı serbest bırakır ve Vamp hakkında


[Yalancı serbest](https://martinfowler.com/bliki/CanaryRelease.html) olan Netflix, Facebook ve Spotify gibi yenilikçi kuruluşlar tarafından benimsenen akıllı Dağıtım stratejisi. Sorunları azaltır, ağ güvenliği tanıtır ve yenilik artırır bulunduğundan, mantıklı bir yaklaşımdır. Bu nedenle neden tüm şirketlerin kullanmadığınız? CI/CD ardışık yalancı stratejileri içerecek şekilde genişletme karmaşıklık ekler ve kapsamlı bir devops bilgi ve deneyimlerine gerektirir. Daha küçük şirket ve kuruluş aynı şekilde bile çalışmaya başlamadan önce engellemek için yeterli olur. 

[Vamp](http://vamp.io/) bu geçişi kolaylaştırmak ve canary hale getirmek için tasarlanmış bir açık kaynak sistemi serbest bırakma, tercih edilen kapsayıcı Zamanlayıcı Özellikleri. Yüzde tabanlı sürülmeleri vamp'ın yalancı işlevselliği gider. Trafik filtre ve çok çeşitli koşullar, örneğin hedef belirli kullanıcılar, IP aralıkları veya cihazlar için bölme. Vamp izler ve performans ölçümleri, gerçek verilerine dayalı Otomasyon izin vermeyi analiz eder. Otomatik geri alma işlemi hatalarla ayarlayın veya bireysel hizmet çeşitleri yük veya gecikme göre ölçeklendirin.

## <a name="set-up-azure-container-service-with-dcos"></a>Azure kapsayıcı hizmeti DC/OS ile ayarlama



1. [DC/OS kümesi dağıtma](container-service-deployment.md) bir ana ve varsayılan boyutunun iki aracı ile. 

2. [SSH tüneli oluşturma](../container-service-connect.md) DC/OS kümesine bağlanmak için. Bu makale, tünel yerel bağlantı noktası 80 üzerinde kümeye varsayar.


## <a name="set-up-vamp"></a>Vamp ayarlayın

Çalışan DC/OS kümesine sahip olduğunuza göre DC/OS UI (http://localhost:80) Vamp yükleyebilirsiniz. 

![DC/OS Kullanıcı Arabirimi](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Yükleme, iki aşamada yapılır:

1. **Elasticsearch dağıtmak**.

2. Ardından **Vamp dağıtmak** Vamp DC/OS universe paketi yükleyerek.

### <a name="deploy-elasticsearch"></a>Elasticsearch dağıtma

Vamp Elasticsearch ölçümleri toplama ve toplama için gerektirir. Kullanabileceğiniz [magneticio Docker görüntüleri](https://hub.docker.com/r/magneticio/elastic/) uyumlu Vamp Elasticsearch yığın dağıtmak için.

1. DC/OS kullanıcı arabirimini Git **Hizmetleri** tıklatıp **hizmeti Dağıt**.

2. Seçin **JSON modu** gelen **yeni hizmeti Dağıt** açılır.

  ![JSON modu seçin](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Aşağıdaki JSON öğesini yapıştırın. Bu yapılandırma, 1 GB RAM ve temel sistem durumu denetimi Elasticsearch bağlantı noktası ile kapsayıcı çalıştırır.
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. Tıklatın **dağıtmak**.

  DC/OS Elasticsearch kapsayıcı dağıtır. Üzerinde ilerleme durumunu izleyebilirsiniz **Hizmetleri** sayfası.  

  ![e dağıtabilir? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp dağıtma

Olarak Elasticsearch raporları bir kez **çalıştıran**, DC/OS Universe Vamp paket ekleyebilirsiniz. 

1. Git **Universe** arayın ve **vamp**. 
  ![DC/OS universe üzerinde vamp](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Tıklatın **yükleme** vamp yanındaki paketini bulun ve seçin **gelişmiş yükleme**.

3. Aşağı kaydırın ve aşağıdaki elasticsearch URL'yi girin: `http://elasticsearch.marathon.mesos:9200`. 

  ![Elasticsearch URL'sini girin](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Tıklatın **gözden geçirin ve yükleyin**, ardından **yükleme** dağıtımı başlatmak için.  

  DC/OS gerekli tüm Vamp bileşenlerini dağıtır. Üzerinde ilerleme durumunu izleyebilirsiniz **Hizmetleri** sayfası.
  
  ![Vamp universe paketi olarak dağıtma](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Dağıtım tamamlandıktan sonra Vamp UI erişebilirsiniz:

  ![DC/OS vamp hizmeti](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![UI vamp](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>İlk hizmetiniz dağıtma

Bu Vamp çalışır durumda artık şeması hizmetinden dağıtın. 

En basit biçimiyle içinde bir [Vamp şeması](http://vamp.io/documentation/using-vamp/blueprints/) uç noktaları (ağ geçidi), kümeler ve Hizmetleri dağıtmak için açıklar. Vamp aynı hizmetin farklı türevleri yalancı serbest bırakma veya bir mantıksal gruplar halinde gruplandırmak için kümeler kullanır / B testi.  

Bu senaryo adlı örnek bir tek yapılı uygulama kullanır [ **sava**](https://github.com/magneticio/sava), sürüm 1.0 olduğu. Monolith Docker Hub magneticio/sava:1.0.0 altında olan bir Docker kapsayıcısı paketlenmiştir. Uygulamanın normal olarak bağlantı noktası 8080 üzerinde çalışır, ancak bu durumda bağlantı 9050 altında kullanıma sunmak istediğiniz. Basit şeması kullanarak Vamp aracılığıyla uygulamayı dağıtın.

1. Git **dağıtımları**.

2. **Ekle**'ye tıklayın.

3. Aşağıdaki şeması YAML yapıştırın. Bu şeması sizi bir sonraki adımda değiştirmek yalnızca bir hizmet değişken ile tek bir küme içerir:

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster to create
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. **Kaydet** düğmesine tıklayın. Vamp dağıtımı başlatır.

Dağıtım listelendiğini **dağıtımları** sayfası. Dağıtım durumunu izlemek için tıklatın.

![UI Sava dağıtma - vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Kullanıcı arabiriminde Vamp Sava hizmeti](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

İki ağ geçidi oluşturulur, hangi listelendiğini **ağ geçitleri** sayfa:

* çalışan hizmetin (bağlantı noktası 9050) erişmek için tutarlı bir uç noktası 
* bir Vamp yönetilen iç ağ geçidi (daha sonra bu ağ geçidi hakkında daha fazla). 

![UI - sava ağ geçitleri vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

Sava hizmeti şimdi dağıtmış olan ancak Azure yük dengeleyici, trafik henüz iletmek için bilmiyor olduğundan, dışarıdan erişemez. Hizmete erişmek için Azure ağ yapılandırmasını güncelleştirin.


## <a name="update-the-azure-network-configuration"></a>Azure ağ yapılandırmasını güncelleştirin

Vamp kararlı bir uç nokta bağlantı noktası 9050 gösterme DC/OS Aracısı düğümlerde sava hizmetini dağıtıldı. DC/OS kümesi dışında hizmetinden erişmek için Küme dağıtımı Azure ağ yapılandırmasında aşağıdaki değişiklikleri yapın: 

1. **Azure yük dengeleyici yapılandırmanız** aracıları için (adlı kaynak **dcos-aracı-lb-xxxx**) ile bir durum araştırması ve sava örneklerine 9050 numaralı bağlantı noktasında trafik iletmek için bir kural. 

2. **Ağ güvenlik grubu güncelleştirme** ortak aracılar için (adlı kaynak **XXXX-aracı-genel-nsg-XXXX**) bağlantı noktası 9050 trafiğine izin vermek için.

Azure Portalı'nı kullanarak bu görevleri tamamlamak ayrıntılı adımlar için bkz: [Azure kapsayıcı hizmeti uygulamanın genel erişimi etkinleştirme](container-service-enable-public-access.md). Bağlantı noktasını 9050 tüm bağlantı noktası ayarlarını belirtin.


Her şeyi oluşturulduktan sonra Git **genel bakış** dikey DC/OS Aracısı Yük Dengeleyici (adlı kaynak **dcos-aracı-lb-xxxx**). Bul **genel IP adresi**ve adres 9050 noktasındaki sava erişmek için kullanabilirsiniz.

![Azure portal - get genel IP adresi](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Yalancı yayın çalıştırın

İstediğiniz bu uygulamanın üretime yalancı sürüm için yeni bir sürümü olduğunu varsayalım. Magneticio/sava:1.1.0 kapsayıcılı olması ve başlamaya hazırsınız. Vamp yeni hizmetler çalışan dağıtımının kolayca eklemenize olanak sağlar. "Birleştirilmiş" hizmetlerin kümedeki varolan hizmetleri yanında dağıtılan ve Ağırlık % 0 olarak atanmış. Trafik dağılımı ayarlamak kadar hiçbir trafik yeni birleştirilmiş bir hizmete yönlendirilir. Ağırlık kaydırıcıyı Vamp kullanıcı arabiriminde artımlı ayarlamalar (yalancı sürüm) ya da anlık bir geri alma için izin vererek bu dağıtım üzerinde tam denetim verir.

### <a name="merge-a-new-service-variant"></a>Yeni bir hizmet değişken birleştirme

Çalışan dağıtımının yeni sava 1.1 hizmetiyle birleştirmek için:

1. Vamp UI'ı tıklatın **planlar**.

2. Tıklatın **Ekle** ve aşağıdaki şeması YAML yapıştırın: var olan küme içinde (sava_cluster) dağıtmak için bir yeni hizmet değişken (sava: 1.1.0) bu şeması açıklar.

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster to update
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
  ```
  
3. **Kaydet** düğmesine tıklayın. Şeması depolanır ve listelenen **planlar** sayfası.

4. Sava: 1.1 şeması Eylem menüsünde açıp tıklatın **birleştirilecek**.

  ![UI - planlar vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Seçin **sava** dağıtım ve tıklatın **birleştirme**.

  ![UI - dağıtım için birleştirme şeması vamp](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp dağıtır sava: 1.0.0 yanında şeması açıklanan yeni sava: 1.1.0 hizmeti değişken **sava_cluster** çalışan dağıtım. 

![UI - güncelleştirilmiş sava dağıtım vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

**Sava_cluster/sava/webport** ağ geçidi (küme uç noktası) de güncelleştirilir, yeni dağıtılan sava: 1.1.0 bir yol ekleme. Bu noktada, hiçbir trafik burada yönlendirilen ( **ağırlık** %0 olarak ayarlanır).

![UI - küme ağ geçidi vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Yalancı sürüm

Her iki aynı kümede dağıtılan sava sürümleriyle taşıyarak aralarındaki trafik dağıtım Ayarla **ağırlık** kaydırıcı.

1. Tıklatın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) yanına **ağırlık**.

2. % 50 / % 50'ye tıklayın ağırlık dağıtımı ayarlayın **kaydetmek**.

  ![UI - ağ geçidi ağırlık kaydırıcı vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Tarayıcınıza geri dönün ve birkaç kere sava sayfayı yenileyin. Sava uygulamayı şimdi sava: 1.0 sayfa sava: 1.1 sayfa arasında geçiş yapar.

  ![değişen sava1.0 ve sava1.1 Hizmetleri](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Bu değişim sayfasının en iyi "Incognito" veya "Anonim" modu tarayıcınızın statik varlıklarını önbelleğe alma nedeniyle çalışır.
  >

### <a name="filter-traffic"></a>Trafik filtre

Dağıtımdan sonra bir uyumsuzluk sava: 1.1.0 nedenleri Firefox tarayıcılarda sorunları görüntülemek içinde bulunan varsayalım. Gelen trafiği filtrelemek ve bilinen kararlı sava: 1.0.0 için tüm Firefox kullanıcıları geri doğrudan Vamp ayarlayabilirsiniz. Başka bir herkes geliştirilmiş sava: 1.1.0 avantajlarından devam ederken bu filtre kesintisi Firefox kullanıcılar için anında çözümler.

Kullanır vamp **koşullar** filtre trafiği bir ağ geçidi yollar arasında. Trafik ilk filtre ve her yolu uygulanan koşullar göre yönlendirilmiş. Tüm kalan trafik, ağ geçidi ağırlığı ayarına göre dağıtılır.

Tüm Firefox kullanıcıları filtrelemek ve eski sava: 1.0.0 yönlendirmek için bir koşul oluşturabilirsiniz:

1. Sava/sava_cluster/webport üzerinde **ağ geçitleri** sayfasında, ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) eklemek için bir **KOŞULU** rota sava/sava_cluster/sava:1.0.0/webport için. 

2. Koşul girin **kullanıcı aracısı Firefox ==** tıklatıp ![Vamp UI - Kaydet](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp varsayılan gücü koşuluyla %0 ekler. Trafik filtreleme başlatmak için koşul gücü ayarlamanız gerekir.

3. Tıklatın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) değiştirmek için **gücü** durumuna uygulanır.
 
4. Ayarlama **gücü** % 100 ve tıklatın ![Vamp UI - Kaydet](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) kaydetmek için.

  Vamp şimdi sava: 1.0.0 durumuna (tüm Firefox kullanıcılar) ile eşleşen tüm trafik gönderir.

  ![UI vamp - ağ geçidi koşul Uygula](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Son olarak, tüm kalan trafiği (tüm Firefox olmayan kullanıcılar) yeni sava: 1.1.0 göndermek için ağ geçidi ağırlığı ayarlayın. Tıklatın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) yanına **ağırlık** ve Ağırlık dağıtımı % 100 rota sava/sava_cluster/sava:1.1.0/webport yönergelerine uygun şekilde ayarlayın.

  Koşul tarafından filtrelenmiş olmayan tüm trafiği şimdi yeni sava: 1.1.0 yönlendirilir.

6. Eylem filtresinde görmek için iki farklı tarayıcılar (bir Firefox ve başka bir tarayıcı) açın ve sava hizmet hem de erişin. Diğer tüm tarayıcılar sava: 1.1.0 yönlendirilir sırada tüm Firefox istekleri sava: 1.0.0 gönderilir.

  ![UI - filtre trafiği vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Özetle

Bu makalede bir giriş Vamp bir DC/OS kümesinde oluştu. Yeni başlayanlar Vamp aldı ve, Azure kapsayıcı hizmeti DC/OS üzerinde çalışan küme, hizmet Vamp şeması ile dağıtılan ve (ağ geçidi) sunulan uç noktada erişilebilir.

Biz de güçlü özelliklerinden bazıları Vamp üzerinde işlemdeki: çalışan dağıtımına yeni bir hizmet değişken birleştirme ve artımlı olarak Tanıtımı sonra bilinen uyumsuzluğu gidermek için trafik filtreleme.


## <a name="next-steps"></a>Sonraki adımlar

* Vamp Eylemler ile yönetme hakkında bilgi edinin [Vamp REST API](http://vamp.io/documentation/api/api-reference/).

* Node.js içinde Vamp Otomasyon betikleri oluşturmak ve bunları olarak çalıştırma [Vamp iş akışları](http://vamp.io/documentation/tutorials/create-a-workflow/).

* Bkz: ek [VAMP öğreticileri](http://vamp.io/documentation/tutorials/overview/).

