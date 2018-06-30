---
title: OMS portalı Azure'a taşıma | Microsoft Docs
description: OMS portalı, Azure portalına taşınacağını tüm işlevselliğiyle sunsetted yapılıyor. Bu makalede, bu geçiş hakkında ayrıntılar sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e47e8cbd209ea34317ca9b176a2c4b0fef10a2b2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133159"
---
# <a name="oms-portal-moving-to-azure"></a>OMS portalı Azure'a taşıma
OMS portalı kullandığınız için teşekkür ederiz. Biz ile destek teşvik ve yoğun olarak izleme ve yönetim hizmetlerimizle yatırım devam eder. Bir geri bildirim art arda müşterilerden heard izlemek ve şirket içi ve Azure iş yükleri yönetmek tek bir kullanıcı deneyimi gereksinimini parçasıdır. Azure portalı hub tüm Azure hizmetlerini ve zengin bir yönetim deneyimi sabitleme kaynakları, bulma kaynakları akıllı arayın ve kaynak yönetimi için etiketleme için panoları gibi özellikler ile sunar bildiğiniz. Birleştirmek ve izleme ve yönetim iş akışını kolaylaştırmak için Azure portalında içine OMS portalı yetenekleri ekleme başladı. OMS portalı özelliklerin çoğunu şimdi Azure portal parçası olan duyurmaktan Mutluluk duyuyoruz. Aslında, bazı yeni özellikler gibi trafik Yöneticisi yalnızca Azure portalında kullanılabilir. Kalan, yalnızca birkaç boşluklar vardır en işleminde Azure portalına taşınacağını hala beş çözümler olan impactful. Bu özellikler kullanmıyorsanız her şeyi Azure portalı ve daha fazla ile OMS portalında yaptığınızı gerçekleştirmek mümkün olacaktır. Zaten yapmadıysanız, Azure portalını kullanmaya bugün başlamak öneririz! 

Ağustos 2018 tarafından iki portalları arasında kalan aralıklar aşağı kapatmak bekliyoruz. Müşterilerin görüşleri bağlı olarak, biz sunsetting OMS portalı için zaman çizelgesi iletişim kurar. Azure portalına taşıyın ve kolay olması için geçiş beklediğiniz Mutluluk duyuyoruz. Ancak biz değişiklikleri zordur ve işlemleri karışıklığa neden olabilir anlayın. Herhangi bir sorunuz, geri bildirim veya sorunları için Gönder LAUpgradeFeedback@microsoft.com. Bu makalenin geri kalanında senaryoları, geçerli boşluklar ve bu geçiş için yol haritası üzerinden gider. 


## <a name="what-will-change"></a>Değişir mi? 
Aşağıdaki değişiklikleri OMS portalı kullanımdan ile bildirilir. Bu değişiklikler her aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

- Yeni uyarı yönetim deneyimi uyarı yönetim çözümü yerini alır.
- Kullanıcı erişim yönetimi Azure rol tabanlı erişim denetimi kullanarak Azure portalında yapılacaktır.
- Uygulama Öngörüler Bağlayıcısı artık gerekli bu yana aynı işlevselliği arası çalışma sorguları etkinleştirilebilir.
- OMS mobil uygulama kullanım dışı kalacaktır. 
- NSG çözüm gelişmiş işlevselliği trafiğini analiz çözümü kullanılabilir olan değiştirilmektedir.



## <a name="current-known-gaps"></a>Geçerli bilinen boşluklar 
Şu anda hala OMS portalı kullanmak ihtiyaç duyduğunuz bazı işlevselliği boşluklar vardır. Bu boşluklar kapatılır ve bu belgenin uygun şekilde güncelleştirilir. İçin başvurmalıdır [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=log-analytics) uzantıları ve değişiklikler hakkında devam eden duyuruları için.

