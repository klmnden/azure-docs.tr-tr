---
title: Azure Önizleme Gözcü Symantec ICDX verilere | Microsoft Docs
description: Azure Gözcü için Symantec ICDX veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 1a6843fb1668307aa442011233999c648901d404
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502091"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Symantec ICDX gerecinize bağlanma 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Symantec ICDX Bağlayıcısı, Azure panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek için Gözcü ile tüm, Symantec güvenlik çözümü günlüklerini kolayca bağlanmanıza olanak sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir. REST API'si kullanın Symantec ICDX ve Azure Gözcü arasında tümleştirme sağlar.


> [!NOTE]
> Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="configure-and-connect-symantec-icdx"></a>Yapılandırma ve Symantec ICDX bağlanın 

Symantec ICDX, tümleştirme ve doğrudan Azure Gözcü için günlükleri dışarı aktarabilirsiniz.

1. ICDX Yönetim Konsolu'nu açın.
2. Sol gezinti menüsünde seçin **yapılandırma** ardından **ileticileri** sekmesi.
3. Microsoft Azure Log Analytics satırının **daha fazla**çizgidir **Düzenle**. 
4. İçinde **Microsoft Azure Log Analytics ileticisi** penceresinde aşağıdakileri ayarlayın:
    - Özel günlük adı SymantecICDX varsayılan olarak bırakın.
    - Çalışma alanı kimliği kopyalayın ve yapıştırın **Müşteri Kimliği** alan. Kopyalama **birincil anahtar** ve paylaşılan anahtar alanına yapıştırın. Bu değerleri seçerek Gözcü Azure portalından kopyalayabilirsiniz **veri bağlayıcıları** ardından **Symantec ICDX**.
6. İlgili şema için Symantec ICDX olayları Log Analytics'te kullanmak için arama **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Symantec ICDX bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

