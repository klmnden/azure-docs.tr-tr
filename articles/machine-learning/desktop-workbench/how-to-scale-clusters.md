---
title: Machine Learning için bir Azure Container Service kümesini ölçeklendirme | Microsoft Docs
description: Bir ACS kümesi - otomatik ölçeklendirme ve statik ölçeklendirme ölçeklendirme; Kümedeki düğüm sayısını ölçeklendirme
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9688b9ba305a2eb59b80b02c0b41a7f4855dd051
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024571"
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Web hizmeti aktarım hızını yönetmek için kümeyi ölçeklendirme

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


## <a name="why-scale-the-cluster"></a>Neden bir kümenin ölçeğini?

Azure Container Service (ACS) küme ölçeklendirme, maliyetini azaltmak için en az için küme boyutu tutarken hizmet aktarım hızını iyileştirmek için etkili bir yoludur. 

Otomatik ölçeklendirme daha iyi anlamak için aşağıdaki üç web hizmeti çalıştıran bir kümeye örneği göz önünde bulundurun:

![Örnek: Bir kümede üç Hizmetleri](media/how-to-scale-clusters/three-services.png)

Çeşitli tepe noktasındaki talepleri hizmetlere sahip: Hizmet 1 (mavi çizgi) gerektirir, en üst düzey talepleri, 40 pod'ları, hizmet 2 (turuncu çizgi) 38 en yoğun ve hizmet 3 (gri çizgi) gerektirir yoğun 50. Her hizmet için gereken en yüksek kapasite ayrı ayrı ayırırsanız, bu küme en az 40 + 38 + 50 = 128 gerekir pod'ların toplam sayısı.

Ancak herhangi bir noktada gerçek bir pod kullanım zaman içinde siyah kesik çizgi grafikte gösterilen göz önünde bulundurun. Bu durumda, *pod'ları herhangi bir zamanda kullanılan en yüksek sayıda* 64, hangi saat 20:00 Service 3, yüksek olduğunda gerçekleşir. Şu anda hizmet 3 50 pod'ları, ancak yalnızca 9 pod'ları Service 2 kullanır, ve hizmet 1 yalnızca 5 kullanır. Unutmayın, bu *en yüksek kullanımı* bu küme için. Başka bir deyişle, hiçbir zaman 64'ten fazla pod--128 pod'ların en yüksek kullanıma yönelik bağımsız olarak ölçeklendirilebilir üç Hizmetleri için yarı hesaplanan gereksinim küme kullanmaz.

Ölçeklendirme tarafından--,--kümedeki diğer bir deyişle, pod'ların atayarak yerine her hizmetin güncel talebi karşılamak için yeterli kaynakları tüm hizmetleri için en üst düzey talepleri yalnızca gerektirir, kümenizin boyutunu azaltabilirsiniz. Bu basit örnekte, otomatik ölçeklendirme 128 pod'ların gerekli küme boyutu yarı yarıya kısaltma 64, gerekli sayısını azaltır.

Pod'ların sayısını ölçeklendirme bir dakikadan az, hizmetin yanıt hızını ciddi etkilemeyecek şekilde gerektiren bir görece işlemdir.

> [!NOTE]
> Küme ölçeklendirme isteği gecikme sorunlarıyla yardımcı olmaz. Kullanıma hazır hale getirme amacıyla büyütme başarı sayısı artırın ve hizmet kullanılamıyor hatalarıyla azaltın. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>ACS kümenize Web hizmetlerini ölçeklendirme

Küme kurulum seçeneğini varsayılan olarak model Yönetimi CLI ortamınıza iki aracı ve bir pod yapılandırır. Kubernetes CLI de yükler.

ACS'ye ayarlayarak dağıttığınız web hizmetleri ölçeklendirebilirsiniz:

* Küme içindeki aracı düğümlerinin sayısı
* Aracı düğümler üzerinde çalışan Kubernetes pod çoğaltmalarının sayısı

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Kümedeki düğüm sayısını ölçeklendirme

Aşağıdaki komut, kümede aracı düğüm sayısı ayarlar:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

