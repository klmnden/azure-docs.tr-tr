---
title: "Azure Güvenlik Merkezi'nde İş Ortağı tümleştirmesi | Microsoft Belgeleri"
description: "Azure Güvenlik Merkezi’nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin."
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
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 4d0909e926de14a0cbe9799b969ac7a1946d69d1
ms.contentlocale: tr-tr
ms.lasthandoff: 07/10/2017


---
# <a name="partner-integration-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde İş Ortağı tümleştirmesi

Bu makalede, genel güvenliği geliştirmenize yardımcı olmak amacıyla Azure Güvenlik Merkezi’nin iş ortaklarıyla nasıl tümleştirildiği açıklanır. Güvenlik Merkezi, Azure’da tümleştirilmiş bir deneyim sunar ve iş ortağı sertifikaları ve faturalama için Azure Market’ten yararlanır.

> [!NOTE] 
> Haziran 2017'den itibaren, Güvenlik Merkezi veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanmaktadır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi platform geçişi](security-center-platform-migration.md). Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Neden Güvenlik Merkezi’nden iş ortağı çözümleri dağıtmalı

Güvenlik Merkezi’nde iş ortağı tümleştirmesinden yararlanmak için dört temel neden:

- **Dağıtım kolaylığı**. Güvenlik Merkezi önerisini izleyerek bir iş ortağı çözümü dağıtmak çok daha kolaydır. Varsayılan bir kurulum ve ağ topolojisi kullanıldığında, dağıtım işlemi tamamen otomatik hale getirilebilir. Alternatif olarak, müşteriler daha fazla esneklik ve özelleştirme için yarı-otomatik bir seçeneği tercih edebilir.
- **Tümleşik algılamalar**. İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetimi**. Müşteriler, tümleşik sistem durumu olaylarını kullanarak bir bakışta tüm iş ortağı çözümlerini izleyebilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.
- **SIEM’e aktarma**. Müşteriler, Azure günlüğü tümleştirmesini (önizleme) kullanarak tüm Güvenlik Merkezi ve iş ortağı uyarılarını Common Event Format (CEF) biçiminde şirket içi Güvenlik Bilgileri ve Olay Yönetimi (SIEM) sistemlerine dışarı aktarabilir.


## <a name="partners-that-integrate-with-security-center"></a>Güvenlik Merkezi ile tümleştirilen iş ortakları

Güvenlik Merkezi şu anda aşağıdaki çözümlerle tümleştirilebilir:

- Uç nokta koruması ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Azure Cloud Services ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma Yazılımı](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Web uygulaması güvenlik duvarı ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) ve [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Yeni nesil güvenlik duvarı ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) ve [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Güvenlik açığı değerlendirmesi ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Zamanla, Güvenlik Merkezi bu kategorilerdeki iş ortaklarının sayısını artıracak ve yeni kategoriler ekleyecektir. 

## <a name="deploy-a-partner-solution"></a>İş ortağı çözümü dağıtma

Azure ortamınızın kurulumuna ve tanımladığınız güvenlik ilkesine bağlı olarak, Güvenlik Merkezi bir iş ortağı çözümünün dağıtılmasını önerebilir. Bu Güvenlik Merkezi önerisi, iş ortağı çözümü seçme ve yükleme işleminde size yardımcı olur. Genel olarak dağıtım deneyimi kullandığınız çözüm türüne ve iş ortağına göre farklılık gösterebilir. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Uç nokta korumasını yükleme](security-center-install-endpoint-protection.md)
- [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md)
- [Yeni nesil güvenlik duvarı ekleme](security-center-add-next-generation-firewall.md)
- [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>İş ortağı çözümlerini yönetme

Dağıtımdan sonra çözümün durumunu görüntülemek ve temel yönetim görevlerini gerçekleştirmek için **Güvenlik Merkezi** dikey penceresinde **İş ortağı çözümleri** kutucuğunu seçin. Güvenlik Merkezi'nde iş ortağı çözümlerini yönetme hakkında daha fazla bilgi edinmek, bkz. [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md).

![İş ortağı tümleştirmesi](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> Symantec uç nokta koruması desteği yalnızca bulma ile sınırlıdır. Sistem durumu uyarıları sağlanmaz.
>

## <a name="see-also"></a>Ayrıca bkz.

Bu makalede, Azure Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)
* [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik Merkezi’nde türlerine göre güvenlik uyarıları](security-center-alerts-type.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

