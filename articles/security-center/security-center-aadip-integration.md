---
title: Azure Active Directory kimlik koruması, Azure Güvenlik Merkezi'ne bağlama | Microsoft Docs
description: Azure Güvenlik Merkezi'nin Azure Active Directory kimlik koruması ile nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: terrylan
manager: MBaldwin
editor: ''
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2018
ms.author: yurid
ms.openlocfilehash: 9c13bd671efee5bc07885320cbaa0bd090cc1390
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51226369"
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Azure Active Directory kimlik koruması, Azure Güvenlik Merkezi'ne bağlama
Bu belge, Azure Active Directory (AD) kimlik koruması ve Azure Güvenlik Merkezi arasında tümleştirmesini yapılandırmak için yardımcı olur.

## <a name="why-connect-azure-ad-identity-protection"></a>Azure AD kimlik koruması neden bağlamalıyız?
[Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılamaya yardımcı olur. Bağlandığınızda, Güvenlik Merkezi'nde Azure AD kimlik koruması uyarıları görüntüleyebilirsiniz. Bu tümleştirme, görüntülemek, ilişkilendirmenize ve karma bulut iş yüklerinizin Güvenlik Merkezi'nde ilgili tüm güvenlik uyarıları araştırmanıza olanak sağlar. 

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirmenin nasıl yapılandırabilirim?
Kuruluşunuz Azure AD kimlik koruması zaten kullanılıyorsa tümleştirmesini yapılandırmak için aşağıdaki adımları izleyin:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede, tıklayın **güvenlik çözümlerini**. Güvenlik Merkezi, kuruluşunuz için Azure AD kimlik koruması etkin olup olmadığını otomatik olarak bulur.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Tıklayın **CONNECT**.
4. İçinde **tümleştirmek, Azure AD kimlik koruması** sayfasında, aşağı kaydırın ve uygun çalışma alanı seçin:

    ![çalışma alanı](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. **Bağlan**'a tıklayın.

Bu yapılandırmayı tamamladıktan sonra Azure AD kimlik koruması çözümü görünür **güvenlik çözümlerini** sayfasındaki **bağlantılı çözümler**. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure AD kimlik koruması Güvenlik Merkezi'ne bağlama öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Microsoft Advanced Threat Analytics'i Azure Güvenlik Merkezi'ne bağlama](security-center-ata-integration.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verileri nasıl yönetildiği ve korunduğu Güvenlik Merkezi'nde öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.


