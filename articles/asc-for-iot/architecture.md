---
title: IOT çözüm mimarisinin Önizleme için Azure Güvenlik Merkezi'ni anlama | Microsoft Docs
description: IOT hizmeti için Azure Güvenlik Merkezi'nde bilgi akışını hakkında bilgi edinin.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: a0eb459391da65f8d0e2ae251809805924d07ad1
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862374"
---
# <a name="azure-security-center-for-iot-architecture"></a>IOT mimarisi için Azure Güvenlik Merkezi

Bu makalede, IOT çözümü için Azure Güvenlik Merkezi (ASC), işlevsel sistem mimarisi açıklanmaktadır. 

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="asc-for-iot-components"></a>ASC IOT bileşenleri

IOT için ASC aşağıdaki bileşenlerden oluşur:
- Cihaz Aracısı
- Güvenlik iletisi SDK'sı gönderin
- IOT hub'ı tümleştirme
- Analytics işlem hattı
 
### <a name="asc-for-iot-workflow"></a>ASC IOT iş akışı

IOT cihaz aracılar için ASC kolayca cihazlarınızdan ham güvenlik olaylarını toplamanıza olanak sağlar. Ham güvenlik olaylarını IP bağlantılar, işlem oluşturma, kullanıcı oturum açma bilgileri ve güvenlikle ilgili diğer bilgileri içerebilir. ASC IOT cihaz aracılar için de yüksek ağ verimliliğini önlemeye yardımcı olmak için Olay toplama işleyin. Aracılar daha yüksek hizmet maliyetlerini önleme hızlı SLA yalnızca önemli bilgileri gönderme gibi belirli görevleri veya daha büyük parçalara kapsamlı güvenlik bilgileri ve bağlam toplayarak kullanmanıza olanak sağlayan yüksek düzeyde özelleştirilebilir.
 
Cihaz aracılar ve diğer uygulamaların kullanım **Azure ASC göndermek güvenlik ileti SDK** Azure IOT hub'ıyla güvenlik bilgileri göndermek. IOT hub'ı, bu bilgileri alır ve IOT hizmeti ASC iletir.

ASC IOT hizmeti etkinleştirildiğinde iletilen verilerin yanı sıra IOT Hub ayrıca tüm iç verilerini ASC tarafından analiz için IOT gönderir. Bu veriler, cihaz-bulut işlem günlükleri, cihaz kimliklerini ve Hub yapılandırmasını içerir. Tüm bu bilgileri yardımcı olur. ASC IOT analytics işlem hattı oluşturun.
 
IOT analytics işlem hattı için ASC ayrıca alır ek tehdit zekası akışları Microsoft ve Microsoft içindeki farklı kaynaklardan gelen iş ortaklarının. ASC IOT tüm analytics işlem hattı için hizmet (örneğin, özel uyarılar ve gönderilen güvenlik iletiyi SDK kullanımı) yapılan her müşteri yapılandırması ile çalışır.
 
Analytics işlem hattı kullanarak, IOT için ASC tüm akışları eyleme dönüştürülebilir öneriler ve Uyarılar oluşturmak için bilgileri birleştirir. İşlem hattı, makine öğrenimi modellerini standart cihaz davranışını ve risk analizi sapma arama yanı sıra güvenlik Araştırmacıları ve uzmanlar tarafından oluşturulan her iki özel kurallar içerir.
 
ASC IOT öneriler ve uyarılar (çıkış analytics işlem hattı) için her müşteri Log Analytics çalışma alanına yazılır. Ham etkinliklerin çalışma yanı sıra uyarıları ve öneriler dahil olmak üzere, yakından araştırmalar ve algılanan kuşkulu etkinlikleri hakkında tam Ayrıntılar kullanarak sorguları sağlar.  

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, temel mimarisini ve ASC iş akışı IOT çözümü hakkında bilgi edindiniz. Önkoşullar hakkında daha fazla bilgi edinmek için nasıl kullanmaya başlayın ve güvenlik çözümünüzü IOT hub'ında etkinleştirmek için aşağıdaki makalelere bakın:

- [Hizmet önkoşulları](service-prerequisites.md)
- [Başlarken](getting-started.md)
- [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
- [IOT hub'ında güvenliğini etkinleştir](quickstart-onboard-iot-hub.md)
- [ASC için IOT hakkında SSS](resources-frequently-asked-questions.md)
- [ASC IOT güvenlik uyarıları](concept-security-alerts.md)

