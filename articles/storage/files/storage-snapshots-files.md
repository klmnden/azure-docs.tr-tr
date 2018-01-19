---
title: "Azure dosyaları (Önizleme) paylaşımı anlık görüntüleri genel bakış | Microsoft Docs"
description: "Paylaşım anlık bir noktada paylaşımını yedekleme için bir yöntem olarak, zaman içinde alınmış bir Azure dosya paylaşımının salt okunur bir sürümüdür."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: c309804f33fc0e5b2091e18dfe5fe3c9849a2709
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-share-snapshots-for-azure-files-preview"></a>Azure dosyaları (Önizleme) paylaşımı anlık görüntüleri genel bakış
Azure dosyaları dosya paylaşımları paylaşımı anlık görüntüsünü olanağı sunar. Anlık görüntüler (Önizleme) yakalama paylaşım durumu zamandaki o noktada paylaşır. Bu makalede, paylaşım anlık görüntüleri sağlamak hangi özelliklere ve nasıl, bunları özel kullanım durumda yararlanabilir açıklanmaktadır.


## <a name="when-to-use-share-snapshots"></a>Paylaşım anlık görüntüleri kullanma zamanı

### <a name="protection-against-application-error-and-data-corruption"></a>Uygulama hata ve veri bozulmasına karşı koruma

Dosya paylaşımları kullanan uygulamalar yazma, okuma, depolama, iletim ve işleme gibi işlemleri gerçekleştirir. Bir uygulama yanlış yapılandırılmış veya istenmeyen bir hata ortaya, yanlışlıkla üzerine yazma veya hasar birkaç bloklarına durum meydana gelebilir. Bu senaryoya karşı korunmasına yardımcı olmak için yeni uygulama kodu dağıtmadan önce bir paylaşım anlık görüntü alabilir. Bir hata veya uygulama hatası ile yeni dağıtım kullanılırsa, bu dosya paylaşımında verilerinizin önceki sürüme geri dönebilirsiniz. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Yanlışlıkla silmeleri ya da istenmeyen değişikliklere karşı koruma

Bir metin dosyasına bir dosya paylaşımı üzerinde çalıştığınız düşünün. Metin dosyasını kapatıldıktan sonra yaptığınız değişiklikleri geri almak için yönetemez. Bu durumlarda, ardından dosyayı önceki bir sürümünü kurtarmak gerekir. Paylaşım anlık görüntülerin yanlışlıkla yeniden adlandırılmış veya silinmiş olup olmadığını dosyanın önceki sürümlerini kurtarmak için kullanabilirsiniz.

### <a name="general-backup-purposes"></a>Genel yedekleme amaçları

Bir dosya paylaşımı oluşturduktan sonra düzenli aralıklarla veri yedekleme için kullanmak üzere dosya paylaşımının paylaşım anlık görüntü oluşturabilirsiniz. Anlık görüntü, düzenli aralıklarla durumdayken bir paylaşımı gelecekteki denetim gereksinimleri veya olağanüstü durum kurtarma için kullanılabilir verilerin önceki sürümlerini korumaya yardımcı olur.

## <a name="capabilities"></a>Özellikler

Verilerinizi zaman içinde nokta, salt okunur bir kopyasını paylaşımı anlık görüntüsüdür. Oluşturma, silme ve anlık görüntüleri REST API'sini kullanarak yönetin. Aynı yetenekleri de istemci kitaplığı, Azure CLI ve Azure portalında kullanılabilir. 

Bir paylaşım anlık görüntüleri REST API ve SMB kullanarak görüntüleyebilirsiniz. Dizin veya dosya sürümlerinin listesi alabilir ve doğrudan bir sürücü olarak belirli bir sürüme bağlayabilir. 

Paylaşım anlık görüntü oluşturulduktan sonra okumak, kopyalanan, veya silinmiş, ancak değişiklik. Başka bir depolama hesabı için tam paylaşım anlık görüntü kopyalama yapamazsınız. AzCopy veya diğer kopyalama mekanizmalarını kullanarak bu tarafından dosyası, yapmanız gerekir.

Paylaşım anlık görüntü özelliği dosya paylaşımı düzeyinde sağlanır. Alma, tek tek dosyaların geri yüklemek için izin vermek için tek tek dosya düzeyinde sağlanır. SMB, REST API, portal, istemci kitaplığı veya PowerShell/CLI araçları kullanarak, bir tam dosya paylaşımı geri yükleyebilirsiniz.

Dosya paylaşımının paylaşım anlık görüntü, temel dosya paylaşımına aynıdır. Tek fark bir **DateTime** değeri paylaşımına paylaşımı anlık görüntünün alındığı zaman belirtmek için URI eklenir. Bir dosya paylaşımı URI http://storagesample.core.file.windows.net/myshare ise, örneğin, paylaşım anlık görüntü URI benzer:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Açıkça silinene kadar paylaşımı anlık görüntüleri kalıcı olmasını sağlar. Paylaşım anlık görüntü, temel dosyanın paylaşım outlive olamaz. Geçerli anlık izlemek için temel dosya paylaşımı ile ilişkili anlık görüntüleri sıralayabilirsiniz. 

