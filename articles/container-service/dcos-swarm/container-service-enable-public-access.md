---
title: "Azure DC/OS kapsayıcı uygulama erişimi etkinleştir"
description: "Azure kapsayıcı hizmeti DC/OS kapsayıcılarında genel erişimi etkinleştirmek nasıl."
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: aedc97335a0b9ad00cf653477b62bf530b556900
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="enable-public-access-to-an-azure-container-service-application"></a>Azure kapsayıcı hizmeti uygulamanın genel erişimi etkinleştir

ACS herhangi bir DC/OS kapsayıcısına [ortak aracı havuzu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) otomatik olarak internet erişimine açıktır. Varsayılan olarak, bağlantı noktaları **80**, **443**, **8080** açık olan ve bu bağlantı noktalarını dinler tüm (Genel) kapsayıcı erişilebilir. Bu makalede daha fazla bağlantı noktalarının, uygulamalarınız için Azure kapsayıcı Hizmeti'nde nasıl açılacağı gösterilmektedir.

## <a name="open-a-port-portal"></a>Bağlantı noktası (portal) açın
İlk olarak, biz biz istediğiniz bağlantı noktasını açmanız gerekir.

1. Portalda oturum açın.
2. Azure kapsayıcı hizmeti için dağıttığınız kaynak grubunu bulun.
3. Aracı yük dengeleyicinin seçin (adlandırılmış benzer **XXXX-aracı-lb-XXXX**).
   
    ![Azure kapsayıcı hizmeti yük dengeleyici](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Tıklatın **yoklamaları** ve ardından **eklemek**.
   
    ![Azure kapsayıcı hizmeti yük dengeleyici yoklamaları](./media/container-service-enable-public-access/add-probe.png)
5. Araştırma formu doldurun ve **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Yoklama, açıklayıcı bir ad. |
   | Bağlantı noktası |Test etmek için kapsayıcı bağlantı noktası. |
   | Yol |(HTTP modunda olduğunda) Araştırma göreli Web sitesi yolu. HTTPS desteklenmiyor. |
   | Aralık |Araştırma arasındaki süreyi saniye cinsinden çalışır. |
   | Sağlıksız durum eşiği. |Arka arkaya araştırma sayısı kapsayıcı sağlıksız olduğunu düşünmeden önce çalışır. |
6. Geri aracı yük dengeleyicinin, Özellikler **Yük Dengeleme kuralları** ve ardından **Ekle**.
   
    ![Azure kapsayıcı hizmeti yük dengeleyici kuralları](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Yük Dengeleyici formu doldurun ve **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Yük dengeleyicinin açıklayıcı bir ad. |
   | Bağlantı noktası |Genel gelen bağlantı noktası. |
   | Arka uç bağlantı noktası |Kapsayıcı trafiği yönlendirmek için ortak iç bağlantı noktası. |
   | Arka uç havuzu |Bu havuz kapsayıcılarında Bu yük dengeleyici için hedef olacaktır. |
   | Araştırma |Bir hedef belirlemek için kullanılan araştırma **arka uç havuzu** iyi değil. |
   | Oturum kalıcılığı |İstemciden gelen trafiğin oturum boyunca nasıl işleneceğini belirler.<br><br>**Hiçbiri**: aynı istemciden art arda gelen istekleri tüm kapsayıcı tarafından işlenebilir.<br>**İstemci IP**: aynı istemci IP adresinden art arda gelen istekleri aynı kapsayıcı tarafından işlenir.<br>**İstemci IP ve Protokolü**: aynı istemci IP'si ve protokolü bileşiminden art arda gelen istekleri aynı kapsayıcı tarafından işlenir. |
   | Boşta kalma zaman aşımı |(Yalnızca TCP) Dakika cinsinden bir TCP/HTTP istemci saklama süresi açmak öğesine bağlı kalmadan *tutma* iletileri. |

## <a name="add-a-security-rule-portal"></a>Güvenlik Kuralı (portal) Ekle
Ardından, şu güvenlik duvarı aracılığıyla bizim açılmış bağlantı noktasından trafiğini yönlendiren bir güvenlik kuralı eklemeniz gerekir.

1. Portalda oturum açın.
2. Azure kapsayıcı hizmeti için dağıttığınız kaynak grubunu bulun.
3. Seçin **ortak** Aracısı ağ güvenlik grubu (adlandırılmış benzer **XXXX-aracı-genel-nsg-XXXX**).
   
    ![Azure kapsayıcı hizmeti ağ güvenlik grubu](./media/container-service-enable-public-access/agent-nsg.png)
4. Seçin **gelen güvenlik kuralları** ve ardından **Ekle**.
   
    ![Azure kapsayıcı hizmeti ağ güvenlik grubu kuralları](./media/container-service-enable-public-access/add-firewall-rule.png)
5. ' I tıklatın ve genel bağlantı noktanızın izin vermek güvenlik duvarı kuralı dolgu **Tamam**.
   
   | Alan | Açıklama |
   | --- | --- |
   | Ad |Güvenlik duvarı kuralı, açıklayıcı bir ad. |
   | Öncelik |Kural için öncelik derecesi. Düşük sayı öncelik o kadar yüksektir. |
   | Kaynak |Bu kural tarafından izin verilecek ve reddedilecek şekilde gelen IP adres aralığını kısıtlayın. Kullanım **herhangi** bir kısıtlama belirtmemek için. |
   | Hizmet |Bu güvenlik kuralı içindir önceden tanımlanmış Hizmetleri kümesi seçin. Aksi takdirde kullanmak **özel** kendi oluşturmak için. |
   | Protokol |Temel trafiği kısıtlamak **TCP** veya **UDP**. Kullanım **herhangi** bir kısıtlama belirtmemek için. |
   | Bağlantı noktası aralığı |Zaman **hizmet** olan **özel**, bu kural etkileyen bağlantı noktası aralığını belirtir. Tek bir bağlantı noktası gibi kullanabilir **80**, veya bir aralık **1024 1500**. |
   | Eylem |İzin ver veya Reddet ölçütleri karşılayan trafiği. |

## <a name="next-steps"></a>Sonraki adımlar
Arasındaki farklar hakkında bilgi edinin [ortak ve özel DC/OS aracıları](container-service-dcos-agents.md).

Hakkında daha fazla bilgi okuyun [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md).

