---
title: Bir Azure Data Lake Store hesabı - Azure ile birden çok Hdınsight kümeleri kullanın | Microsoft Docs
description: Birden fazla Hdınsight kümesi ile tek bir Data Lake Store hesabını kullanmayı öğrenin
keywords: hdınsight depolama, hdfs, yapılandırılmış veri, yapılandırılmamış verileri, veri gölü deposu
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 1d073732b5dd9b9867813d9ffcfad5caa1131d81
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102627"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Birden çok Hdınsight kümeleri ile bir Azure Data Lake Store hesabınız

Hdınsight sürüm 3.5 ile başlayarak, varsayılan dosya sistemi Azure Data Lake Store hesapları ile Hdınsight kümeleri oluşturabilirsiniz.
Data Lake Store, büyük miktarlarda verinin yalnızca barındırmak için ideal hale getirir sınırsız depolama destekler; ancak da barındırmak için birden çok Hdınsight tek bir Data Lake deposu hesap paylaşan kümeleri. Data Lake Store depolama ile bir Hdınsight kümesi oluşturma hakkında yönergeler için bkz: [hızlı başlangıç: Hdınsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

Bu makalede, Data Lake için öneriler, bir tek ve paylaşılan veri Gölü depolamak birden çok kullanılan hesabı ayarlamak için yönetici depolamak sağlanmaktadır **etkin** Hdınsight kümeleri. Bu öneriler, paylaşılan bir Data Lake store hesabı üzerinde birden çok güvenli yanı sıra güvenli olmayan Hadoop kümeleri barındırmak için geçerlidir.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store dosya ve klasör düzeyinde ACL'leri

Azure Data Lake Store'da yakından açıklanan, dosya ve klasör düzeyinde ACL'ler iyi bir bilgisine sahip, bu makalenin kalanında varsayar [erişim denetimi Azure Data Lake Store'da](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Birden çok Hdınsight kümeleri için Data Lake Store Kurulumu
Bize bir Data Lake Store hesabı ile birden çok Hdınsight kümeleri kullanarak önerileri açıklayan bir iki düzeyli klasör hiyerarşisi alın. Bir Data Lake Store hesabı klasör yapısı ile sahip göz önünde bulundurun **/kümeleri/Finans**. Bu yapı ile Finans kuruluş tarafından gerekli tüm kümelerin /clusters/finance depolama konumu olarak kullanabilirsiniz. Gelecekte, başka bir kuruluştaki pazarlama, Hdınsight kümeleri aynı Data Lake Store hesabı kullanarak, bunlar/kümeleri/pazarlama oluşturabilir oluşturmak isterse söyleyin. Şu an için yalnızca kullanalım **/kümeleri/Finans**.

Bu klasör yapısı etkili bir şekilde Hdınsight kümeleri tarafından kullanılmak üzere etkinleştirmek için Data Lake Store yönetici tabloda açıklandığı gibi uygun izinleri atamanız gerekir. Tabloda gösterilen izinleri erişim ACL'ler ve değil varsayılan ACL'leri karşılık gelir. 


|Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlı kullanıcı | Adlı kullanıcı izinleri | Adlandırılmış Grup | Adlandırılmış grup izinleri |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |Yönetici |Yönetici  |Hizmet sorumlusu |--x  |FINGRP   |r-x         |
|/Clusters | rwxr-x--x |Yönetici |Yönetici |Hizmet sorumlusu |--x  |FINGRP |r-x         |
|/ kümeleri/Finans | rwxr-x--t |Yönetici |FINGRP  |Hizmet sorumlusu |rwx  |-  |-     |

Tabloda

- **Yönetici** oluşturan ve Data Lake Store hesabı yönetici.
- **Hizmet sorumlusu** olan hesapla ilişkili Azure Active Directory (AAD) hizmet sorumlusu.
- **FINGRP** Finans kuruluştan kullanıcıları içeren aad'de oluşturduğunuz bir kullanıcı grubu.

(Bu da bir hizmet sorumlusu oluşturur) bir AAD uygulaması oluşturma hakkında yönergeler için bkz [bir AAD uygulaması oluşturabilir](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). AAD'de bir kullanıcı grubu oluşturma hakkında daha fazla yönerge için bkz: [Azure Active Directory içinde grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Dikkate alınması gereken bazı önemli noktalar.

- İki düzey klasör yapısı (**/ / Finans/kümeleri**) oluşturulur ve uygun izinlere sahip Data Lake Store yönetici tarafından sağlanan **önce** kümeleri için depolama hesabını kullanarak. Bu yapı kümeleri oluşturulurken otomatik olarak oluşturulmaz.
- Yukarıdaki örnekte sahibi olan Grup ayarlanırken önerir **/kümeleri/Finans** olarak **FINGRP** ve sorgulamasına **r-x** tüm klasör hiyerarşisini başlangıç FINGRP için erişim kök dizini. Bu, FINGRP üyeleri kökünden başlayarak klasör yapısı gidebilirsiniz sağlar.
- Farklı AAD hizmet asıl adı altında kümeleri oluşturabilir, bu durumda **/kümeleri/Finans**, Yapışkan-bit (belirlendiğinde **Finans** klasörü) klasörleri bir hizmet sorumlusu tarafından oluşturulan sağlar diğer silinemiyor.
- İzinleri ve klasör yapısı yerinde olduktan sonra Hdınsight küme oluşturma işlemi kümeye özgü depolama loaction altında oluşturur **/ / Finans/kümeleri**. Örneğin, bir küme adı fincluster01 ile depolama olabilir **/clusters/finance/fincluster01**. Hdınsight kümesi tarafından oluşturulan klasörler için izinleri ve sahipliği tabloda burada gösterilir.

    |Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlı kullanıcı | Adlı kullanıcı izinleri | Adlandırılmış Grup | Adlandırılmış grup izinleri |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/Clusters/finanace/fincluster01 | rwxr-x---  |Hizmet sorumlusu |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Önerileri için girdi ve çıktı verilerini işi

Giriş verilerinden bir işi bir iş ve çıkışları klasörünün dışında depolanması öneririz **/kümeleri**. Bu, bazı depolama alanını geri kazanmak için küme özgü bir klasörde silinse bile, iş girişleri ve çıkışları gelecekte kullanılmak üzere hala kullanılabilir olmasını sağlar. Böyle bir durumda, iş girişleri ve çıkışları depolamak için klasör hiyerarşisi için hizmet asıl uygun düzeyde bir erişim verdiğinden emin olun.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Tek bir depolama hesabına paylaşımı kümelerinde sınırla

Tek bir Data Lake Store hesabı paylaşabilirsiniz küme sayısı sınırını bu kümeleri üzerinde çalıştırılan iş yüküne bağlıdır. Çok fazla sayıda küme veya çok yoğun iş yükleri bir depolama hesabını paylaşmak kümelerinde sahip depolama hesabı giriş/kısıtlanan çıkışı neden olabilir.

## <a name="support-for-default-acls"></a>Varsayılan ACL desteği

Adlı kullanıcı erişimi (yukarıdaki tabloda gösterildiği gibi) bir hizmet sorumlusu oluştururken, öneririz **değil** adlı kullanıcı varsayılan ACL ile ekleme. Sahibi olan kullanıcı, sahibi olan Grup ve diğerleri için varsayılan ACL'leri sonuçları 770 izinler atama içinde kullanarak adlı kullanıcının erişim sağlama. Bu varsayılan değeri 770 sahibi olan kullanıcı (7) veya sahibi olan Grup (7) izinlerinin hemen almaz, ancak hemen başkalarının (0) tüm izinleri alır. Bu bilinen bir sorun bir belirli kullanım ayrıntılı olarak ele alınan örneği ile sonuçlanır [bilinen sorunlar ve geçici çözümler](#known-issues-and-workarounds) bölümü.

## <a name="known-issues-and-workarounds"></a>Bilinen sorunlar ve geçici çözümler

Bu bölümde, Hdınsight kullanarak Data Lake Store ve bunların geçici çözümleri için bilinen sorunlar listelenmektedir.

### <a name="publicly-visible-localized-yarn-resources"></a>Herkese görünür yerelleştirilmiş YARN kaynaklar

Yeni bir Azure Data Lake store hesabı oluşturulduğunda, kök dizini otomatik olarak erişim ACL izin bitleri için 770 kümesi ile birlikte sağlanır. Kök klasörün sahibi olan kullanıcı hesabı (Data Lake Store yönetici) oluşturulan kullanıcı ayarlanır ve sahibi olan Grup birincil hesabı oluşturan kullanıcı grubu için. Erişim yok "başkaları için" sağlanır.

Bu ayarlar bir özel Hdınsight kullanım örneği yakalanan etkileyen bilinen [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). İş gönderimlerini aşağıdakine benzer bir hata iletisi ile başarısız olabilir:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Daha önce Genel kaynaklar yerelleştirilirken bağlı YARN JIRA belirtildiği gibi yerelleştiriciye uzak dosya sistemi izinlerini denetleyerek, istenen tüm kaynakları gerçekten ortak olduğunu doğrular. Bu koşul sığmayan LocalResource yerelleştirme için reddedilir. İzinleri denetimini içeren dosyaya okuma erişimi "başkaları için". Bu senaryo Giden kutusu Azure Data Lake başkalarına"" tüm erişimini engellediği beri Azure Data Lake, Hdınsight kümelerinde barındırdığında kök klasör düzeyinde çalışmaz.

#### <a name="workaround"></a>Geçici çözüm
Kümesini yürütmek Okuma izinlerini **başkalarının** hiyerarşisi aracılığıyla örneğin **/**, **/kümeleri** ve   **/kümeleri/Finans** yukarıdaki tabloda gösterilen.

## <a name="see-also"></a>Ayrıca bkz.

* [Hızlı Başlangıç: Hdınsight kümelerinde ayarlayın](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)


