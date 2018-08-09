---
title: Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs
description: Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2018
ms.author: terrylan
ms.openlocfilehash: b0e674eb161af41a848f0456a033d615293a9947
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622798"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay bir şekilde sağlanmasını mümkün kılar. Güvenlik Merkezi, kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için gerekli aracıyı sanal makinelerinize sağlayabilir. Güvenlik duvarı cihazları için ise Güvenlik Merkezi gerekli ağ yapılandırmasının çoğunluğunu gerçekleştirebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetim**: Müşteriler tüm iş ortağı çözümlerini bir bakışta izlemek için tümleşik sistem durumu olaylarını kullanabilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Tümleşik güvenlik çözümleri şu anda aşağıdakileri içermektedir:

- Uç nokta koruması ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/windows/comprehensive-security) ve [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection))
- Web uygulaması güvenlik duvarı ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/products.html) ve [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Yeni nesil güvenlik duvarı ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html) ve [Palo Alto Networks](https://www.paloaltonetworks.com/products))
- Güvenlik açığı değerlendirmesi ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) ve [Rapid7](https://www.rapid7.com/products/insightvm/))

Uç nokta koruma tümleştirme deneyimi, çözüme göre farklılık gösterebilir. Aşağıdaki tabloda her bir çözümün deneyimi hakkında daha fazla bilgi verilmiştir:

| Uç Nokta Koruması               | Platformlar                             | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)                  | Windows Server 2016                   | Hayır, işletim sisteminde yerleşik           | Yes                       |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 | Uzantı ile                | Yes                       |
| Trend Micro – Tüm sürümler         | Windows Server Ailesi                 | Hayır                           | Yes                       |
| Symantec v12.1.1100+              | Windows Server Ailesi                 | Hayır                           | Yes                       |
| McAfee v10+                       | Windows Server Ailesi                 | Hayır                           | Yes                       |
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

1. [Azure Portal](https://azure.microsoft.com/features/azure-portal/)’da oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

  ![Güvenlik Merkezine Genel Bakış](./media/security-center-partner-integration/overview.png)

3. **Genel Bakış** altında **Güvenlik çözümleri**’ni seçin.

**Güvenlik çözümleri** altında tümleşik Azure güvenlik çözümlerinin sistem durumu hakkındaki bilgileri görüntüleyebilir ve temel yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca Common Event Format (CEF) biçimindeki Azure Active Directory Kimlik Koruması uyarılarını ve güvenlik duvarı günlükleri gibi diğer güvenlik veri kaynağı türlerini de bağlayabilirsiniz.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi'ne bağlı olan güvenlik çözümleri ve her çözümün sistem durumu hakkında bilgiler yer alır.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Daha fazla bilgi almak için bkz. [Bağlantılı iş ortağı çözümlerini yönetme](security-center-partner-solutions.md).

### <a name="discovered-solutions"></a>Bulunan çözümler

Güvenlik Merkezi, Azure’da çalışmasına karşın Güvenlik Merkezi’ne bağlı olmayan güvenlik çözümlerini otomatik olarak bulur ve çözümleri **Bulunan çözümler** bölümünde gösterir. Buna [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi Azure çözümlerinin yanı sıra iş ortağı çözümleri dahildir.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. Güvenlik fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

Güvenlik Merkezi ile tümleştirmek ve güvenlik uyarılarını bildirim olarak almak üzere bir çözümün altında **BAĞLAN** öğesini seçin.

![Bulunan çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Güvenlik Merkezi ayrıca Ortak Olay Biçimi (CEF) günlüklerini iletebilen, abonelikte dağıtılmış çözümleri bulur. CEF günlükleri kullanan [bir güvenlik çözümünü](quick-security-solutions.md) Güvenlik Merkezi'ne bağlama hakkında bilgi edinin.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Microsoft Advanced Threat Analytics'i Azure Güvenlik Merkezi'ne bağlama](security-center-ata-integration.md)
* [Azure Active Directory Kimlik Koruması'nı Azure Güvenlik Merkezi'ne bağlama](security-center-aadip-integration.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi’yle iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
