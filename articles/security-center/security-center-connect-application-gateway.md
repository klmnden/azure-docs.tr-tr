---
title: Microsoft Azure uygulama ağ geçidi Azure Güvenlik Merkezi'ne bağlanma | Microsoft Docs
description: Uygulama ağ geçidi ve Azure Güvenlik Merkezi, kaynaklarınızın genel güvenliğini artırmak için tümleştirme öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/07/2018
ms.author: terrylan
ms.openlocfilehash: 7c15e5a86df7ff2a374aa9b62d2775b1eb035fc6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29854496"
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Microsoft Azure uygulama ağ geçidi Azure Güvenlik Merkezi'ne bağlanma
Bu belge, uygulama ağ geçidi web uygulaması Güvenlik Duvarı (WAF) ve Güvenlik Merkezi ile tümleştirme yapılandırmanıza yardımcı olur.

## <a name="why-connect-application-gateway"></a>Uygulama ağ geçidi neden bağlanabilir?
Uygulama ağ geçidi olarak WAF web uygulamaları, SQL ekleme gibi ortak web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini korur. Güvenlik Merkezi, uygulama ve ortamınızda korumasız web uygulamaları tehditleri algılamak için ağ geçidi ile entegre olur.

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirme nasıl yapılandırırım?
Güvenlik Merkezi Abonelikteki önceden dağıtılan WAF örneklerini bulur. Bu çözümlerin uyarıları tümleştirilmesi izin vermek için Güvenlik Merkezi bağlayın.

> [!NOTE]
> Uygulama ağ geçidi WAF, Güvenlik Merkezi'nin sağlanabilir **önerileri** açıklandığı gibi [bir web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md).
>
>

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

2. Üzerinde **Microsoft Azure menü**seçin **Güvenlik Merkezi**. **Güvenlik Merkezi - Genel Bakış** açılır.

3. Altında **genel bakış**seçin **güvenlik çözümleri**.

  ![Güvenlik Merkezi'ne genel bakış](./media/security-center-connect-application-gateway/overview.png)

4. Altında **bulunan çözümleri** Microsoft SaaS tabanlı Web uygulaması güvenlik duvarı bulmak ve seçmek **BAĞLAN**.

  ![Bulunan çözümler](./media/security-center-connect-application-gateway/connect.png)

5. **WAF çözümü bağlantı** açar.  Seçin **Bağlan** WAF ve Güvenlik Merkezi tümleştirmek için.

  ![WAF çözümü bağla](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde uygulama ağ geçidi WAF tümleştirmeyi öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi'nde güvenlik çözümlerini tümleştirmenize](security-center-partner-integration.md)
* [Bağlanan Microsoft Advanced Threat Analytics için Güvenlik Merkezi](security-center-ata-integration.md)
* [Azure Active Directory kimlik koruması Güvenlik Merkezi'ne bağlanma](security-center-aadip-integration.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md).
* [Güvenlik Merkezi’yle iş ortağı çözümlerini izleme](security-center-partner-solutions.md).
* [Azure Güvenlik Merkezi SSS](security-center-faq.md).
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/).
