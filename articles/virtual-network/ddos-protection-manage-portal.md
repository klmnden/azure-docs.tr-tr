---
title: "Azure DDoS koruması Azure portalını kullanarak standart yönetme | Microsoft Docs"
description: "Azure DDoS koruması standart telemetri Azure İzleyicisi'nde bir saldırının etkisini azaltmak için nasıl kullanılacağını öğrenin."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 5c599b4cc867dbc9a081af3a081195b998f63954
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS koruması Azure portalını kullanarak standart yönetme

>[!IMPORTANT]
>Azure DDoS koruması standart (DDoS koruması) şu anda önizlemede değil. Sınırlı sayıda Azure kaynaklarını destek DDoS koruması ve bölgeler select sayısı. Yapmanız [hizmet için kayıt](http://aka.ms/ddosprotection) DDoS koruması, aboneliğiniz için etkin almak için sınırlı Önizleme sırasında. Etkinleştirme işlemi boyunca size yol göstermesi için kayıt sırasında Azure DDoS ekibi tarafından kurulur. DDoS koruması Doğu ABD, Batı ABD ve Batı Orta ABD bölgelerde kullanılabilir. Önizleme sırasında hizmeti kullandığınız için sizden ücret istenmese.

Bu makalede Azure portal DDoS koruması etkinleştirmek, DDoS koruması devre dışı bırakın ve bir saldırının etkisini azaltmak için telemetri kullanmak için nasıl kullanılacağı gösterilmektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="enable-ddos-protection"></a>DDoS koruması etkinleştir

DDoS koruması yeni veya var olan bir sanal ağda etkinleştirin.

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Yeni bir sanal ağ oluşturun ve DDoS korumayı etkinleştir

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Sanal ağ bilgilerini girin. Altında *DDoS koruması*, tıklatın **etkin**ve ardından **oluşturma**.

    ![Sanal ağ oluşturma](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

DDoS koruması etkinleştirme ücretler uygulanan bir uyarı bildirir. DDoS koruması için hiçbir ücret Önizleme sırasında ücrete. Genel kullanılabilirlik (GA adresindeki) ücretler tahakkuk eden ve müşterileri ücretleri ve İST başlangıcı önce 30 günlük bildirimi alırsınız

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Varolan bir sanal ağda DDoS korumayı etkinleştir 

1. Tıklatın **sanal ağlar** Azure portal menüsünde ve sanal ağınızı seçin.
2. Tıklatın **DDoS koruması**, tıklatın **etkin** üzerinde *DDoS koruması* ekran ve ardından **kaydetmek**. 

DDoS koruması etkinleştirme ücretler uygulanan bir uyarı bildirir. DDoS koruması için hiçbir ücret Önizleme sırasında ücrete. Genel kullanılabilirlik (GA adresindeki) ücretler tahakkuk eden ve müşterileri ücretleri ve İST başlangıcı önce 30 günlük bildirimi alırsınız

## <a name="disable-ddos-protection"></a>DDoS koruması devre dışı bırak

1. Tıklatın **sanal ağlar** Azure portal menüsünde ve sanal ağınızı seçin.
2. Tıklatın **DDoS koruması**, tıklatın **devre dışı** üzerinde *DDoS koruması* ekran ve ardından **kaydetmek**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri uyarılarını yapılandırma

Azure İzleyici uyarı yapılandırması yararlanarak, herhangi bir etkin azaltma saldırının sırasında olduğunda uyarı için kullanılabilir DDoS koruması ölçümleri seçebilirsiniz. Koşullar karşılandığında, belirtilen adres çubuğunda bir uyarı e-posta alırsınız.

1. Tıklatın **İzleyici**ve ardından **ölçümleri**.
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP.
3. Ölçüm için bir e-posta uyarı yapılandırmak için tıklatın **bir uyarı eklemek için tıklatın**. Bir e-posta uyarısı üzerinde herhangi bir ölçümü oluşturulabilir, ancak en bariz ölçümüdür **altında DDoS saldırı veya**. Bir Boole değeri 1 veya 0 budur. A **1** Saldırıya uğramış olduğu anlamına gelir. A **0** Saldırıya uğramış kullanmıyorsanız anlamına gelir.
4. Saldırı zaman gönderilecek şekilde ölçüsünü ayarlamak **altında DDoS saldırı veya** ve **son 5 dakikadan sıfır (0) değerinden durumuna**. Benzer uyarıların diğer ölçümleri için ayarlanabilir.

    ![Ölçümlerini yapılandırın](./media/ddos-protection-manage-portal/ddos-metrics.png)

    Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak bildirilir.

    ![Saldırı Uyarısı](./media/ddos-protection-manage-portal/ddos-alert.png) 

Ayrıca daha fazla hakkında bilgi edinebilirsiniz [Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ve [logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) uyarıları oluşturmak için.

## <a name="configure-logging-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri oturum açmayı Yapılandır

1. Tıklatın **İzleyici**ve ardından **tanılama ayarlarını**.
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP.
3. Tıklatın **aşağıdaki veri toplamak için tanılamayı açın**.

Günlüğe kaydetme için kullanılabilen üç seçenek vardır:

- **Arşiv depolama hesabı** – bir depolama hesabına günlükler yazar.
- **Bir olay hub'ına akış** – bir olay hub'ı kullanarak günlüklerini seçmek için bir günlük alıcı sağlar. Bu Splunk veya diğer SIEM sistemler ile tümleştirme sağlar.
- **Günlük analizi için Gönder** – Azure OMS günlük analizi hizmetine günlükler yazar.

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanın

Telemetri bir saldırı Azure İzleyicisi gerçek zamanlı olarak sağlanır. Telemetri, bir ortak IP adresi azaltma altında olan süresi için kullanılabilir. Telemetri önce veya sonra bir saldırı azaltıldığından görmezsiniz.

1. Tıklatın **İzleyici**ve ardından **ölçümleri**. 
2. Üzerinde *ölçümleri* ekranında, kaynak türü, kaynak grubu seçin **genel IP adresi**ve Azure genel IP. Kullanılabilir ölçümler bir dizi ekranın sol tarafında görünür. Seçili olduğunda, bu ölçümleri grafiği çizilecek genel bakış ekranında Azure İzleyici ölçümleri grafikte. 

Ölçüm adları farklı paket türleri ve paketleri karşılaştırması bayt her ölçümü etiket adları, temel bir yapı ile aşağıdaki gibi sunar:

- **Bırakılan etiket adı (örneğin gelen paketler bırakılan DDoS):** paketi bırakılan/DDoS koruması sistem tarafından iptal etti.
- **İletilen etiket adı (örneğin: gelen paketler iletilen DDoS):** VIP – filtre uygulanmamış trafiği hedef DDoS sistem tarafından iletilen paket sayısı.
- **Hiçbir etiket adı (örneğin: gelen paketler DDoS):** bırakılan ve iletilen paketler toplamını temsil eden temizleme sisteme – gelen paketlerin toplam sayısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tanılama günlükleri hakkında daha fazla bilgi](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)