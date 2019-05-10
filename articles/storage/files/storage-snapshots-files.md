---
title: Azure dosyaları için paylaşım anlık görüntülerine genel bakış | Microsoft Docs
description: Paylaşım anlık görüntüsü, bir noktada paylaşımı için bir yöntem olarak, zaman içinde alınmış bir Azure dosya paylaşımının salt okunur bir sürümüdür.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 01/17/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d83cf20c856d37d337f4eb22c30ee9b6823d096b
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235806"
---
# <a name="overview-of-share-snapshots-for-azure-files"></a>Azure dosyaları için paylaşım anlık görüntülerine genel bakış 
Azure dosyaları, dosya paylaşımları paylaşım anlık görüntüsünü olanağı sağlar. Anlık görüntüleri yakalama paylaşım durumu zamandaki o noktada paylaşın. Bu makalede, hangi özelliklerin paylaşım anlık görüntüleri sağlar ve bunları kendi özel kullanım örneğindeki özelliklerinden nasıl gerçekleştirebileceğiniz açıklanmaktadır.

## <a name="when-to-use-share-snapshots"></a>Paylaşım anlık görüntüleri kullanmak ne zaman

### <a name="protection-against-application-error-and-data-corruption"></a>Uygulama hata ve veri bozulmasına karşı koruma
Dosya paylaşımları kullanan uygulamaları yazma, okuma, depolama, aktarım ve işlem gibi işlemleri gerçekleştirin. Bir uygulama yanlış yapılandırılmış veya yanlışlıkla bir hata ortaya, yanlışlıkla üzerine yaz veya hasar birkaç bloklarına gerçekleşebilir. Bu senaryoya karşı korumaya yardımcı olmak için yeni uygulama kodu dağıtmadan önce bir paylaşım anlık görüntüsünü alabilir. Bir hata veya uygulama hatası yeni dağıtımla birlikte kullanılırsa, bu dosya paylaşımında verilerinizin önceki bir sürüme geri dönebilirsiniz. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>İstenmeden yapılmış olabilecek değişiklikleri veya yanlışlıkla silinmekten karşı koruma
Bir metin dosyasına bir dosya paylaşımı üzerinde çalıştığınız düşünün. Metin dosyası kapatıldıktan sonra yaptığınız değişiklikleri geri alıp kaybedersiniz. Bu durumlarda, daha sonra bir dosyanın önceki bir sürümünü kurtarmak gerekir. Paylaşım anlık görüntüleri, yanlışlıkla yeniden adlandırılmış veya silinmiş bir dosyanın önceki sürümlerini kurtarmak için kullanabilirsiniz.

### <a name="general-backup-purposes"></a>Genel yedekleme amaçları
Bir dosya paylaşımı oluşturduktan sonra düzenli aralıklarla veri yedekleme için kullanılacak dosya paylaşımının paylaşım anlık görüntüsü oluşturabilirsiniz. Bir paylaşım anlık görüntüsü, düzenli aralıklarla durumdayken gelecekteki denetim gereksinimleri veya olağanüstü durum kurtarma için kullanılabilir verilerin önceki sürümlerine yardımcı olur.

## <a name="capabilities"></a>Özellikler
Paylaşım anlık görüntüsü, verilerinizin zaman içinde nokta, salt okunur bir kopyasıdır. Oluşturma, silme ve REST API kullanarak anlık görüntüleri yönetme. Aynı özellikleri, ayrıca istemci kitaplığı, Azure CLI ve Azure portalında kullanılabilir. 

