---
title: "Operations Management Suite güvenlik ve denetim çözümü kaynakları izleme | Microsoft Docs"
description: "Bu belge OMS güvenlik kullanmanıza yardımcı olur ve denetim kaynaklarınızı izlemek ve güvenlik sorunlarını tanımlamak için özellikleri."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 2f73266b65a4eda6c8751a2d56bc3f11bf4e6a57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Operations Management Suite güvenlik ve denetim çözümü kaynaklarında izleme
Bu belge kaynaklarınızı izlemek ve güvenlik sorunlarını tanımlamak için OMS güvenlik ve denetim özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-oms"></a>OMS nedir?
Microsoft Operations Management Suite (OMS) Microsoft'un bulut yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan BT yönetim çözümü dayalıdır. OMS hakkında daha fazla bilgi için bkz. [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>İzleme kaynakları
Her güvenlik olaylarına ilk başta tekrarlamasını önlemek istediğiniz, mümkündür. Ancak, tüm güvenlik olayları önlemek mümkün değildir. Bir güvenlik olayı gerçekleştiğinde, bu etkisini en aza indirilir emin olmak gerekir.  Numarası ve güvenlik olayların etkisini en aza indirmek için kullanılan üç önemli öneriler şunlardır:

* Güvenlik açıkları, ortamınızda düzenli olarak değerlendirin.
* Düzenli olarak tüm bilgisayar sistemleri ve yüklü en son düzeltme eklerini tümüne sahip olmasını sağlamak için ağ aygıtlarını denetleyin.
* Tüm günlükleri ve işletim sistemi olay günlüklerini, uygulama belirli günlüklerini ve izinsiz giriş algılama sistem günlükleri gibi günlük mekanizmaları düzenli olarak denetleyin.

OMS güvenlik ve denetim çözüm etkinleştirir BT etkin olarak yardımcı olabilecek tüm kaynakları izlemek için güvenlik olayların etkisini en aza indirin. OMS güvenlik ve denetim kaynakları izlemek için kullanılan güvenlik etki alanları bulunur. Güvenlik etki alanları için şu etki alanlarına daha ayrıntılı olarak ele alınacak güvenlik izleme seçenekleri, hızlı erişim sağlar:

* kötü amaçlı yazılım değerlendirmesi
* Güncelleştirme değerlendirmesi
* Kimlik ve Erişim

> [!NOTE]
> Tüm bu seçenekler genel bakış için okuma [Operations Management Suite güvenlik ve denetim çözümü ile çalışmaya başlama](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Sistem Koruması izleme
Bir derinlemesine savunma derinliği yaklaşımda, her koruma katmanı Varlığınızı genel güvenlik durumu için önemlidir. Tehdit algılanan bilgisayarlar ve yetersiz korumalı bilgisayarlar gösterilir kötü amaçlı yazılım değerlendirmesi döşemesinin güvenlik etki alanları altında. Kötü amaçlı yazılım değerlendirmesi bilgileri kullanarak, gerekli sunucular koruma uygulamak için bir plan tanımlayabilirsiniz. Bu seçenek erişmek için aşağıdaki adımları izleyin:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
   
    ![Güvenlik ve Denetim](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. İçinde **güvenlik ve Denetim** panoyu tıklatın **kötü amaçlı yazılımdan koruma değerlendirme** altında **güvenlik etki alanları**. **Kötü amaçlı yazılımdan koruma değerlendirme** Pano, aşağıda gösterildiği gibi görünür:

![kötü amaçlı yazılım değerlendirmesi](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Kullanabileceğiniz **kötü amaçlı yazılım değerlendirmesi** Pano için aşağıdaki güvenlik sorunlarını tanımlamak için:

* **Etkin tehditleri**: güvenliği ve sistemi etkin tehditleri sahip bilgisayarlar.
* **Düzeltilen tehditleri**: ancak tehditler güvenliği bilgisayarlar düzeltilebilir.
* **İmza güncel**: kötü amaçlı yazılımdan koruma etkinleştirilmiş bilgisayarlar ancak imza güncel değil.
* **Gerçek zamanlı koruma yok**: kötü amaçlı yazılımdan koruma yüklü olmayan bilgisayarlar.

### <a name="monitoring-updates"></a>güncelleştirmeleri izleme
En son güvenlik güncelleştirmelerini uygulamak bir güvenlik en iyi uygulamadır ve güncelleştirme yönetimi stratejinizde birleştirilmesi. Microsoft İzleme Aracısı hizmeti (HealthService.exe) izlenen bilgisayarlardan güncelleştirme bilgilerini okur ve işleme için OMS hizmetine bulutta bu güncelleştirilmiş bilgileri gönderir. Microsoft İzleme Aracısı hizmeti otomatik bir hizmet olarak yapılandırılır ve bu her zaman hedef bilgisayarda çalışıyor olması gerekir.

![güncelleştirmeleri izleme](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Mantığı güncelleştirme verilere uygulanır ve bulut hizmeti verilerini kaydeder. Eksik güncelleştirmeleri bulunursa, bunlar üzerinde gösterilir **güncelleştirmeleri** Pano. Kullanabileceğiniz **güncelleştirmeleri** Pano eksik güncelleştirmelerle çalışmaya ve bunlara ihtiyacınız sunucularına uygulamak için bir plan geliştirin. Erişim için aşağıdaki adımları izleyin **güncelleştirmeleri** Pano:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
2. İçinde **güvenlik ve Denetim** panoyu tıklatın **güncelleştirme değerlendirme** altında **güvenlik etki alanları**. Güncelleştirme Pano, aşağıda gösterildiği gibi görünür:

![Güncelleştirme değerlendirme](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Bu Panoda bilgisayarlarınızı geçerli durumunu anlamak ve en önemli tehditleri çözmek için güncelleştirme değerlendirmesi gerçekleştirebilirsiniz. Kullanarak **kritik güncelleştirmeler veya güvenlik güncelleştirmeleri** kutucuğu, BT yöneticileri erişebilir aşağıda gösterildiği gibi eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere erişmek:

![arama sonucu](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Bu rapor, bu sistem güvenlik güncelleştirme ve güvenlik açığı hakkında daha fazla ayrıntı sahip MS Bulletin ile ilişkili Microsoft KB makalelerinin içeren için savunmasızdır tehdit türünü tanımlamak için kullanılan kritik bilgiler içerir.

### <a name="monitoring-identity-and-access"></a>Kimlik ve erişim izleme
Farklı cihaz kullanarak ve Bulut ve şirket içi uygulamalar çok büyük miktarda erişme yerden çalışmaları kullanıcılarla kimlik bilgilerini korunduğunu zorunludur. Kimlik bilgisi hırsızlığı saldırıları, saldırganın ağda sistem erişmek için normal bir kullanıcının kimlik bilgilerini erişim başlangıçta kazanır izinlerdir. Çoğu zaman, bu ilk saldırının ağa erişmek için yalnızca bir yolu, nihai amacıyla ayrıcalık hesaplarını bulmak için. 

Saldırganlar ağda diğer oturum açan hesaplar oturumlardan kimlik bilgileri ayıklamak için tooling ücretsiz kullanarak kalır. Sistem yapılandırmasına bağlı olarak, bu kimlik bilgileri karmaları, raporları veya bile düz metin parola formunda ayıklanabilir.  

> [!NOTE]
> doğrudan Internet'e açık makineler tüm tür iyi bilinen kullanıcı adları (örneğin, yönetici) kullanarak oturum açmayı deneyin birçok başarısız girişim görürsünüz. Çoğu durumda, bilinen kullanıcı adları kullanılmıyorsa ve parola yeterince güçlü ise normaldir.
> 
> 

Ayrıcalıklı bir hesap tehlikeye önce bu saldırganların tanımlamak mümkündür. Yararlanabileceğiniz **OMS güvenlik ve denetim çözümü** İzleyici kimlik ve erişim için. Erişim için aşağıdaki adımları izleyin **kimlik ve erişim** Pano:

1. İçinde **Microsoft Operations Management Suite** ana Pano tıklatın güvenlik ve denetim döşeme.
2. İçinde **güvenlik ve Denetim** panoyu tıklatın **kimlik ve erişim** altında **güvenlik etki alanları**. **Kimlik ve erişim** Pano, aşağıda gösterildiği gibi görünür:

![kimlik ve erişim](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Normal izleme stratejinizin bir parçası olarak, kimlik izleme eklemeniz gerekir. BT yöneticisi, birçok denemeleri olan belirli geçerli bir kullanıcı adı ise görünmelidir. Bu edinilen gerçek kullanıcı adı ya da bir saldırganı işaret ve yanılma veya otomatik bir aracı süresi dolmuş kullandığı sabit kodlanmış parola deneyin.

Bu Pano kimlik ve şirket kaynaklarına erişim ile ilgili olası tehditler hızlı bir şekilde tanımlamak için BT etkinleştir. Belirli önemli de olası eğilimleri belirlemek, örneğin oturum açma zaman içinde parçasında, süre başarısız oturum açma girişimi gerçekleştirilir kaç kez görebilirsiniz. Bu bilgisayar durumda **DosyaSunucusu** 35 oturum açma girişimlerinin aldı. Bu bilgisayar hakkında daha fazla ayrıntı tıklayarak keşfedebilirsiniz. 

![Daha fazla ayrıntı](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Bu bilgisayar için oluşturulan rapor bu deseni değerli ayrıntılarını getirir. Fark **hesap** sütun size sistem erişmeyi denemek için kullanılan kullanıcı hesabına **TIMEGENERATED** sütun size zaman aralığı denemesi yapılır ve **LOGONTYPENAME** sütun, burada bu girişim yapıldı konumu verir. Bu deneme yerel sistemde bir program tarafından gerçekleştirilen varsa **işlem** sütun işlemin adını gösteren. Oturum açma girişimi bir programdan geldiği yere senaryolarda kullanılabilir işlem adı zaten var ve artık size daha fazla araştırma hedef sistemde gerçekleştirebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, OMS güvenlik kullanmak üzere öğrendiniz ve kaynaklarınızı izlemek için çözüm denetleme. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite güvenlik ve denetim çözümü ile çalışmaya başlama](oms-security-getting-started.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)

