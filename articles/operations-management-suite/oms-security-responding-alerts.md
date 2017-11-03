---
title: "İzleme ve Operations Management Suite güvenlik ve denetim çözümü güvenlik uyarılarını yanıtlama | Microsoft Docs"
description: "Bu belge, güvenlik uyarılarını yanıtlama ve izlemek için OMS güvenlik ve denetim kullanılabilir tehdit Intelligence seçeneği kullanmak için yardımcı olur."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: df82afab2c38431e134146143524edc080ee38f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>İzleme ve Operations Management Suite güvenlik ve denetim çözüm güvenlik uyarılarını yanıtlama
Bu belge, izlemek ve güvenlik uyarılarını yanıtlama OMS güvenlik ve denetim kullanılabilir tehdit Intelligence seçeneği kullanmanıza yardımcı olur.

## <a name="what-is-oms"></a>OMS nedir?
Microsoft Operations Management Suite (OMS) Microsoft'un bulut yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan BT yönetim çözümü dayalıdır. OMS hakkında daha fazla bilgi için bkz. [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Tehdit bilgileri
Kullanıcıların geniş ağ erişimi ve şirket verilerine bağlanmak için çeşitli aygıtlardan kullanmak bir kuruluş ortamında, etkin olarak kaynaklarınızı izlemek ve hızlı bir şekilde güvenlik olaylarına yanıt zorunludur. Belirli tehditlere değil yükseltmek bazı siber güvenlik uyarıları çünkü güvenlik yaşam döngüsü açısından önemli ya da teknik geleneksel güvenlik denetimleri tarafından tanımlanan kuşkulu etkinlikleri budur. 

