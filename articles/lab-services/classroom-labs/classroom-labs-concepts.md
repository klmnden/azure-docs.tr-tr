---
title: Classroom Labs kavramları - Azure Lab Services | Microsoft Docs
description: Lab Services ve nasıl, oluşturma ve Laboratuvarları yönetme kolaylaştırmak temel kavramlarını öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: spelluru
ms.openlocfilehash: 8bbb486b0dbf1a5e25f5ee4d1f8e5e01b999a8ba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067387"
---
# <a name="classroom-labs-concepts"></a>Sınıf Laboratuvarları kavramları
Aşağıdaki liste, temel Lab Services kavramları ve tanımları içerir:

## <a name="quota"></a>Kota
Kota olduğu zaman, bir Öğretmen ayarlayabilirsiniz sınır (saat) için bir öğrenci bir laboratuvar sanal makinesi kullanın. 0 veya belirli bir saat sayısı olarak ayarlanabilir. Kotası 0 olarak ayarlarsanız, bir öğrenci bir zamanlama çalıştırırken veya bir Öğretmen Öğrenci için sanal makinede el ile açtığında yalnızca sanal makine kullanabilirsiniz.
 
## <a name="schedules"></a>Zamanlamalar
Sınıf için bir Öğretmen oluşturabilmeniz için zaman dilimini (bir kez veya yinelenen) zamanlamaları var. Zamanlamanın başında Laboratuvardaki tüm sanal makineleri otomatik olarak başlatılır ve zamanlama sonunda durdurulsa. Bir zamanlama çalışırken kota saat kullanılmaz.

## <a name="template-virtual-machine"></a>Sanal makine şablonu
Bir laboratuvar şablonu bir sanal makinede, tüm kullanıcıların sanal makineleri oluşturulduğu temel sanal makine görüntüsüdür. Eğitmenler/Laboratuvar creators şablon sanal makine kurun ve Laboratuvarları yapmak için eğitim katılımcılara sağlamak istedikleri yazılım ile yapılandırın. Bir VM şablonu yayımladığınızda, Azure Lab Services oluşturur veya VM şablonu temel alan Laboratuvar VM'ler güncelleştirir. 


## <a name="user-profiles"></a>Kullanıcı profilleri
Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır. 

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi
Genellikle, bir kuruluşun bulut kaynaklarının, aynı zamanda Azure aboneliğinin sahibi olan BT yöneticisi, laboratuvar hesap sahibi olarak hareket eder ve aşağıdaki görevleri yerine getirir:   

- Kuruluşunuz için bir laboratuvar hesabı ayarlar.
- Tüm laboratuvarlardaki ilkeleri yönetir ve yapılandırır.
- Kuruluştaki kişilere laboratuvar hesabı altında laboratuvar oluşturma izinleri verir.

### <a name="professor"></a>Profesör
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
