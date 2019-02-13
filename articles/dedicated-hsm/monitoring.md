---
title: İzleme seçenekleri - Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM izleme seçenekleri ve sorumlulukları izlemeye genel bakış
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 76a50c483b9246e3e2f9df97d5287f3e2fbd9897
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104627"
---
# <a name="azure-dedicated-hsm-monitoring"></a>Ayrılmış HSM Azure izleme

Azure ayrılmış HSM hizmetini tek müşteri kullanmak için fiziksel bir cihaz ile tam yönetimsel denetim ve yönetim sorumluluk sağlar. Kullanılabilir cihaz bir [Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).  Microsoft bir kez fiziksel seri bağlantı noktası ek izleme rolü olarak ötesinde bir müşteri tarafından sağlanan yönetim erişimi olacaktır. Sonuç olarak, müşteriler kapsamlı izleme de dahil olmak üzere normal işlem etkinlikleri için sizin sorumluluğunuzdadır ve analiz oturum.
Müşteriler, HSM'ler kullanın ve Gemalto ile destek veya danışmanlık Yardım için iş uygulamaları için tam olarak sorumludur. İşletimsel sağlığı müşteri sahipliğini kapsamını nedeniyle, Microsoft bu hizmeti için yüksek kullanılabilirlik garantisi herhangi bir türden sunmak için mümkün değildir. Bu, müşteri sorumluluk uygulamalarını sağlamak için yüksek kullanılabilirlik elde etmek için yapılandırılmış doğru olur. Microsoft, izleyin ve cihaz sistem durumu ve ağ bağlantısı sağlamak.

## <a name="microsoft-monitoring"></a>Microsoft izleme

Gemalto SafeNet aygıtı kullanımda cihaz izleme seçenekleri varsayılan SNMP ve seri bağlantı noktası tarafından sahiptir. Microsoft, fiziksel bir cihaz sistem durumu hakkında temel telemetri almak için cihaza bağlanmak oluşturucusunu seri bağlantı kullandı. Bu, güç kaynakları ve Fanlar gibi sıcaklık ve bileşen durumunu gibi öğeleri içerir.
Bunu başarmak için Microsoft Gemalto cihaza ayarlanan Yönetici olmayan bir "İzle" rolünü kullanır. Bu rol, telemetri alma olanağı sağlar, ancak herhangi bir erişim, cihaz yönetim görevini açısından veya şifreleme bilgilerini görüntüleyerek herhangi bir şekilde vermez. Müşterilerimiz, cihazlarının gerçekten hassas yönetmek, yönetmek ve kullanmak için kendi şifreleme anahtarı depolama alanı olduğunu olabilirsiniz. Herhangi bir müşteri temel sistem durumu izleme bu en az düzeyde erişim memnun değil durumunda, izleme hesabı devre dışı bırakma seçeneği vardır. Bu belirgin adımlamayla Microsoft hiçbir bilgi olacaktır ve bu nedenle hiçbir sağlayabilme proaktif herhangi bir bildirim cihaz sistem durumu sorunları var. Bu durumda, müşteri cihaz sistem durumu için sorumludur.
İzleyici işlevi cihaz sistem durumu verilerini almak için her 10 dakikada yoklamak için ayarlanır. Hata yapmaya açık yapısı seri iletişim nedeniyle, birden çok negatif sistem durumu göstergeleri üzerinden bir saat sonra yalnızca bir uyarı oluşturulması. Bu uyarı, sorunu bildiren bir proaktif müşteri iletişimi sonuçta yol açar.
Sorunun doğasına bağlı olarak uygun kursu eyleminin etkisini azaltmak ve düşük riskli düzeltme sağlamak için alınabileceğinden. Örneğin, hiçbir sonuç hot takas yordamla değiştirmesine düşük etki ve işlem için en az risk ile gerçekleştirilmesi için olay güç kaynağı hatalı olur. Diğer yordamlar zeroized ve müşteri için herhangi bir güvenlik riski en aza indirmek için sağlaması için bir cihaz gerektirebilir. Bu durumda, bir müşteri alternatif bir cihazı sağlama, böylece cihaz eşitleme tetikleme eşleştirme bir yüksek kullanılabilirlik alanına katın. Normal işlem en az ve en düşük güvenlik riskini mümkün olduğunca az zamanda sürdürün.  

## <a name="customer-monitoring"></a>Müşteri izleme

Ayrılmış HSM hizmeti bir değer önerisi, müşteri cihaz, cihaz sunulan bir bulut olan özellikle dikkate alır denetimidir. Bu denetimin bir sonuç cihaz durumunu yönetmek ve izlemek için sorumluluğundadır. Gemalto SafeNet aygıt SNMP ve Syslog uygulaması için yönergeler bulunur. Ayrılmış HSM hizmetini müşterileri, Microsoft izleme hesabı'nın etkin kalır ve bunlar Microsoft izleme hesabı devre dışı bırakırsanız zorunlu düşünmelisiniz bile bunu kullanmak için önerilir.
Kullanılabilen iki tekniği sorunlarını tanımlamak ve uygun düzeltme çalışma başlatmak için Microsoft desteği çağrısı bir müşteri çalıştırmasına olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmete tüm temel kavramlarını Örneğin, iyi bir cihaz sağlama ve uygulama tasarım veya dağıtımınızın önce anlaşıldığından önerilir. Daha fazla kavramı düzey konular:

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
