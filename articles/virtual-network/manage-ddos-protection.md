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
ms.date: 03/29/2018
ms.author: jdial
ms.openlocfilehash: dd094f2b9cdb9b5eb164dda2925d094cafa7cd89
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33895621"
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS koruması Azure portalını kullanarak standart yönetme

Etkinleştirme ve dağıtılmış engelleme (DDoS) hizmeti koruma devre dışı bırakın ve Azure DDoS koruması standart ile DDoS saldırı azaltmak için telemetri kullanma öğrenin. DDoS koruması standart korur sanal makineler, yük Dengeleyiciler ve Azure sahip uygulama ağ geçitleri gibi Azure kaynakları [genel IP adresi](virtual-network-public-ip-address.md) atanmış. DDoS koruması standart ve özelliklerini hakkında daha fazla bilgi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md).

Bu öğreticideki adımlar herhangi Tamamlanıyor önce Azure portalında oturum https://portal.azure.com atanmış bir hesap ile [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanan uygun listelenen eylemler [izinleri](#permissions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-ddos-protection-plan"></a>DDoS koruması planı oluşturma

DDoS koruması planı DDoS koruması standart, abonelikler arasında etkin olan sanal ağlar kümesini tanımlar. Kuruluş ve bağlantı sanal ağlardan birden çok abonelik aynı planı için bir DDoS koruma planı yapılandırabilirsiniz. DDoS koruması Planın kendisi de plan oluşturma sırasında seçtiğiniz bir abonelik ile ilişkilidir. Korumalı ortak IP adresi sayısı 100 değerini aşmasına durumda abonelik planı uygulanan için fazla kullanım ücretleri yanı sıra planı için yinelenen aylık fatura ilişkilidir. DDoS fiyatlandırma hakkında daha fazla bilgi için bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/ddos-protection/).

