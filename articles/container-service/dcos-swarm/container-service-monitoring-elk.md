---
title: (KULLANIM DIŞI) Bir Azure DC/OS kümesini - ELK yığını izleme
description: Bir DC/OS kümesinde ELK (Elasticsearch, Logstash ve Kibana) ile Azure Container Service kümesini izleme.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 342cf23db2df7d7c79a2b56df96d1a78d6ba215e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61467777"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-elk"></a>(KULLANIM DIŞI) ELK ile bir Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, sizi Azure Container Service DC/OS kümesinde ELK (Elasticsearch, Logstash, Kibana) yığını dağıtma gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
[Dağıtma](container-service-deployment.md) ve [bağlanma](../container-service-connect.md) Azure Container Service tarafından yapılandırılan bir DC/OS kümesi. Pano DC/OS ve Marathon hizmetlerini keşfedin [burada](container-service-mesos-marathon-ui.md). Ayrıca yükleme [Marathon yük dengeleyici](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK yığını Elasticsearch, Logstash ve izlemek ve kümenizde günlüklerini çözümlemek için kullanılan bir uçtan uca yığın sağlayan Kibana birleşimidir.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>DC/OS kümesinde ELK yığını yapılandırma
DC/OS kullanıcı arabiriminize erişin [ http://localhost:80/ ](http://localhost:80/) DC/OS Arabiriminde bir kez gidin **Evreni**. Arama ve DC/OS Evreni ve bu belirli bir sırayla Elasticsearch, Logstash ve kibana'yı yükleyin. Giderseniz yapılandırması hakkında daha fazla bilgi edinebilirsiniz **gelişmiş yükleme** bağlantı.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Bir kez çalışır durumdadır ve ELK kapsayıcılar, Marathon-LB erişilecek kibana'yı etkinleştirmeniz gerekir. Gidin **Hizmetleri** > **kibana**, tıklatıp **Düzenle** aşağıda gösterildiği gibi.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Geç **JSON modu** ve etiketler bölümüne kaydırın.
Eklemek istediğiniz bir `"HAPROXY_GROUP": "external"` giriş burada gösterildiği gibi aşağıdaki.
' A tıkladığınızda **dağıtma değişiklikleri**, kapsayıcı yeniden başlatılır.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Kibana HAPROXY panosunda hizmet olarak kayıtlı olduğunu doğrulamak istiyorsanız, HAPROXY 9090 bağlantı noktasında çalışan Aracısı kümede 9090 bağlantı noktası açmanız gerekir.
Varsayılan olarak, şu bağlantı noktası 80, 8080 açın ve DC/OS Aracısı kümedeki 443.
Bağlantı noktası ve genel değerlendirme sağlamak için yönergeler verilmiştir [burada](container-service-enable-public-access.md).

HAPROXY panoya erişmek için Marathon-LB yönetici arabirimin açın: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
URL'ye gidin, sonra aşağıda gösterildiği gibi HAPROXY Pano görmeniz gerekir ve Kibana için bir hizmet girişi görmeniz gerekir.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


5601 numaralı dağıtılır, Kibana panoya erişmek için 5601 numaralı bağlantı noktasını açmanız gerekir. Yönergeleri izleyerek [burada](container-service-enable-public-access.md). Kibana panonuza açılacağını: `http://localhost:5601`.

## <a name="next-steps"></a>Sonraki adımlar

* Sistem ve uygulama günlüğü iletme ve Kurulum [DC/OS ELK ile İzleme Günlüğü Yönetimi](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* Günlükleri filtrelemek için bkz: [ELK ile izleme günlüklerini filtreleme](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

