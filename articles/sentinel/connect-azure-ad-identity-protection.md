---
title: Gözcü Azure Önizleme için Azure AD kimlik koruması verilere bağlanma | Microsoft Docs
description: Azure Gözcü için Azure AD kimlik koruması veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 10dc31e21f20618450de6d99b3fce40d63272d31
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204366"
---
# <a name="connect-data-from-azure-ad-identity-protection"></a>Azure AD kimlik Koruması ' veri bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlüklerinden akışını [Azure AD kimlik koruması](https://docs.microsoft.com/azure/information-protection/reports-aip) Azure Gözcü stream uyarılar Azure panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek için Gözcü içine içine. Azure Active Directory kimlik koruması, risk altındaki kullanıcıların, risk olaylarının ve güvenlik açıklarının birleştirilmiş bir görünüm riski hemen düzeltme ve gelecekte gerçekleşecek olayları otomatik olarak düzeltmeye yönelik ilkeler ayarlama olanağı sağlar. Hizmet müşteri kimliklerini koruma Microsoft'un deneyimini temel alır ve günlük 13 milyar oturum bileşenler düzeltebilmeniz doğruluk. 


## <a name="prerequisites"></a>Önkoşullar

- Olmalıdır bir [Azure Active Directory Premium P1 veya P2 lisansı](https://azure.microsoft.com/pricing/details/active-directory/)
- Genel yönetici veya güvenlik yöneticisi izinleri ile kullanıcı


## <a name="connect-to-azure-ad-identity-protection"></a>İçin Azure AD kimlik koruması bağlantısı kurma

Azure AD kimlik koruması varsa olduğundan emin olun [ağınızda etkin](../active-directory/identity-protection/enable.md).
Varsa Azure AD kimlik koruması dağıtılan ve veri alma, uyarı verileri kolayca Azure Gözcü aktarılabilir.


1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Azure AD kimlik koruması** Döşe.

2. Tıklayın **Connect** Azure Gözcü Azure AD kimlik koruması olaylarının akışını başlatmak için.


6. İlgili şema Log Analytics'te Azure AD kimlik koruması uyarılarını kullanmak için arama **IdentityProtectionLogs_CL**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure AD kimlik koruması bağlantısı kurma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