Birden fazla plan oluşturulmasını çoğu kuruluş için gerekli değildir. Bir planı abonelikler arasında taşınamaz. Bir planın abonelik değiştirmek istiyorsanız, zorunda [varolan planını silmek](#work-with-ddos-protection-plans) ve yeni bir tane oluşturun.

1. Seçin **kaynak oluşturma** Azure portalında sol üst köşesindeki.
2. Arama *DDoS*. Zaman **DDos koruma planı** arama sonuçlarında görünür.
3. **Oluştur**’u seçin.
4. Girin veya kendi değerlerinizi seçin veya girin veya aşağıdaki örneği değerleri seçin ve ardından **oluşturma**:

    |Ayar        |Değer                                              |
    |---------      |---------                                          |
    |Ad           | myDdosProtectionPlan                              |
    |Abonelik   | Aboneliğinizi seçin.                         |
    |Kaynak grubu | Seçin **Yeni Oluştur** ve girin *myResourceGroup* |
    |Konum       | Doğu ABD                                           |

## <a name="enable-ddos-for-a-new-virtual-network"></a>Yeni bir sanal ağ için DDoS etkinleştir

1. Seçin **kaynak oluşturma** Azure portalında sol üst köşesindeki.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. Girin veya ENTER kendi değerlerinizi seçin veya aşağıdaki örneği değerleri seçin, kalan Varsayılanları kabul edin ve ardından **oluşturma**:

    | Ayar         | Değer                                                        |
    | ---------       | ---------                                                    |
    | Ad            | myVirtualNetwork                                             |
    | Abonelik    | Aboneliğinizi seçin.                                    |
    | Kaynak grubu  | Seçin **var olanı kullan**ve ardından **myResourceGroup** |
    | Konum        | Doğu ABD                                                      |
    | DDos koruması | Seçin **standart** altında ve **DDoS koruması**seçin **myDdosProtectionPlan**. Seçtiğiniz planı sanal ağı'den aynı veya farklı Abonelikteki olabilir, ancak her iki aboneliğin aynı Azure Active Directory Kiracı ilişkilendirilmiş olması gerekir.|

DDoS standart sanal ağ için etkinleştirildiğinde, bir sanal ağı başka bir kaynak grubuna veya aboneliğe taşınamıyor. Taşımanız gerekirse, bir sanal ağ DDoS standart etkin, DDoS standart önce devre dışı bırakın, sanal ağ taşıma ve DDoS standart etkinleştirin. Taşımadan sonra tüm korumalı genel IP adresleri sanal ağ için otomatik olarak ayarlanmış ilke eşikler sıfırlanır.

## <a name="enable-ddos-for-an-existing-virtual-network"></a>Varolan bir sanal ağ için DDoS etkinleştir

1. İçindeki adımları tamamlayarak bir DDoS koruma planı oluşturmak [bir DDoS koruma planı oluşturmak](#create-a-ddos-protection-plan), var olan bir DDoS koruması planı yoksa.
2. Seçin **kaynak oluşturma** Azure portalında sol üst köşesindeki.
3. DDoS koruması için standart, etkinleştirmek istediğiniz sanal ağın adını girin **arama kaynakları, hizmetleri ve belgeleri kutusunu** portalı üstündeki. Sanal ağın adını arama sonuçlarında görüntülendiğinde, onu seçin.
4. Seçin **DDoS koruması**altında **ayarları**.
5. Seçin **standart**. Altında **DDoS koruma planı**, var olan bir DDoS koruması planı ya da 1. adımda oluşturduğunuz planı seçin ve ardından **kaydetmek**. Seçtiğiniz planı sanal ağı'den aynı veya farklı Abonelikteki olabilir, ancak her iki aboneliğin aynı Azure Active Directory Kiracı ilişkilendirilmiş olması gerekir.

## <a name="disable-ddos-for-a-virtual-network"></a>Bir sanal ağ için DDoS devre dışı bırak

1. DDoS koruması standardında için devre dışı bırakmak sanal ağın adını girin **arama kaynakları, hizmetleri ve belgeleri kutusunu** portalı üstündeki. Sanal ağın adını arama sonuçlarında görüntülendiğinde, onu seçin.
2. Seçin **DDoS koruması**altında **ayarları**.
3. Seçin **temel** altında **DDoS koruma planı** ve ardından **kaydetmek**.

## <a name="work-with-ddos-protection-plans"></a>DDoS koruması planları ile çalışma

1. Seçin **tüm hizmetleri** portalın sol üst üzerinde.
2. Girin *DDoS* içinde **filtre** kutusu. Zaman **DDoS koruması planları** sonuçlarında görünmesini, onu seçin.
3. Listeden görüntülemek istediğiniz koruma planı seçin.
4. Planla ilişkili tüm sanal ağları listelenir.
5. Bir planı silmek istiyorsanız, tüm sanal ağlardan onu ilişkilendirmesini kaldırmanız gerekir. Bir sanal ağ planından ilişkilendirmesi için bkz: [bir sanal ağ için devre dışı DDoS](#disable-ddos-for-a-virtual-network).

## <a name="configure-alerts-for-ddos-protection-metrics"></a>DDoS koruması ölçümleri için uyarıları Yapılandır

Herhangi bir etkin azaltma bir saldırı sırasında olduğunda sizi uyarmak için kullanılabilir DDoS koruması ölçümleri Azure İzleyicisi uyarı yapılandırması'nı kullanarak seçebilirsiniz. Koşullar karşılandığında, belirtilen adres bir uyarı e-postası alır:

1. Seçin **tüm hizmetleri** portalın sol üst üzerinde.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Seçin **ölçümleri** altında **paylaşılan hizmetlerini**.
4. Girin veya kendi değerlerinizi seçin veya aşağıdaki örneği değerleri girin, kalan Varsayılanları kabul edin ve ardından **Tamam**:

    |Ayar                  |Değer                                                                                               |
    |---------                |---------                                                                                           |
    |Ad                     | myDdosAlert                                                                                        |
    |Abonelik             | İçin uyarılar almak istediğiniz ortak IP adresi içeren aboneliği seçin.        |
    |Kaynak grubu           | İçin uyarılar almak istediğiniz ortak IP adresi içeren kaynak grubunu seçin.      |
    |Kaynak                 | İçin uyarılar almak istediğiniz ortak IP adresi içeren ortak IP adresi seçin. DDoS bir sanal ağ içindeki kaynaklar için atanan ortak IP adresleri izler. Sanal ağ genel IP adreslerine sahip herhangi bir kaynağa sahip değilseniz, bir ortak IP adresiyle bir kaynak önce oluşturmanız gerekir. Tüm kaynakları kaynak listelenen Yöneticisi aracılığıyla (Klasik olmadan) dağıtılabilir ortak IP adresini izleyebilirsiniz [Azure Hizmetleri için sanal ağ](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network), Azure App Service ortamları ve Azure VPN ağ geçidi dışında. Bu öğretici ile devam etmek için kolayca oluşturabileceğiniz bir [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine.                   |
    |Ölçüm                   | DDoS altında saldırı veya değil                                                                            |
    |Eşik                | 1 - **1** Saldırıya uğramış olduğu anlamına gelir. **0** Saldırıya uğramış kullanmıyorsanız anlamına gelir.                         |
    |Dönem                   | Seçtiğiniz herhangi bir değer seçin.                                                                   |
    |E-posta ile bildir         | Onay kutusunu işaretleyin                                                                                  |
    |Ek yönetici | Bir e-posta sahibi, katkıda bulunan veya abonelik için okuyucu değilseniz, e-posta adresinizi girin. |

    Saldırı algılama birkaç dakika içinde aşağıdaki resimde benzer Azure İzleyici ölçümleri bir e-posta alırsınız:

    ![Saldırı Uyarısı](./media/manage-ddos-protection/ddos-alert.png)


Uyarınız doğrulamak için bir DDoS saldırı benzetimini yapmak için bkz: [doğrulamak DDoS algılama](#validate-ddos-detection).

Ayrıca daha fazla hakkında bilgi edinebilirsiniz [Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [logic apps](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uyarıları oluşturmak için.

## <a name="configure-logging-for-ddos-protection-metrics"></a>DDoS koruması ölçümleri için günlüğe kaydetmeyi yapılandırma

1. Seçin **tüm hizmetleri** portalın sol üst üzerinde.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Altında **ayarları**seçin **tanılama ayarlarını**.
4. Seçin **abonelik** ve **kaynak grubu** oturum istediğiniz genel IP adresini içerir.
5. Seçin **genel IP adresi** için **kaynak türü**, ölçümleri oturum istediğiniz belirli ortak IP adresini seçin.
6. Seçin **aşağıdaki veri toplamak için tanılamayı açın** ve gereksinim duyduğunuz kadar çok aşağıdaki seçeneklerden birini seçin:

    - **Arşiv depolama hesabı**: verileri Azure depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi için bkz: [arşiv tanılama günlükleri](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Bir olay hub'ına akış**: Azure olay hub'ı kullanarak günlüklerini seçmek için bir günlük alıcı sağlar. Olay hub'ları Splunk veya diğer SIEM sistemler ile tümleştirmeyi etkinleştir. Bu seçenek hakkında daha fazla bilgi için bkz: [akışı bir olay hub'ına tanılama günlükleri](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Günlük analizi için Gönder**: Azure OMS günlük analizi hizmeti günlükler yazar. Bu seçenek hakkında daha fazla bilgi için bkz: [toplamak için günlük analizi kullanımda günlüklerini](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Günlük kaydı doğrulamak için bir DDoS saldırı benzetimini yapmak için bkz: [doğrulamak DDoS algılama](#validate-ddos-detection).

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanın

Telemetri bir saldırı Azure İzleyicisi gerçek zamanlı olarak sağlanır. Telemetri, bir ortak IP adresi azaltma altında olan süresi için kullanılabilir. Telemetri önce veya sonra bir saldırı azaltıldığından görmüyorum.

1. Seçin **tüm hizmetleri** portalın sol üst üzerinde.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Seçin **ölçümleri**altında **paylaşılan hizmetlerini**.
4. Seçin **abonelik** ve **kaynak grubu** telemetri için istediğiniz ortak IP adresini içerir.
5. Seçin **genel IP adresi** için **kaynak türü**, telemetri için istediğiniz belirli ortak IP adresini seçin.
6. Bir dizi **kullanılabilir ölçümler** ekranın sol tarafında görünür. İçinde seçili olduğunda, bu ölçümleri grafiği çizilecek **Azure İzleyici ölçümleri grafik** genel bakış ekranında.

Ölçüm adları farklı paket türleri ve paketleri karşılaştırması bayt her ölçümü etiket adları, temel bir yapı ile aşağıdaki gibi sunar:

- **Bırakılan etiket adı** (örneğin, **gelen paketler bırakılan DDoS**): paketi bırakılan/DDoS koruması sistem tarafından iptal etti.
- **İletilen etiket adı** (örneğin **gelen paketler iletilen DDoS**): VIP – filtre uygulanmamış trafiği hedef DDoS sistem tarafından iletilen paket sayısı.
- **Hiçbir etiket adı** (örneğin **gelen paketler DDoS**): bırakılan ve iletilen paketler toplamını temsil eden temizleme sisteme – gelen paketlerin toplam sayısı.

Bir DDoS saldırısının telemetri doğrulamak için benzetimini yapmak için bkz: [doğrulamak DDoS algılama](#validate-ddos-detection).

## <a name="view-ddos-mitigation-policies"></a>DDoS azaltma ilkeleri görüntülemek

DDoS koruması standart her ortak IP adresi korumalı kaynağının DDoS etkin olan sanal ağ için üç otomatik olarak ayarlanmış azaltma ilkeleri (TCP Eşitlemeye, TCP ve UDP) uygular. İlke eşikleri seçerek görüntüleyebilirsiniz **DDoS azaltma tetiklemek için gelen TCP paketleri** ve **DDoS azaltma tetiklemek için gelen UDP paketlerini** aşağıdaki resimde gösterildiği gibi ölçümleri:

![Görünüm azaltma ilkeleri](./media/manage-ddos-protection/view-mitigation-policies.png)

İlke eşikleri, Azure machine learning tabanlı ağ trafiği profil aracılığıyla otomatik yapılandırılmış. Yalnızca ilke eşik aşıldığında DDoS azaltma saldırıya IP adresi için oluşuyor.

## <a name="validate-ddos-detection"></a>DDoS algılama doğrula

Microsoft ile ortaklık [BreakingPoint bulut](https://www.ixiacom.com/products/breakingpoint-cloud) Burada, üretebilir trafiği ortak IP adreslerini benzetimleri DDoS koruması etkin karşı bir arabirim oluşturmak için. Kesme noktası bulut benzetimi sağlar:

- Microsoft Azure DDoS koruması Azure kaynaklarınızı DDoS saldırılara karşı nasıl koruduğunu doğrula
- Olay yanıtlama işleminizi DDoS saldırıya en iyi duruma getirme
- Belge DDoS uyumluluk
- Ağ güvenlik gruplarını eğitme

## <a name="permissions"></a>İzinler

DDoS koruması planları ile çalışmak için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                            | Ad                                     |
| ---------                                         | -------------                            |
| Microsoft.Network/ddosProtectionPlans/read        | DDoS koruması planı okuma              |
| Microsoft.Network/ddosProtectionPlans/write       | DDoS koruması planı güncelle  |
| Microsoft.Network/ddosProtectionPlans/delete      | DDoS koruması planı silme            |
| Microsoft.Network/ddosProtectionPlans/join/action | DDoS koruması planı katılma              |

Bir sanal ağ DDoS korumayı etkinleştirmek için hesabınızı de uygun atanmalıdır [sanal ağlar için Eylemler](manage-virtual-network.md#permissions).

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma ve uygulama [Azure ilke](policy-samples.md) sanal ağlar için