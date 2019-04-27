---
title: Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin | Microsoft Docs
description: Bu makale kaynaklarınızı Azure Güvenlik Merkezi'nde izleme ile güvenlik duruşunuzu güçlendirin yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 28b4667a9ceb4b3534d85ba28668f06c750e22c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60703870"
---
# <a name="strengthen-your-security-posture-with-azure-security-center"></a>Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin
Bu makalede güvenlik duruşunuzu güçlendirin yardımcı olur. İzleme özellikleri, Azure Güvenlik Merkezi'nde kaynak güvenlik ilkeleriyle uyumluluğunu mümkün ve izleme gibi sıkı olduğundan emin olmak için kullanın.

## <a name="how-do-you-strengthen-your-security-posture"></a>Nasıl güvenlik duruşunuzu güçlendirin?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenlik duruşunuzu güçlendirme, kuruluş standartlarıyla veya en iyi karşılayan olmayan sistemleri tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olma anlamına gelir.

Bir aboneliğin kaynakları için [güvenlik ilkelerini](tutorial-security-policy.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Aracının yüklü olduğu VM'ler ve bilgisayarların sayısına bağlı olarak, güvenlik güncelleştirmesi durumu ve işletim sistemi yapılandırması gibi bilgisayar yapılandırma ayarları ve VM’ler hakkında bilgi toplamak bir saat veya daha fazla sürebilir. Sorunlar ve ağınızı sağlamlaştırmak ve risk düzeltmek için yollar tam bir listesini görüntüleyebileceğiniz **önerileri** Döşe.

Güvenlik durumunu, kaynaklarınızı ve kaynak türü başına herhangi bir sorun görüntüleyebilirsiniz:

- Bilgisayar kaynaklarınızın ve uygulamalarınızın durumunu izleyin ve bu aboneliklerin güvenliğini artırmak için önerileri almak için bkz: [, makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- Sanal makineler, ağ güvenlik grupları ve uç noktaları gibi ağ kaynaklarınızı izleyin ve bunların güvenliği iyileştirmeye yönelik öneriler almak için bkz: [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md) daha fazla bilgi için bilgiler. 
- SQL sunucularını ve depolama hesapları gibi veri ve depolama kaynaklarınızı izleyin ve bunların güvenliği iyileştirmeye yönelik öneriler almak için bkz: [koruyan Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri](security-center-sql-service-recommendations.md) daha fazla bilgi için . 
- MFA ve hesap izinleri de dahil olmak üzere, kimlik ve erişim kaynaklarınızı izleyin ve bunların güvenliği iyileştirmeye yönelik öneriler almak için bkz: [kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md) daha fazla bilgi için. 
- Tam zamanlı erişim kaynaklarınıza izlemek için bkz: [tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](security-center-just-in-time.md) daha fazla bilgi için. 


Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.



![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırmayı öğrenin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
