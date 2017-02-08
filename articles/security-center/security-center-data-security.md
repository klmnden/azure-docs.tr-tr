---
title: "Azure Güvenlik Merkezi Veri Güvenliği | Microsoft Belgeleri"
description: "Bu belgede Azure Güvenlik Merkezi&quot;nde verilerin yönetilmesi ve korunması açıklanmaktadır."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 3522f06702ad5bd3c1d73f0b510610f57cc5cc6c
ms.openlocfilehash: 5c24c91cb2ee260b7740df8baa150a7b2e1ec7cd


---
# <a name="azure-security-center-data-security"></a>Azure Güvenlik Merkezi Veri Güvenliği
Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, güvenlikle ilgili veriler, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

Bu makalede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması açıklanmaktadır.


## <a name="data-sources"></a>Veri kaynakları
Azure Güvenlik Merkezi, güvenlik durumunuzu görüntüleme, güvenlik açıklarını tanımlayıp çözümler önerme ve etkin tehditleri saptama amacıyla aşağıdaki kaynaklarda bulunan verileri analiz eder:

- Azure Hizmetleri: Dağıttığınız Azure hizmetlerinin yapılandırmasına ilişkin bilgileri, ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak kullanır.
- Ağ Trafiği: Microsoft’un altyapısından kaynak/hedef IP/bağlantı noktası, paket boyutu ve ağ protokolü gibi örneği alınmış ağ trafiği meta verilerini kullanır.
- İş Ortağı Çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını kullanır. 
- Sanal Makineleriniz: Sanal makinelerinizdeki yapılandırma bilgilerini ve Windows olay ve denetim günlükleri, IIS günlükleri, syslog iletileri ve kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri kullanır. Ayrıca, bir uyarı oluşturulduğunda Azure Güvenlik Merkezi etkilenen VM diskinin anlık görüntüsünü oluşturur ve adli amaçlar için kayıt defteri dosyası gibi uyarıyla ilgili makine yapıtlarını VM diskinden ayıklar.


## <a name="data-protection"></a>Veri koruma
**Veri ayırma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. 

**Veri erişimi**: Microsoft personeli, güvenlik önerileri sunmak ve olası güvenlik tehditlerini araştırmak amacıyla Azure hizmetleri tarafından sunulan, Müşteri Verilerini ya da sanal makinelerinizdeki kişisel verileri de içerebilecek kilitlenme dökümü dosyaları, işlem oluşturma olayları, VM diski anlık görüntüleri ve yapıtları dahil toplanan veya analiz edilen bilgilere erişebilir. Microsoft’un Müşteri Verilerini kullanmayacağını veya reklam ya da benzeri ticari amaçlarla bundan bilgi türetmeyeceğini belirten [Microsoft Çevrimiçi Hizmet Koşulları ve Gizlilik Bildirimi](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) belgesine bağlı kalıyoruz. Müşteri Verileri yalnızca size Azure hizmetlerini sağlamak için, bu hizmetleri sağlamayla uyumlu amaçlar da dahil olmak üzere gerektiğinde kullanılır. Müşteri Verileri üzerindeki tüm haklarınız saklıdır.

**Veri kullanımı**: Microsoft önleme ve algılama özelliklerimizi geliştirmek amacıyla birden fazla kiracıda görülen modelleri ve tehdit bilgilerini kullanır; bunu [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) belgemizde açıklanan gizlilik taahhütlerine uygun şekilde yaparız.

## <a name="data-location"></a>Veri konumu
**Depolama Hesaplarınız**: Sanal makinelerin çalıştığı her bölge için bir depolama hesabı belirtilir. Bunun yapılması verileri, verilerin toplandığı sanal makine ile aynı bölgede depolamanızı sağlar. Kilitlenme döküm dosyaları da dahil olmak üzere bu veriler, depolama hesabınıza kalıcı olarak depolanır. VM diski anlık görüntüleri, VM diski ile aynı depolama hesabına kaydedilir. 

**Azure Güvenlik Merkezi Depolama**: İş ortağı uyarıları, öneriler ve güvenlik durumu gibi güvenlik uyarıları hakkında bilgiler, şu anda Birleşik Devletler’de bulunan merkezde depolanmaktadır. Bu bilgiler size güvenlik uyarısı, öneri veya sistem durumu sağlamak üzere gerektiğinde sanal makinelerinizden toplanan ilgili yapılandırma bilgilerini ve güvenlik olaylarını içerebilir. 

Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder. Azure Güvenlik Merkezi bu analizi çalışma alanıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler.

Makine yapıları VM ile aynı bölgede merkezi olarak depolanır.


## <a name="managing-data-collection-from-virtual-machines"></a>Sanal makinelerden veri toplamayı yönetme
Azure Güvenlik Merkezi’ni etkinleştirmeyi seçtiğinizde her aboneliğiniz için veri toplama etkinleştirilir. Veri toplamayı Azure Güvenlik Merkezi’nin Güvenlik İlkesi bölümünden de açabilirsiniz. Veri Toplama açık olduğunda Azure Güvenlik Merkezi desteklenen tüm mevcut sanal makinelerde ve yeni oluşturulanlarda Azure Monitoring Agent’ı hazırlar. Azure Güvenlik İzleme uzantısı, güvenlikle ilgili çeşitli yapılandırmaları ve olayları [Windows için Olay İzleme (ETW)](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) izlerinde tarar. Ayrıca, işletim sistemi, makinenin çalışması sırasında olay günlüğü olaylarını ortaya koyar. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri, oturum açmış kullanıcı ve kiracı kimliği. Azure Monitoring Agent olay günlüğü girişleri ile ETW izlerini okur ve analiz için depolama hesabınıza kopyalar.

Sanal makinelerden veri toplamayı dilediğiniz zaman devre dışı bırakabilirsiniz; bunun yapılması Azure Güvenlik Merkezi tarafından daha önce yüklenmiş tüm İzleme Aracılarını kaldırır. Veri toplama devre dışı bırakılsa bile, VM diski anlık görüntüleri ve yapıt toplama işlemi etkin olmaya devam eder.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması hakkında bilgi aldınız. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek bkz.:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz




<!--HONumber=Jan17_HO5-->


