---
title: "Azure kapsayıcı hizmeti kümesi Machine Learning için ölçeklendirme | Microsoft Docs"
description: "Bir ACS küme - otomatik ölçeklendirme ve statik ölçeklendirme ölçeklendirme; Kümedeki düğüm sayısını ölçeklendirme"
services: machine-learning
author: AashishB
ms.author: AashishB
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ms.openlocfilehash: ea91cbf1fb8825563fa9f7e084da7869c973715b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Web hizmeti üretilen işini yönetmenizi küme ölçeklendirme

## <a name="why-scale-the-cluster"></a>Neden küme ölçeklendirme?

Azure kapsayıcı Hizmeti'ni (ACS) küme ölçeklendirme maliyetini azaltmak için en az küme boyutu tutarken hizmetini işleme iyileştirmek için etkili bir yoldur. 

Otomatik ölçeklendirmeyi daha iyi anlamak için aşağıdaki üç web hizmeti çalıştıran bir kümeye örneği göz önünde bulundurun:

![Örneği: Bir kümede üç hizmeti](media/how-to-scale-clusters/three-services.png)

Hizmetler çeşitli yoğun taleplerini sahiptir: Service 1 (mavi satır) gerektirir en yoğun isteğe bağlı, 40 pod'ları, hizmet 2 (turuncu satır) 38 en yoğun gerektirir ve Service 3 (gri çizgi) yoğun 50 gerektirir. Her hizmet için gereken en yüksek kapasiteyi ayrı ayrı ayırırsanız, bu küme en az 40 + 38 + 50 = 128 gerekir toplam pod'ları.

Ancak herhangi bir noktada gerçek pod kullanımı, zaman içindeki siyah kesikli çizgi grafikte tarafından temsil edilen göz önünde bulundurun. Bu durumda, *pod'ları herhangi bir zamanda kullandığı en yüksek sayıda* 64, hangi saat 20:00 Service 3 yoğun olduğunda gerçekleşir. Şu anda hizmet 3 50 pod'ları, ancak yalnızca 9 pod'ları Service 2 kullanır, ve Service 1 yalnızca 5 kullanır. Unutmayın, bu *en yüksek kullanımı* bu küme için. Bu, hiçbir zaman 64 taneden fazla pod'ları--bağımsız olarak en yüksek kullanımı için genişletilmiş üç hizmetler için 128 pod'ları yarım hesaplanan gereksinim küme kullanıyor mu anlamına gelir.

Rescaling tarafından--pod'ları--kümedeki başka bir deyişle, atayarak yerine her hizmetin geçerli talebi karşılamak için yeterli kaynakları yoğun isteğe bağlı tüm hizmetleri için yalnızca gerektirir, küme boyutunu azaltabilirsiniz. Bu basit örnekte, otomatik ölçeklendirmeyi pod'ları 128 gerekli küme boyutu ikiye kesme 64 için gereken sayısını azaltır.

Pod'ları sayısını ölçeklendirmeye hizmetin yanıtlama ciddi etkilenmez şekilde bir dakikadan gerektiren oldukça hızlı bir işlemi var.

> [!NOTE]
> Küme ölçeklendirme isteği gecikmesi sorunları yardımcı olmayacaktır. Operationalization amacıyla ölçeklendirmeyi başarı sayısını artırmak ve hizmet kullanılamıyor hataları azaltır. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>ACS kümenizde Web hizmetlerini ölçeklendirme

Varsayılan model yönetim CLI Küme kurulumu seçeneğini iki aracı ve bir pod ortamınızda yapılandırır. Ayrıca, Kubernetes CLI yükler.

ACS ayarlayarak dağıttığınız web hizmetleri ölçeklendirebilirsiniz:

* Küme Aracısı düğümlerin sayısı
* Aracı düğümler üzerinde çalışan Kubernetes pod çoğaltmaların sayısı

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Kümedeki düğüm sayısını ölçeklendirme

Aşağıdaki komut, kümedeki Aracısı düğümleri sayısı ayarlar:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

Bu işlemin tamamlanması birkaç dakika sürecek. Kümedeki düğüm sayısını ölçeklendirme ile ilgili daha fazla bilgi için bkz: [bir kapsayıcı hizmeti kümesinde Aracısı düğümleri ölçeklendirme](https://docs.microsoft.com/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Bir kümede Kubernetes pod çoğaltmaların sayısı ölçeklendirme
 
Azure Machine Learning CLI veya [Kubernetes Pano] kullanarak kümeye atanan pod çoğaltmaların sayısı ölçeklendirebilirsiniz (https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Kubernetes çoğaltma pod'ları hakkında daha fazla bilgi için bkz: [Kubernetes pod'ları](https://kubernetes.io/docs/concepts/workloads/pods/pod/) belgeleri.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Azure Machine Learning CLI ile bir küme ölçeklendirme

CLI kullanarak bir küme ölçeklendirmek için iki yolu vardır:

- Otomatik Ölçeklendirme
- Statik ölçek

Hizmet oluşturulduğunda ve çoğu durumda tercih edilen ölçeklendirme yöntemdir otomatik ölçeklendirme varsayılan olarak etkindir.

##### <a name="autoscale"></a>Otomatik Ölçeklendirme

Aşağıdaki komut otomatik ölçek sağlar ve çoğaltmaları hizmeti için minimum ve maksimum sayısını ayarlar.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Örneğin, ayarlama `autoscale-min-replicas` 5 beş çoğaltmaları oluşturur. Web hizmeti için en uygun bir numaradan ulaşması için 10 gibi değerlere ayarlamak ve 503 hata iletilerinin sayısını izleyin. Ardından sayısını uygun şekilde ayarlayın.


| Parametre adı | Tür | Açıklama |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | boole | Otomatik ölçeklendirme etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan: true |
| `autoscale-min-replicas` | tamsayı | Pod'ları minimum sayısını belirtir. 0 veya daha büyük olmalıdır. Varsayılan: 1 |
| `autoscale-max-replicas` | tamsayı | Pod'ları üst sınırını belirtir. 1 veya daha büyük olmalıdır. Otomatik ölçeklendirme max çoğaltmaları otomatik ölçeklendirme-min-çoğaltmaları küçük ise, otomatik ölçeklendirme max çoğaltmaları yoksayılacak. Varsayılan: 10 |
| `autoscale-refresh-period-seconds` | tamsayı | Otomatik ölçeklendirme yenilemeleri arasında saniye cinsinden süreyi belirtir. Varsayılan: 1 |
| `autoscale-target-utilization` | tamsayı | 1 ile 100 arasında otomatik ölçeklendirme hedefleyen yüzdesi kullanımı belirtir. Varsayılan: 70 |

Aşağıdaki iki koşul sağlamak için otomatik ölçeklendirme çalışır:

1. Hedef kullanımı karşılanır.
2. Hiçbir zaman ölçeklendirme minimum ve maksimum ayarları aşıyor

Bir küme Hizmetleri'nde küme kaynakları için rekabete giriyorsa. Bir autoscaled hizmeti, kendi isteklerini ikinci (RPS) artar başına olarak küme kaynak kullanımını artırır ve kaynaklar RPS düşüşleri olarak yavaş sunacaktır. Bu tür kaynakları edinmeye hizmeti için mevcut olduğu sürece küme kaynakları isteğe bağlı olarak alınan.

Otomatik ölçeklendirme parametreleri kullanma hakkında daha fazla bilgi için bkz: [Model yönetim komut satırı arabirimi başvurusu](model-management-cli-reference.md) belgeleri.

##### <a name="static-scale"></a>Statik ölçek

Otomatik ölçeklendirmeyi küme boyutu azalmasına izin vermediğinden bu yana genel olarak, statik ölçeklendirme, kaçınılmalıdır. Buna rağmen bazı durumlarda statik ölçeklendirme belirlemeleri. Örneğin, bir küme için tek bir hizmeti ayrılmış, otomatik ölçeklendirmeyi hiçbir avantajı sağlar; tüm küme kaynakları bu hizmete atanması gerekir.

Statik olarak bir küme ölçeklendirmek için otomatik ölçeklendirmeyi kapatılması gerekir. Otomatik ölçeklendirme aşağıdaki komutu kullanarak devre dışı bırakın:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Otomatik ölçeklendirme devre dışı etkinleştirdikten sonra aşağıdaki komutu bir hizmet için yineleme sayısı doğrudan ölçeklendirir.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Kümedeki düğüm sayısını ölçeklendirme ile ilgili daha fazla bilgi için bir kapsayıcı hizmeti kümesindeki Ölçek aracı düğümler bakın.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Ölçeklendirme Kubernetes Panoyu kullanarak çoğaltmaların sayısı

Komut satırında şunu girin:

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

Bağlantı noktası zaten kullanımda olduğunda, aşağıdaki örneğe benzer bir ileti görürsünüz:

```
F0612 21:49:22.459111   59621 proxy.go:137] listen tcp 127.0.0.1:8001: bind: address already in use
```

Kullanarak bir alternatif bağlantı noktası numarası belirtin *--bağlantı noktası* parametresi.

```
kubectl proxy --port=8010
Starting to serve on 127.0.0.1:8010
```

Pano sunucusu başladıktan sonra bir tarayıcı açın ve aşağıdaki URL'yi girin:

```
127.0.0.1:<port number>/ui
```

Pano Ana ekranından tıklatın **dağıtımları** sol gezinti çubuğunda. Gezinti bölmesinde görüntülenmiyorsa bu simgeyi seçin ![üç kısa yatay çizgiler oluşan menü](media/how-to-scale-clusters/icon-hamburger.png) üst sol.

Bu simgeyi tıklatın ve değiştirmek için dağıtımını bulun ![üç dikey noktalarından oluşan menüsü simgesini](media/how-to-scale-clusters/icon-kebab.png) sağ tıkladıktan sonra **Görüntüle/Düzenle YAML**.

Düzen dağıtım ekranda bulun *spec* düğümü değiştirme *çoğaltmaları* değeri ve'ı tıklatın **güncelleştirme**.
