---
title: "Azure Güvenlik Merkezi'nde iş ortağı ve çözüm tümleştirmesi | Microsoft Docs"
description: "Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin."
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
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: a6998c997840f1a9f349b85a4274908b611cd315
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde iş ortağı ve çözüm tümleştirmesi

Bu makalede, genel güvenliği geliştirmenize yardımcı olmak amacıyla Azure Güvenlik Merkezi’nin iş ortaklarıyla nasıl tümleştirildiği açıklanır. Güvenlik Merkezi, Azure'da tümleştirilmiş bir deneyim sunar ve iş ortağı sertifikaları ve faturalama için Azure Marketi'nden yararlanır.

## <a name="deploy-partner-solutions-from-security-center"></a>Güvenlik Merkezi'nden iş ortağı çözümleri dağıtma

Aşağıda Güvenlik Merkezi'nde iş ortağı tümleştirmesini kullanmak için dört temel neden verilmiştir:

- **Dağıtım kolaylığı**. Güvenlik Merkezi önerisini izleyerek bir iş ortağı çözümü dağıtmak çok daha kolaydır. Varsayılan bir kurulum ve ağ topolojisi kullanıldığında, dağıtım işlemi tamamen otomatik hale getirilebilir. Alternatif olarak, müşteriler daha fazla esneklik ve özelleştirme için yarı-otomatik bir seçeneği tercih edebilir.
- **Tümleşik algılamalar**. İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetimi**. Müşteriler, tümleşik sistem durumu olaylarını kullanarak bir bakışta tüm iş ortağı çözümlerini izleyebilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.
- **SIEM’e aktarma**. Müşteriler, Azure günlüğü tümleştirmesini (önizleme) kullanarak tüm Güvenlik Merkezi ve iş ortağı uyarılarını Common Event Format (CEF) biçiminde şirket içi Güvenlik Bilgileri ve Olay Yönetimi (SIEM) sistemlerine dışarı aktarabilir.


## <a name="partners-that-integrate-with-security-center"></a>Güvenlik Merkezi ile tümleştirilen iş ortakları

Şu anda Azure Marketi'nde Güvenlik Merkezi ile kullanılabilen iş ortağı çözümü yerel tümleştirmeleri şunlardır:

