---
title: Azure Güvenlik Merkezi Microsoft Azure uygulama ağ geçidine bağlanma | Microsoft Docs
description: Uygulama ağ geçidi ve Azure Güvenlik Merkezi, kaynaklarınızın genel güvenliğini geliştirmek amacıyla tümleştirmeyi öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: 5638b71147592ae71c741ca86da68ddfec668af5
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299075"
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Azure Güvenlik Merkezi Microsoft Azure uygulama ağ geçidine bağlanma
Bu belge, Güvenlik Merkezi ile Application Gateway web uygulaması Güvenlik Duvarı (WAF) ile tümleştirmeyi yapılandırmak için yardımcı olur.

## <a name="why-connect-application-gateway"></a>Application Gateway neden bağlamalıyız?
WAF Application Gateway içindeki web uygulamalarını SQL ekleme gibi yaygın web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturum ele geçirme korur. Güvenlik Merkezi ortamınızda korumasız web uygulamalarını tehditleri algılamak ve engellemek için uygulama ağ geçidi ile tümleştirilir.

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirmenin nasıl yapılandırabilirim?
Güvenlik Merkezi, abonelikte daha önceden dağıtılan WAF örneklerini bulur. Bu çözümleri tümleştirme uyarı izin vermek için Güvenlik Merkezi'ne bağlayın.

> [!NOTE]
> Application Gateway WAF, Güvenlik Merkezi'nin sağlanabilir **önerileri** açıklandığı [bir web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md).
>
>

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin.

3. Altında **kaynak güvenlik SAĞLIĞI**seçin **güvenlik çözümlerini**.

  ![Güvenlik Merkezine Genel Bakış](./media/security-center-connect-application-gateway/overview.png)

4. Altında **bulunan çözümler** Microsoft SaaS tabanlı Web uygulaması Güvenlik Duvarı'nı bulun ve seçin **CONNECT**.

  ![Bulunan çözümler](./media/security-center-connect-application-gateway/connect.png)

5. **WAF çözümü Bağla** açılır.  Seçin **Connect** WAF ve Güvenlik Merkezi ile tümleştirmek için.

  ![WAF çözümü bağla](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde Application Gateway WAF tümleştirmeyi öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi'nde güvenlik çözümlerini tümleştirme](security-center-partner-integration.md)
* [Bağlanan Microsoft Advanced Threat Analytics için Güvenlik Merkezi](security-center-ata-integration.md)
* [Azure Active Directory kimlik koruması Güvenlik Merkezi'ne bağlama](security-center-aadip-integration.md)
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md).
* [Güvenlik Merkezi’yle iş ortağı çözümlerini izleme](security-center-partner-solutions.md).
* [Azure Güvenlik Merkezi SSS](security-center-faq.md).
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/).
