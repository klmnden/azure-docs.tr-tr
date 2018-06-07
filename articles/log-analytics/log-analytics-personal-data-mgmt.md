---
title: Azure günlük analizi depolanan kişisel veriler için Kılavuzu | Microsoft Docs
description: Bu makalede, kişisel verileri tanımlamak ve kaldırmak için Azure günlük analizi ve yöntemleri depolanan yönetmek açıklar.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: magoedte
ms.openlocfilehash: 056779943d05ca743db63f1bc91be058cfae7b30
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655587"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics"></a>Günlük analizi depolanan kişisel veriler için yönergeler

Günlük analizi, kişisel verileri bulunma olasılığı olduğu bir veri deposudur. Bu makalede, burada günlük analizi bu tür veriler genellikle, bu tür veriler işlemek için özellikleri yanı sıra bulunan ele alınacaktır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Kişisel veri işleme için stratejisi

Bu, şirketinizin en fazla ve sonuçta ile özel işleyecek stratejisini belirlemek için veriler (varsa) olacaktır olsa da, bazı olası yaklaşım verilmiştir. Bir teknik açısından çoğu gelen tercih sırasına göre listelenen az tercih için:

* Mümkünse, koleksiyonunu durdurmak, belirsizleştirirseniz, Anonimleştir veya aksi takdirde "özel" kabul dışlanacak toplanmakta olan verileri ayarlayın. Bu _kullanarak_ stratejisi işleme çok pahalı ve etkili bir veri oluşturmak için gereken kaydetme tercih edilen yaklaşım.
* Bir veri platformu ve performans üzerindeki etkisini azaltmak için verileri normalleştirmek değil, mümkün olduğunda, çalışır. Örneğin, açık bir kullanıcı kimliği günlüğü yerine, kullanıcı adı ve ardından başka bir yerde günlüğe kaydedilebilir bir iç kimlik ayrıntıları ilişkilendirmek arama verileri oluşturun. Bu şekilde, kullanıcılarınızdan biri, kendi kişisel bilgilerini silmek için istemelisiniz yalnızca kullanıcıya karşılık gelen arama tablosunda satırın silinmesi yeterli olması mümkündür. 
* Son olarak, özel veri toplanması gereken, temizleme API yol ve dışa aktarma ve bir kullanıcıyla ilişkili herhangi bir özel veri silme geçici olabilir yükümlülüklerin karşılamak için varolan sorgu API yol geçici bir işlem oluşturun. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Günlük analizi özel veri arayın nerede?

Günlük analizi, verilerinizi şemaya prescribing sırasında özel değerlere sahip her bir alan geçersiz kılma olanak tanıyan esnek bir mağaza ' dir. Ayrıca, herhangi bir özel şema alınan. Bu nedenle, tam olarak burada özel veri belirli çalışma alanınızda bulunacaktır söylemek mümkün değildir. Aşağıdaki konumlardan ancak iyi envanterinize başlangıç noktaları:

* *IP adreslerini*: günlük analizi birçok farklı tablolar arasında IP bilgilerini, çeşitli toplar. Örneğin, aşağıdaki sorgu, IPv4 adresleri son 24 saat boyunca toplanan burada tüm tabloları gösterir:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Kullanıcı kimliklerini*: kullanıcı kimlikleri büyük çeşitli çözümler ve tablolar içinde bulundu. Belirli bir kullanıcı adı için Ara komutunu kullanarak, tüm veri kümesinin arayabilirsiniz:
    ```
    search "[username goes here]"
    ```
Yalnızca okunabilir kullanıcı adları aynı zamanda doğrudan geri belirli bir kullanıcıya izlenebilir GUID'leri için aranacak unutmayın!
* *Cihaz kimlikleri*: gibi "kullanıcı kimlikleri, cihaz kimlikleri bazen özel" olarak kabul edilir. Aynı yaklaşımı tablolarını tanımlamak için kullanıcı kimlikleri için yukarıdaki gibi kullanın burada bu ilgili bir sorun olabilir. 
* *Özel veri*: günlük analizi çeşitli yöntemler koleksiyonunda sağlar: özel günlükler ve özel alanlar, [HTTP veri toplayıcı API](log-analytics-data-collector-api.md) , ve sistem olay günlüklerini bir parçası olarak toplanan özel veriler. Bunların tümü özel veri içeren açıktır ve bu tür veriler var olup olmadığını doğrulamak için incelenmelidir.
* *Çözüm Yakalanan veriler*: uçlu bir çözüm mekanizması olduğu için uyumluluğu sağlamak için çözümler tarafından oluşturulan tüm tabloların incelemeniz önerilir.