Paylaşım anlık görüntüsünü bir dosya paylaşımı oluşturduğunuzda, dosya paylaşımının Sistem özellikleri aynı değerleri paylaşım anlık görüntü kopyalanır. Oluşturduğunuzda anlık görüntü paylaşımı için ayrı meta verileri belirtmediğiniz sürece temel dosyaları ve dosya paylaşımının meta verileri de paylaşımı anlık görüntüye kopyalanır.

Tüm paylaşım anlık görüntüleri silmeniz sürece, paylaşım anlık görüntülere sahip bir paylaşımı silemezsiniz.


## <a name="space-usage"></a>Alanı kullanımı 

Doğası gereği artımlı paylaşımı anlık görüntüler. Yalnızca en son paylaşımı anlık kaydedildikten sonra yalnızca değişen verileri. Bu paylaşım anlık görüntü oluşturmak için gereken süreyi en aza indirir ve depolama maliyetlerini kaydeder. Nesneye herhangi bir yazma işlemi veya özellik veya meta veri güncelleştirme işlemi "doğru değiştirilen içerik" kabul edilir ve Paylaşım anlık görüntü depolanır. 

Alanından tasarruf etmek için ne zaman karmaşıklığı en yüksek süre paylaşımı anlık görüntü silebilirsiniz.

Artımlı olarak kaydedilmiş paylaşımı anlık olsa bile paylaşımını geri yükleme için yalnızca en son paylaşımı anlık görüntü bekletmeniz gerekir. Paylaşım anlık görüntüyü sildiğinizde, yalnızca o paylaşımı anlık görüntüye benzersiz verileri kaldırılır. Etkin anlık görüntüler göz atmak ve verilerinizi (paylaşım anlık görüntü alındıktan zamandan) özgün konuma veya alternatif bir konuma geri yüklemek için gereken tüm bilgileri içerir. Öğe düzeyinde geri yükleyebilirsiniz.

Anlık görüntüler, 5 TB paylaşımı sınırında sayılmaz. Ne kadar alan paylaşımı anlık görüntüleri toplam kaplar bir sınır yoktur. Depolama hesabı sınırları hala geçerlidir.

## <a name="limits"></a>Sınırlar

Maksimum sayıda Azure dosyaları bugün verir paylaşımı anlık görüntü 200'dür. 200 paylaşımı anlık görüntüleri sonra yeni kampanya oluşturmak için eski paylaşımı anlık görüntüleri silmeniz gerekir. 

Paylaşım anlık görüntüleri oluşturmak için eş zamanlı çağrıları için bir sınır yoktur. Belirli dosya paylaşımı anlık görüntüleri tüketebileceği paylaşan alanı için sınır yoktur. 

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Paylaşım anlık görüntüden geri bir paylaşıma veri kopyalama

Bu kurallar dosyaları içerir ve anlık görüntüleri paylaşan kopyalama işlemleri izleyin:

Tek bir dosya paylaşımı anlık dosyalarında üzerinden temel paylaşımı veya başka bir konuma kopyalayabilirsiniz. Bir dosyanın önceki bir sürümünü geri yükleyin veya paylaşım anlık görüntüden dosyası tarafından kopyalayarak tam dosya paylaşımını geri yükleme. Paylaşım anlık görüntü temel paylaşımına yükseltilmez. 

Kopyaladıktan sonra paylaşımı anlık görüntü değişmeden kalır, ancak temel dosya paylaşımının paylaşım anlık kullanılabilir verilerin bir kopyasını üzerine yazılır. Doğru tüm geri yüklenen dosya sayısı "içeriği değiştirildi."

Farklı bir adla bir hedefe paylaşımı anlık görüntüde bir dosyaya kopyalayabilirsiniz. Sonuçta elde edilen hedef dosyasını yazılabilir bir dosya paylaşımı anlık ise.

Bir hedef dosya bir kopya ile yazılır, özgün hedef dosya ile ilişkili tüm paylaşım anlık görüntüleri değişmeden kalır.

## <a name="general-best-practices"></a>Genel en iyi yöntemler 

Azure üzerinde altyapı çalıştırırken, mümkün olduğunda veri kurtarma için yedeklemeleri otomatikleştirin. Otomatik eylemler veri koruma ve kurtarılabilirliği artırmasına yardımcı el ile işlemleri daha güvenlidir. REST API, istemci SDK'sını ya da Otomasyon için komut dosyalarını kullanabilirsiniz.

Paylaşım anlık görüntü Zamanlayıcı'yı dağıtmadan önce paylaşım anlık görüntü sıklığı ve gereksiz ücret oluşmasını önlemek için saklama ayarları dikkatlice düşünün.

Paylaşım anlık görüntüleri yalnızca dosya düzeyinde korumasını sağlar. Paylaşım anlık görüntüler bir dosya paylaşımı veya depolama hesabında fat parmak silme engellemez. Bir depolama hesabını yanlışlıkla silinmesini korunmasına yardımcı olmak için depolama hesabı veya kaynak grubu kilitleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Paylaşım anlık görüntüleri ile çalışma](storage-how-to-use-files-snapshots.md)
* [Anlık görüntü SSS paylaşma](storage-files-faq.md)

