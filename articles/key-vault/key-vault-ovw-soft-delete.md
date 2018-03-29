---
ms.assetid: ''
title: Azure anahtar kasası geçici silme | Microsoft Docs
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 09/25/2017
ms.openlocfilehash: 6a3573cf31418309a31126b2a0c6a43ea2e0c745
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-key-vault-soft-delete-overview"></a>Azure anahtar kasası soft-delete genel bakış

Anahtar Kasası'nın geçici silme özelliği soft-Sil olarak bilinen kasası nesneleri ve Silinen kasalarını kurtarılmasını sağlar. Özellikle, aşağıdaki senaryolarda adresi:

- Bir anahtar kasası kurtarılabilir silinmesini desteği
- Anahtar kasası nesnelerin (örneğin kurtarılabilir silinmesi için destek anahtarları, gizli, Sertifikalar)

## <a name="supporting-interfaces"></a>Arabirimleri destekleme

Soft-delete özellik .NET KULLANILMADIKLARI başlangıçta kullanılabilir / C# ' ta, PowerShell ve, CLI arabirimleri.

Daha fazla ayrıntı için bu başvurular genel bilgi için bkz [anahtar kasası başvurusu](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Senaryolar

Azure anahtar kasası, Azure Resource Manager tarafından yönetilen, izlenen kaynaklardır. Azure Resource Manager, aynı zamanda başarılı bir silme işlemi artık erişilebilir olmaması, kaynak neden gerekir gerektirir silme işlemi için iyi tanımlanmış bir davranış belirtir. Silme yanlışlıkla veya kasıtlı olup soft-delete özelliği silinen nesnenin kurtarma giderir.

1. Tipik senaryo, bir kullanıcı yanlışlıkla bir anahtar kasası ya da bir anahtar kasası nesnesi silmiş; Kasa veya anahtar kasası nesne anahtarı, önceden belirlenmiş bir süre boyunca kurtarılabilir olacak şekilde, kullanıcı silme işlemi geri almak ve verilerini kurtarma.

2. Farklı bir senaryoda dolandırıcı bir kullanıcı bir anahtar kasası veya iş kesintiye neden bir kasa içinde bir anahtarı gibi bir anahtar kasası nesnesi silme girişiminde bulunabilir. Temel alınan veri gerçek silinmeye karşı anahtar kasası ya da anahtar kasası nesnesinin silinmesini ayıran bir güvenlik önlemi olarak, örneğin, farklı, veri silme izinlerini kısıtlayarak rolü güvenilir olarak kullanılabilir. Bu yaklaşım, aksi takdirde bir hemen veri kaybına neden bir işlem için etkili bir şekilde çekirdek gerektirir.

### <a name="soft-delete-behavior"></a>Soft-delete behavior

Bu özellik ile bir anahtar kasası veya anahtar kasası nesne silme işlemini bir yumuşak etkili bir şekilde nesne silinir görünüm vermiş sırasında ek olarak, belirli Bekletme dönemi için kaynakları bulunduran silme, ' dir. Daha fazla hizmet temelde silme işlemi geri alma Silinmiş nesne kurtarmak için bir mekanizma sağlar. 

Soft-delete isteğe bağlı bir anahtar kasası davranışı ve **varsayılan olarak etkin değildir** bu sürümde. 

### <a name="key-vault-recovery"></a>Anahtar kasası kurtarma

Bir anahtar kasasını silme bağlı hizmet kurtarma için yeterli meta veri ekleme, abonelik altında bir proxy kaynak oluşturur. Proxy kaynak, silinen anahtar kasası ile aynı konumda kullanılabilir saklanan bir nesne değil. 

### <a name="key-vault-object-recovery"></a>Anahtar kasası nesne kurtarma

Bir anahtarı gibi bir anahtar kasası nesnesi silme bağlı hizmet nesne silinmiş durumda, böylece alma işlemleri için erişilemez hale yerleştirir. Bu durumundayken, anahtar kasası nesne yalnızca, kurtarılan veya zorla/kalıcı olarak silinmiş listelenebilir. 

Aynı anda anahtar kasası silinen anahtar kasası veya önceden belirlenmiş bekletme aralığından sonra yürütme için anahtar kasası nesnesinin karşılık gelen temel alınan verilerin silinmesini zamanlar. Kasası'na karşılık gelen DNS kaydı da bekletme aralığı boyunca tutulur.

### <a name="soft-delete-retention-period"></a>Saklama dönemi Soft-Sil

Geçici silinen kaynaklar ayarlanmış saat, 90 gün boyunca saklanır. Soft-delete bekletme aralığı boyunca aşağıdaki Uygula:

- Tüm anahtar kasalarını ve bunlarla ilgili erişim silme ve kurtarma bilgileri yanı sıra, abonelik için geçici silme durumunda anahtar kasası nesneleri listeleyebilir.
    - Yalnızca özel izinlere sahip kullanıcılar, silinen kasalarını listeleyebilirsiniz. Kullanıcılarımızın silinmiş işleme kasaları için bu özel izinlere sahip olan özel bir rol oluşturmanızı öneririz.
- Aynı ada sahip bir anahtar kasası aynı konumda oluşturulamıyor; Bu anahtar kasası ile aynı ada ve silinmiş durumda olduğu bir nesne içeriyorsa, buna bağlı olarak, bir anahtar kasası nesne belirli bir kasada oluşturulamıyor 
- Özellikle ayrıcalıklı bir kullanıcı bir anahtar kasası veya anahtar kasası nesne, karşılık gelen proxy kaynak Kurtar komutunda vererek geri yükleyebilirsiniz.
    - Kullanıcı, kaynak grubu altında bir anahtar kasası oluşturmak için ayrıcalığına sahip olduğunuz özel rol üyesi kasa geri yükleyebilirsiniz.
- Yalnızca özellikle ayrıcalıklı kullanıcı zorla bir anahtar kasası veya anahtar kasası nesne silme komutunu karşılık gelen proxy kaynakta yayımlayarak silebilir.

Bir anahtar kasası veya anahtar kasası nesne kurtarılan sürece, bekletme aralığı sona erdiğinde hizmeti geçici olarak silinen anahtar kasası veya anahtar kasası nesne ve içeriğini temizleme gerçekleştirir. Kaynak silme yeniden değil.

### <a name="permitted-purge"></a>İzin verilen temizleme

Kalıcı olarak silmek, temizleme, bir anahtar kasası proxy kaynak üzerinde bir POST işlemi aracılığıyla mümkündür ve özel ayrıcalıklar gerektirir. Genellikle, yalnızca abonelik sahibi bir anahtar kasası temizleme mümkün olacaktır. Bu kasaya hemen ve kurtarılamaz silinmesini gönderme işlemini tetikler. 

Azure aboneliği olarak işaretlendiği takdirde bu konuda bir özel durumdur *silinemez*. Bu durumda, yalnızca hizmet gerçek silme sonra gerçekleştirebilir ve zamanlanmış bir işlem olarak bunu yapar. 

### <a name="billing-implications"></a>Faturalama etkileri

Genel olarak, bir nesne (bir anahtar kasası veya bir anahtar veya gizli) silinmiş durumda olduğunda, yalnızca iki işlem olmadığından olası: 'temizleme' ve 'Kurtar'. Diğer tüm işlemler başarısız olur. Bu nedenle, nesne mevcut olsa bile, hiçbir işlemleri gerçekleştirilebilir ve bu nedenle hiçbir kullanım oluşur, böylece hiçbir fatura. Özel durumlar vardır ancak takip:

- 'temizleme' ve 'Kurtar' eylemleri normal anahtar kasası işlemleri sayılacaktır ve Fatura edilecek.
- Nesne bir HSM anahtar ise, bir anahtar sürüm son 30 gün içinde kullandıysanız her ay ücretsiz olarak başına anahtar sürümü 'HSM korumalı anahtara' ücret uygulanır. Nesne, hiçbir işlem gerçekleştirilebilir silinmiş durumda olduğundan bundan sonra bu nedenle ücretsiz olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki iki kılavuzları soft-delete kullanmak için birincil kullanım senaryoları sunar.

- [Key Vault geçici silmeyi PowerShell ile kullanma](key-vault-soft-delete-powershell.md) 
- [Key Vault geçici silmeyi CLI ile kullanma](key-vault-soft-delete-cli.md)

