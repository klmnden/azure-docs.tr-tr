---
title: Azure Lab Services Hakkında | Microsoft Docs
description: Lab Services’in geliştiriciler, test uzmanları, eğitimciler, öğrenciler ve diğerleri tarafından kullanılabilen sanal makinelerle laboratuvar oluşturma, yönetme ve güvenli hale getirmeyi nasıl kolaylaştırabildiğini öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e9c3cae7c7129cc489ddd38b5b2de18dd6f52e58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660598"
---
# <a name="introduction-to-classroom-labs"></a>Sınıf laboratuvarlarına giriş
Azure Lab Services, bulutta hızla bir sınıf laboratuvarı ortamı oluşturmanıza imkan tanır. Eğitimci sınıf laboratuvarını oluşturur, Windows veya Linux sanal makinelerini sağlar, sınıfta gerekli yazılım ve araç laboratuvarlarını yükler ve öğrenciler için kullanılabilir hale getirir. Sınıftaki öğrenciler, laboratuvardaki sanal makinelere (VM) bağlanır ve bunları projeleri, ödevleri, sınıf egzersizleri için kullanır. 

Sınıf laboratuvarları yönetilen laboratuvarlardır ve bunlar Azure tarafından yönetilir. Hizmet, yönetilen bir laboratuvar için sanal makineleri (VM) tasarlamaktan hataları işlemeye ve altyapıyı ölçeklendirmeye varan tüm altyapı yönetimi konularını ele alır. Ne tür bir altyapıya ihtiyacınız olduğunu siz belirlersiniz ve sınıf için gereken tüm araçları ve yazılımları yüklersiniz. Yönetilen laboratuvarlar şu an için önizleme aşamasındadır.  

## <a name="scenarios"></a>Senaryolar
İşte Azure Lab Services'in sınıf laboratuvarlarını destekleyen ana senaryo: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Bulutta sınıfınız için yeniden boyutlandırılabilen bir bilgisayar laboratuvarı oluşturma  

- Yönetilen bir sınıf laboratuvarı oluşturun. Hizmete tam olarak ihtiyacınız olanları söylediğinizde laboratuvarın altyapısını sizin için oluşturup yönetir, böylece siz de laboratuvarın ayrıntılarına değil, ders vermeye odaklanabilirsiniz. 
- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, sınıf çalışmaları için VM’leri kullanabilecekleri sınırlı sayıda saat verin.  
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin. 
- İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 

## <a name="user-profiles"></a>Kullanıcı profilleri
Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır. 

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi
Genellikle, bir kuruluşun bulut kaynaklarının, aynı zamanda Azure aboneliğinin sahibi olan BT yöneticisi, laboratuvar hesap sahibi olarak hareket eder ve aşağıdaki görevleri yerine getirir:   

- Kuruluşunuz için bir laboratuvar hesabı ayarlar.
- Tüm laboratuvarlardaki ilkeleri yönetir ve yapılandırır.
- Kuruluştaki kişilere laboratuvar hesabı altında laboratuvar oluşturma izinleri verir.

### <a name="educator"></a>Eğitimci
Normalde, öğretmen veya çevrimiçi eğitimci gibi kullanıcılar sınıf laboratuvarlarını bir laboratuvar hesabı altında oluşturur. Eğitimci aşağıdaki görevleri yerine getirir: 

- Sınıf laboratuvarı oluşturur.
- Laboratuvarda sanal makineler oluşturur. 
- Sanal makinelere uygun yazılımları yükler.
- Laboratuvara kimlerin erişebileceğini belirtir.
- Öğrencilere laboratuvarın kayıt bağlantısını sağlar.

### <a name="student"></a>Öğrenci
Öğrenci aşağıdaki görevleri yerine getirir:

- Laboratuvar kullanıcısının laboratuvara kaydolmak için laboratuvar oluşturucusundan aldığı kayıt bağlantısını kullanır. 
- Laboratuvardaki bir sanal makineye bağlanır ve sınıf çalışmalarını, ödevleri ve projeleri yapmak için onu kullanır. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak sınıf laboratuvarı oluşturmak için gereken laboratuvar hesabını ayarlamaya başlayın:

- [Bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)
