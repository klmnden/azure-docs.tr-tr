---
title: Bir Azure Data Lake Store hesabına - Azure ile birden çok HDInsight kümesi kullanma
description: Birden fazla HDInsight kümesi ile tek bir Data Lake Storage hesabını kullanmayı öğrenin
keywords: hdınsight depolama, hdfs, yapılandırılmış veriler, yapılandırılmamış veriler, data lake store
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: b580890b1663aa6ce742443e927e4d760585d4ce
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700282"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-storage-account"></a>Birden çok HDInsight kümesi ile bir Azure Data Lake Storage hesabını kullanın.

HDInsight sürüm 3.5 ile başlayarak, varsayılan dosya sistemi Azure Data Lake depolama hesapları ile HDInsight kümeleri oluşturabilirsiniz.
Data Lake Store, büyük miktarlarda verinin yalnızca barındırmak için ideal hale getirir, sınırsız depolama destekler; ancak ayrıca barındırmak için birden çok HDInsight bu tek bir Data Lake depolama hesabının paylaşıma kümeleri. Depolama alanı olarak Data Lake Store ile HDInsight kümesi oluşturma hakkında yönergeler için bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

Bu makalede, bir tek ve paylaşılan Data Lake depolama birden çok kullanılan hesabı ayarlamak için Data Lake Storage yöneticisine önerileri sağlar **etkin** HDInsight kümeleri. Bu öneriler, paylaşılan bir Data Lake Storage hesabında birden fazla güvenli olarak güvenli olmayan Apache Hadoop kümelerini barındırmak için geçerlidir.


## <a name="data-lake-storage-file-and-folder-level-acls"></a>Data Lake depolama dosya ve klasör ACL'leri düzeyi

Bu makalenin geri kalanında adresinde ayrıntılı olarak açıklanan Azure Data Lake Storage, dosya ve klasör düzeyinde ACL'leri iyi bilgi olduğunu varsayar [erişim denetimi, Azure Data Lake Storage](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-storage-setup-for-multiple-hdinsight-clusters"></a>Birden çok HDInsight kümeleri için Data Lake depolama Kurulumu
Bize bir Data Lake Storage hesabıyla birden çok HDInsight kümeleri kullanarak önerileri açıklayan bir iki düzey klasör hiyerarşisi yararlanın. Klasör yapısı ile bir Data Lake Store hesabına sahip göz önünde bulundurun **/kümeleri/Finans**. Bu yapı ile Finans kuruluş tarafından gerekli tüm kümelerin /clusters/finance depolama konumu olarak kullanabilirsiniz. İleride, başka bir kuruluştaki, pazarlama, HDInsight kümeleri aynı Data Lake Storage hesabını kullanarak, bunlar oluşturabilir/kümeleri/pazarlama oluşturmak isterse varsayalım. Şimdilik yalnızca kullanalım **/kümeleri/Finans**.

Bu klasör yapısını etkili bir şekilde HDInsight kümeleri tarafından kullanılmak üzere etkinleştirmek için Data Lake Storage yönetici tabloda açıklandığı gibi uygun izinleri atamanız gerekir. Tabloda belirtilen izinlere erişim ACL'leri ve varsayılan ACL değil karşılık gelir. 


|Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlandırılmış kullanıcılar | Adlandırılmış kullanıcı izinleri | Adlandırılmış Grup | Adlandırılmış grup izinleri |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |Yönetici |Yönetici  |Hizmet sorumlusu |--x  |FINGRP   |r-x         |
|/Clusters | rwxr-x--x |Yönetici |Yönetici |Hizmet sorumlusu |--x  |FINGRP |r-x         |
|/ kümeleri/Finans | rwxr-x--t |Yönetici |FINGRP  |Hizmet sorumlusu |rwx  |-  |-     |

Tabloda,

- **Yönetici** oluşturan ve Data Lake Storage hesabının Yöneticisi.
- **Hizmet sorumlusu** olan hesapla ilişkili Azure Active Directory (AAD) hizmet sorumlusu.
- **FINGRP** Finans kuruluşun kullanıcıları içeren bir AAD içinde oluşturulan bir kullanıcı grubu.

