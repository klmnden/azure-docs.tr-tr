---
title: Gözcü Azure önizlemede Azure AD veri toplama | Microsoft Docs
description: Gözcü Azure, Azure Active Directory verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: e145807b3170b7b822eabba09ce2e97da5467761
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993261"
---
# <a name="collect-data-from-azure-active-directory"></a>Azure Active Directory'den veri toplama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel sağlar, verileri toplamak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) ve Azure Gözcü akış. Seçebileceğiniz akışına [oturum açma günlükleri](../active-directory/reports-monitoring/concept-sign-ins.md) ve [denetim günlükleri](../active-directory/reports-monitoring/concept-audit-logs.md) .

## <a name="prerequisites"></a>Önkoşullar

- Active Directory oturum açma verilerini dışarı aktarmak istiyorsanız, bir Azure AD P1 veya P2 lisansı olması gerekir.

- Kullanıcı kayıtları akışla aktarmak istediğiniz Kiracı üzerinde genel yönetici veya güvenlik yönetici izinlerine sahip.


## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Azure Active Directory** Döşe.

2. Azure Gözcü akışı yapmak istediğiniz günlükleri yanındaki tıklayın **Connect**.






## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure AD connect öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
