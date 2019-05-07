---
title: Azure Önizleme Gözcü Barracuda verilere | Microsoft Docs
description: Gözcü Azure için Barracuda veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 6b0285903537dafb004b5aca033b50560247c605
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204451"
---
# <a name="connect-your-barracuda-appliance"></a>Barracuda gerecinize bağlanma 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Barracuda Web uygulaması Güvenlik Duvarı (WAF) Bağlayıcısı, Azure panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek için Gözcü ile Barracuda günlüklerinizi kolayca bağlanmanızı sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir. Azure Sentinel arasındaki yerel tümleştirme yararlanır **Barracuda** ve Microsoft Azure OMS'nin sorunsuz bir tümleştirme sağlar. 


> [!NOTE]
> Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="configure-and-connect-barracuda-waf"></a>Barracuda WAF bağlayın ve yapılandırın
Barracuda Web uygulaması güvenlik duvarı, tümleştirme ve doğrudan Azure Gözcü aracılığıyla Azure OMS sunucusu için günlükleri dışarı aktarabilirsiniz.
1. Git [Barracuda WAF yapılandırması akış](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/), bu parametreleri kullanarak bağlantı kurmak için yönergeleri izleyin:
    - **Çalışma alanı kimliği**: Azure Gözcü Barracuda Bağlayıcısı sayfasından, çalışma alanı kimliği değerini kopyalayın.
    - **Birincil anahtar**: Azure Gözcü Barracuda Bağlayıcısı sayfasından, birincil anahtar değerini kopyalayın.
2. Gözcü Azure portalında, dağıtılan Azure Gözcü çalışma alanına gidin ve üç noktasını (...) seçin ve satır sonunda **Gelişmiş ayarlar**. 
1. Seçin **veri** ardından **Syslog**.
1. Barracuda ayarladığınız tesis var ve önem derecesini ayarlayın ve tıklayın emin **Kaydet**.
6. İlgili şema Barracuda olayları Log Analytics'te kullanmak için arama **CommonSecurityLog**.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Barracuda cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

