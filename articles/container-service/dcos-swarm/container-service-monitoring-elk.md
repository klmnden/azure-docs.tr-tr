---
title: "Bir Azure DC/OS kümesi - ELK yığın izleme | Microsoft Docs"
description: "Azure kapsayıcı hizmeti kümesi ELK (Elasticsearch, Logstash ve Kibana) ile DC/OS kümesinde izleyin."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Kapsayıcılar, DC/OS, Azure, izleme, elk"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: fcfa277cdd0f3cebc0fbbb23e771fb23ffbe2ca6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a>Azure kapsayıcı hizmeti kümesi ELK ile izleme
Bu makalede, Azure kapsayıcı hizmeti DC/OS kümesinde ELK (Elasticsearch, Logstash, Kibana) yığında dağıtma gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
[Dağıtma](container-service-deployment.md) ve [bağlanmak](../container-service-connect.md) Azure kapsayıcı hizmeti tarafından yapılandırılan bir DC/OS kümesi. DC/OS Pano ve Marathon Hizmetleri keşfedin [burada](container-service-mesos-marathon-ui.md). Ayrıca yükleme [Marathon yük dengeleyici](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK yığın Elasticsearch, Logstash ve kümenizdeki günlüklerini analiz edin ve izlemek için kullanılan bir uçtan uca yığını sağlar Kibana birleşimidir.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>DC/OS kümesinde ELK yığını yapılandırma
DC/OS kullanıcı Arabirimi aracılığıyla erişim [http://localhost:80 /](http://localhost:80/) DC/OS kullanıcı Arabiriminde bir kez gidin **Universe**. Arama ve DC/OS Universe ve bu belirli sırayla Elasticsearch, Logstash ve Kibana yükleyin. Giderseniz yapılandırma hakkında daha fazla bilgiyi **gelişmiş yükleme** bağlantı.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Bir kez ELK kapsayıcıları ve çalışır durumdadır, Marathon-LB erişilebilmesi Kibana etkinleştirmeniz gerekir. Gidin **Hizmetleri** > **kibana**, tıklatıp **Düzenle** aşağıda gösterildiği gibi.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Geç **JSON modu** etiketleri bölümüne kaydırın.
Eklemek gereken bir `"HAPROXY_GROUP": "external"` girişi aşağıda gösterildiği gibi aşağıdaki.
Tıkladığınızda **dağıtmak değişiklikleri**, kapsayıcı yeniden başlatılır.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Kibana HAPROXY panosunda hizmet olarak kaydedildiğini doğrulamak istiyorsanız, HAPROXY 9090 bağlantı noktası üzerinde çalışırken bağlantı noktası 9090 Aracısı kümede açmanız gerekir.
Varsayılan olarak, biz bağlantı noktası 80, 8080 açar ve DC/OS Aracısı kümedeki 443'tür.
Bir bağlantı noktasını açmak ve genel değerlendirme sağlamak için yönergeler verilmiştir [burada](container-service-enable-public-access.md).

HAPROXY panoya erişmek için Marathon-LB yönetici arabirimin açın: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
URL'ye gidin sonra HAPROXY Pano aşağıda gösterildiği gibi görmeniz gerekir ve bir hizmet girişi için Kibana görmeniz gerekir.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


Bağlantı noktası 5601 dağıtılır, Kibana panoya erişmek için bağlantı noktası 5601 açmanız gerekir. Yönergeleri izleyerek [burada](container-service-enable-public-access.md). Kibana Panosu'nda açın: `http://localhost:5601`.

## <a name="next-steps"></a>Sonraki adımlar

* Sistem ve uygulama günlüğü için bkz: iletme ve Kurulum, [DC/OS ELK ile günlük yönetiminde](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* Günlükleri filtreleyin, bkz: [ELK filtreleme günlükleriyle](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

