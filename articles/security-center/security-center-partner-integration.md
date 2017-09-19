---
title: "Azure Güvenlik Merkezi'nde iş ortağı ve çözüm tümleştirmesi | Microsoft Docs"
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
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8cc44da0f61362018d2757da58ca4fb3a9a43764
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde İş Ortağı ve Çözüm Tümleştirmesi

Bu makalede, genel güvenliği geliştirmenize yardımcı olmak amacıyla Azure Güvenlik Merkezi’nin iş ortaklarıyla nasıl tümleştirildiği açıklanır. Güvenlik Merkezi, Azure’da tümleştirilmiş bir deneyim sunar ve iş ortağı sertifikaları ve faturalama için Azure Market’ten yararlanır.

## <a name="why-deploy-partner-solutions-from-security-center"></a>Neden Güvenlik Merkezi’nden iş ortağı çözümleri dağıtmalı

Güvenlik Merkezi’nde iş ortağı tümleştirmesinden yararlanmak için dört temel neden:

- **Dağıtım kolaylığı**. Güvenlik Merkezi önerisini izleyerek bir iş ortağı çözümü dağıtmak çok daha kolaydır. Varsayılan bir kurulum ve ağ topolojisi kullanıldığında, dağıtım işlemi tamamen otomatik hale getirilebilir. Alternatif olarak, müşteriler daha fazla esneklik ve özelleştirme için yarı-otomatik bir seçeneği tercih edebilir.
- **Tümleşik algılamalar**. İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetimi**. Müşteriler, tümleşik sistem durumu olaylarını kullanarak bir bakışta tüm iş ortağı çözümlerini izleyebilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.
- **SIEM’e aktarma**. Müşteriler, Azure günlüğü tümleştirmesini (önizleme) kullanarak tüm Güvenlik Merkezi ve iş ortağı uyarılarını Common Event Format (CEF) biçiminde şirket içi Güvenlik Bilgileri ve Olay Yönetimi (SIEM) sistemlerine dışarı aktarabilir.


## <a name="partners-that-integrate-with-security-center"></a>Güvenlik Merkezi ile tümleştirilen iş ortakları

Şu anda Azure Market'te Güvenlik Merkezi ile kullanılabilen iş ortağı çözümü yerel tümleştirmeleri şunlardır:

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

Dağıtımdan sonra çözümün durumunu görüntülemek ve temel yönetim görevlerini gerçekleştirmek için **Güvenlik Merkezi** panosunda **İş ortağı çözümleri** seçeneğini belirleyin.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Güvenlik Çözümleri sayfasını açtığınızda görüntülenen içerik, altyapınıza göre değişiklik gösterebilir. Bir örneği önceki resimde verilen bu sayfada üç bölüm yer alır:

- **Bağlantılı çözümler**: Güvenlik Merkezi'ne bağlı çözümler görüntülenir.
- **Bulunan çözümler**: Güvenlik Merkezi'ne bağlı olmayan çözümler görüntülenir. Bu çözümleri bağlayarak bağlantılı çözümler altında görüntülenmesini sağlayabilirsiniz.  Güvenlik Merkezi bağlı olmayan herhangi bir çözüm algılamazsa bu bölüm gizlenir.
- **Veri kaynağı ekle**: Güvenlik Merkezi'ne ekleyebileceğiniz Azure ve Azure harici veri kaynakları görüntülenir.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi ile bağlı olan tüm güvenlik çözümleri gösterilir. 

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig10.png)

Her bağlantıda gördüğünüz bilgiler çözüme göre değişiklik gösterebilir. Her kutucukta yer alan bilgilerin arasında şunlar bulunabilir:

- İş ortağı için şirket simgesi.  Güvenlik Merkezi'nde şirket simgesi yoksa iş ortağının adının ilk karakteri görüntülenir.
- Çözüm türü.
- Bilgisayar adı görüntülenebilir.
- Sistem durumu.  Sistem durumu göstergesi gönderilmediyse Güvenlik Merkezi'nde gerecin rapor gönderip göndermediğini bildirmek üzere alınan son olayın tarihi ve saati gösterilir. Güvenlik Merkezi belirli bir çözümden sistem durumu göstergesi almazsa ilgili çözümün kutucuğu bu bölümde görünmez.