- **Uç nokta koruması**. [Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Azure Cloud Services ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma Yazılımı](https://docs.microsoft.com/azure/security/azure-security-antimalware).
- **Web uygulaması güvenlik duvarı**. [Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) ve [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/). 
- **Yeni nesil güvenlik duvarı**. [Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) ve [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html). 
- **Güvenlik açığı değerlendirmesi**. [Qualys](https://www.qualys.com/public-clouds/microsoft-azure/). 

Zamanla, Güvenlik Merkezi bu kategorilerdeki iş ortaklarının sayısını artıracak ve yeni kategoriler ekleyecektir. 

## <a name="deploy-a-partner-solution"></a>İş ortağı çözümü dağıtma

Azure ortamınızın kurulumuna ve tanımladığınız güvenlik ilkesine bağlı olarak, Güvenlik Merkezi bir iş ortağı çözümünün dağıtılmasını önerebilir. Bu Güvenlik Merkezi önerisi, iş ortağı çözümü seçme ve yükleme işleminde size yardımcı olur. Genel olarak dağıtım deneyimi kullandığınız çözüm türüne ve iş ortağına göre farklılık gösterebilir. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Uç nokta korumasını yükleme](security-center-install-endpoint-protection.md)
- [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md)
- [Yeni nesil güvenlik duvarı ekleme](security-center-add-next-generation-firewall.md)
- [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>İş ortağı çözümlerini yönetme

Dağıtımdan sonra çözümün durumunu görüntülemek ve temel yönetim görevlerini gerçekleştirmek için **Güvenlik Merkezi** panosunda **İş ortağı çözümleri**'ni seçin.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Güvenlik Çözümleri sayfasını açtığınızda görüntülenen içerik, altyapınıza göre değişiklik gösterebilir. Bir örneği önceki resimde verilen bu sayfada üç bölüm yer alır:

- **Bağlantılı çözümler**. Güvenlik Merkezi'ne bağlı çözümler görüntülenir.
- **Bulunan çözümler**. Güvenlik Merkezi'ne bağlı olmayan çözümler görüntülenir. Bu çözümleri bağlayarak **Bağlantılı çözümler** altında görüntülenmesini sağlayabilirsiniz. Güvenlik Merkezi bağlı olmayan herhangi bir çözüm algılamazsa bu bölüm gizlenir.
- **Veri kaynağı ekleme**. Güvenlik Merkezi'ne ekleyebileceğiniz Azure ve Azure harici veri kaynakları görüntülenir.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi ile bağlı olan tüm güvenlik çözümleri gösterilir. 

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Gördüğünüz bilgiler çözüme göre değişiklik gösterebilir. Her kutucukta yer alan bilgilerin arasında şunlar bulunabilir:

- **İş ortağı için şirket simgesi**. Güvenlik Merkezi'nde şirket simgesi yoksa iş ortağının adının ilk karakteri görüntülenir.
- **Çözüm türü**. Çözüm türü görüntülenir.
- **Bilgisayar adı**. Bilgisayar adı görüntülenir.
- **Sistem durumu**. Sistem durumu göstergesi gönderilmediyse Güvenlik Merkezi'nde gerecin rapor gönderip göndermediğini bildirmek üzere alınan son olayın tarihi ve saati gösterilir. Güvenlik Merkezi belirli bir çözümden sistem durumu göstergesi almazsa ilgili çözümün kutucuğu bu bölümde görünmez.

> [!NOTE]
> Güvenlik Merkezi'nde gerecin rapor gönderip göndermediğini bildirmek üzere alınan son olayın tarihi ve saati gösterilir. Sistem durumu göstergesi göndermeyen çözümler son 14 günde uyarı veya olay göndermediyse bağlı olarak gösterilir.
>  

Bu çözümlerin bazıları Azure ile tamamen tümleştirilmiş, diğerleri ise şirket içi çözümler olabilir. Güvenlik Merkezi [CEF](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef) desteği sunduğundan CEF destekli güvenlik duvarı gibi CEF kullanan çözümlere bağlanabilir. Bu çözüm Güvenlik Merkezi'ne eklendikten sonra güvenlik duvarı günlükleri CEF biçiminde Güvenlik Merkezi'ne gönderir ve bu günlükler [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) ile işlenir. Güvenlik duvarı Azure dışı bir kaynaktır ve olayları gönderir ancak sistem durumu göstergesi değildir. Sistem durumu hakkında Güvenlik Merkezi'nde bulunan tek bilgi, gerecin son olay gönderdiği zamandır. Azure dışı kaynaklar söz konusu olduğunda Güvenlik Merkezi, kutucuğun sistem durumu alanında son olayın alındığı tarihi ve saati gösterir. Bu bilgiler Azure dışı kaynağın rapor göndermeyi sürdürdüğünü belirtir.

### <a name="discovered-solutions"></a>Bulunan çözümler

**Bulunan çözümler** bölümü Azure aracılığıyla eklenmiş olan tüm çözümleri gösterir. Güvenlik Merkezi'nin bağlamayı önerdiği tüm çözümleri de gösterir.

![Bulunan çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Güvenlik Merkezi [Azure Active Directory (Azure AD) Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi yerleşik Azure çözümleriyle tümleştirilebilir. Azure AD Kimlik Koruması lisansınız varsa ancak Güvenlik Merkezi'ne bağlı değilse AD Kimlik Koruması **Bulunan çözümler** altında listelenir. Bu çözümü Güvenlik Merkezi ile tümleştirmek için **Azure AD Kimlik Koruması** kutucuğu üzerindeki **BAĞLAN** seçeneğini belirleyin. Aşağıdaki sayfa açılır:

![Azure AD Kimlik Koruması](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

Azure AD Kimlik Koruması bağlantısını tamamlamak için verilerin kaydedildiği bir çalışma alanı seçin. Azure AD Kimlik Koruması kaynaklı tüm veriler bu adımda seçilen çalışma alanı bölgesinden alınır. Çalışma alanı seçiciden çalışma alanını seçtikten sonra veri akışı başlayacaktır.

Güvenlik Merkezi'ne bağlanmak için genel yönetici veya güvenlik yöneticisi olmanız gerekir. Bu izinlere sahip değilseniz **Bağlan** düğmesi devre dışı kalır. Düğmenin devre dışı kalma nedenini açıklayan bir ileti görüntülenir.

Azure AD Kimlik Koruması uyarıları Güvenlik Merkezi algılama hattından geçer. Bu sayede hem Güvenlik Merkezi hem de Azure AD Kimlik Koruması uyarılarını alırsınız. Güvenlik Merkezi [güvenlik olayı](https://docs.microsoft.com/azure/security-center/security-center-incident) oluşturmak için ilgili görünen tüm uyarıları birleştirir. Güvenlik olayının açıklamasında şüpheli etkinlik hakkında daha fazla bilgi verilir.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

Azure ve Azure harici bilgisayarları Güvenlik Merkezi ile tümleştirmek üzere ekleyebilirsiniz. Azure dışı bilgisayarları ekleme imkanı şirket içi bir bilgisayarı veya CEF destekli bir gereci eklemenizi sağlar. 

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Ayrıca bkz.

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)
* [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik Merkezi’nde türlerine göre güvenlik uyarıları](security-center-alerts-type.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi’yle iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
