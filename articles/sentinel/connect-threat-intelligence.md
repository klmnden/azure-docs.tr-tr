---
title: Tehdit bilgisi verilerini Azure Önizleme Gözcü bağlayın | Microsoft Docs
description: Azure Gözcü için tehdit bilgisi verilerini bağlanma hakkında bilgi edinin.
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 56412543-5664-44c1-b026-2dbaf78a9a50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: a33b00e4e553824f1f719be2c8518487241f0a11
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59494842"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Tehdit bilgisi sağlayıcılarından veri bağlama 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü veri akışı sonra bu akış tehdit zekası ile kuruluşunuzda kullandığınız zenginleştirebilirsiniz. 

Platformlar arası, uyarılar ve kuralları doğru tehdit zekası, belirli bir IP adresinden bir uyarı alırsanız, örneğin danışın sağlamak için tehdit zekası sağlayıcısı tümleştirmenizi kötü amaçlı olarak, bu IP adresi kısa bir süre önce bulunup bulunmadığını size bildirmek mümkün olacaktır , Azure Sentinel ile tümleştirme sağlayan [tehdit zekası sağlayıcıları](https://aka.ms/graphsecuritytips). 

Tek bir tıklamayla Azure Gözcü tehdit zekası sağlayıcıları günlüklerinden düzenleyebilir. Bu bağlantı, gözlemlenenler IP adresi, etki alanı, URL gibi çeşitli türlerde içeren göstergeleri eklemenizi sağlar ve dosya karması arama ve özel oluşturmak için Azure Gözcü kurallarında uyarır.  
> [!NOTE]
> Size özelleştirilmiş tehlike göstergeleri Azure Gözcü kullanılmak üzere Uyarı kuralları, panolar ve avcılık senaryoları ile tümleştirerek girebilirsiniz [Microsoft Graph güvenlik tiIndicator](https://aka.ms/graphsecuritytiindicators) varlık kullanarak veya bir [Microsoft Tümleşik tehdit zekası platformu grafik güvenlik](https://aka.ms/graphsecuritytips).

## <a name="prerequisites"></a>Önkoşullar  

- Genel yönetici veya güvenlik yöneticisi izinleri ile kullanıcı 

- Tehdit zekası uygulama Microsoft Intelligent Security Graph ile tümleşik 

## <a name="connect-to-threat-intelligence"></a>Tehdit bilgilerini kullanarak bağlan 

1. Tehdit zekası sağlayıcısı kullanıyorsanız, ipucu uygulamanıza gidin ve izni göstergeleri, Microsoft'a ve hizmet Azure Gözcü belirtmek için emin olun.  

2. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **tehdit zekası** Döşe.

3. **Bağlan**'a tıklayın. 

4. Şemanın ilgili tehdit bilgileri akışlarından için Log Analytics'te kullanmak için arama **ThreatIntelligenceIndicator**. 

 
## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Gözcü için tehdit bilgileri sağlayıcınıza bağlanmak öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın.

- Gözcü Azure ile çalışmaya başlamak için Microsoft Azure aboneliği gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Bilgi nasıl [ekleme verilerinizi Azure Gözcü](quickstart-onboard.md), ve [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
