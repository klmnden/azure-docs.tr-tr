---
title: Azure DDoS koruması Azure portalını kullanarak standart yönetme
titlesuffix: Azure Virtual Network
description: Azure DDoS koruması standart telemetri Azure İzleyici'de bir saldırının etkilerini hafifletmek için nasıl kullanılacağını öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/17/2019
ms.author: kumud
ms.openlocfilehash: 53185caa6a0492702035041a893f20a78cf1ea4d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65911251"
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS koruması Azure portalını kullanarak standart yönetme

Etkinleştir ve dağıtılmış engelleme (DDoS) hizmeti koruma devre dışı bırakın ve telemetri ile standart Azure DDoS koruması, DDoS saldırılarının etkisini azaltmak için kullanma hakkında bilgi edinin. DDoS koruması standart sanal makineler, yük Dengeleyiciler ve sahip bir Azure uygulama ağ geçitleri gibi Azure kaynaklarını koruyan [genel IP adresi](virtual-network-public-ip-address.md) atanmış. DDoS koruması standart ve özellikleri hakkında daha fazla bilgi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md).

Bu öğreticide herhangi tamamlama adımları önce adresinden Azure portalında oturum https://portal.azure.com atanmış bir hesap ile [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanan uygun listelenen eylemler [izinleri](#permissions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-ddos-protection-plan"></a>Bir DDoS koruması planı oluşturma

Bir DDoS koruma planı DDoS koruma standardını, abonelikler arasında etkin olan sanal ağlar kümesi tanımlar. Bir DDoS koruma planı kuruluşunuz ve bağlantı aynı planı için birden çok aboneliklerden Sanal Ağları için yapılandırabilirsiniz. DDoS koruma planı kendisi de plan oluşturma sırasında seçtiğiniz abonelik ile ilişkilidir. DDoS koruma planı, bölgeler ve abonelikler üzerinde çalışır. Örnek-kiracınızda bölge Doğu ABD ve bağlantı #1. abonelik planı oluşturabilirsiniz. Aynı planı için sanal ağlar farklı bölgelerde, diğer Aboneliklerdeki kiracınız bağlanabilir. Korumalı genel IP adresi sayısı 100 değerini aşmasına durumunda abonelik planı doğurur için fazla kullanım ücretleri, yanı sıra planı için yinelenen aylık fatura ilişkilidir. DDoS fiyatlandırması hakkında daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/ddos-protection/).