(Bu da bir hizmet sorumlusu oluşturur) bir AAD uygulaması oluşturma hakkında yönergeler için bkz [bir AAD uygulaması oluşturmak](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application). AAD içinde bir kullanıcı grubu oluşturma hakkında yönergeler için bkz: [Azure Active Directory'de grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Dikkate alınması gereken bazı önemli noktalar.

- İki düzey klasör yapısı (**/kümeleri/Finans/**) oluşturulmalı ve uygun izinlere sahip Data Lake Storage yönetici tarafından sağlanan **önce** kümeleri için depolama hesabı kullanarak. Bu yapı, küme oluşturulurken otomatik olarak oluşturulmaz.
- Yukarıdaki örnek grubu ayarını önerir **/kümeleri/Finans** olarak **FINGRP** ve sorgulamasına **r-x** başlangıç klasörün tamamını hiyerarşisine FINGRP erişimi kök dizini. Bu, FINGRP üyelerinin kökünden başlayarak klasör yapısını gidebilirsiniz sağlar.
- Farklı AAD hizmet sorumlusu altında kümeleri oluşturabilir, bu durumda **/kümeleri/Finans**, Yapışkan bit (belirlendiğinde **Finans** klasör) klasörleri bir hizmet sorumlusu tarafından oluşturulan sağlar tarafından silinemiyor.
- İzinleri ve klasör yapısı yerinde olduğunda, HDInsight kümesi oluşturma işlemi bir kümeye özgü depolama konumu altında oluşturur **/kümeleri/Finans/**. Örneğin, depolama alanı için bir küme adı fincluster01 olabilir **/clusters/finance/fincluster01**. HDInsight kümesi tarafından oluşturulan klasörler için izinleri ve sahiplik tabloda burada gösterilir.

    |Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlandırılmış kullanıcılar | Adlandırılmış kullanıcı izinleri | Adlandırılmış Grup | Adlandırılmış grup izinleri |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/Clusters/finanace/fincluster01 | rwxr-x---  |Hizmet Sorumlusu |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Öneriler için girdi ve çıktı verilerini iş

Bir işi bir iş ve çıkışları için giriş verileri dışında bir klasöre depolanması önerilir **/kümeleri**. Bu, bazı depolama alanını boşaltmak için küme özgü bir klasörde silinse bile, iş girdileri ve çıktıları gelecekte kullanım için hala kullanılabilir olmasını sağlar. Böyle bir durumda, iş giriş ve çıkışlarını depolamak için klasör hiyerarşisi hizmet sorumlusu için uygun erişim düzeyini verdiğinden emin olun.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Tek bir depolama hesabında paylaşımı kümelerinde sınırla

Tek bir Data Lake Store hesabına paylaşabilirsiniz küme sayısı sınırı bu kümelerinde çalıştırılan iş yüküne bağlıdır. Çok fazla küme veya çok yoğun iş yükleri bir depolama hesabını paylaşmak kümelerinde sahip depolama hesabı giriş/kısıtlanmazsınız çıkış neden olabilir.

## <a name="support-for-default-acls"></a>Varsayılan ACL'ler için destek

(Yukarıdaki tabloda gösterilen) adlı kullanıcının erişimi olan bir hizmet sorumlusu oluştururken öneririz **değil** adlı kullanıcı varsayılan ACL ekleme. Adlı kullanıcının erişim sahibi olan kullanıcı, sahip olan Grup ve diğerleri için varsayılan ACL'ler sonuçları 770 izinleri atamasını kullanarak sağlama. Bu varsayılan değeri 770 izinlerine sahip olan kullanıcı (7) veya sahip olan Grup (7) hemen almaz, ancak diğerleri (0) ait tüm izinleri hemen alır. Bir belirli kullanım ayrıntılı olarak açıklanan örneği bilinen bir sorundur sonuçlanır [bilinen sorunlar ve geçici çözümler](#known-issues-and-workarounds) bölümü.

## <a name="known-issues-and-workarounds"></a>Bilinen sorunlar ve çözümleri

Bu bölümde, HDInsight kullanarak Data Lake Storage ve bunların geçici çözümleri için bilinen sorunlar listelenmektedir.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Herkese görünür yerelleştirilmiş Apache Hadoop YARN kaynaklar

Yeni bir Azure Data Lake Store hesabı oluşturulduğunda, kök dizinine erişim ACL'si izin bitleri 770 kümesine ile otomatik olarak sağlanır. Kök klasörün sahibi olan kullanıcı (Data Lake Storage yönetici) hesabı oluşturan kullanıcıya ayarlanır ve sahip olan Grup, hesabı oluşturan kullanıcı birincil grubu olarak ayarlanır. Erişim yok "başkaları için" sağlanır.

Bu ayarlar bir belirli HDInsight kullanım örneği yakalanan içinde etkileyen bilinen [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). İş gönderimleri şuna benzer bir hata iletisiyle başarısız olabilir:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Daha önce ortak kaynaklara yerelleştirilirken bağlı YARN jıra'da belirtildiği gibi uzak dosya sistemi izinlerini kontrol ederek istenen tüm kaynakları gerçekte herkese açık yerelleştiriciye doğrular. Bu koşul uymayan herhangi LocalResource yerelleştirme için reddedilir. İzinleri denetleme içerir dosya için okuma erişimi "başkaları için". Bu senaryo kullanıma hazır Azure Data Lake "diğerleri" tüm erişimi engeller beri Azure Data Lake, HDInsight kümelerinde barındırırken kök klasör düzeyinde çalışmaz.

#### <a name="workaround"></a>Geçici çözüm
Küme yürütme Okuma izinlerini **başkalarının** hiyerarşisi aracılığıyla örneğin **/**, **/kümeleri** ve   **/kümeleri/Finans** yukarıdaki tabloda gösterildiği gibi.

## <a name="see-also"></a>Ayrıca bkz.

* [Hızlı Başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