İşlemin tamamlanması birkaç dakika sürebilir. Kümedeki düğüm sayısını ölçeklendirme hakkında daha fazla bilgi için bkz. [bir kapsayıcı hizmeti kümesinde aracı düğümü ölçeklendirme](https://docs.microsoft.com/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Kubernetes pod çoğaltmalarının sayısı küme ölçeklendirme
 
Azure Machine Learning CLI kullanarak kümeye atanan pod çoğaltmalarının sayısı ölçeklendirebilirsiniz veya [Kubernetes panosunu](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Kubernetes çoğaltma pod'ları hakkında daha fazla bilgi için bkz. [Kubernetes pod'ların](https://kubernetes.io/docs/concepts/workloads/pods/pod/) belgeleri.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Azure Machine Learning CLI ile bir küme ölçeklendirme

CLI kullanarak bir kümeyi ölçeklendirmek için iki yol vardır:

- Otomatik Ölçeklendirme
- Statik ölçek

Otomatik ölçeklendirme hizmeti oluşturulduğunda ve çoğu durumda tercih edilen ölçeklendirme yöntemidir, varsayılan olarak etkindir.

##### <a name="autoscale"></a>Otomatik Ölçeklendirme

Aşağıdaki komut, otomatik ölçeklendirme olanağı sağlar ve çoğaltmaları hizmeti için minimum ve maksimum sayısını ayarlar.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Örneğin, ayarlamak `autoscale-min-replicas` beş çoğaltmalar 5 oluşturacaksınız. Web hizmeti için en iyi bir numaradan ulaşması için 10 gibi değerlere ayarlayın ve 503 hatalarını sayısını izleyin. Ardından numarası buna uygun şekilde ayarlayabilirsiniz.


| Parametre adı | Tür | Açıklama |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | boole | Otomatik ölçeklendirme etkin olup olmadığını belirtir. Varsayılan: true |
| `autoscale-min-replicas` | integer | Pod'ların en az sayısını belirtir. 0 veya daha büyük olmalıdır. Varsayılan: 1 |
| `autoscale-max-replicas` | integer | Pod'ların maksimum sayısını belirtir. 1 veya daha büyük olmalıdır. Otomatik ölçeklendirme en fazla çoğaltmaları otomatik ölçeklendirme-min-çoğaltmaları daha küçük ise otomatik ölçeklendirme en fazla yineleme göz ardı edilir. Varsayılan: 10 |
| `autoscale-refresh-period-seconds` | integer | Otomatik ölçeklendirme yenilemeler arasındaki saniye cinsinden süreyi belirtir. Varsayılan: 1 |
| `autoscale-target-utilization` | integer | 1 ile 100 arasında otomatik ölçeklendirme hedefleyen yüzdesi kullanımı belirtir. Varsayılan: 70 |

Otomatik ölçeklendirme, aşağıdaki iki koşul emin olmak için birlikte çalışır:

1. Hedef kullanım karşılanır.
2. Hiçbir zaman ölçeklendirme için minimum ve maksimum ayarları aşıyor

Bir kümedeki hizmetlerin küme kaynakları için yarışın. Bir autoscaled hizmeti, istekleri (RP'ler) arttıkça başına olarak küme kaynağı kullanımını artıracaktır ve yavaş RPS düşüşleri kaynakları serbest bırakır. Hizmet almaya böyle kaynaklar var olduğu sürece küme kaynakları isteğe bağlı olarak alınan.

Otomatik ölçeklendirme parametreleri kullanma hakkında daha fazla bilgi için bkz. [Model yönetim komut satırı arabirimi başvuru](model-management-cli-reference.md) belgeleri.

##### <a name="static-scale"></a>Statik ölçek

Otomatik ölçeklendirme, küme boyutu azaltma izin vermediğinden bu yana genel olarak, statik ölçeklendirme kaçınılmalıdır. Buna rağmen bazı durumlarda statik ölçeklendirme önerilir. Örneğin, bir küme için tek bir hizmet ayrılmış, otomatik ölçeklendirme hiçbir avantajı sağlar. Bu hizmet için tüm küme kaynaklarını atanması gerekir.

Statik bir kümenin ölçeğini için otomatik ölçeklendirmeyi devre dışı olmalıdır. Aşağıdaki komutla otomatik ölçeklendirmeyi devre dışı bırakın:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Otomatik ölçeklendirmeyi kapatın etkinleştirdikten sonra aşağıdaki komutu bir hizmet için yineleme sayısı doğrudan ölçeklendirir.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Bir kapsayıcı hizmeti kümesindeki aracı düğümlerini ölçekleme kümedeki düğüm sayısını ölçeklendirme hakkında daha fazla bilgi için bkz.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Kubernetes panosunu kullanarak çoğaltma ölçeklendirme sayısı

Komut satırında girin:

```
kubectl proxy
```

Windows üzerinde Kubernetes yükleme konumu yolu otomatik olarak eklenmez. İlk yükleme klasörüne gidin:

```
c:\users\<user name>\bin
```

Komutu çalıştırdıktan sonra aşağıdaki bilgi iletisini görmeniz gerekir:

```
Starting to serve on 127.0.0.1:8001
```

Bağlantı noktası zaten kullanımdaysa aşağıdaki örneğe benzer bir ileti görürsünüz:

```
F0612 21:49:22.459111   59621 proxy.go:137] listen tcp 127.0.0.1:8001: bind: address already in use
```

Kullanarak bir alternatif bağlantı noktası numarası belirtin *--bağlantı noktası* parametresi.

```
kubectl proxy --port=8010
Starting to serve on 127.0.0.1:8010
```

Pano sunucusu başlattıktan sonra bir tarayıcı açın ve aşağıdaki URL'yi girin:

```
127.0.0.1:<port number>/ui
```

Pano Ana ekranından tıklayın **dağıtımları** sol gezinti çubuğunda. Gezinti Bölmesi görüntülenmiyorsa, bu simgeyi seçin ![kısa üç yatay çizgiler menü](media/how-to-scale-clusters/icon-hamburger.png) sol üstte.

Bu simgeye tıklayın ve değiştirmek için dağıtımını bulun ![menüsü simgesi üç dikey noktayı oluşan](media/how-to-scale-clusters/icon-kebab.png) sağ tıkladıktan sonra **Görüntüle/Düzenle YAML**.

Düzenleme dağıtım ekranındaki bulun *spec* düğümünü değiştirme *çoğaltmaları* değeri ve'ı tıklatın **güncelleştirme**.
