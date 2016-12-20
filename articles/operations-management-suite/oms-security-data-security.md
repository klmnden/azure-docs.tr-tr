---
title: "Operations Management Suite Güvenlik ve Denetim Çözümü Veri Güvenliği | Microsoft Belgeleri"
description: "Bu belgede verilerin Operations Management Suite Güvenlik ve Denetim Çözümünde nasıl yönetildiği ve korunduğu açıklanmaktadır."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 6af6c10cef317453131197db74a9380518afadf0


---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Operations Management Suite Güvenlik ve Denetim çözümü veri güvenliği
Müşterilerin tehditleri önlemesine, tespit etmesine ve müdahale etmesine yardımcı olmak için [Operations Management Suite (OMS) Güvenlik ve Denetim Çözümü](operations-management-suite-overview.md), kaynaklarınız hakkında aşağıdakileri içeren verileri toplar ve işler:

* Güvenlik olay günlüğü
* Windows için Olay İzleme (ETW) olayları
* AppLocker denetim olayları
* Windows Güvenlik Duvarı günlüğü
* Gelişmiş Threat Analytics olayları
* Temel değerlendirmesinin sonuçları
* Kötü amaçlı yazılımdan koruma değerlendirmesinin sonuçları
* Güncelleştirme/düzeltme eki değerlendirmesinin sonuçları
* Aracı üzerinde açıkça etkinleştirilen Syslog akışları

Bu verilerin gizlilik ve güvenliğini korumak için önemli taahhütlerde bulunuyoruz. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.
Bu makalede, OMS Güvenlik ve Denetim Çözümü'nde verilerin nasıl yönetildiği ve korunduğu açıklanmaktadır.

## <a name="data-sources"></a>Veri kaynakları
OMS Güvenlik ve Denetim Çözümü, OMS Aracısı'nın yüklü olduğu Sanal Makinelerinizdeki ve fiziksel bilgisayarlarınızdaki verileri analiz eder. OMS Güvenlik ve Denetim Çözümü; Windows olayı, denetim günlükleri, IIS günlükleri ve syslog iletileri gibi güvenlik olayları hakkında yapılandırma bilgilerini toplayabilir. Bu verilere örnek olarak işletim sistemi türü ve sürümü, devam eden işlemler, makine adı, IP adresleri, oturum açmış kullanıcı ve kiracı kimliği verilebilir.  

## <a name="data-protection"></a>Veri koruma
**Veri ayırma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. 

**Veri erişimi**: Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli, hizmetler tarafından toplanan veya çözümlenen bilgilere erişebilir. Microsoft’un Müşteri Verilerini kullanmayacağını veya reklam ya da benzeri ticari amaçlarla bundan bilgi türetmeyeceğini belirten [Microsoft Çevrimiçi Hizmet Koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ve [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) belgelerine bağlı kalıyoruz. Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli, hizmetler tarafından toplanan veya çözümlenen bilgilere erişebilir. Müşteri Verileri yalnızca size Azure hizmetlerini sağlamak için, bu hizmetleri sağlamayla uyumlu amaçlar da dahil olmak üzere gerektiğinde kullanılır. Kendi verileriniz üzerindeki tüm haklarınız saklıdır.

**Veri kullanımı**: Microsoft önleme ve algılama özelliklerimizi geliştirmek amacıyla birden fazla kiracıda görülen modelleri ve tehdit bilgilerini kullanır; bunu [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) belgemizde açıklanan gizlilik taahhütlerine uygun şekilde yaparız.

> [!NOTE]
> Veri konumu, ilk OMS Güvenlik ve Denetim yapılandırma işleminin bir parçası olan çalışma alanı oluşturma işlemi sırasında OMS çalışma alanı düzeyinde yapılandırılır.
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, OMS'de verilerin nasıl yönetileceğine ve korunacağına ilişkin bilgi edindiniz. OMS Güvenlik ve Denetim çözümü hakkında daha fazla bilgi edinmek için bkz.

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)




<!--HONumber=Dec16_HO1-->


