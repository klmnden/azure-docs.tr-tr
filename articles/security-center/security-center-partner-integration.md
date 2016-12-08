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
ms.date: 11/28/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 0c946ce6a96f2e3644b9890dad5d60a35ad4bcb7
ms.openlocfilehash: 10184c52d532eb56e66212fafdea3d059b0c43e3


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

Zaten Güvenlik Merkezi’nde dağıtılmış olan iş ortağı çözümlerine Güvenlik Merkezi panosundaki İş ortağı çözümü kutucuğundan kolayca erişilebilir:

![İş Ortağı Tümleştirmesi](./media/security-center-partner-integration/security-center-partner-integration-fig1.png)

Bir Güvenlik Merkezi önerisini temel alarak yeni bir iş ortağı çözümü dağıtmak için aşağıdaki adımları uygulayın:

> [!NOTE]
> Aşağıdaki örnekte verilen adımlar, bir web uygulaması güvenlik duvarıyla korumak istediğiniz bir iş yükünüz olduğunu varsayar.

1. Güvenlik Merkezi panosunda **Öneriler** kutucuğuna tıklayın.
2. **Öneriler** dikey penceresinde **Yeni web uygulaması güvenlik duvarı ekle**’ye tıklayın.
3. **Web uygulaması güvenlik duvarı ekle** dikey penceresi altından uygulama adına tıklayın.
4. **Web Uygulaması Güvenlik Duvarı ekle** dikey penceresinde **Yeni Oluştur**’a tıklayın.
5. **Yeni Web Uygulaması Güvenlik Duvarı oluştur** dikey penceresinde web uygulaması güvenlik duvarı özelliği sunan geçerli iş ortaklarının listesi gösterilir.
6. Uygun iş ortağı çözümünü seçin ve adımları izleyin (bu adımlar iş ortağına göre değişiklik gösterir).

Bu noktada, genel dağıtım deneyimi iş ortağına göre farklılık gösterebilir. Güvenlik Merkezi’nde iş ortağı çözümlerini yönetme hakkında daha fazla bilgi edinmek için Azure Güvenlik Merkezi ile [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) konusunu okuyun.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde iş ortağı çözümünün nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi’nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Nov16_HO5-->