REST API ve SMB kullanarak bir paylaşım anlık görüntülerini görüntüleyebilirsiniz. Dizin veya dosya sürümlerinin listesini alabilir ve doğrudan bir sürücü olarak belirli bir sürüme bağlayabilir (yalnızca Windows - kullanılabilir bkz [sınırları](#limits)). 

Paylaşım anlık görüntüsü oluşturulduktan sonra okuyun, kopyalanır, veya silinmiş, ancak değiştirilmedi. Başka bir depolama hesabı için tam paylaşım anlık görüntüsü kopyalanamıyor. Bu dosya tarafından dosya, AzCopy veya kopyalama başka mekanizmalar kullanılarak yapmak zorunda.

Paylaşım anlık görüntü özelliği, dosya paylaşımı düzeyinde sağlanır. Alma, tek tek dosyaları geri yüklemek için izin vermek için tek tek dosya düzeyinde sağlanır. SMB, REST API, portal, istemci kitaplığı veya PowerShell/CLI araçları kullanarak, bir tam dosya paylaşımı geri yükleyebilirsiniz.

Bir dosya paylaşımının paylaşım anlık görüntüsü, kendi temel dosya paylaşımına aynıdır. Tek fark bir **DateTime** değeri, paylaşım için paylaşım anlık görüntünün alındığı zaman göstermek için URI eklenir. Örneğin, URI bir dosya paylaşımı ise http://storagesample.core.file.windows.net/myshare, URI benzer paylaşım anlık görüntüsü:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Paylaşım anlık görüntüleri kalıcı olarak açıkça silinene kadar. Paylaşım anlık görüntüsü, kendi temel dosya paylaşımı daha uzun sürmesi olamaz. Geçerli anlık görüntüleriniz izlemek için temel dosya paylaşımıyla ilişkili anlık görüntüleri sıralayabilirsiniz. 

Bir dosya paylaşımının paylaşım anlık görüntüsü oluşturduğunuzda, aynı değerlerle paylaşım anlık görüntüsünün dosya paylaşımının Sistem özellikleri kopyalanır. Oluşturduğunuzda anlık görüntü paylaşımı için ayrı bir meta veri belirtmediğiniz sürece temel dosyalar ve dosya paylaşımının meta verileri de paylaşım anlık görüntüsü için kopyalanır.

Önce tüm paylaşım anlık görüntüleri silmeniz sürece paylaşım anlık görüntülerine sahip bir paylaşım nelze odstranit.

## <a name="space-usage"></a>Alanı kullanımı 
Paylaşım anlık görüntüleri, doğası gereği artımlı. En son paylaşım anlık görüntüsü kaydedildikten sonra değişmiş yalnızca verileri. Bu paylaşım anlık görüntüsü oluşturmak için gereken süreyi en aza indirir ve depolama maliyetlerinden kaydeder. Herhangi bir nesneye yazma işlemi veya özellik veya meta veri güncelleştirme işlemi "değiştirilen içerik doğru" olarak sayılır ve Paylaşım anlık görüntüsüne içinde depolanır. 

Alan tasarrufu yapmak için değişim sıklığı, en yüksek süre için paylaşım anlık görüntüsünü silebilirsiniz.

Paylaşım anlık görüntüleri artımlı olarak kaydedilmiş olsa bile, paylaşımı geri yüklemek için yalnızca en son paylaşım anlık görüntüsüne bekletmeniz gerekir. Paylaşım anlık görüntüsü sildiğinizde, yalnızca bu paylaşım anlık görüntüsü için benzersiz verileri kaldırılır. Etkin bir anlık görüntü göz atmak ve verilerinizi (paylaşım anlık görüntünün alındığı zamanından) özgün konuma veya alternatif bir konuma geri yüklemek için gereken tüm bilgileri içerir. Öğe düzeyinde geri yükleyebilirsiniz.

Anlık görüntüler, 5 TB paylaşımı sınırında sayılmaz. Ne kadar toplam alanı paylaşım anlık görüntüleri kaplaması için sınır yoktur. Depolama hesabı sınırları hala geçerlidir.

## <a name="limits"></a>Limits
Azure dosyaları'nı bugün sağlayan bir paylaşım anlık görüntüleri sayısı 200'dür. 200 paylaşım anlık görüntüleri sonra yenilerini oluşturmak için önceki paylaşım anlık görüntüleri silmeniz gerekir. 

Paylaşım anlık görüntüleri oluşturmak için eş zamanlı çağrı için sınır yoktur. Belirli bir dosya paylaşımı anlık görüntülerini kullanabilir paylaşan boşluk miktarının sınırı yoktur. 

Bugün, paylaşım anlık görüntüleri Linux'ta bağlama mümkün değildir. Linux SMB istemcisinin Windows gibi bağlama anlık görüntülerini desteklemiyor olmasıdır.

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Paylaşım anlık görüntüsünden geri bir paylaşıma veri kopyalama
İlgili dosyaları ve anlık görüntüleri paylaşan kopyalama işlemleri bu kuralları izleyin:

Kendi temel paylaşımı veya başka bir konuma üzerinden tek bir dosya paylaşımı anlık görüntüsü dosyalarında kopyalayabilirsiniz. Bir dosyanın önceki bir sürümünü geri yüklemek veya paylaşım anlık görüntüsünden dosyası tarafından kopyalayarak tam dosya paylaşımını geri yükleyebilirsiniz. Paylaşım anlık görüntüsüne temel paylaşımına yükseltilmez. 

Paylaşım anlık görüntüsüne kopyaladıktan sonra değişmeden kalır, ancak temel dosya paylaşımının paylaşım anlık görüntüsüne içinde kullanılabilir olan verilerin bir kopyasını ile yazılır. Doğru geri yüklenen dosya sayısı "içeriği değiştirildi."

Farklı bir ada sahip bir hedef için bir paylaşım anlık görüntüde bir dosyaya kopyalayabilirsiniz. Elde edilen gereken hedef dosyayı bir yazılabilir dosya ve Paylaşım anlık ' dir.

Kopyalama işlemiyle bir hedef dosyanın üzerine yazıldığında, özgün hedef dosya ile ilgili herhangi bir paylaşım anlık görüntüleri değişmeden kalır.

## <a name="general-best-practices"></a>Genel en iyi yöntemler 
Azure'da bir altyapı çalıştırıyorsanız, yedeklemeler için veri kurtarma mümkün olduğunca otomatik hale getirin. Otomatik Eylemler, veri koruma ve kurtarılabilirliği artırma sağlamanın el ile gerçekleştirilen işlemleri daha güvenilirdir. REST API, istemci SDK'sı veya Otomasyon için komut dosyalarını kullanabilirsiniz.

Paylaşım anlık görüntüsü Zamanlayıcı'yı dağıtmadan önce paylaşım anlık görüntü sıklığı ve bekletme ayarları gereksiz geçmeyecekseniz ücretlendirmeden kaçınmak için dikkatlice düşünün.

Paylaşım anlık görüntüleri, yalnızca dosya düzeyinde koruma sağlar. Paylaşım anlık görüntüleri bir dosya paylaşımı veya depolama hesabını silme işlemi fat parmak engellemez. Bir depolama hesabını yanlışlıkla silinmesini korumak için depolama hesabı veya kaynak grubu kilitleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Paylaşım anlık görüntüleri ile çalışma:
    - [PowerShell](storage-how-to-use-files-powershell.md)
    - [CLI](storage-how-to-use-files-cli.md)
    - [Windows](storage-how-to-use-files-windows.md#accessing-share-snapshots-from-windows)
    - [Anlık görüntü ile ilgili SSS](storage-files-faq.md#share-snapshots)
