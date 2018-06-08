---
title: Örnek Azure Hdınsight Hive tablolarındaki verileri | Microsoft Docs
description: Azure Hdınsight (Hadopop) Hive tablolarındaki verileri örnekleme aşağı
services: machine-learning,hdinsight
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 558f7d684453c8b5040f586820bd2a8a9ac0f9c8
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838441"
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Azure HDInsight Hive tablolarındaki örnek veriler
Bu makalede aşağı örnekli analiz için daha kolay yönetilebilir bir boyutunu küçültmek için Hive sorguları kullanarak Azure Hdınsight Hive tabloları depolanan verileri açıklar. Bu üç özellik kullanılan örnekleme yöntemleri kapsar:

* Tekdüzen rastgele örnekleme
* Gruplara göre rastgele örnekleme
* Stratified örnekleme

Aşağıdaki **menü** çeşitli depolama ortamlarından veri örneği nasıl açıklayan konulara bağlantılar.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Neden verilerinizi örnek?**
Analiz etmek için planlama dataset büyükse, genellikle aşağı örnek için daha küçük, ancak temsili ve daha kolay yönetilebilir bir boyutunu azaltmak için veri için iyi bir fikir değil. Aşağı örnekleme veri anlama, keşfi ve özellik Mühendisliği kolaylaştırır. Takım veri bilimi işleminde rolü hızlı prototipi oluşturulurken veri işleme işlevleri ve makine öğrenimi modellerinin oluşturulmasına etkinleştirmektir.

Bir adımda bu örnekleme görevdir [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Hive sorguları gönderme
Hive sorguları, Hadoop küme baş düğümü üzerinde Hadoop komut satırı konsolundan gönderilebilir. Bunu yapmak için Hadoop küme baş düğümüne oturum, Hadoop komut satırı konsolunu açın ve buradan Hive sorguları göndermek. Hadoop komut satırı konsolunda Hive sorguları gönderme ile ilgili yönergeler için bkz: [Hive sorguları göndermek için nasıl](move-hive-tables.md#submit).

## <a name="uniform"></a> Tekdüzen rastgele örnekleme
Tekdüzen rastgele örnekleme veri kümesindeki her satır örneklenen şansı eşittir sahip olduğu anlamına gelir. Bir ek alan rand() veri kümesine iç sorgu "Seç" ve "Seç" dış sorgu bu koşul, rastgele alan üzerinde ekleyerek uygulanabilir.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Burada, `<sample rate, 0-1>` kullanıcıları örneklemek istediğiniz kayıtları oranını belirtir.

## <a name="group"></a> Gruplara göre rastgele örnekleme
Örnekleme kategorik verileri dahil etmek veya hariç tüm örnekleri için bazı değişkenin değeri olarak kategorik istediğinizde. Bu tür bir örnekleme "grubu tarafından örnekleme" adı verilir. Kategorik bir değişken varsa, örneğin, "*durumu*" NY, MA, CA, NJ ve PA gibi değerler vardır, kayıtların her durumundan veya örneklenen birlikte olmasını istediğiniz.

Grup tarafından bu örnekleri örnek bir sorgu şöyledir:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratified örnekleme
Elde edilen örnekleri üst popülasyonun oldukları gibi aynı oranı var olan kategorik değerlere sahip olduğunda rastgele örnekleme göre Kategorik bir değişken stratified. Yukarıdaki, aynı örneği kullanarak varsayalım verilerinizi durumları tarafından aşağıdaki gözlemleri vardır: NJ 100 gözlemleri, NY sahip 60 gözlemleri ve WA varsa 300 gözlemleri. 0,5 olarak stratified örnekleme oranını belirtirseniz, ardından elde örnek yaklaşık 50, 30 ve 150 gözlemleri NJ, NY ve WA sırasıyla olmalıdır.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Kovanında kullanılabilir daha gelişmiş örnekleme yöntemleri hakkında daha fazla bilgi için bkz: [LanguageManual örnekleme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

