---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: d3d33c87dc1adf65a53b71cc4c833e7f4a191670
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66171279"
---
## <a name="what-are-service-bus-queues"></a>Service Bus kuyrukları nelerdir?
Service Bus kuyrukları **aracılı mesajlaşma** iletişim modelini destekler. Kuyruklar kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir kuyruk aracılığıyla iletileri değiş tokuş eder (aracı). İleti üreticisi (gönderen) iletiyi kuyruğa aktarır ve ardından işleme devam eder. Zaman uyumsuz olarak, ileti tüketicisi (alıcı) iletiyi kuyruktan alır ve bunu işler. Üreticinin işleme devam etmesi ve daha fazla ileti göndermesi için tüketiciden yanıt beklemesi gerekmez. Kuyruklar, bir veya birden çok rakip tüketiciye **İlk Giren İlk Çıkar (FIFO)** yöntemine göre ileti teslimi sunar. Bu da, genellikle iletilerin kuyruğa eklendiği bir düzende alıcılar tarafından alınıp işleneceği ve her iletinin tek bir ileti tüketicisi tarafından alınıp işleneceği anlamına gelir.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Service Bus kuyrukları çok sayıda çeşitli senaryolar için kullanılabilen genel amaçlı bir teknolojidir:

* Çok katmanlı bir Azure uygulamasında web ve çalışan rolleri arasındaki iletişim.
* Karma bir çözümde şirket içi uygulamalar ve Azure barındırmalı uygulamalar arasındaki iletişim.
* Farklı kuruluşlarda veya bir kuruluşun farklı departmanlarında şirket içi çalışan dağıtılmış bir uygulamanın bileşenleri arasındaki iletişim.

Kuyrukların kullanılması uygulamalarınızı daha kolay ölçeklendirmenizi ve mimarinizi daha dayanıklı hale getirmenizi sağlar.


