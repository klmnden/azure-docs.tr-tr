---
title: "Örnek Azure Hdınsight Hive tablolarındaki verileri | Microsoft Docs"
description: "Azure Hdınsight (Hadopop) Hive tablolarındaki verileri örnekleme aşağı"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 357307a034b277e8c37e99bda1ed6a9a76e13f41
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Azure HDInsight Hive tablolarındaki örnek veriler
Bu makalede, Azure Hdınsight Hive tabloları Hive sorgularını kullanarak depolanan veriler aşağı-sample değiştireceğinizi açıklar. Şu üç özellik kullanılan örnekleme yöntemleri kapsar:

* Tekdüzen rastgele örnekleme
* Gruplara göre rastgele örnekleme
* Stratified örnekleme

Aşağıdaki **menü** çeşitli depolama ortamlarından veri örneği nasıl açıklayan konulara bağlantılar.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Neden verilerinizi örnek?**
Analiz etmek için planlama dataset büyükse, genellikle aşağı örnek için daha küçük, ancak temsili ve daha kolay yönetilebilir bir boyutunu azaltmak için veri için iyi bir fikir değil. Bu, veri anlama, keşfi ve özellik Mühendisliği kolaylaştırır. Takım veri bilimi işleminde rolü hızlı prototipi oluşturulurken veri işleme işlevleri ve makine öğrenimi modellerinin oluşturulmasına etkinleştirmektir.

Bir adımda bu örnekleme görevdir [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Hive sorguları gönderme
Hive sorguları, Hadoop küme baş düğümü üzerinde Hadoop komut satırı konsolundan gönderilebilir. Bunu yapmak için Hadoop küme baş düğümüne oturum, Hadoop komut satırı konsolunu açın ve buradan Hive sorguları göndermek. Hadoop komut satırı konsolunda Hive sorguları gönderme ile ilgili yönergeler için bkz: [Hive sorguları göndermek için nasıl](move-hive-tables.md#submit).

## <a name="uniform"></a>Tekdüzen rastgele örnekleme
Tekdüzen rastgele örnekleme veri kümesindeki her satır örneklenen şansı eşittir sahip olduğu anlamına gelir. Bu, ek alan rand() veri kümesine iç sorgu "Seç" ve "Seç" dış sorgu bu koşul, rastgele alan üzerinde ekleyerek uygulanabilir.

Örnek bir sorgu şöyledir:

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

## <a name="group"></a>Gruplara göre rastgele örnekleme
Örnekleme kategorik verileri dahil etmek veya hariç tüm belirli bazı Kategorik bir değişkenin değerini örneklerinin istediğinizde. "Grubu tarafından örnekleme" tarafından amacı budur.
Örneğin, "Durum" Kategorik bir değişken varsa, NY, MA, CA, NJ, PA vb. değerleri olan, her zaman birlikte veya örneklenen olması aynı durumu kayıtları istersiniz.

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
Elde örnekleri değerleri olduğunda rastgele örnekleme kategorik, örnekler elde üst popülasyon olduğu gibi aynı oranı olduğundan göre Kategorik bir değişken stratified. Aynı örnek olarak kullanarak yukarıdaki verilerinizi alt yerleştirme tarafından durumda varsayalım, 100 gözlemleri NJ sahiptir, NY 60 gözlemleri ve WA 300 gözlemleri varsa söyleyin. 0,5 olarak stratified örnekleme oranını belirtirseniz, ardından elde örnek yaklaşık 50, 30 ve 150 gözlemleri NJ, NY ve WA sırasıyla olmalıdır.

Örnek bir sorgu şöyledir:

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