Kullanarak **tehdit bilgileri** seçeneği kullanılabilir OMS güvenlik ve denetim, BT yöneticileri ortam güvenlik tehditlerinden örneğin tanımlamak, belirli bir bilgisayarın parçası olup olmadığını belirleyebilmek bir [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Bilgisayar korsanları yasa dışı yollarla bu bilgisayarı bir komut veya denetime bağlayan kökü amaçlı bir yazılım yüklediklerinde, bilgisayarlar botnette düğümlere dönüşür. Yeraltı iletişim kanalları, gelen gibi olası tehditler tanımlayabilirsiniz [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Bu tehdit bilgileri oluşturmak için Microsoft içindeki birden fazla kaynaktan gelen veri OMS güvenlik ve denetim çözüm kullanır. OMS güvenlik ve Denetim olası tehditlere karşı ortamınızın tanımlamak için bu verileri özelliğinden yararlanır.

Tehdit Bilgileri bölmesi üç ana seçenekten oluşur:

* Giden kötü amaçlı trafiğe sahip sunucular
* Algılanan tehditler türleri
* Tehdit bilgileri haritası

> [!NOTE]
> Tüm bu seçenekler genel bakış için okuma [Operations Management Suite güvenlik ve denetim çözümü ile çalışmaya başlama](oms-security-getting-started.md).
> 
> 

### <a name="responding-to-security-alerts"></a>Güvenlik uyarılara yanıt verme
Adımlardan biri bir [güvenlik olay yanıtlama](https://technet.microsoft.com/library/cc512623.aspx) işlemdir güvenliğinin aşılmasına sistemleri önemini belirlemek için. Bu aşamada aşağıdaki görevleri gerçekleştirmeniz:

* Saldırının yapısını belirleme
* Saldırının kaynak noktasını belirleme
* Saldırının amacını belirleme. Saldırı kuruluşunuza özellikle belirli bilgileri elde etme amacıyla mı yapıldı yoksa rastgele bir saldırı mıydı?
* Gizliliği tehlikeye girmiş sistemleri tanımlayın
* Erişilen ve bu dosyaları duyarlılığını belirlemek dosyaları belirle

Yararlanabileceğiniz **tehdit bilgileri** bu görevleri yardımcı olması için OMS güvenlik ve denetim çözüm bilgileri. Bunlar erişim için aşağıdaki adımları izleyin **tehdit bilgileri** seçenekleri:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
   
    ![Güvenlik ve Denetim](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. İçinde **güvenlik ve Denetim** panoyu görürsünüz **tehdit bilgileri** aşağıda gösterildiği gibi sağ seçenekleri:
   
    ![Tehdit Intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Bu üç kutucuklar size geçerli tehditleri genel bir bakış sunar. İçinde **giden kötü amaçlı trafiği sunucusuyla** izlemekte olduğunuz herhangi bir bilgisayarda ise belirlemek mümkün olacaktır (içinde veya ağınızın dışında), kötü amaçlı trafiğin Internet'e gönderiyor. 

**Algılanan tehdit türlerini** döşeme "içinde joker" geçerli tehditleri özetini gösterir, bu kutucuğa tıkladığınızda bu tehditler hakkında daha fazla ayrıntı Göster aşağıdaki görürsünüz:

![Algılanan tehdit türleri](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Tıklayarak her tehdit hakkında daha fazla bilgi ayıklayabilirsiniz. Aşağıdaki örnek, Botnet hakkında daha fazla ayrıntı gösterir:

![bir tehdit hakkında daha fazla ayrıntı](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Bu bölüm başına içinde açıklandığı gibi bu bilgileri bir olay yanıtlama çalışması sırasında çok kullanışlı olabilir. Bunu ayrıca bir adli araştırma sırasında nerede kaynağını ve saldırı, hangi sistem aşılmış zaman çizelgesi bulmanız gereken önemli olabilir. Bu raporda, kolayca belirleyebilir saldırı hakkında bazı önemli ayrıntıları gibi: kaynağı saldırı, aşılmış yerel IP ve bağlantı geçerli oturum durumu. 

**Tehdit Intelligence harita** kötü amaçlı trafiği, mevcut konumları dünya belirlemenize yardımcı olur. Turuncu (gelen) ve bu okları birini tıklatırsanız, trafiği yönünü tanımlayan kırmızı (giden) okları bu haritadaki tehdit ve aşağıda gösterildiği gibi trafik yönü türünü gösterir:

![Tehdit bilgisi haritası](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Sunu izleyerek bir olay yanıtlama işlemi sırasında bu özelliği kullanmak hakkında bir tanıtım bakın [Operations Management Suite kullanarak destekli araştırma ile Veri Merkezi güvenlik tehditleri azaltmak](https://myignite.microsoft.com/videos/5000) Microsoft Ignite teslim.
> 

### <a name="responding-to-distinct-malicious-ip-accessed"></a>Erişilen ayrı kötü amaçlı IP yanıt
Bazı senaryolarda, izlenen bir bilgisayardan erişilen olası bir kötü amaçlı IP ile karşılaşabilirsiniz:

![Tehdit bilgisi haritası](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.PNG)

Bu uyarı ve aynı kategoride bulunan diğer uyarılar OMS Güvenliği aracılığıyla, [Microsoft Tehdit Bilgileri](https://youtu.be/O4WtxgUrDc8)'nden yararlanılarak oluşturulur. Microsoft tarafından toplananların yanı sıra önde gelen tehdit bilgisi sağlayıcılarından satın alınan Tehdit Bilgisi verileri de bulunur. Bu veriler sıklıkla güncelleştirilir ve hızlı hareket eden tehditlere uyarlanır. Doğası gereği bu verilerin, bir güvenlik uyarısını [araştırma](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) sürecinde diğer güvenlik bilgisi kaynaklarıyla birleştirilmesi gerekir. 

## <a name="customize-alerts-received-via-e-mail"></a>E-posta yoluyla alınan uyarıları özelleştirme

Güvenlik Uyarıları OMS güvenliği tarafından tetiklendiğinde, kuruluşunuzda hangi kullanıcıların bildirilecek özelleştirebilirsiniz. Bu seçenek altında genel bakış kullanılabilir / OMS Pano ayarları:

![E-posta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, kullanılacak öğrendiniz **tehdit bilgileri** güvenlik uyarılarını yanıtlama için OMS güvenlik ve denetim çözümde seçeneği. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite güvenlik ve denetim çözümü ile çalışmaya başlama](oms-security-getting-started.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)

