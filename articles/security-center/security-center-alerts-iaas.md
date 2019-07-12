---
title: Tehdit algılama VM'ler için ve Azure Güvenlik Merkezi'nde sunucular | Microsoft Docs
description: Bu konuda, VM ve sunucu uyarılarını kullanılabilir Azure Güvenlik Merkezi'nde sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 5487b4f49f5dbf7b968cd45d40555c69b54c329a
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571588"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Vm'lerinizi ve Azure Güvenlik Merkezi'nde sunucuları için tehdit algılama

Bu konuda, aşağıdaki işletim sistemi ile farklı türde algılama yöntemleri ve uyarılar kullanılabilir sanal makineleri ve sunucular için sunulmaktadır. Desteklenen sürümlerin listesi için bkz. [platformları ve Azure Güvenlik Merkezi tarafından desteklenen özellikler](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Güvenlik Merkezi, izlemek ve Windows tabanlı makinelerinizi korumak için Azure Hizmetleri ile tümleştirilir.  Güvenlik Merkezi uyarıları ve düzeltme önerileri tüm hizmetlerin kullanımı kolay bir biçimde sunar.

### Microsoft Server Defender ATP <a nanme="windows-atp"></a>

Azure Güvenlik Merkezi, Windows Defender Gelişmiş tehdit Koruması (ATP ile) tümleştirerek, bulut iş yükü koruması platformları genişletir. Bu kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar.

> [!NOTE]
> Windows Server Defender ATP algılayıcısını eklenen Azure Güvenlik Merkezi'ne Windows sunucuları otomatik olarak etkinleştirilir.

Windows Server Defender ATP bir tehdit algıladığında bir uyarı tetikler. Uyarı, Güvenlik Merkezi panosunda gösterilir. Panodan, saldırı kapsamını ortaya çıkarmak için ayrıntılı bir araştırma gerçekleştirmek için Windows Defender ATP Konsolu Özet. Windows Server Defender ATP hakkında daha fazla bilgi için bkz: [ekleme sunucuları Windows Defender ATP hizmetine](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Kilitlenme dökümü analizi <a nanme="windows-dump"></a>

Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar.

Kötü amaçlı yazılım tarafından bir kilitlenme kaynaklanmış olabilir veya kötü amaçlı yazılım içeremez. Kötü amaçlı yazılım çeşitli tür güvenlik ürünleri tarafından algılanma önlemek için disk veya yazılmış şifreleme yazılım bileşenlerini yazılmasını önler bir fileless saldırı kullanmanıza diske. Bu tür bir saldırı geleneksel disk tabanlı yaklaşımlarıyla algılanmasını zor olabilir.

Ancak, bu tür bir saldırıya bellek analizi kullanılarak algılanabilir. Güvenlik Merkezi, kilitlenme bellek analiz ederek, yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve bir makineye gizlice kalıcı saldırı kullanarak teknikleri algılayabilir. Bu, ana bilgisayarlar için en az etki ile Güvenlik Merkezi arka ucu tarafından gerçekleştirilir.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Kod ekleme bulundu**|Kod ekleme, çalışmakta olan işlemlere veya iş parçacıklarına yürütülebilir modüllerin eklenmesidir. Bu teknik verilerine erişmek için kötü amaçlı yazılım tarafından başarıyla engellemek için kendisine gizleme bulundu ve çıkarılmakta olan sırasında kullanılır. <br/>Bu uyarı, eklenen bir modülün kilitlenme bilgi dökümünde mevcut olduğunu belirtir. Güvenlik Merkezi kötü amaçlı olan ve olmayan eklenen modülleri birbirinden ayırt etmek için eklenen modülün bir şüpheli davranış profiline uygun olup olmadığını denetler.|
|**Şüpheli kod kesimi bulunan**|Bir kod kesiminin yansıtmalı ekleme ve boş işlem gibi standart olmayan yöntemler kullanılarak ayrıldığını belirtir. Uyarı özellikleri için bağlam sağlamak için işlenen kod kesiminin ek özelliklerini ve bildirilen kod kesiminin davranışlar sağlar.|
|**Kabuk kodu bulundu**|Kabuk Kodu, kötü amaçlı yazılım bir yazılım güvenlik açığından yararlandıktan sonra çalıştırılan yüktür.<br/>Bu uyarı, kilitlenme dökümü analizinin kötü amaçlı yükler tarafından yaygın olarak gerçekleştirilen davranışları sergileyen yürütülebilir kod algıladığını belirtir. Kötü amaçlı olmayan yazılımlar da bu davranış olabilir, ancak normal yazılım geliştirme uygulamaları için alışıldık değil.|

### Fileless saldırı algılama <a nanme="windows-fileless"></a>

Azure'da, düzenli olarak müşterilerimizin uç noktaları hedefleyen fileless saldırıları görüyoruz.

Algılama önlemek için kötü amaçlı yükler fileless saldırıları belleğe yerleştirir. Saldırgan güvenliği aşılmış işlemlerinin bellek içinde kalıcı hale getirmek ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirmek.

Fileless saldırı algılama ile otomatik bellek adli teknikleri fileless saldırı araç Setleri, teknikler ve davranışları tanımlar. Bu çözüm, düzenli aralıklarla çalışma zamanında, makinenizi tarayan ve doğrudan güvenlik açısından kritik işlemler bellekten ınsights ayıklar.

Bu, yararlanılması, kod ekleme ve kötü amaçlı yükler yürütülmesini kanıt bulur. Fileless saldırı algılama uyarısı önceliklendirme, bağıntı ve aşağı akış yanıt süresini hızlandırmak için ayrıntılı güvenlik uyarıları oluşturur. Bu yaklaşım, olay tabanlı EDR çözümleri daha büyük olan algılama kapsamının sağlama tamamlar.

> [!NOTE]
> İndirme ile Windows uyarıları benzetimini yapabilirsiniz [Azure Güvenlik Merkezi Playbook](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef): Güvenlik uyarıları ve sağlanan yönergeleri izleyin

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Algılanan fileless saldırı yöntemi**|Aşağıda belirtilen işlem belleğini fileless saldırı Araç Seti içerir: Meterpreter. Fileless saldırı araç Setleri genellikle bir varlık tarafından geleneksel virüsten koruma algılama zorlaşır dosya sisteminde yok.|

### <a name="further-reading"></a>Daha fazla bilgi

Örnekler ve Güvenlik Merkezi algılama hakkında daha fazla bilgi için:

* [Azure Güvenlik Merkezi algılama siber saldırı nasıl otomatikleştirir](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Azure Güvenlik Merkezi güvenlik açıklarını Yönetimsel Araçlar'ı kullanarak nasıl algılar](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

## Linux <a name="linux-machines"></a>

Güvenlik Merkezi toplanan denetim kayıtları kullanarak Linux makineleri **auditd**, bir çerçeveleri en yaygın Linux denetimi. auditd uzun ve ana hat çekirdek yaşayan süredir avantajına sahiptir. 

### Linux auditd uyarıları ve Microsoft Monitoring Agent (MMA) Tümleştirmesi <a name="linux-auditd"></a>

Sistem çağrıları izleme, bunları belirli bir kural kümesi tarafından filtre ve ileti kendileri için yuvaya yazılıyor sorumlu bir çekirdek düzeyindeki alt auditd sistem oluşur. Güvenlik Merkezi işlevlerini auditd paketinden içindeki Microsoft Monitoring Agent (MMA) tümleştirir. Bu tümleştirme, herhangi bir önkoşulu olmadan tüm desteklenen Linux dağıtımları auditd olayları koleksiyonda sağlar.  

auditd kayıtları zenginleştirilmiş, toplanan ve Linux MMA aracısını kullanarak olayları toplanır. Güvenlik Merkezi, yeni analizi, Linux bulut üzerinde kötü amaçlı davranışları algılamak bildirir ve şirket içi Linux makineleri bu Dengeleme ekleme sürekli çalışmaktadır. Benzer şekilde Windows özellikleri, bu analizleri şüpheli işlemlerin, güvenilmez oturum açma denemesi, çekirdek Modül yükleme ve bir makine ya da saldırı altında veya bir ihlal belirtmek diğer etkinlikler arasında yayılır.  

Biz farklı aşamalarında saldırı yaşam döngüsü nasıl span göstermek analytics, çeşitli örnekleri aşağıdadır.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**SSH yetkili anahtarlar dosyasına olağan dışı bir şekilde erişme görülen işlem**|SSH yetkili anahtarlar dosyası, bilinen kötü amaçlı yazılım kampanyalarına için benzer bir yöntem içinde erişilebilir. Bu erişim bir saldırganın bir makine kalıcı erişim kazanmak çalışıyor olduğunu gösteriyor olabilir|
|**Algılanan Kalıcılık denemesi**|Konak veri analizi, tek kullanıcı moduna için başlatma betiği yüklendiğini algıladı. <br/>Herhangi bir yasal işlem bu modda çalıştırmak için gereken seyrek olduğundan, bu saldırgan her çalışma kalıcılığı sağlamak için düzeyi için kötü amaçlı bir işleme eklemiştir gösterebilir.|
|**Algılanan zamanlanmış görev düzenleme**|Konak veri analizi, zamanlanmış görevlerin olası işleme algıladı. Saldırganlar, Kalıcılık sağlamak için gizliliğinin tehlikeye makinelerine genellikle zamanlanmış görevler ekleyin.|
|**Şüpheli dosya zaman damgası değişikliği**|Konak veri analizi şüpheli zaman damgası değişiklik algılandı. Saldırganlar zaman damgaları genellikle bu yeni bırakılan dosyalar algılanmasını önlemek için yeni araçlar mevcut yasal dosyaların kopyalanacağı|
|**Sudoers grubuna yeni bir kullanıcı eklendi**|Konak veri analizi, üyelerinin yüksek ayrıcalıklarla komutlar sağlayan sudoers grubuna bir kullanıcı eklendiğini algıladı.|
|**Dhcp istemci DynoRoot güvenlik olası yararlanma**|Konak veri analizi dhclient betiğinin üst işlemin olağan dışı bir komut yürütülmesiyle algılandı.|
|**Şüpheli bir çekirdek modülü algılandı**|Konak veri analizi bir çekirdek modülü yüklenen bir paylaşılan nesne dosyası algılandı. Bir gösterge makinelerinizi birinin tehlikeye girdiği veya bu yasal etkinlik olabilir.|
|**Algılanan dijital para birimi araştırma ile ilişkili işlem**|Konak veri analizi, normalde dijital para birimi araştırma ile ilişkili bir işlemin yürütülmesi algılandı|
|**Dış IP adresi için olası bir bağlantı noktası iletme**|Konak veri analizi başlatma bağlantı noktası iletme için dış IP adresi algılandı.|

> [!NOTE]
> İndirerek Windows uyarıları benzetimini yapabilirsiniz [Azure Güvenlik Merkezi Playbook: Güvenlik Uyarıları](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) ve sağlanan yönergeleri izleyin.


Daha fazla bilgi için şu makalelere bakın:  

* [Azure Güvenlik Merkezi saldırı Linux riskli makineleri algılayabilir için yararlanın](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)

* [Azure Güvenlik Merkezi, Linux'ta ortaya çıkan güvenlik açıklarını algılama](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

 