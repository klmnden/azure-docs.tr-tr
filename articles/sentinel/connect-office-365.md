---
title: Gözcü Azure önizlemesinde Office 365 veri toplama | Microsoft Docs
description: Gözcü Azure, Office 365 verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/26/2019
ms.author: rkarlin
ms.openlocfilehash: ad501958a5f88c821e48a3e21f69a960160b3c8e
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574867"
---
# <a name="collect-data-from-office-365-logs"></a>Office 365 günlüklerinden verileri toplama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Denetim günlükleri akışını [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) Azure Gözcü tek bir tıklamayla içine. Azure Gözcü içinde tek bir çalışma alanı için denetim günlüklerini birden çok kiracıdan gelen akışını yapabilirsiniz. Office 365 etkinlik günlüğü Bağlayıcısı, devam eden kullanıcı etkinlikleri hakkında Öngörüler sağlar. Office 365'ten çeşitli kullanıcı, yönetici, sistem ve ilke Eylemler ve olaylar hakkında bilgi alırsınız. Office 365 günlüklerini Azure Gözcü bağlanarak, panoları görüntülemesine, özel uyarıları oluşturma ve araştırma süreci iyileştirmek için bu verileri kullanabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

- Kiracınızda genel yönetici veya güvenlik yöneticisi olmanız gerekir
- Bilgisayarınızda, bağlantı oluşturmak için Azure Gözcü oturum web trafiğinin yapma suretha bağlantı noktası 4433 açıktır.

## <a name="connect-to-office-365"></a>Office 365’e bağlanın

1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Office 365** Döşe.

2. Zaten, altında değil etkinleştirdiyseniz **bağlantı** kullanın **etkinleştirme** Office 365 çözümü etkinleştirmek için düğme. Zaten etkinleştirilmişse, bağlantı ekranın etkinleştirilmiş olarak tanımlanır.
1. Office 365 Azure Gözcü için birden çok kiracıdan gelen akış verileri olanak tanır. Bağlanmak istediğiniz her Kiracı için kiracısı altında ekleme **kiracılar bağlanmak için Azure Gözcü**. 
1. Bir Active Directory ekranı açılır. Her Kiracı için Azure Gözcü bağlamak ve Azure Gözcü onun günlüklerini okumak için gerekli izinleri sağlamanız için istediğiniz bir genel yönetici kullanıcı kimlik doğrulaması istenir. 
5. Stream Office 365 etkinlik günlükleri altında tıklayın **seçin** Azure Gözcü için akış gerçekleştirmek istediğiniz günlük türlerini seçmek için. Şu anda Azure Gözcü Exchange ve SharePoint'i desteklemektedir.

4. Tıklayın **Değişiklikleri Uygula**.

3. İlgili şema için Office 365 günlükleri Log Analytics'te kullanmak için arama **OfficeActivity**.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Office 365'i bağlama öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

