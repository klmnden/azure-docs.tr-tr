---
title: (KULLANIM DIŞI) Vamp ile canary sürümü Azure DC/OS kümesinde
description: Vamp hizmetlerini canary sürümü kullanarak ve akıllı trafik bir Azure Container Service DC/OS kümesinde filtreleme uygulama
services: container-service
author: gggina
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: f1b3c08cce2cb33feab899ea082fc6fb40225182
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61458286"
---
# <a name="deprecated-canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>(KULLANIM DIŞI) Azure Container Service DC/OS kümesinde vamp mikro hizmetler ile canary sürümü

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu kılavuzda, biz canary sürümü Azure Container Service'te DC/OS kümesi ile ayarlayın. Kanarya biz canary sürümü tanıtım hizmeti "sava" bırakın ve sonra Akıllı trafik filtreleme uygulayarak bir uyumsuzluk hizmetinin Firefox ile çözümleyin. 

> [!TIP] 
> Bu izlenecek yolda canary sürümü bir DC/OS kümesinde çalışır, ancak canary sürümü ile Kubernetes orchestrator kullanabilirsiniz.
>

## <a name="about-canary-releases-and-vamp"></a>Kanarya sürümleri ve canary sürümü


[Kanarya serbest](https://martinfowler.com/bliki/CanaryRelease.html) olduğu Spotify Netflix ve Facebook gibi yenilikçi kuruluşlar tarafından benimsenen bir akıllı Dağıtım stratejisi. Sorunları azaltır, ağ güvenliği tanıtır ve yenilik artar çünkü bu mantıklı bir yaklaşımdır. Bu nedenle neden tüm şirketlerin kullanmadığınız? Kanarya stratejileri dahil etmek için bir CI/CD işlem hattı genişletme karmaşıklık ekler ve kapsamlı bir devops bilgi ve deneyimlerine gerektirir. Bile çalışmaya başlamadan önce daha küçük şirketler ve kuruluşlar benzer engellemek için yeterli olur. 

[Vamp](https://vamp.io/) kanarya getirin ve bu geçişi kolaylaştırmak için tasarlanmış bir açık kaynak sistemi serbest bırakılması, tercih edilen kapsayıcı Zamanlayıcı Özellikleri. Kanarya işlevselliği canary sürümü'nın yüzde tabanlı piyasaya çıkarma gider. Trafik, filtre ve çeşitli koşullar, örneğin hedef belirli kullanıcılara, IP aralıkları veya cihazları için bölme. Canary sürümü izler ve performans ölçümlerini gerçek verilere dayalı Otomasyon için izin verme, analiz eder. Otomatik geri alma hataları üzerinde ayarlayın veya bireysel hizmet çeşitleri yükü veya gecikme süresi göre ölçeklendirin.

## <a name="set-up-azure-container-service-with-dcos"></a>Azure Container Service DC/OS ile ayarlama



1. [DC/OS kümesi dağıtma](container-service-deployment.md) bir ana ve iki aracı varsayılan boyutu. 

2. [SSH tüneli oluşturma](../container-service-connect.md) DC/OS kümesine bağlanmak için. Bu makalede, tünel 80 numaralı yerel bağlantı noktası üzerindeki kümeye varsayılır.


## <a name="set-up-vamp"></a>Canary sürümü ayarlayın

Bir çalışan DC/OS kümeniz olduğuna göre DC/OS kullanıcı Arabiriminden canary sürümü yükleyebilirsiniz (http:\//localhost:80). 

![DC/OS Kullanıcı Arabirimi](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Yükleme iki aşamada gerçekleştirilir:

1. **Elasticsearch dağıtma**.

2. Ardından **canary sürümü dağıtma** Vamp DC/OS evreni paketini yükleyerek.

### <a name="deploy-elasticsearch"></a>Elasticsearch dağıtma

Canary sürümü ölçümleri toplama ve toplama için elasticsearch'ü gerektirir. Kullanabileceğiniz [magneticio Docker görüntüleri](https://hub.docker.com/r/magneticio/elastic/) uyumlu bir Elasticsearch Vamp yığını dağıtma.

1. DC/OS Arabiriminde Git **Hizmetleri** tıklatıp **hizmeti Dağıt**.

2. Seçin **JSON modu** gelen **yeni hizmeti Dağıt** açılır.

   ![JSON modunu seçin](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Aşağıdaki JSON öğesini yapıştırın. Bu yapılandırma kapsayıcıya 1 GB RAM ve temel sistem durumu denetimi Elasticsearch bağlantı noktası ile çalışır.
  
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
  

3. Tıklayın **dağıtma**.

   DC/OS Elasticsearch kapsayıcı dağıtır. İlerleme durumunu izleyebilirsiniz **Hizmetleri** sayfası.  

   ![deploy e?Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Canary sürümü dağıtma

Elasticsearch olarak raporlar sonra **çalıştıran**, DC/OS Evreni Vamp paket ekleyebilirsiniz. 

1. Git **Evreni** araması **vamp**. 
   ![Vamp üzerinde DC/OS evreni](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Tıklayın **yükleme** canary sürümü yanındaki paketini ve seçin **gelişmiş yükleme**.

3. Ekranı aşağı kaydırın ve aşağıdaki elasticsearch-URL'yi girin: `http://elasticsearch.marathon.mesos:9200`. 

   ![Elasticsearch URL'sini girin](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Tıklayın **gözden geçirmek ve yüklemek**, ardından **yükleme** dağıtımı başlatmak için.  

   DC/OS canary sürümü gerekli tüm bileşenleri dağıtır. İlerleme durumunu izleyebilirsiniz **Hizmetleri** sayfası.
  
   ![Canary sürümü evreni paket olarak dağıtmaya](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Dağıtım tamamlandıktan sonra Vamp UI erişebilirsiniz:

   ![DC/OS hizmette canary sürümü](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
   ![Vamp kullanıcı Arabirimi](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>İlk hizmetinizi dağıtma

Bu canary sürümü çalışmaya şimdi, bir şema hizmetinden dağıtın. 

En basit haliyle, bir [canary sürümü şema](https://vamp.io/documentation/using-vamp/blueprints/) uç noktaları (ağ geçidi), kümeleri ve Hizmetleri dağıtmayı açıklar. Aynı hizmetin farklı çeşitleri kanarya serbest bırakılması veya bir mantıksal gruplar halinde gruplandırmak için canary sürümü kullanan kümeleri / B testi.  

Bu senaryo olarak adlandırılan bir örnek tek parça bir uygulamayı kullanan [ **sava**](https://github.com/magneticio/sava), 1.0 sürümünde olduğu. Docker Hub'ında magneticio/sava:1.0.0 altında olan bir Docker kapsayıcısında tek paketlenmiştir. Uygulama bağlantı noktası 8080 normal olarak çalışır, ancak bu durumda bağlantı altında 9050 kullanıma sunmak istediğiniz. Basit bir şema kullanarak canary sürümü aracılığıyla uygulamayı dağıtın.

1. Git **dağıtımları**.

2. **Ekle**'yi tıklatın.

3. Aşağıdaki şema YAML içinde yapıştırın. Bu şema daha sonraki bir adımda değiştirmemizi yalnızca bir hizmet değişken, bir küme içerir:

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

4. **Kaydet**’e tıklayın. Canary sürümü dağıtımı başlatır.

Dağıtım listelenir **dağıtımları** sayfası. Dağıtım durumunu izlemek için tıklayın.

![Vamp UI - Sava dağıtma](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Kullanıcı arabiriminde Vamp Sava hizmeti](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

İki ağ geçidi oluşturulur, üzerinde listelenme **ağ geçitleri** sayfası:

* çalışan hizmetin (bağlantı noktası 9050) erişmek için tutarlı bir uç noktası 
* canary sürümü yönetilen iç ağ geçidi (daha sonra bu ağ geçidi üzerinde daha fazla). 

![Vamp UI - sava ağ geçitleri](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

Sava hizmeti artık dağıtıldığını, ancak Azure Load Balancer, trafiği henüz iletmek için bilmediği dışarıdan erişim. Hizmete erişmek için Azure ağ yapılandırmasını güncelleştirin.


## <a name="update-the-azure-network-configuration"></a>Azure ağ yapılandırmasını güncelleştirin

Canary sürümü sava hizmet bağlantı noktası 9050 kararlı bir uç noktayı kullanıma sunmayı DC/OS aracı düğümleri üzerinde dağıtılır. Hizmeti DC/OS kümesi dışından erişmek için Küme dağıtımı, Azure ağ yapılandırması için aşağıdaki değişiklikleri yapın: 

1. **Azure Load Balancer yapılandırma** aracıları için (adlı kaynağın **dcos-Aracısı-lb-xxxx**) ile bir durum araştırması ve sava örneklerine 9050 numaralı bağlantı noktasında trafiği iletmek için bir kural. 

2. **Ağ güvenlik grubu güncelleştirme** genel aracıları için (adlı kaynağın **XXXX-Aracısı-public-nsg-XXXX**) 9050 bağlantı noktası üzerinde trafiğe izin vermek için.

Azure portalını kullanarak bu görevleri tamamlamak ayrıntılı adımlar için bkz: [bir Azure Container Service uygulama genel erişimi etkinleştirme](container-service-enable-public-access.md). Bağlantı noktasını 9050 tüm bağlantı noktası ayarlarını belirtin.


Her şeyi oluşturulduktan sonra Git **genel bakış** dikey penceresinde DC/OS aracı yük dengeleyicinin (adlı kaynağın **dcos-Aracısı-lb-xxxx**). Bulma **genel IP adresi**ve bağlantı noktası 9050 sava erişmeye adresini kullanın.

![Azure portalı - genel IP adresini alın](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Bir vamp çalıştırın

Bu uygulamada vamp üretime için istediğiniz yeni bir sürümü olduğunu varsayalım. Artık bunu magneticio/sava:1.1.0 kapsayıcılı hale olduğuna ve kullanıma hazır. Canary sürümü kolayca çalışan dağıtım için yeni hizmetler eklemenizi sağlar. "Birleştirilmiş" hizmetlerin küme içindeki mevcut hizmetlere birlikte dağıtılan ve Ağırlık % 0 atanır. Trafik dağılımı Ayarla kadar yeni birleştirilmiş bir hizmet için trafik yönlendirilir. Vamp UI ağırlık kaydırıcı artımlı ayarlamalar (vamp) ya da anlık bir geri alma için izin vererek dağıtım üzerinde tam denetim verir.

### <a name="merge-a-new-service-variant"></a>Yeni bir hizmet değişken Birleştir

Yeni sava 1.1 hizmeti çalışan dağıtım ile birleştirmek için:

1. Vamp Arabiriminde tıklayın **şemaları**.

2. Tıklayın **Ekle** aşağıdaki blueprint'te YAML yapıştırın: Bu şema (sava_cluster) mevcut küme içinde dağıtmak için bir yeni hizmet değişken (sava: 1.1.0) açıklar.

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
  
3. **Kaydet**’e tıklayın. Blueprint depolanır ve listelenen **şemaları** sayfası.

4. Sava: 1.1 şema Eylem menüsünü açın ve tıklayın **birleştirilecek**.

   ![Vamp UI - şemaları](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Seçin **sava** dağıtım ve tıklatın **birleştirme**.

   ![Vamp UI - birleştirme şema dağıtımı](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Blueprint sava: 1.0.0 yanı sıra açıklanan yeni sava: 1.1.0 hizmet değişken canary sürümü dağıtır **sava_cluster** çalışan dağıtım. 

![UI - güncelleştirilmiş sava dağıtım vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

**Sava_cluster/sava/webport** (küme uç noktası) ağ geçidi de güncelleştirilir, yeni dağıtılan sava: 1.1.0 bir rota ekleniyor. Bu noktada, hiçbir trafik burada yönlendirilir ( **ağırlık** % 0'a ayarlanır).

![Vamp UI - küme ağ geçidi](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Vamp

Her iki sava aynı kümede dağıtılan sürümleri ile taşıyarak aralarındaki trafik dağıtımı ayarlamak **ağırlık** kaydırıcı.

1. Tıklayın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) yanındaki **ağırlık**.

2. % 50/50 %'e tıklayın ekibindeki ağırlık dağıtımı ayarlamak **Kaydet**.

   ![UI - ağ geçidi ağırlık kaydırıcı vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Tarayıcınıza dönün ve birkaç kez daha sava sayfayı yenileyin. Sava uygulamayı şimdi sava: 1.0 sayfa sava: 1.1 sayfası arasında geçiş yapar.

   ![değişen sava1.0 ve sava1.1 Hizmetleri](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Bu değişim sayfasının en iyi "Gizli" veya "Anonim" modu tarayıcınızın statik varlıkları önbelleğe almayı nedeniyle çalışır.
  >

### <a name="filter-traffic"></a>Trafik filtreleme

Dağıtımdan sonra bir uyumsuzluk nedenleri Firefox tarayıcılarda sorunları görüntülemek sava: 1.1.0 içinde bulunan varsayalım. Gelen trafiği filtrelemek ve tüm Firefox kullanıcılar bilinen kararlı sava: 1.0.0 sürümüne yeniden yönlendirmek için canary sürümü ayarlayabilirsiniz. Başka bir herkes geliştirilmiş sava: 1.1.0 avantajlardan yararlanmaya devam ederken bu filtre Firefox kullanıcılar için kesinti anında çözümler.

Vamp kullandığı **koşullar** arasında bir ağ geçidi yolları giden trafiği filtrelemek için. Trafik ilk filtre ve her yol için geçerli olan koşullar göre yönlendirilir. Kalan tüm trafiği, ağ geçidi ağırlığı ayarına göre dağıtılır.

Tüm Firefox kullanıcıları filtrelemek ve bunları eski sava: 1.0.0 yönlendirmek için bir koşul oluşturabilirsiniz:

1. Sava/sava_cluster/webport üzerinde **ağ geçitleri** sayfasında ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) eklemek için bir **koşul** rota sava/sava_cluster/sava:1.0.0/webport için. 

2. Bir koşul girin **Kullanıcı aracısını Firefox ==** tıklatıp ![Vamp UI - Kaydet](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

   % 0 koşulunu varsayılan gücü ile canary sürümü ekler. Trafiği filtrelemeye başlamak için koşul gücü ayarlamanız gerekir.

3. Tıklayın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) değiştirmek için **gücü** durumuna uygulanır.
 
4. Ayarlama **gücü** % 100 ve tıklatın ![Vamp UI - Kaydet](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) kaydetmek için.

   Canary sürümü artık sava: 1.0.0 durumuna (tüm Firefox kullanıcılar) ile eşleşen tüm trafik gönderir.

   ![UI vamp - ağ geçidi için koşul Uygula](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Son olarak, tüm kalan trafik (tüm Firefox olmayan kullanıcılar) yeni sava: 1.1.0 göndermek için ağ geçidi ağırlık ayarlayın. Tıklayın ![Vamp UI - Düzenle](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) yanındaki **ağırlık** ve ekibindeki ağırlık dağıtımı % 100 için rota sava/sava_cluster/sava:1.1.0/webport yönergelerine uygun olacak şekilde ayarladınız.

   Koşul filtrelenmediğinden tüm trafiği, artık yeni sava: 1.1.0 yönlendirilir.

6. Eylem filtresinde görmek için iki farklı tarayıcılar (bir Firefox ve başka bir tarayıcı) açın ve hem de sava hizmete erişim. Diğer tüm tarayıcılar sava: 1.1.0 yönlendirilmiş sırada tüm Firefox istekleri sava: 1.0.0 gönderilir.

   ![UI - giden trafiği filtrelemek vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Özetle

Bu makalede bir DC/OS kümesi üzerinde hızlı bir giriş canary sürümü için oluştu. Yeni başlayanlar için canary sürümü alındı ve, Azure Container Service DC/OS üzerinde çalışan küme, dağıtılan bir hizmetle canary sürümü şema ve ortaya çıkarılan uç noktada (ağ geçidi) erişilebilir.

Biz de güçlü özelliklerinden bazıları canary sürümü üzerinde dokunulan: çalışan dağıtım için yeni bir hizmet değişken birleştirme ve artımlı olarak giriş ve bilinen bir uyumsuzluk çözmek için trafik filtreleme.


## <a name="next-steps"></a>Sonraki adımlar

* İle canary sürümü eylemleri yönetme hakkında daha fazla bilgi [Vamp REST API](https://vamp.io/documentation/api/api-reference/).

* Node.js'de canary sürümü Otomasyon betikleri oluşturmak ve bunları olarak çalıştırın [Vamp iş akışları](https://vamp.io/documentation/using-vamp/v1.0.0/workflows/#create-a-workflow).

* Bkz: ek [canary sürümü öğreticiler](https://vamp.io/documentation/tutorials/).