Birden fazla plan oluşturulmasını çoğu kuruluş için gerekli değildir. Bir plan, abonelikler arasında taşınamaz. Bir planın aboneliğinizi değiştirmek istiyorsanız, zorunda [var olan bir planı silme](#work-with-ddos-protection-plans) ve yeni bir tane oluşturun.

1. Seçin **kaynak Oluştur** Azure portalında sol üst köşesindeki.
2. Arama *DDoS*. Zaman **DDos koruma planı** arama sonuçlarında görünür.
3. **Oluştur**’u seçin.
4. Girin veya kendi değerlerinizi seçin veya girin veya aşağıdaki örneği değerleri seçin ve ardından **Oluştur**:

    |Ayar        |Değer                                              |
    |---------      |---------                                          |
    |Ad           | myDdosProtectionPlan                              |
    |Abonelik   | Aboneliğinizi seçin.                         |
    |Kaynak grubu | Seçin **Yeni Oluştur** girin *myResourceGroup* |
    |Location       | Doğu ABD                                           |

## <a name="enable-ddos-for-a-new-virtual-network"></a>Yeni bir sanal ağ için DDoS etkinleştir

1. Seçin **kaynak Oluştur** Azure portalında sol üst köşesindeki.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. Girin veya ENTER kendi değerlerinizi seçin veya aşağıdaki örneği değerleri seçin, kalan varsayılan değerleri kabul edin ve ardından **Oluştur**:

    | Ayar         | Değer                                                        |
    | ---------       | ---------                                                    |
    | Ad            | myVirtualNetwork                                             |
    | Abonelik    | Aboneliğinizi seçin.                                    |
    | Kaynak grubu  | **Var olanı kullan**’ı seçin ve sonra **myResourceGroup** seçeneğini belirleyin |
    | Location        | Doğu ABD                                                      |
    | DDos koruması | Seçin **standart** altındaki **DDoS koruması**seçin **myDdosProtectionPlan**. Seçtiğiniz planı aynı veya farklı Abonelikteki sanal ağdan olabilir ancak her iki aboneliğin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir.|

DDoS standart sanal ağ için etkinleştirildiğinde bir sanal ağ başka bir kaynak grubuna veya aboneliğe taşınamıyor. Taşımanız gerekirse, DDoS standart bir sanal ağda etkin, DDoS standart ilk devre dışı bırakın, sanal ağ taşıma ve DDoS standart'ı etkinleştirin. Taşıma sonrasında, tüm korumalı genel IP adresleri sanal ağ için otomatik olarak ayarlanmış ilke eşikler sıfırlanır.

## <a name="enable-ddos-for-an-existing-virtual-network"></a>Mevcut bir sanal ağı DDoS etkinleştir

1. İçindeki adımları tamamlayarak bir DDoS koruma planı oluşturma [bir DDoS koruma planı oluşturma](#create-a-ddos-protection-plan), var olan bir DDoS koruma planı yoksa.
2. Seçin **kaynak Oluştur** Azure portalında sol üst köşesindeki.
3. DDoS koruması için standart, etkinleştirmek istediğiniz sanal ağ adını **arama kaynakları, hizmetleri ve belgeleri kutusu** portalın üst kısmındaki. Sanal ağın adı arama sonuçlarında görüntülendiğinde seçin.
4. Seçin **DDoS koruması**altında **ayarları**.
5. Seçin **standart**. Altında **DDoS koruma planı**var olan bir DDoS koruma planı ya da 1. adımda oluşturduğunuz planı seçin ve ardından **Kaydet**. Seçtiğiniz planı aynı veya farklı Abonelikteki sanal ağdan olabilir ancak her iki aboneliğin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir.

## <a name="disable-ddos-for-a-virtual-network"></a>Sanal ağı DDoS devre dışı bırak

1. İçinde DDoS koruma standardını devre dışı bırakmak istediğiniz sanal ağ adını **arama kaynakları, hizmetleri ve belgeleri kutusu** portalın üst kısmındaki. Sanal ağın adı arama sonuçlarında görüntülendiğinde seçin.
2. Seçin **DDoS koruması**altında **ayarları**.
3. Seçin **temel** altında **DDoS koruma planı** seçip **Kaydet**.

## <a name="work-with-ddos-protection-plans"></a>DDoS koruma planları ile çalışma

1. Seçin **tüm hizmetleri** portalın sol üst köşesindeki,.
2. Girin *DDoS* içinde **filtre** kutusu. Zaman **DDoS koruma planları** sonuçlarda görüntülenmesi, onu seçin.
3. Listeden görüntülemek istediğiniz koruma planı seçin.
4. Planla ilişkili tüm sanal ağlar listelenir.
5. Bir planı silmek istiyorsanız, tüm sanal ağları verilerden ilişkilendirmesini kaldırmanız gerekir. Bir sanal ağdan bir plan ile ilişkisini kaldırma edinmek için bkz [bir sanal ağ için devre dışı DDoS](#disable-ddos-for-a-virtual-network).

## <a name="configure-alerts-for-ddos-protection-metrics"></a>DDoS koruma ölçümleri için uyarıları yapılandırma

Azure İzleyici uyarı yapılandırması'nı kullanarak herhangi bir saldırı sırasında etkin bir risk azaltma olduğunda sizi uyarmak için kullanılabilir DDoS koruma ölçümleri seçebilirsiniz. Belirtilen adresi, koşullar karşılandığında bir uyarı e-posta alır:

1. Seçin **tüm hizmetleri** portalın sol üst köşesindeki,.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Seçin **ölçümleri** altında **paylaşılan hizmetleri**.
4. Girin veya kendi değerlerinizi seçin veya aşağıdaki örneği değerleri girin, geri kalan varsayılan ayarları kabul edin ve ardından **Tamam**:

    |Ayar                  |Değer                                                                                               |
    |---------                |---------                                                                                           |
    |Ad                     | myDdosAlert                                                                                        |
    |Abonelik             | Uyarıları almak istediğiniz genel IP adresini içeren aboneliği seçin.        |
    |Kaynak grubu           | Uyarıları almak istediğiniz genel IP adresini içeren kaynak grubunu seçin.      |
    |Resource                 | Uyarıları almak istediğiniz genel IP adresini içeren bir genel IP adresi seçin. Bir sanal ağdaki kaynaklara atanan genel IP adresleri DDoS izler. Herhangi bir kaynağa genel IP adresleri sanal ağ içinde yoksa, öncelikle bir genel IP adresiyle bir kaynak oluşturmanız gerekir. Genel IP adresini kaynak listelenen Yöneticisi aracılığıyla (Klasik değil) dağıtılan tüm kaynakları izleyebilirsiniz [Azure Hizmetleri için sanal ağ](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network), Azure App Service ortamları ve Azure VPN ağ geçidi dışında. Bu öğretici ile devam etmek için hızlı bir şekilde oluşturabileceğiniz bir [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine.                   |
    |Ölçüm                   | Altında DDoS saldırı veya değil                                                                            |
    |Eşik                | 1 - **1** saldırı altında olduğu anlamına gelir. **0** Saldırıya uğramış olmamak anlamına gelir.                         |
    |Dönem                   | Seçtiğiniz herhangi bir değer seçin.                                                                   |
    |E-posta ile bildir         | Onay kutusunu işaretleyin                                                                                  |
    |Ek yönetici | Bir e-posta sahibi, katkıda bulunan veya okuyucu aboneliğin değilseniz, e-posta adresinizi girin. |

    Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri şu resme benzer bir e-posta alırsınız:

    ![Saldırı uyarı](./media/manage-ddos-protection/ddos-alert.png)


Uyarınız doğrulamak için bir DDoS saldırısının benzetimini yapmak için bkz: [doğrulama DDoS algılama](#validate-ddos-detection).

Ayrıca daha fazla bilgi edinebilirsiniz [Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [logic apps](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uyarılar oluşturmak için.

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanma

Telemetri bir saldırı, gerçek zamanlı olarak Azure İzleyici sağlanır. Telemetri, genel bir IP adresi altında azaltma süresi için kullanılabilir. Önce veya bir saldırıya ortadan kalktıktan sonra telemetrisi görmüyorum.

1. Seçin **tüm hizmetleri** portalın sol üst köşesindeki,.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Seçin **ölçümleri**altında **paylaşılan hizmetleri**.
4. Seçin **abonelik** ve **kaynak grubu** için telemetri istediğiniz genel IP adresini içerir.
5. Seçin **genel IP adresi** için **kaynak türü**, ardından telemetri için istediğiniz belirli genel IP adresini seçin.
6. Bir dizi **kullanılabilir ölçümler** ekranın sol tarafında görünür. İçinde seçili olduğunda, bu ölçümleri grafiği çizilecek **Azure İzleyici ölçüm grafiği** genel bakış ekranında.
7. Seçin **toplama** olarak yazın **Maks**

Ölçüm adları farklı paket türleri ve paketleri karşılaştırması bayt etiket adları her ölçümü üzerinde temel bir yapısı ile aşağıdaki gibi sunar:

- **Bırakılan etiket adı** (örneğin, **gelen paketlerin bırakılan DDoS**): Paket DDoS koruma sistemi tarafından bırakılan/tersten sayısı.
- **İletilen etiket adı** (örneğin **gelen paketlerin iletilmesi DDoS**): ' % S'hedef VIP – filtre uygulanmamış trafik DDoS sistem tarafından iletilen paketler sayısı.
- **Hiçbir etiket adı** (örneğin **gelen paketlerin DDoS**): Bırakılan ve iletilen paketlerinin toplamını temsil eden temizleme sisteme – gelen paketlerinin toplam sayısı.

Telemetri doğrulamak için bir DDoS saldırısının benzetimini yapmak için bkz: [doğrulama DDoS algılama](#validate-ddos-detection).

## <a name="view-ddos-mitigation-policies"></a>DDoS önleme ilkelerini görüntüle

DDoS koruması standart DDoS etkin olan sanal ağda korumalı kaynağa her genel IP adresi için üç otomatik olarak ayarlanmış bir risk azaltma ilkeleri (TCP SYN, TCP ve UDP) uygular. İlke eşikleri seçerek görüntüleyebilirsiniz **DDoS saldırılarının tetiklemek için TCP gelen paketleri** ve **DDoS saldırılarının tetiklemek için gelen UDP paketlerini** Ölçümleriyle **toplama** 'Max' aşağıdaki resimde gösterildiği gibi yazın:

![Risk azaltma ilkeleri görüntüle](./media/manage-ddos-protection/view-mitigation-policies.png)

Azure machine learning tabanlı ağ trafiği profil aracılığıyla otomatik yapılandırılmış ilke eşikleri. Yalnızca ilke eşik aşıldığında DDoS saldırılarının saldırıya IP adresi için oluşuyor.

## <a name="configure-ddos-attack-analytics"></a>DDoS saldırı analytics yapılandırın
Azure DDoS koruması standart saldırı ayrıntılı Öngörüler ve görselleştirme DDoS saldırı analizlerle sağlar. Müşterilerin sanal ağlarında DDoS saldırılarına karşı koruma, saldırı trafiği ve saldırı azaltma raporlar ve risk azaltma akış günlükleri aracılığıyla saldırının etkilerini hafifletmek için gerçekleştirilen eylemleri görünürlük ayrıntılı. 

## <a name="configure-ddos-attack-mitigation-reports"></a>DDoS saldırı azaltma raporlarını yapılandırma
Saldırı azaltma raporlarını kaynağınızda saldırı hakkında ayrıntılı bilgi sağlamak için toplanır Netflow Protokolü verileri kullanır. Bir genel IP kaynağı, Saldırıya uğramış olduğundan her zaman, risk azaltma başlar başlamaz rapor oluşturma başlatılır. Olacaktır artımlı bir rapor için tüm risk azaltma süresi 5 dakikada bir ve azaltma sonrası raporu oluşturulur. Bu, DDoS saldırılarının daha uzun bir süre devam eder, bir olay, her 5 dakikada en geçerli anlık görüntü azaltma raporu görüntülemek mümkün olmayacak ve tam bir kez saldırısı riskini azaltma özeti üzerinde olduğundan emin olun içindir. 

1. Seçin **tüm hizmetleri** portalın sol üst köşesindeki,.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Altında **ayarları**seçin **tanılama ayarları**.
4. Seçin **abonelik** ve **kaynak grubu** oturum istediğiniz genel IP adresini içerir.
5. Seçin **genel IP adresi** için **kaynak türü**, ölçümler için oturumu istediğiniz belirli genel IP adresi'ı seçin.
6. Seçin **DDoSMitigationReports günlükleri toplamak için tanılamayı açın** ve gereksinim duyduğunuz kadar çok aşağıdaki seçeneklerden birini seçin:

    - **Bir depolama hesabında arşivle**: Veriler bir Azure depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi için bkz. [tanılama günlüklerini arşivleme](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Olay hub'ına Stream**: Günlük alıcı günlüklerini kullanarak bir Azure olay hub'ı seçmenizi sağlar. Event hubs Splunk veya diğer SIEM sistemleriyle tümleştirme olanak tanır. Bu seçenek hakkında daha fazla bilgi için bkz. [Stream tanılama günlükleri Olay hub'ına](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Log Analytics'e gönderme**: Günlükleri Azure İzleyici'hizmetine yazar. Bu seçenek hakkında daha fazla bilgi için bkz. [kullanımı Azure İzleyici günlüklerine ait günlükleri toplayın](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Artımlı ve sonrası saldırı azaltma raporları aşağıdaki alanları içerir.
- Saldırı vektörlerinin
- Trafik istatistikleri
- Atılan paketlerin nedeni
- Protokollerin çeşitliliğinden
- Kaynak ülke veya bölgelerde 10 üst
- İlk 10 kaynak Asn'ler

## <a name="configure-ddos-attack-mitigation-flow-logs"></a>DDoS saldırı azaltma akış günlüklerini yapılandırma
Saldırı azaltma akış günlükleri trafiğin bırakılmasına gözden geçirmek izin trafiği ve diğer ilginç datapoints'ler neredeyse gerçek zamanlı etkin bir DDoS saldırı sırasında iletilir. Bu veri akışını sabit SIEM Sistemlerinizle neredeyse gerçek zamanlı izleme, olay hub'ı aracılığıyla içine alma, olası eylemleri ve savunma işlemlerinizin ihtiyacını ele. 

1. Seçin **tüm hizmetleri** portalın sol üst köşesindeki,.
2. Girin *İzleyici* içinde **filtre** kutusu. Zaman **İzleyici** göründüğü sonuçlarında seçin.
3. Altında **ayarları**seçin **tanılama ayarları**.
4. Seçin **abonelik** ve **kaynak grubu** oturum istediğiniz genel IP adresini içerir.
5. Seçin **genel IP adresi** için **kaynak türü**, ölçümler için oturumu istediğiniz belirli genel IP adresi'ı seçin.
6. Seçin **DDoSMitigationFlowLogs günlükleri toplamak için tanılamayı açın** ve gereksinim duyduğunuz kadar çok aşağıdaki seçeneklerden birini seçin:

    - **Bir depolama hesabında arşivle**: Veriler bir Azure depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi için bkz. [tanılama günlüklerini arşivleme](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Olay hub'ına Stream**: Günlük alıcı günlüklerini kullanarak bir Azure olay hub'ı seçmenizi sağlar. Event hubs Splunk veya diğer SIEM sistemleriyle tümleştirme olanak tanır. Bu seçenek hakkında daha fazla bilgi için bkz. [Stream tanılama günlükleri Olay hub'ına](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Log Analytics'e gönderme**: Günlükleri Azure İzleyici'hizmetine yazar. Bu seçenek hakkında daha fazla bilgi için bkz. [kullanımı Azure İzleyici günlüklerine ait günlükleri toplayın](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
1. Azure analiz panosunda akış günlükleri verilerini görüntülemek için örnek panoyu içeri aktarabilirsiniz https://github.com/Anupamvi/Azure-DDoS-Protection/raw/master/flowlogsbyip.zip

Akış günlükleri aşağıdaki alanları olacaktır: 
- Kaynak IP
- Hedef IP
- Kaynak Bağlantı Noktası 
- Hedef bağlantı noktası 
- Protokol türü 
- Risk azaltma sırasında gerçekleştirilen eylem



## <a name="validate-ddos-detection"></a>DDoS algılama doğrula

Microsoft ile kurdu [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) burada oluşturun DDoS koruması etkinleştirilmiş genel IP adresleri simülasyonlar için trafiği bir arabirim oluşturmak için. Kesme noktası bulut benzetim yapmanıza olanak sağlar:

- Microsoft Azure DDoS koruması, Azure kaynaklarınızı DDoS saldırılarına karşı nasıl koruduğunu doğrula
- Olay yanıtlama işleminize DDoS saldırıya en iyi duruma getirme
- Belge DDoS uygunluk
- Ağ güvenlik gruplarını eğitin

## <a name="view-ddos-protection-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki DDoS koruması Uyarıları görüntüle

Azure Güvenlik Merkezi bir listesini sağlar [güvenlik uyarıları](/azure/security-center/security-center-managing-and-responding-alerts), araştırın ve sorunları düzeltmenize yardımcı olacak bilgiler ile. Bu özellik sayesinde, uyarılar, DDoS saldırı ilgili uyarıları ve yakın zamanda saldırının etkilerini hafifletmek için yapılan Eylemler dahil olmak üzere, birleşik bir görünümünü alırsınız.
İki belirli uyarılar için herhangi bir DDoS göreceği saldırı algılama ve önleme vardır:

- **DDoS saldırı algılandığında genel IP için**: DDoS koruması hizmeti, genel IP adreslerinden birini bir DDoS saldırısının hedef olduğunu algıladığında, bu uyarı oluşturulur.
- **DDoS saldırılarının azaltılabilir genel IP için**: Genel IP adresini bir saldırı azaltılabilir olduğunda bu uyarı oluşturulur.
Uyarıları görüntülemek için Aç **Güvenlik Merkezi** Azure portalında. Altında **tehdit koruması**seçin **güvenlik uyarıları**. Aşağıdaki ekran görüntüsünde, DDoS saldırı uyarılar örneği gösterilmektedir.

![Azure Güvenlik Merkezi'nde DDoS uyarı](./media/manage-ddos-protection/ddos-alert-asc.png)

Uyarılar, saldırı, coğrafi ve tehdit bilgileri ve düzeltmeler adımları altında olan ortak IP adresi hakkında genel bilgiler içerir.

## <a name="permissions"></a>İzinler

DDoS koruma planları ile çalışmak için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                            | Ad                                     |
| ---------                                         | -------------                            |
| Microsoft.Network/ddosProtectionPlans/read        | Bir DDoS koruma planı okuma              |
| Microsoft.Network/ddosProtectionPlans/write       | Bir DDoS koruma planı güncelle  |
| Microsoft.Network/ddosProtectionPlans/delete      | DDoS koruma planı silme            |
| Microsoft.Network/ddosProtectionPlans/join/action | Bir DDoS koruma planı katılın              |

DDoS koruması için bir sanal ağ'ı etkinleştirmek için hesabınıza da uygun atanmalıdır [sanal ağlar için Eylemler](manage-virtual-network.md#permissions).

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için