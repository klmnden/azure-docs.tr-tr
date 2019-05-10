---
title: Azure Güvenlik Merkezi Veri Güvenliği | Microsoft Belgeleri
description: Bu belgede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması açıklanmaktadır.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: rkarlin
ms.openlocfilehash: cd91b83bc808d811fc50293fbf1726d609ad5b46
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234076"
---
# <a name="azure-security-center-data-security"></a>Azure Güvenlik Merkezi Veri Güvenliği
Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, güvenlikle ilgili veriler, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

Bu makalede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması açıklanmaktadır.

## <a name="data-sources"></a>Veri kaynakları
Azure Güvenlik Merkezi, güvenlik durumunuzu görüntüleme, güvenlik açıklarını tanımlayıp çözümler önerme ve etkin tehditleri saptama amacıyla aşağıdaki kaynaklarda bulunan verileri analiz eder:

- Azure Hizmetleri: Azure hizmetlerinin yapılandırmasına ilişkin ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak dağıttığınız bilgileri kullanır.
- Ağ trafiği: Microsoft'un altyapısından kaynak/hedef IP/bağlantı noktası gibi ağ trafiği meta verilerini, paket boyutu ve ağ protokolü kullanan örnek.
- İş ortağı çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını kullanır.
- Sanal makinelerinizi ve sunucuları: Yapılandırma bilgilerini ve Windows olay ve Denetim günlükleri, IIS günlükleri, syslog iletileri ve sanal makinelerinizdeki kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri kullanır. Ayrıca, bir uyarı oluşturulduğunda Azure Güvenlik Merkezi etkilenen VM diskinin anlık görüntüsünü oluşturur ve adli amaçlar için kayıt defteri dosyası gibi uyarıyla ilgili makine yapıtlarını VM diskinden ayıklar.


## <a name="data-protection"></a>Veri koruma
**Veriler arasında ayrım yapma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır.

**Veri erişimi**: Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli, kilitlenme dökümü dosyaları, işlem oluşturma olayları, VM diski anlık görüntüleri ve yapıtları dahil olmak üzere Azure hizmetler tarafından toplanan veya çözümlenen bilgilere erişebilir, hangi istemeden müşteri verilerini ya da sanal makinelerinizdeki kişisel verileri içerebilir. Microsoft’un Müşteri Verilerini kullanmayacağını veya reklam ya da benzeri ticari amaçlarla bundan bilgi türetmeyeceğini belirten [Microsoft Çevrimiçi Hizmet Koşulları ve Gizlilik Bildirimi](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) belgesine bağlı kalıyoruz. Müşteri Verileri yalnızca size Azure hizmetlerini sağlamak için, bu hizmetleri sağlamayla uyumlu amaçlar da dahil olmak üzere gerektiğinde kullanılır. Müşteri Verileri üzerindeki tüm haklarınız saklıdır.

**Veri kullanımı**: Microsoft desenleri ve birden fazla kiracıda görülen tehdit önleme ve algılama özelliklerimizi geliştirmek için kullanır; Bunu açıklanan gizlilik taahhütlerine uygun şekilde yaptığımız bizim [gizlilik bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Veri konumu

**Alanlarınıza**: Aşağıdaki coğrafi bölgeler için bir çalışma alanı belirtilir ve Azure sanal kilitlenme bilgi dökümleri ve bazı uyarı verisi türleri de dahil olmak üzere makinelerinizden, toplanan veriler en yakın çalışma alanında depolanır.

| VM'nin Bulunduğu Coğrafi Bölge                              | Çalışma Alanının Bulunduğu Coğrafi Bölge |
|-------------------------------------|---------------|
| Amerika Birleşik Devletleri, Brezilya, Güney Afrika | Amerika Birleşik Devletleri |
| Kanada                              | Kanada        |
| Avrupa (Birleşik Krallık hariç)   | Avrupa        |
| Birleşik Krallık                      | Birleşik Krallık |
| Asya (Hindistan, Japonya, Kore, Çin dışında)   | Asya Pasifik  |
| Güney Kore                              | Asya Pasifik  |
| Hindistan                               | Hindistan         |
| Japonya                               | Japonya         |
| Çin                               | Çin         |
| Avustralya                           | Avustralya     |


VM diski anlık görüntüleri, VM diski ile aynı depolama hesabına kaydedilir.

Başka ortamlarda çalışan sanal makineler ve sunucular için (ör. şirket içi), toplanan verilerin depolanacağı çalışma alanını ve bölgeyi belirtebilirsiniz.

**Azure Güvenlik Merkezi deposunda**: İş ortağı uyarılarının da dahil olduğu güvenlik uyarıları hakkında bilgi depolanan bölgesel ilgili Azure kaynağının konumuna göre güvenlik sistem durumu ve öneriler hakkında bilgi merkezi Amerika Birleşik Devletleri depolanır veya Müşterinin konumuna göre Avrupa.
Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder. Azure Güvenlik Merkezi bu analizi çalışma alanıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler.

Makine yapıları VM ile aynı bölgede merkezi olarak depolanır.


## <a name="managing-data-collection-from-virtual-machines"></a>Sanal makinelerden veri toplamayı yönetme

Azure'da Güvenlik Merkezi'ni etkinleştirdiğinizde her Azure aboneliğiniz için veri toplama etkinleştirilir. Abonelikleriniz için veri toplamayı Azure Güvenlik Merkezi'nin Güvenlik İlkesi bölümünden de etkinleştirebilirsiniz. Veri toplama etkin olduğunda Azure Güvenlik Merkezi, desteklenen tüm mevcut Azure sanal makinelerinde ve yeni oluşturulan sanal makinelerde Microsoft Monitoring Agent'ı hazırlar.
Microsoft Monitoring Agent, [Windows için Olay İzleme](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) izlerinde güvenlikle ilgili çeşitli yapılandırmaları ve olayları tarar. Ayrıca, işletim sistemi, makinenin çalışması sırasında olay günlüğü olaylarını ortaya koyar. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri, oturum açmış kullanıcı ve kiracı kimliği. Microsoft Monitoring Agent, olay günlüğü girişleri ile ETW izlerini okur ve analiz için çalışma alanlarınıza kopyalar. Microsoft Monitoring Agent ayrıca kilitlenme bilgi dökümü dosyalarını çalışma alanlarınıza kopyalar, işlem oluşturma olaylarını etkinleştirir ve komut satırı denetimine olanak tanır.

Azure Güvenlik Merkezi Ücretsiz sürümünü kullanıyorsanız Güvenlik İlkesi'nde sanal makinelerden veri toplamayı devre dışı bırakabilirsiniz. Veri Toplama, Standart katmandaki abonelikler için gereklidir. Veri toplama devre dışı bırakılsa bile, VM diski anlık görüntüleri ve yapıt toplama işlemi etkin olmaya devam eder.

## <a name="data-consumption"></a>Veri Tüketimi

Müşteriler, farklı veri akışlarındaki Güvenlik Merkezi ile ilişkili verileri aşağıda gösterildiği gibi kullanabilir:

* **Azure Etkinliği**: Tüm güvenlik uyarıları, onaylı Güvenlik Merkezi [uyarlamalı uygulama denetimleri](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) tarafından oluşturulan [zamanında](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) istekler ve tüm uyarılar.
* **Azure İzleyici günlüklerine**: tüm güvenlik uyarıları.


> [!NOTE]
> Güvenlikle ilgili öneriler, REST API’si tarafından da kullanılabilir. Daha fazla bilgi için bkz. [Güvenlik Kaynağı Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/mt704034(Azure.100).aspx).

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması hakkında bilgi aldınız. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek bkz.:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz
