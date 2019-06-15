---
title: Fusion Gözcü Azure önizlemesinde etkinleştirme | Microsoft Docs
description: Fusion Azure Gözcü içinde etkinleştirmeyi öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 82becf50-6628-47e4-b3d7-18d7d72d505f
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: bf9a78006d13614a73a3fccfc57f28ce850456d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204482"
---
# <a name="enable-fusion"></a>Fusion etkinleştirme

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Gözcü Azure Machine Learning'de en baştan doğru yerleşik olarak sunulmaktadır. Sistem bağlarla ile güvenlik analistleri, güvenlik veri Bilim insanlarının ve mühendislerin verimli hale amaçlayan ML yeniliklerini tasarladık. Özellikle uyarı yorulma azaltmak için Azure Gözcü Fusion yerleşik bir yenilik olan.

Fusion, Azure AD kimlik koruması gibi farklı ürün daha düşük doğruluk anormal etkinlikler milyonlarca Microsoft Cloud App yönetilebilir bir dizi birleştirilecek Security, arasındaki ilişkilendirmeyi desteklenen graf makine öğrenimi algoritması kullanır. ilgi çekici güvenlik çalışmaları.

## <a name="enable-fusion"></a>Fusion etkinleştirme

1. Azure portalında Cloud Shell'i açmak için simgeyi seçin.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)

2.  İçinde **Hoş Geldiniz Cloud Shell** açar, aşağıdaki windows PowerShell seçin.

3.  Dağıtılan Azure Gözcü, aboneliği seçin ve **depolama oluşturma**.

4. Kimlik doğrulaması yaptıktan sonra Azure sürücüsü oluşturulmuştur, komut isteminde aşağıdaki komutları çalıştırın:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Fusion devre dışı bırak

Yukarıdaki yordamı izleyin ve aşağıdaki komutu çalıştırın:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>Fusion durumunu görüntüleme

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Gözcü içinde Fusion etkinleştirme öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

