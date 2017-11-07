---
title: "Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs"
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
ms.date: 10/26/2017
ms.author: yurid
ms.openlocfilehash: 0c0029d2dea293e71c6e3daf74b85f0234bfdffd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay bir şekilde sağlanmasını mümkün kılar. Güvenlik Merkezi, kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için gerekli aracıyı sanal makinelerinize sağlayabilir. Güvenlik duvarı cihazları için ise Güvenlik Merkezi gerekli ağ yapılandırmasının çoğunluğunu gerçekleştirebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetim**: Müşteriler tüm iş ortağı çözümlerini bir bakışta izlemek için tümleşik sistem durumu olaylarını kullanabilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Tümleşik güvenlik çözümleri şu anda aşağıdakileri içermektedir:

- Uç nokta koruması ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, Windows Defender, ve System Center Endpoint Protection (SCEP))
- Web uygulaması güvenlik duvarı ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) ve [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Yeni nesil güvenlik duvarı ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) ve [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))
- Güvenlik açığı değerlendirmesi ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Uç nokta koruma tümleştirme deneyimi, çözüme göre farklılık gösterebilir. Aşağıdaki tabloda her bir çözümün deneyimi hakkında daha fazla bilgi verilmiştir:

| Uç Nokta Koruması               | Platformlar                             | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)                  | Windows Server 2016                   | Hayır, işletim sisteminde yerleşik           | Evet                       |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 | Uzantı ile                | Evet                       |
| Trend Micro – Tüm sürümler         | Windows Server Ailesi                 | Uzantı ile                | Evet                       |
| Symantec v12+                     | Windows Server Ailesi                 | Hayır                           | Evet                        |
| MacAfee                           | Windows Server Ailesi                 | Hayır                           | Hayır                        |
| Kaspersky                         | Windows Server Ailesi                 | Hayır                           | Hayır                        |
| Sophos                            | Windows Server Ailesi                 | Hayır                           | Hayır                        |



## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. Aşağıdakiler dahil olmak üzere diğer güvenlik verisi kaynaklarına da bağlanabilirsiniz:

- Azure AD Kimlik Koruması
- Şirket içinde veya diğer bulutlarda çalışan bilgisayarlar
- Common Event Format (CEF) destekli güvenlik çözümü
- Microsoft Advanced Threat Analytics

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

Dağıtımdan sonra tümleşik Azure güvenlik çözümünün sistem durumu hakkındaki bilgileri görüntüleyebilir ve temel yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca Common Event Format (CEF) biçimindeki Azure Active Directory Kimlik Koruması uyarılarını ve güvenlik duvarı günlükleri gibi diğer güvenlik veri kaynağı türlerini de bağlayabilirsiniz. Güvenlik Merkezi panosunda Güvenlik çözümlerini seçin.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi'ne bağlı olan güvenlik çözümleri ve her çözümün sistem durumu hakkında bilgiler yer alır.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

### <a name="discovered-solutions"></a>Bulunan çözümler

**Bulunan çözümler** bölümü Azure aracılığıyla eklenmiş olan tüm çözümleri gösterir. Güvenlik Merkezi'nin bağlamayı önerdiği tüm çözümleri de gösterir.

![Bulunan çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Güvenlik Merkezi, Azure'da çalışan diğer güvenlik çözümlerini otomatik olarak bulur. Buna [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi Azure çözümlerinin yanı sıra Azure'da çalışan iş ortağı çözümleri dahildir. Bu çözümleri Güvenlik Merkezi ile tümleştirmek için **BAĞLAN**'ı seçin.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)
* [Microsoft Advanced Threat Analytics'i Azure Güvenlik Merkezi'ne bağlama](security-center-ata-integration.md)
* [Azure Active Directory Kimlik Koruması'nı Azure Güvenlik Merkezi'ne bağlama](security-center-aadip-integration.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi’yle iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
