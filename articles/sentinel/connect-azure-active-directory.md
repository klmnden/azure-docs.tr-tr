---
title: Gözcü Azure Önizleme için Azure AD verilerine bağlanma | Microsoft Docs
description: Azure Active Directory verilerini Azure Gözcü için bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 357435b8a4ac396c1548c89206f269730e871f6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204490"
---
# <a name="connect-data-from-azure-active-directory"></a>Azure Active Directory'den veri bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel sağlar, verileri toplamak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) ve Azure Gözcü akış. Seçebileceğiniz akışına [oturum açma günlükleri](../active-directory/reports-monitoring/concept-sign-ins.md) ve [denetim günlükleri](../active-directory/reports-monitoring/concept-audit-logs.md) .

## <a name="prerequisites"></a>Önkoşullar

- Active Directory oturum açma verilerini dışarı aktarmak istiyorsanız, bir Azure AD P1 veya P2 lisansı olması gerekir.

- Kullanıcı kayıtları akışla aktarmak istediğiniz Kiracı üzerinde genel yönetici veya güvenlik yönetici izinlerine sahip.


## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Azure Active Directory** Döşe.

2. Azure Gözcü akışı yapmak istediğiniz günlükleri yanındaki tıklayın **Connect**.

6. İlgili şema Log Analytics'te Azure AD'ye uyarılarını kullanmak için arama **SigninLogs** ve **AuditLogs**.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure AD connect öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
