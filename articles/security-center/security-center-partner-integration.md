---
title: "Azure Güvenlik Merkezi&quot;nde İş Ortağı Tümleştirmesi | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi’nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini açıklar."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 095b5c9d1a888a4061450234f80c52c5834fbf53


---
# <a name="partner-integration-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde İş Ortağı Tümleştirmesi
Bu belge, Azure Güvenlik Merkezi’nin iş ortaklarıyla nasıl tümleştirildiğini açıklar. Azure’da genel güvenliği geliştirmek ve tümleşik bir deneyim sunmak amacıyla gerçekleştirilen bu tümleştirme, iş ortaklarının sertifika ve faturalandırma işlemleri için Azure Market’ten yararlanma olanağı da sunar.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Neden Güvenlik Merkezi’nden iş ortağı çözümleri dağıtmalıyım?

Güvenlik Merkezi’nde iş ortağı tümleştirmesinden yararlanmak için dört temel neden şunlardır:

- **Dağıtım kolaylığı**: Güvenlik Merkezi’nin önerisini izleyerek bir iş ortağı çözümü dağıtmak çok daha kolaydır. Dağıtım süreci varsayılan bir yapılandırma ve ağ topolojisi kullanılarak tamamen otomatikleştirilebilir veya müşteriler daha fazla yapılandırma esnekliği ve özelleştirmesi sağlayan yarı otomatik bir seçeneği tercih edebilir.
- **Tümleşik Algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik Sistem Durumu İzleme ve Yönetim**: Tümleşik sistem durumu olayları, müşterilerin tüm iş ortağı çözümlerini bir bakışta izlemesine olanak sağlar. İş ortağı çözümü kullanarak gelişmiş yapılandırmalara kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.
- **SIEM’ye aktarma**: Müşteriler artık Microsoft Azure Günlük Tümleştirmesi’ni (önizleme) kullanarak CEF biçimindeki tüm Güvenlik Merkezi ve iş ortağı uyarılarını şirket içi SIEM sistemlerine aktarabilir


## <a name="what-partners-are-integrated-with-security-center"></a>Güvenlik Merkezi hangi iş ortakları ile tümleştirilebilir?
Güvenlik Merkezi şu anda aşağıdaki iş ortaklarıyla tümleştirilebilir:

- Uç Nokta Koruması (Trend Micro), 
- Web Uygulaması Güvenlik Duvarı (Barracuda, F5, Imperva ve yakında Microsoft WAF ve Fortinet), 
- Yeni Nesil Güvenlik Duvarı (Check Point, Barracuda ve yakında Fortinet ve Cisco) çözümleri. 
- Güvenlik Açığı Değerlendirmesi (Qualys - önizleme) çözümleri. 

Zamanla, Güvenlik Merkezi bu mevcut kategorilerdeki ortakların sayısını genişletecek ve yeni kategoriler ekleyecektir. 

## <a name="how-to-deploy-a-partner-solution"></a>Bir iş ortağı çözümü nasıl dağıtılır?

Azure ortamınızın yapılandırmasına ve tanımladığınız güvenlik ilkesine bağlı olarak Güvenlik Merkezi, bir iş ortağı çözümünün dağıtılmasını önerebilir. Bu öneri, iş ortağı çözümü seçme ve yükleme işleminde size yardımcı olacaktır. Bu noktada, genel dağıtım deneyimi çözüm türüne ve iş ortağına göre farklılık gösterebilir. Daha fazla bilgi için aşağıdaki bağlantıları inceleyin:

- [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md)
- [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md)
- [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md)
- [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md)

## <a name="how-to-manage-partner-solutions"></a>İş ortağı çözümlerimi nasıl yönetebilirim?

Bir iş ortağı çözümü dağıtıldıktan sonra, ana Güvenlik Merkezi panosundaki İş ortağı çözümü kutucuğundan çözümün durumu hakkında bilgi alabilir ve temel yönetim görevlerini gerçekleştirebilirsiniz. Güvenlik Merkezi'nde iş ortağı çözümlerini yönetme hakkında daha fazla bilgi edinmek için [Azure Güvenlik Merkezi ile İş ortağı çözümlerini izleme](security-center-partner-solutions.md) konusunu okuyun.

![İş Ortağı Tümleştirmesi](./media/security-center-partner-integration/security-center-partner-integration-fig1.png)


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde iş ortağı çözümünün nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi’nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Dec16_HO1-->