- Aşağıdaki çözümlerden henüz Azure portalında tam olarak işlevsel değildir. Güncelleştirilmiş kadar Klasik portalda bu çözümleri kullanmaya devam etmelidir.

    - Windows analiz çözümleri ([Yükseltme Hazırlık](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [cihaz sistem durumu](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), ve [güncelleştirme Uyumluluk](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [DNS Analizi](log-analytics-dns.md) 
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Azure günlük analizi kaynağa erişmek için kullanıcı üzerinden erişim izni [Azure rol tabanlı erişim](#user-access-and-role-migration).
- OMS Portalı'yla oluşturulmuş güncelleştirme zamanlamaları zamanlanmış güncelleştirme dağıtımları yansımayabilir veya Azure portalında Güncelleştirme Yönetim Panosu iş geçmişini güncelleştirin. Bu aralık, Haziran 2018 ucu tarafından ele alınması için bekleniyor.
- Özel günlükler önizleme özelliği yalnızca OMS Portalı aracılığıyla etkinleştirilebilir. Haziran 2018 sonuna kadar bu otomatik olarak tüm çalışma alanları için etkinleştirilecek.
 
## <a name="what-should-i-do-now"></a>Ne yapmalıyım?  
İçin başvurmalıdır [ortak sorular için günlük analizi kullanıcılar için Azure portalı OMS portalı geçiş](../log-analytics/log-analytics-oms-portal-faq.md) Azure portalına geçiş hakkında bilgi için. Varsa [boşluklar yukarıda açıklanan](#current-known-gaps) birincil deneyiminizi Azure portal kullanarak başlatma düşünmelisiniz sonra ortamınız için geçerli değil. Geri bildirim, sorularınız veya kaygılarınız için Gönder LAUpgradeFeedback@microsoft.com.

## <a name="changes-to-alerts"></a>Uyarıları yapılan değişiklikler 

### <a name="alert-extension"></a>Uyarı uzantısı  
Uyarılar olan sürecinde [Azure portalına Genişletilmiş](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Bu işlem tamamlandıktan sonra uyarıları yönetim eylemleri yalnızca Azure portalında kullanılabilir. Var olan uyarılar OMS Portalı'nda listelenmesi devam eder. Uyarıları programlı olarak günlük analizi uyarı REST API veya günlük analizi uyarı kaynak şablonu kullanarak erişirseniz, Eylemler API çağrıları, Azure Resource Manager şablonları ve PowerShell komutlarını yerine eylem gruplarını kullanmanız gerekir.

### <a name="alert-management-solution"></a>Uyarı yönetimi çözümü
Yerine [uyarı yönetim çözümü](log-analytics-solution-alert-management.md), kullanabileceğiniz [Azure izleyicinin uyarı arabirimi birleşik](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) görselleştirmek ve uyarılarınızı yönetmek için. Bu yeni deneyim günlük analizi dahil olmak üzere Azure günlük uyarıları içinde birden fazla kaynaktan uyarılarını toplar. Uyarılarınızı dağıtımlarını bkz, akıllı grupları ile ilgili uyarıları otomatik gruplandırması yararlanmak ve zengin filtreleri uygulanırken arasında birden çok abonelik uyarıları görüntüleyin. Bu özelliklerin tümü 4 Haziran 2018 başlangıç Önizleme'de kullanılabilir. Uyarı yönetimi çözümü Azure portalında kullanılamaz. 

Uyarı yönetimi çözümü (uyarı türünü kayıtlarıyla) tarafından toplanan veri günlük analizi çalışma alanı için çözüm yüklü olduğu sürece olacak şekilde devam eder. Ağustos 2018 başlayarak, birleştirilmiş çalışma alanları halinde uyarı gelen uyarılar akışını, bu özellik değiştirme etkinleştirilecek. Bazı şema değişiklikleri beklenir ve daha sonraki bir tarihte duyurulacaktır.

## <a name="user-access-and-role-migration"></a>Kullanıcı erişim ve rol geçirme
Azure portal erişim yönetimi daha zengin ve erişim yönetimi OMS portalında daha güçlü, ancak bazı dönüşümleri gerektirir. Bkz: [çalışma alanlarını yönet](log-analytics-manage-access.md#manage-accounts-and-users) günlük analizi erişim yönetimi ayrıntıları için.

Haziran 25 başlatma ve Temmuz devam etmeden, otomatik dönüştürme erişim izinleri başlatacak Azure portalına OMS Portalı'ndan izinleri denetler. Dönüştürme tamamlandıktan sonra OMS portalı kullanıcı yönetimi bölümü, Azure erişim denetimi (IAM) kullanıcılar rota. 

Dönüştürme sırasında sistem her bir kullanıcı veya OMS portalında izinlerine sahip bir güvenlik grubu denetleyin ve aynı düzeyi veya izinleri Azure içinde olup olmadığını belirler. İzinleri eksikse, ilgili çalışma alanları ve çözümler için aşağıdaki rollerin atar.

| OMS portalı izni | Azure rol |
|:---|:---|
| Salt okunur | Log Analytics Okuyucusu |
| Katılımcı | Log Analytics Katkıda Bulunan |
| Yönetici | Sahip |

Hiçbir aşırı izinleri kullanıcılara atanan emin olmak için sistem otomatik olarak bu kaynak grubu düzeyinde izinleri atar değil. Sonuç olarak, çalışma alanı Yöneticiler el ile kendilerini atamanız gerekir _sahibi_ veya _katkıda bulunan_ aşağıdaki eylemleri gerçekleştirmek için kaynak grubu veya abonelik düzeyinde rolleri.

- Çözümleri Ekle Kaldır
- Yeni özel görünümler tanımlayın
- Uyarıları yönetme 

Bazı durumlarda otomatik dönüştürme izni uygulanamıyor ve el ile izinleri atamak için yönetici ister.

## <a name="oms-mobile-app"></a>OMS mobil uygulama
OMS mobil uygulama OMS portalı birlikte sunsetted olacaktır. OMS mobil uygulama yerine, access, BT altyapısı, panolar ve kaydedilmiş sorguları hakkında bilgi için Azure Portalı'nı doğrudan tarayıcınızdan mobil Cihazınızı erişebilirsiniz. Uyarıları alma için yapılandırmanız [Azure Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) SMS veya sesli arama şeklinde bildirimleri almak için

## <a name="application-insights-connector-and-solution"></a>Uygulama Öngörüler Bağlayıcısı ve çözümü
[Uygulama Öngörüler Bağlayıcısı](log-analytics-app-insights-connector.md) Application Insights verileri bir günlük analizi çalışma alanına getirmek için bir yol sağlar. Bu veri çoğaltma görünürlük altyapı ve uygulama verilerinin etkinleştirmek için gerekli.

Desteği ile [arası kaynak sorgularını](log-analytics-cross-workspace-search.md), artık verileri çoğaltmak için bu gereksinimi yoktur. Bu nedenle, varolan bir Application Insights çözümü kullanım dışı kalacaktır. Temmuz başlayarak, siz yeni Application Insights kaynaklar için günlük analizi çalışma alanları bağlamak mümkün olmaz. Varolan bağlantıları ve panolarını Kasım 2018 kadar çalışmaya devam eder.


## <a name="azure-network-security-group-analytics"></a>Azure Ağ Güvenlik Grubu Analizi
[Azure ağ güvenlik grubu analiz çözümü](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) değiştirilecek son başlatılan ile [trafiği Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) bulut ağlarda kullanıcı ve uygulama etkinlik görünürlük sağlar. Trafik analizi, kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri denetim, iş yükü performansını iyileştirmek ve uyumlu kalmak yardımcı olur. 

Bu çözüm NSG akış günlüklerini analiz eder ve aşağıdaki Öngörüler sağlar.

- Azure ve Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasında ağlar arasında trafiği akar.
- Uygulamalar ve protokolleri algılayıcılar veya ayrılmış akış koleksiyon uygulamaları gerek kalmadan, ağınızdaki.
- Etkin noktalarına üst talkers, chatty uygulamaları, bulut VM görüşmeleri trafiği.
- Kaynakları ve sanal ağlar, kritik iş Hizmetleri ve uygulamaları arasındaki arası ilişkileri üzerinden trafik hedefler.
- Kötü amaçlı trafiği, Internet, uygulama veya Internet erişmeye VM'ler açık bağlantı noktalarını da dahil olmak üzere güvenlik.
- Kapasite kullanımı, sağlama veya yetersiz kullanım üzerinden sorunları ortadan kaldırmanıza yardımcı olacak.

NSG günlüklerini günlük varolan kaydedilmiş şekilde aramalar, uyarılar, panolar analizi için çalışmaya devam edecek göndermek için tanılama ayarları yararlanmaya devam edebilirsiniz. Çözüm'i zaten yüklemiş olan müşterilerin, bir uyarı kadar kullanmaya devam edebilirsiniz. Ağ güvenlik grubu analiz çözümü Haziran 20 başlangıç marketten kaldırılacak ve topluluk yoluyla kullanılabilir hale bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [ortak sorular için günlük analizi kullanıcılar için Azure portalı OMS portalı geçiş](log-analytics-oms-portal-faq.md) OMS Portalı'ndan Azure portalına taşınacağını konusunda yönergeler için.