---
title: Azure Key Vault geçici silmeyi | Microsoft Docs
ms.service: key-vault
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/19/2019
ms.openlocfilehash: 330337620f1732b9ccecfb2c95a0b4495476f97b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64720515"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Azure Key Vault geçici silmeyi genel bakış

Key Vault'un geçici silme özelliği silinen kasa ve geçici silme bilinen kasası nesne kurtarılmasını sağlar. Özellikle aşağıdaki senaryolar adresi:

- Bir anahtar kasası kurtarılabilir silinmesi için destek
- Anahtar kasası nesne (ör kurtarılabilir silme desteği. anahtarları, parolaları, sertifikaları)

## <a name="supporting-interfaces"></a>Arabirimleri destekleme

Geçici silme özelliği aracılığıyla kullanılabilir başlangıçta [REST](/rest/api/keyvault/), [CLI](key-vault-soft-delete-cli.md), [PowerShell](key-vault-soft-delete-powershell.md) ve [.NET /C# ](/dotnet/api/microsoft.azure.keyvault?view=azure-dotnet) arabirimleri.

## <a name="scenarios"></a>Senaryolar

Azure Key Vault, Azure Resource Manager tarafından yönetilen, izlenen kaynaklardır. Azure Resource Manager, başarılı bir silme işlemi artık erişilememesinden Bu kaynakta sonuçlanmalıdır gerektiren silme işlemi için iyi tanımlanmış bir davranışı da belirtir. Geçici silme özelliği, yanlışlıkla veya kasıtlı silme işlemi olup olmadığını silinen nesnenin kurtarma yöneliktir.

1. Tipik bir senaryoda, bir kullanıcı yanlışlıkla bir anahtar kasası veya bir anahtar kasasını silmiş olabilirsiniz; vault veya key vault nesne anahtarı, önceden belirlenmiş bir süre için kurtarılabilir olacak şekilde, kullanıcı silme işlemini geri al ve kendi veri kurtarma.

2. Farklı bir senaryoda, dolandırıcı bir kullanıcı bir anahtar kasası veya iş kesintisi neden bir kasa içinde bir anahtar gibi bir anahtar kasası nesne silme girişiminde bulunabilir. Anahtar kasası ya da anahtar kasası nesne silme işlemi temel alınan verileri gerçek silinmeye karşı ayıran bir güvenlik önlemi olarak, örneğin, farklı, verileri silme izinlerini kısıtlayarak rolü güvenilir olarak kullanılabilir. Bu yaklaşım, aksi takdirde bir anında veri kaybına neden bir işlem için etkili bir şekilde çekirdek gerektirir.

### <a name="soft-delete-behavior"></a>Geçici silme davranışı

Bu özellik ile nesne silindikten görünümünü sağlarken, geçici etkili bir şekilde belirtilen saklama dönemi (90 gün) için kaynakları barındıran silme, bir anahtar kasası veya anahtar kasası nesne silme işlemi kullanılabilir. Daha fazla hizmet aslında silme işlemini geri alma silinen nesnesini kurtarmak için bir mekanizma sağlar. 

Geçici silme isteğe bağlı bir Key Vault davranışı ve **varsayılan olarak etkin değildir** bu sürümde. Bunu aracılığıyla açılabilir [CLI](key-vault-soft-delete-cli.md) veya [Powershell](key-vault-soft-delete-powershell.md).

### <a name="purge-protection"></a>Koruma Temizle 

Ne zaman temizleme korumadır, kasa veya 90 gün saklama süresi bitene kadar bir nesne silinmiş durumda temizlenemiyor. Bu kasalar ve nesneler hala, bekletme ilkesi izlenir müşteriler işlemlerini kurtarılabilir. 

Koruma temizleme isteğe bağlı bir Key Vault davranışı ve **varsayılan olarak etkin değildir**. Bunu aracılığıyla açılabilir [CLI](key-vault-soft-delete-cli.md#enabling-purge-protection) veya [Powershell](key-vault-soft-delete-powershell.md#enabling-purge-protection).

### <a name="permitted-purge"></a>İzin verilen temizleme

Kalıcı olarak siliniyor, temizleme, anahtar kasası proxy kaynak üzerinde bir POST işlemi aracılığıyla mümkündür ve özel ayrıcalıklar gerektirir. Genellikle, yalnızca abonelik sahibi, bir anahtar kasasını Temizle mümkün olacaktır. GÖNDERME işlemi, kasa anında ve kurtarılamaz silme işlemi tetikler. 

Özel durumlar şunlardır:
- Ne zaman Azure aboneliğini olarak işaretlendi *silinemez*. Bu durumda, yalnızca hizmet ardından gerçek silme işlemini gerçekleştirebilir ve zamanlanmış bir işlem olarak bunu yapar. 
- Zaman enable-temizleme-koruma bayrağı, kasa üzerinde etkindir. Bu durumda, özgün gizli nesne silme nesne kalıcı olarak silmek için işaretlendiğinde 90 gün içinde Key Vault bekler.

### <a name="key-vault-recovery"></a>Anahtar kasası kurtarma

Bir anahtar kasası siliniyor bağlı hizmet kurtarma için yeterli meta verilerin eklenmesi, abonelik kapsamında bir proxy kaynağı oluşturur. Proxy, mevcut silinen anahtar kasası ile aynı konumda depolanan bir nesne kaynaktır. 

### <a name="key-vault-object-recovery"></a>Anahtar kasası nesne kurtarma

Bir anahtar gibi bir anahtar kasası nesne silme bağlı hizmet nesne silinmiş bir durumda herhangi bir alma işlemi için erişilemez hale getirme yerleştirmeniz gerekir. Bu durumdayken, anahtar kasası nesne yalnızca, kurtarılan veya zorla/kalıcı olarak silinmiş listelenebilir. 

Aynı anda Key Vault silinen anahtar kasasını veya önceden belirlenmiş bekletme aralığından sonra yürütme için anahtar kasası nesne karşılık gelen temel alınan verileri silme zamanlar. Kasaya karşılık gelen DNS kaydını da saklama aralığı süresince korunur.

### <a name="soft-delete-retention-period"></a>Geçici silme saklama süresi

Geçici silinen kaynaklar süre 90 gün boyunca belirli bir süre için korunur. Geçici silme bekletme aralığı boyunca aşağıdaki Uygula:

- Tüm anahtar kasalarını ve bunlarla ilgili erişim silme ve kurtarma bilgilerin yanı sıra, abonelik için geçici silme durumu anahtar kasası nesnelerindeki listeleyebilir.
    - Yalnızca özel izinlere sahip kullanıcılar, silinen kasa listeleyebilirsiniz. Kullanıcılarımızın silinmiş işleme kasaları için bu özel izinler ile özel bir rol oluşturmanız önerilir.
- Aynı ada sahip bir anahtar kasası aynı konumda oluşturulamıyor; Bu anahtar kasası ile aynı ada ve silinmiş bir durumda olduğu bir nesne içeriyorsa, karşılık olarak, bir anahtar kasası nesne belirli bir kasada oluşturulamıyor 
- Yalnızca özel ayrıcalıklı bir kullanıcısı bir anahtar kasası veya anahtar kasası nesne, karşılık gelen proxy kaynak kurtarma komutu göndererek geri yükleyebilirsiniz.
    - Kullanıcı kasa ayrıcalığı kaynak grubunda bir key vault oluşturma için olan özel rolünün üyesi geri yükleyebilirsiniz.
- Yalnızca özel ayrıcalıklı bir kullanıcısı zorla bir anahtar kasası veya anahtar kasası nesne karşılık gelen proxy kaynak üzerinde bir silme komutu göndererek silebilir.

Bir anahtar kasası veya anahtar kasası nesne, kurtarılır sürece, bekletme aralığın sonunda bir temizleme işlemini geçici silinen anahtar kasasını veya anahtar kasası nesne ve içeriğini hizmet gerçekleştirir. Kaynak silme işlemini yeniden değil.

### <a name="billing-implications"></a>Faturalandırma etkileri

Genel olarak, bir nesne (bir anahtar kasası veya bir anahtar veya gizli anahtarı) silinmiş durumda olduğunda yalnızca iki işlem olası: 'Temizle' ve 'Kurtar'. Diğer tüm işlemler başarısız olur. Bu nedenle, nesne mevcut olsa da, hiçbir işlemleri gerçekleştirilebilir ve bu nedenle hiçbir kullanım gerçekleşir, bu nedenle hiçbir fatura. Ancak, özel durumlar takip ettiğiniz:

- 'Temizle' ve 'geri' eylemleri normal anahtar kasası işlemleri hesaplanır ve faturalandırılır.
- Nesne bir HSM anahtarı ise son 30 gün içinde bir anahtar sürümü kullandıysanız 'HSM korumalı anahtara' ücretsiz olarak aylık ücreti anahtar sürümü başına uygulanır. Nesne hiçbir işlem, karşı gerçekleştirilebilir silinmiş durumda olduğundan bundan sonra bu nedenle ücret uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Geçici silmeyi kullanma için birincil kullanım senaryoları aşağıdaki iki kılavuzları sunar.

- [Key Vault geçici silmeyi PowerShell ile kullanma](key-vault-soft-delete-powershell.md) 
- [Key Vault geçici silmeyi CLI ile kullanma](key-vault-soft-delete-cli.md)