> [!NOTE]
> İzleme Güvenlik Merkezi'nde gerecin rapor gönderip göndermediğini bildirmek üzere alınan son olayın tarihi ve saati gösterilir. Sistem durumu göstergesi göndermeyen çözümler son 14 günde uyarı veya olay göndermediyse bağlı olarak gösterilir.
>  

Bu çözümlerin bazıları Azure ile tamamen tümleştirilmiş, diğerleri ise şirket içi çözümler olabilir. Güvenlik Merkezi [Common Event Format (CEF)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef) desteği sunduğundan CEF destekli güvenlik duvarı gibi CEF kullanan çözümlere bağlanabilir. Bu çözüm Güvenlik Merkezi'ne eklendikten sonra güvenlik duvarı günlükleri CEF biçiminde Güvenlik Merkezi'ne gönderir ve bu günlükler [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) ile işlenir. Güvenlik duvarı Azure harici bir kaynaktır ve olayları gönderir ancak bir sistem durumu göstergesi değildir.  Sistem durumu hakkında Güvenlik Merkezi'nde bulunan tek bilgi, gerecin son olay gönderdiği zamandır.  Azure harici tüm kaynaklar söz konusu olduğunda Güvenlik Merkezi, kutucuğun sistem durumu alanında son olayın alındığı tarihi ve saati gösterir ve bu da Azure harici kaynağın rapor göndermeyi sürdürdüğünü belirtir.

### <a name="discovered-solutions"></a>Bulunan çözümler

**Bulunan çözümler** bölümünde Azure aracılığıyla eklenmiş olan ve Güvenlik Merkezi'nin bağlantı önerisinde bulunduğu tüm çözümler gösterilir.

![Bulunan çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Güvenlik Merkezi [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi yerleşik Azure çözümleriyle tümleştirilebilir. Azure AD Kimlik Koruması lisansınız varsa ancak Güvenlik Merkezi'ne bağlı değilse AD Kimlik Koruması **Bulunan çözümler** altında listelenir. Bu çözümü Güvenlik Merkezi ile tümleştirmek için **Azure AD Kimlik Koruması** kutucuğu üzerindeki **BAĞLAN** seçeneğine tıklayın. Aşağıdaki sayfa açılır:

![Azure AD Kimlik Koruması](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

Azure AD Kimlik Koruması bağlantısını tamamlamak için verilerin kaydedildiği bir çalışma alanı seçmeniz gerekir. Azure AD Kimlik Koruması kaynaklı tüm veriler bu adımda seçilen çalışma alanı bölgesinden alınır.  Çalışma alanı seçiciden çalışma alanını seçtikten sonra veri akışı başlayacaktır.

Güvenlik Merkezi'ne bağlanmak için genel yönetici veya güvenlik yöneticisi olmanız gerekir.  Gerekli izinlere sahip değilseniz **Bağlan** düğmesi devre dışı bırakılır ve düğmenin devre dışı bırakılma nedenini açıklayan bir mesaj görüntülenir.

Azure AD Kimlik Koruması uyarıları Güvenlik Merkezi'nin algılama kanalından geçer ve bu sayede hem Güvenlik Merkezi'nden, hem de Azure Active Directory Kimlik Koruması'ndan uyarılar alabilirsiniz. Güvenlik Merkezi'nde [güvenlik olayı](https://docs.microsoft.com/azure/security-center/security-center-incident) oluşturmak için alakalı görünen tüm uyarılar birleştirilir. Güvenlik olayının açıklamasında şüpheli etkinlik hakkında daha fazla bilgi verilir.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

Azure ve Azure harici bilgisayarları Güvenlik Merkezi ile tümleştirmek üzere ekleyebilirsiniz.  Azure harici bilgisayarları ekleme imkanı şirket içi bir bilgisayarı veya CEF destekli bir gereci eklemenizi sağlar. 

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig11.png)


## <a name="see-also"></a>Ayrıca bkz.

Bu makalede, Azure Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)
* [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik Merkezi’nde türlerine göre güvenlik uyarıları](security-center-alerts-type.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

