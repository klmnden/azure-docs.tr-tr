---
title: Azure DDoS koruması Azure portalını kullanarak standart yönetme | Microsoft Docs
description: Azure DDoS koruması standart telemetri Azure İzleyicisi'nde bir saldırının etkisini azaltmak için nasıl kullanılacağını öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 5cc03189124dbea56535af2fed84f5ca74aac6cd
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS koruması Azure portalını kullanarak standart yönetme

Etkinleştirme ve dağıtılmış engelleme (DDoS) hizmeti koruma devre dışı bırakın ve Azure DDoS koruması standart ile DDoS saldırı azaltmak için telemetri kullanma öğrenin. DDoS koruması standart korur sanal makineler, yük Dengeleyiciler ve Azure sahip uygulama ağ geçitleri gibi Azure kaynakları [genel IP adresi](virtual-network-public-ip-address.md) atanmış. DDoS koruması standart ve özelliklerini hakkında daha fazla bilgi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS koruması standart (DDoS koruması) şu anda önizlemede değil. DDoS koruması Azure kaynaklarını sınırlı sayıda destek ve yalnızca bölgeleri select bir dizi içinde kullanılabilir. Kullanılabilir bölgelerin bir listesi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md). Yapmanız [hizmet için kayıt](http://aka.ms/ddosprotection) DDoS koruması, aboneliğiniz için etkin almak için sınırlı Önizleme sırasında. Kaydolduktan sonra etkinleştirme işleminde size kılavuzluk Azure DDoS ekibi tarafından kurulur. 

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>DDoS koruması standart - yeni bir sanal ağ etkinleştir

1. http://portal.azure.com adresinden Azure portalında oturum açın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
3. Seçin **ağ**ve ardından **sanal ağ**.
4. Seçilen ayarlarla bir sanal ağ oluşturun. Sanal ağlar oluşturma hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network). Altında **DDoS koruması**, tıklatın **etkin**ve ardından **oluşturma**. Görmüyorsanız, **DDoS koruması**, aboneliğinizi özelliği için kayıtlı değil bir nedeni olması. Tamamlamanız gereken [kayıt](http://aka.ms/ddosprotection), aboneliğinizi özelliği için önce etkinleştirildi bildirim alıp **DDoS koruması** görüntülenir.

    ![Sanal ağ oluşturma](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

    > [!WARNING]
    > Bir bölge seçerken, desteklenen bir bölge listesinden seçin [Azure DDoS koruması standart genel bakış](ddos-protection-overview.md). Desteklenen bir bölge seçmezseniz, sanal ağ oluşturma başarısız olur.

    DDoS koruması etkinleştirme ücretler uygulanan bir uyarı bildirir. DDoS koruması için hiçbir ücret Önizleme sırasında ücrete. Genel kullanılabilirliğine ücret uygulanabilir. Ücretleri ve genel Kullanılabilirlik başlangıcı önce 30 günlük bildirimi alırsınız.

## <a name="enable-ddos-protection-standard---existing-virtual-network"></a>DDoS koruması standart - var olan sanal ağ etkinleştir 

1. Tıklatın **sanal ağlar** Azure portal menüsünde ve sanal ağınızı seçin.
2. Tıklatın **DDoS koruması**, tıklatın **etkin** üzerinde *DDoS koruması* ekran ve ardından **kaydetmek**. Görmüyorsanız, **DDoS koruması**, aboneliğinizi özelliği için kayıtlı değil bir nedeni olması. Tamamlamanız gereken [kayıt](http://aka.ms/ddosprotection), aboneliğinizi özelliği için önce etkinleştirildi bildirim alıp **DDoS koruması** görüntülenir. 

    > [!WARNING]
    > Sanal ağ, desteklenen bir bölgede bulunmalıdır. Desteklenen bölgelerin bir listesi için bkz: [Azure DDoS koruması standart genel bakış](ddos-protection-overview.md).

    DDoS koruması etkinleştirme ücretler uygulanan bir uyarı bildirir. DDoS koruması için hiçbir ücret Önizleme sırasında ücrete. Genel kullanılabilirliğine ücret uygulanabilir. Ücretleri ve genel Kullanılabilirlik başlangıcı önce 30 günlük bildirimi alırsınız.

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Bir sanal ağ üzerinde DDoS koruması devre dışı bırak

1. Tıklatın **sanal ağlar** Azure portal menüsünde ve sanal ağınızı seçin.
2. Tıklatın **DDoS koruması**, tıklatın **devre dışı** üzerinde *DDoS koruması* ekran ve ardından **kaydetmek**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri uyarılarını yapılandırma

Herhangi bir etkin azaltma Azure İzleyicisi uyarı yapılandırması'nı kullanarak bir saldırı sırasında olduğunda uyarı için kullanılabilir DDoS koruması ölçümleri seçebilirsiniz. Belirtilen adresi koşullar gerçekleştiğinde uyarı bir e-posta alır.

1. Tıklatın **İzleyici**ve ardından **ölçümleri**.
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP adresi.
3. Ölçüm için bir e-posta uyarı yapılandırmak için tıklatın **ölçüm uyarı Ekle**. Bir e-posta uyarısı üzerinde herhangi bir ölçümü oluşturulabilir, ancak en bariz ölçümüdür **altında DDoS saldırı veya**. Bir Boole değeri 1 veya 0 budur. A **1** Saldırıya uğramış olduğu anlamına gelir. A **0** Saldırıya uğramış kullanmıyorsanız anlamına gelir.
4. Saldırı zaman gönderilecek şekilde ölçüsünü ayarlamak **altında DDoS saldırı veya** ve **son 5 dakikadan sıfır (0) değerinden durumuna**. Benzer uyarıların diğer ölçümleri için ayarlanabilir.

    ![Ölçümleri yapılandırma](./media/ddos-protection-manage-portal/ddos-metrics.png)

    Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak bildirilir.

    ![Saldırı Uyarısı](./media/ddos-protection-manage-portal/ddos-alert.png) 

Ayrıca daha fazla hakkında bilgi edinebilirsiniz [Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ve [logic apps](../logic-apps/logic-apps-overview.md) uyarıları oluşturmak için.

## <a name="configure-logging-on-ddos-protection-standard-metrics"></a>DDoS koruması standart ölçümleri oturum açmayı Yapılandır

1. Tıklatın **İzleyici**ve ardından **tanılama ayarlarını**.
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP adresi.
3. Tıklatın **aşağıdaki veri toplamak için tanılamayı açın**.

Günlüğe kaydetme için kullanılabilen üç seçenek vardır:

- **Arşiv depolama hesabı**: bir depolama hesabına günlükler yazar.
- **Bir olay hub'ına akış**: bir olay hub'ı kullanarak günlüklerini seçmek için bir günlük alıcı sağlar. Bu Splunk veya diğer SIEM sistemler ile tümleştirme sağlar.
- **Günlük analizi için Gönder**: Azure günlük analizi hizmeti günlükler yazar.

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanın

Telemetri bir saldırı Azure İzleyicisi gerçek zamanlı olarak sağlanır. Telemetri, bir ortak IP adresi azaltma altında olan süresi için kullanılabilir. Telemetri önce veya sonra bir saldırı azaltıldığından görmezsiniz.

1. Tıklatın **İzleyici**ve ardından **ölçümleri**. 
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP adresi. Bir dizi **kullanılabilir ölçümler** ekranın sol tarafında görünür. İçinde seçili olduğunda, bu ölçümleri grafiği çizilecek **Azure İzleyici ölçümleri grafik** genel bakış ekranında. 

Ölçüm adları farklı paket türleri ve paketleri karşılaştırması bayt her ölçümü etiket adları, temel bir yapı ile aşağıdaki gibi sunar:

- **Bırakılan etiket adı (örneğin gelen paketler bırakılan DDoS)**: paketi bırakılan/DDoS koruması sistem tarafından iptal etti.
- **İletilen etiket adı (örneğin: gelen paketler iletilen DDoS)**: VIP – filtre uygulanmamış trafiği hedef DDoS sistem tarafından iletilen paket sayısı.
- **Hiçbir etiket adı (örneğin: gelen paketler DDoS):** bırakılan ve iletilen paketler toplamını temsil eden temizleme sisteme – gelen paketlerin toplam sayısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tanılama günlükleri hakkında daha fazla bilgi](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)