---
title: "Azure Güvenlik Merkezi'nde İş Ortağı Tümleştirmesi | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi’nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini açıklar."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 85460ea37d251b53074de9d832610d253700f2c1
ms.contentlocale: tr-tr
ms.lasthandoff: 06/24/2017


---
<a id="partner-integration-in-azure-security-center" class="xliff"></a>

# Azure Güvenlik Merkezi'nde İş Ortağı Tümleştirmesi
Bu belge, Azure Güvenlik Merkezi’nin iş ortaklarıyla nasıl tümleştirildiğini açıklar. Azure’da genel güvenliği geliştirmek ve tümleşik bir deneyim sunmak amacıyla gerçekleştirilen bu tümleştirme, iş ortaklarının sertifika ve faturalandırma işlemleri için Azure Market’ten yararlanma olanağı da sunar.

>[!NOTE] 
>Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanacak. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Platform Geçişi](security-center-platform-migration.md) makalesine bakın. Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>

<a id="why-deploy-partners-solutions-from-security-center" class="xliff"></a>

## Neden Güvenlik Merkezi’nden iş ortağı çözümleri dağıtmalıyım?

Güvenlik Merkezi’nde iş ortağı tümleştirmesinden yararlanmak için dört temel neden şunlardır:

- **Dağıtım kolaylığı**: Güvenlik Merkezi’nin önerisini izleyerek bir iş ortağı çözümü dağıtmak çok daha kolaydır. Dağıtım süreci varsayılan bir yapılandırma ve ağ topolojisi kullanılarak tamamen otomatikleştirilebilir veya müşteriler daha fazla yapılandırma esnekliği ve özelleştirmesi sağlayan yarı otomatik bir seçeneği tercih edebilir.
- **Tümleşik Algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik Sistem Durumu İzleme ve Yönetim**: Tümleşik sistem durumu olayları, müşterilerin tüm iş ortağı çözümlerini bir bakışta izlemesine olanak sağlar. İş ortağı çözümü kullanarak gelişmiş yapılandırmalara kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.
- **SIEM'ye aktarma**: Müşteriler artık Microsoft Azure Günlük Tümleştirmesi'ni (önizleme) kullanarak CEF biçimindeki tüm Güvenlik Merkezi ve iş ortağı uyarılarını şirket içi SIEM sistemlerine aktarabilir


<a id="what-partners-are-integrated-with-security-center" class="xliff"></a>

## Güvenlik Merkezi hangi iş ortakları ile tümleştirilebilir?
Güvenlik Merkezi şu anda aşağıdaki çözümlerle tümleştirilebilir:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma Yazılımı](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Web Uygulaması Güvenlik Duvarı ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), [App Gateway WAF](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Yeni Nesil Güvenlik Duvarı ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) ve [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Güvenlik Açığı Değerlendirmesi ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Zamanla, Güvenlik Merkezi bu mevcut kategorilerdeki ortakların sayısını genişletecek ve yeni kategoriler ekleyecektir. 

<a id="how-to-deploy-a-partner-solution" class="xliff"></a>

## Bir iş ortağı çözümü nasıl dağıtılır?

Azure ortamınızın yapılandırmasına ve tanımladığınız güvenlik ilkesine bağlı olarak Güvenlik Merkezi, bir iş ortağı çözümünün dağıtılmasını önerebilir. Bu öneri, iş ortağı çözümü seçme ve yükleme işleminde size yardımcı olacaktır. Bu noktada, genel dağıtım deneyimi çözüm türüne ve iş ortağına göre farklılık gösterebilir. Daha fazla bilgi için aşağıdaki bağlantıları inceleyin:

- [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md)
- [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md)
- [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md)
- [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md)

<a id="how-to-manage-partner-solutions" class="xliff"></a>

## İş ortağı çözümlerimi nasıl yönetebilirim?

Bir iş ortağı çözümü dağıtıldıktan sonra, ana Güvenlik Merkezi panosundaki İş ortağı çözümü kutucuğundan çözümün durumu hakkında bilgi alabilir ve temel yönetim görevlerini gerçekleştirebilirsiniz. Güvenlik Merkezi'nde iş ortağı çözümlerini yönetme hakkında daha fazla bilgi edinmek için [Azure Güvenlik Merkezi ile İş ortağı çözümlerini izleme](security-center-partner-solutions.md) konusunu okuyun.

![İş Ortağı Tümleştirmesi](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> Symantec Endpoint Protection desteği yalnızca bulma ile sınırlıdır ve sistem durum uyarıları sunulmaz.
>

<a id="see-also" class="xliff"></a>

## Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde iş ortağı çözümünün nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi’nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

