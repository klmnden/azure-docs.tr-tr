---
title: "Azure Active Directory kimlik koruması Azure Güvenlik Merkezi'ne bağlanma | Microsoft Docs"
description: "Azure Güvenlik Merkezi Azure Active Directory kimlik koruması ile nasıl tümleşik çalıştığını öğrenin."
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 7562dd5e1c303a6cb97d25bda5aa080bb5643583
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Azure Active Directory kimlik koruması Azure Güvenlik Merkezi'ne bağlanma
Bu belge, Azure Active Directory (AD) kimlik koruması ile Azure Güvenlik Merkezi arasında tümleştirme yapılandırmanıza yardımcı olur.

## <a name="why-connect-azure-ad-identity-protection"></a>Neden Azure AD Identity Protection bağlanabilir?
[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılamaya yardımcı olur. Bağlandığınızda, Güvenlik Merkezi'nde Azure AD Identity Protection uyarıları görüntüleyebilirsiniz. Bu tümleştirme, görüntülemek, bağıntılı ve Güvenlik Merkezi, karma bulut iş yüklerini ilgili tüm güvenlik uyarılarını araştırmak sağlar. 

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirme nasıl yapılandırırım?
Kuruluşunuz Azure AD Identity Protection kullanıyorsa, tümleştirme yapılandırmak için aşağıdaki adımları izleyin:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede, tıklatın **güvenlik çözümleri**. Güvenlik Merkezi otomatik olarak, kuruluşunuz için Azure AD kimlik koruması etkin olup olmadığını bulur.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Tıklatın **BAĞLAN**.
4. İçinde **tümleştirmek Azure AD Identity Protection** sayfasında aşağı kaydırın ve uygun çalışma alanını seçin:

    ![Çalışma alanı](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. **Bağlan**'a tıklayın.

Bu yapılandırmayı tamamladıktan sonra Azure AD kimlik koruması çözümü görünür **güvenlik çözümleri** sayfasında **bağlı çözümleri**. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure AD Identity Protection Güvenlik Merkezi'ne bağlanmak öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Microsoft Advanced Threat Analytics'i Azure Güvenlik Merkezi'ne bağlama](security-center-ata-integration.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) — nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verilerin yönetilmesi ve diğer Güvenlik Merkezi'nde korunması nasıl öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.


