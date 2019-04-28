---
title: (KULLANIM DIŞI) Azure DC/OS kapsayıcısını uygulama erişimi etkinleştirme
description: Azure Container Service DC/OS kapsayıcılarında genel erişimi etkinleştirmek nasıl.
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3e4ba15fa1925ca40ad7760acbd14331fbdd1343
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457382"
---
# <a name="deprecated-enable-public-access-to-an-azure-container-service-application"></a>(KULLANIM DIŞI) Azure Container Service uygulamaya genel erişimini etkinleştirme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bir ACS DC/OS kapsayıcısında [genel aracı havuzu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) internet'e otomatik olarak sunulur. Varsayılan olarak, bağlantı noktaları **80**, **443**, **8080** açılan ve bu bağlantı noktalarını dinlediğini tüm (Genel) kapsayıcı erişilebilir. Bu makalede, Azure Container Service'te uygulamalarınız için daha fazla bağlantı noktalarını açmak gösterilmektedir.

## <a name="open-a-port-portal"></a>Bağlantı noktası (portal)
İlk olarak biz istediğimiz bağlantı noktasını açmanız gerekir.

1. Portalda oturum açın.
2. Azure Container Service'e dağıttığınız kaynak grubunu bulun.
3. Aracı Yük Dengeleyiciyi seçin (benzer olarak adlandırılmış **XXXX-Aracısı-lb-XXXX**).
   
    ![Azure kapsayıcı hizmeti yük dengeleyici](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Tıklayın **araştırmaları** ardından **ekleme**.
   
    ![Azure kapsayıcı hizmeti load balancer araştırmaları](./media/container-service-enable-public-access/add-probe.png)
5. Araştırma formu doldurun ve **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Açıklayıcı bir ad ve araştırma. |
   | Bağlantı noktası |Test etmek için kapsayıcının bağlantı noktası. |
   | Yol |(HTTP modunda olduğunda) Araştırma için göreli bir Web sitesi yolu. HTTPS desteklenmiyor. |
   | Interval |Araştırma arasındaki süre miktarını saniye olarak çalışır. |
   | İyi durumda olmayan eşik |Ardışık araştırma sayısı, kapsayıcı sağlıksız olduğunu düşünmeden önce çalışır. |
6. Geri özelliklerini aracı yük dengeleyicinin, tıklayın **Yük Dengeleme kuralları** ardından **Ekle**.
   
    ![Azure kapsayıcı hizmeti yük dengeleyici kuralları](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Yük Dengeleyici formu doldurun ve **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Yük dengeleyicinin açıklayıcı bir ad. |
   | Bağlantı noktası |Ortak gelen bağlantı noktası. |
   | Arka uç bağlantı noktası |İç ortak bağlantı noktası kapsayıcısının trafiği yönlendirmek için. |
   | Arka uç havuzu |Bu havuzdaki kapsayıcıları Bu yük dengeleyici için hedef olur. |
   | Araştırma |Bir hedef olarak belirlemek için kullanılan araştırma **arka uç havuzu** kötü durumda. |
   | Oturum kalıcılığı |İstemciden gelen trafiğin oturum boyunca nasıl işleneceğini belirler.<br><br>**Hiçbiri**: Art arda gelen istekleri aynı istemciden gelen herhangi bir kapsayıcı tarafından işlenebilir.<br>**İstemci IP**: Aynı istemci IP art arda gelen istekleri aynı kapsayıcı tarafından işlenir.<br>**İstemci IP ve protokol**: Aynı istemci IP'si ve protokolü bileşiminden art arda gelen istekleri aynı kapsayıcı tarafından işlenir. |
   | Boşta kalma zaman aşımı |(Yalnızca TCP) Dakikalar içinde bir TCP/HTTP istemci saklanacağı süre açık bağlı kalmadan *tutma* iletileri. |

## <a name="add-a-security-rule-portal"></a>(Portal) bir güvenlik Kuralı Ekle
Ardından, biz bizim açılan bağlantı noktası güvenlik duvarı üzerinden gelen trafiği yönlendiren bir güvenlik kuralı eklemeniz gerekir.

1. Portalda oturum açın.
2. Azure Container Service'e dağıttığınız kaynak grubunu bulun.
3. Seçin **genel** Aracısı ağ güvenlik grubu (benzer olarak adlandırılmış **XXXX-Aracısı-public-nsg-XXXX**).
   
    ![Azure kapsayıcı hizmeti ağ güvenlik grubu](./media/container-service-enable-public-access/agent-nsg.png)
4. Seçin **gelen güvenlik kuralları** ardından **Ekle**.
   
    ![Azure kapsayıcı hizmeti ağ güvenlik grubu kuralları](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Genel bağlantı noktasına izin verin ve güvenlik duvarı kuralı dolgu **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Güvenlik duvarı kuralı, açıklayıcı bir ad. |
   | Öncelik |Kuralın önceliğini boyut. Düşük sayı öncelik o kadar yüksektir. |
   | Kaynak |Gelen IP adresi aralığı izin verilen ya da bu kural tarafından reddedildi kısıtlayın. Kullanım **herhangi** bir kısıtlama belirtmek için. |
   | Hizmet |Bu güvenlik kuralının içindir önceden tanımlanmış bir hizmetler kümesi seçin. Aksi takdirde kullanın **özel** kendi oluşturmak için. |
   | Protokol |Temel trafiği kısıtlamak **TCP** veya **UDP**. Kullanım **herhangi** bir kısıtlama belirtmek için. |
   | Bağlantı noktası aralığı |Zaman **hizmet** olduğu **özel**, bu kural etkiler bağlantı noktası aralığını belirtir. Tek bir bağlantı noktası gibi kullanabileceğiniz **80**, veya bir aralığı **1024-1500**. |
   | Eylem |İzin vermek veya kriterleri karşılayan trafiği reddetmek. |

## <a name="next-steps"></a>Sonraki adımlar
Arasındaki farklar hakkında bilgi edinin [ortak ve özel DC/OS aracılarının](container-service-dcos-agents.md).

Daha fazla bilgi okuyun [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md).