## <a name="how-to-export-and-delete-private-data"></a>Dışarı aktarma ve özel verileri silme

Bölümünde belirtildiği gibi [kişisel veri işleme için stratejisi](#strategy-for-personal-data-handling) bölümüne, __kesinlikle__ if koleksiyonunu devre dışı bırakmak için veri toplama İlkesi yapılandırılacak olası tüm, önerilir Aksi takdirde "özel" kabul kaldırmak için değiştirme obfuscating veya onu anonymizing özel veriler. Sizin ve ekibinizin tanımlamak ve bir strateji otomatikleştirmek müşterilerinizin verilerine etkileşim için bir arabirim oluşturmak için maliyetleri ve devam eden bakım maliyetlerinin veri en önemli sonuç işleme. Ayrıca, günlük analizi ve eş zamanlı sorgu büyük miktarda pkı'ya maliyetli olabilir veya temizleme API çağrıları günlük analizi işlevselliği ile tüm diğer etkileşimi olumsuz olanağına sahip. Başka bir deyişle, burada özel veri gerekir toplanmasını geçerli gerçekten bazı senaryolar vardır. Bu durumlarda, bu bölümde açıklandığı gibi veri yapılmalıdır.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Görünüm ve dışarı aktarma

Her ikisi de görüntüleyebilir ve veri istekleri, dışarı aktarma için [sorgu API](https://dev.loganalytics.io/) kullanılmalıdır. Kullanıcılarınıza sunmak için uygun bir veri şekli dönüştürmek için mantığı uygulamak size olacaktır. [Azure işlevleri](https://azure.microsoft.com/services/functions/) böyle bir mantık barındırmak için harika bir yer sağlar.

### <a name="delete"></a>Sil

> [!WARNING]
> Günlük analizi siler bozucu ve çevrilemez! Lütfen bunların yürütme dikkatli.

Biz kullanılabilir hale gizlilik işleme bir parçası olarak bir *Temizleme* API yolu. Bu yol, böylece ile ilişkili riski nedeniyle olabildiğince az kullanılmalıdır olası performans etkisini ve tüm yukarı toplamalar, ölçümleri ve günlük analizi verilerinizin diğer yönlerini eğme olma olasılığı. Bkz: [kişisel veri işleme için stratejisi](#strategy-for-personal-data-handling) özel verileri işlemek alternatif yaklaşımlar bölümü.

Temizleme, hiçbir kullanıcı veya (bile kaynak sahibi dahil olmak üzere) açıkça alınmadan verilen Azure Kaynak Yöneticisi'ndeki rol yürütme iznine sahip olduğu yüksek ayrıcalıklı bir işlemdir. Bu rol _veri Purger_ ve olası veri kaybını nedeniyle dikkatli temsilci. 

Azure Resource Manager rol atandıktan sonra iki yeni API yolları kullanılabilir: 

* [Temizleme sonrası] (https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - silmek için veri parametrelerini belirten nesnesini alır ve bir başvuru GUID döndürür 
* GET Temizle durumu - POST temizleme çağrısı temizleme API'nizi durumunu belirlemek için arayabileceğiniz bir URL içeren bir 'x-ms-durum-location' üstbilgisi döndürür. Örneğin:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

Temizleme işlemleri aşırı günlük analizi tarafından kullanılan veri platformu etkilerini nedeniyle bizim SLA'den çok daha hızlı tamamlamak çoğunluğu bekliyoruz sırada resmi 30 günde SLA temizleme işlemlerinin tamamlanması için ayarlanır. 

## <a name="next-steps"></a>Sonraki adımlar
Verileri nasıl toplanan, işlenen ve güvenliği hakkında daha fazla bilgi için bkz: [günlük analizi veri güvenliği](log-analytics-data-security.md).