---
title: Özel MapReduce programlarını - Azure HDInsight çalıştırma
description: Ne zaman ve nasıl HDInsight içinde özel MapReduce programlarını çalıştırma.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: 5ed82fc21aedc9af394922059859f81cfba1867e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713096"
---
# <a name="run-custom-mapreduce-programs"></a>Özel MapReduce programlarını çalıştırma

Apache Hadoop tabanlı büyük veri sistemlerini HDInsight gibi çok çeşitli araçlar ve teknolojiler kullanarak veri işleme etkinleştirin. Aşağıdaki tabloda ana avantajları ve her biri için ilgili önemli noktalar açıklanmaktadır.

| Sorgu mekanizması | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- |
| **HiveQL kullanarak Apache Hive** | <ul><li>Toplu işleme ve analizi, büyük miktarda sabit veri, veri özetleme ve için mükemmel bir çözüm üzerinde sorgulama isteğe bağlı. Tanıdık SQL benzeri bir sözdizimi kullanır.</li><li>Kalıcı kolayca bölümlenmiş ve dizine veri tabloları oluşturmak için kullanılabilir.</li><li>Birden çok dış tablolar ve görünümler, aynı veriler üzerinde oluşturulabilir.</li><li>Bu, veri depolama ve işleme için büyük ölçeklendirme ve dayanıklılık özellikleri sağlayan basit bir veri ambarı uygulaması destekler.</li></ul> | <ul><li>Kaynak veri en azından bazı tanımlanabilen yapısı gerektirir.</li><li>Gerçek zamanlı sorgular ve satır düzeyinde güncelleştirmeler için uygun değil. Büyük veri kümelerinde toplu işlemler için en iyi şekilde kullanılır.</li><li>Karmaşık bir işlem görevlerin bazı türleri gerçekleştirmek mümkün olmayabilir.</li></ul> |
| **Apache Pig Latin kullanarak Pig** | <ul><li>Mükemmel çözümünü ayarlar gibi veri işleme, birleştirme ve veri kümeleri kayıtları veya kayıt gruplarını işlevleri uygularken, filtreleme ve veri sütunları tanımlayarak, gruplama değerleri veya sütunları satırlara dönüştürerek alanlarını yeniden yapılandırma.</li><li>İş akışı tabanlı bir yaklaşım, veriler üzerinde işlem öğesinin bir dizisi olarak kullanabilirsiniz.</li></ul> | <ul><li>SQL kullanıcıları, Pig Latin daha az bildiğiniz ve HiveQL kullanmayı daha zor bulabilirsiniz.</li><li>Varsayılan çıkış, genellikle bir metin dosyasıdır ve bu nedenle Excel gibi görselleştirme araçlarıyla kullanmak daha zor olabilir. Genellikle bir Hive tablosu çıkışı üzerinden Katman.</li></ul> |
| **Özel Harita/azaltın** | <ul><li>Harita üzerinde tam kontrol sağlar ve aşamaları ve yürütme azaltın.</li><li>Kümeden gelen en yüksek performansı elde etmek için ya da sunucuları ve ağ üzerindeki yükü en aza indirmek için en iyi duruma getirilmesi sorguları sağlar.</li><li>Bileşenleri iyi bilinen dilleri aralığındaki yazılabilir.</li></ul> | <ul><li>Kendi eşlemesi oluşturma ve bileşenleri azaltmak için Pig veya Hive kullanmaktan daha zordur.</li><li>Veri kümelerini katılma gerektiren işlemler uygulamak daha zordur.</li><li>Olsa bile test çerçevelerini kullanılabilir, kodda hata ayıklama kodu Hadoop işi Zamanlayıcı denetimi altında bir toplu iş olarak çalıştığı için normal bir uygulama çok daha karmaşıktır.</li></ul> |
| **Apache HCatalog** | <ul><li>Bu yönetim kolaylaştırmaya ve kullanıcıların verileri nerede depolandığından haberdar olma gereksinimini kaldırma depolama yolu ayrıntılarını soyutlar.</li><li>Bu verme işlemlerini gerçekleştiğinde algılamak için Oozie gibi diğer araçları, veri kullanılabilirliği gibi olayların bildirimini sağlar.</li><li>Veri bölümlendirme anahtarı tarafından da dahil olmak üzere, ilişkisel bir görünümünü sunar ve veri erişimi kolay hale getirir.</li></ul> | <ul><li>RCFile, CSV metin destekler, JSON metin, SequenceFile ve ORC dosya biçimleri varsayılan olarak, ancak diğer biçimleri için özel bir SerDe yazma gerekebilir.</li><li>HCatalog iş parçacığı açısından güvenli değildir.</li><li>HCatalog yükleyici Pig betikleri kullanıldığında sütunların veri türlerini bazı kısıtlamalar vardır. Daha fazla bilgi için [HCatLoader veri türleri](https://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) Apache HCatalog belgelerinde.</li></ul> |

Genellikle, basit ihtiyaç duyduğunuz sonuçlar sağlayan bu yaklaşımların kullanırsınız. Örneğin, yalnızca Hive'ı kullanarak bu tür sonuçları elde etmek mümkün olabilir, ancak daha karmaşık senaryolarda, Pig, kullanma veya hatta kendi eşleme yazma ve altına düşürün gerekebilir. Ayrıca, Hive veya Pig, bu özel eşleme ile denemeler sonrasında karar ve azaltmak bileşenleri ince ayar yapma ve işleme iyileştirmenize olanak sağlayarak daha iyi performans sağlayabilir.

## <a name="custom-mapreduce-components"></a>Özel Harita/azaltmak bileşenleri

Kod Haritası ve azaltmak oluşur olarak uygulanan iki ayrı işlevlerin **harita** ve **azaltmak** bileşenleri. **Harita** bileşen çalıştırılır paralel olarak birden çok küme düğümlerinde eşleme verileri düğümün kendi alt kümesine uygulama her düğüm. **Azaltmak** bileşeni harmanlar ve tüm eşlemesi işlevleri sonuçları özetler. Bu iki bileşenin hakkında daha fazla bilgi için bkz. [, HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md).

Çoğu HDInsight işleme senaryolarda bu daha basit ve Pig veya Hive gibi daha üst düzey bir soyutlama daha verimli olur. Özel eşleme oluşturun ve daha karmaşık bir işlem gerçekleştirmek için Hive betiklerini içinde kullanmak için altına düşürün.

Özel map/azaltmak bileşenler genellikle Java dilinde yazılır. Hadoop gibi diğer dillerde geliştirilen kullanılacak bileşenler de sağlayan bir akış arabirimi sağlayan C#, F#, Visual Basic, Python ve JavaScript.

* Özel Java MapReduce programları geliştirme hakkında kılavuz için bkz. [geliştirme Java MapReduce programları HDInsight üzerinde Hadoop için](apache-hadoop-develop-deploy-java-mapreduce-linux.md).

Kendi eşleme oluşturmayı göz önünde bulundurun ve aşağıdaki koşulları için altına düşürün:

* Veri ayrıştırma ve yapılandırılmış bilgiler elde edileceği özel mantığı kullanılarak tamamen yapılandırılmamış verileri işlemek gerekir.
* Zor (ya da olanaksız) karmaşık görevler gerçekleştirmek istediğiniz Pig ifade veya Hive bir UDF oluşturmaya başvurmadan. Örneğin, enlem ve boylam koordinatları veya kaynak verilerde IP adresleri için coğrafi konum adlarını dönüştürmek için bir dış coğrafi kodlama hizmeti kullanmanız gerekebilir.
* Mevcut .NET, Python veya JavaScript kod eşleme ve azaltma bileşenlerinde arabirimi akış Hadoop kullanarak yeniden kullanmak istiyorum.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Özel MapReduce programını çalıştırın ve karşıya yükleme

En yaygın MapReduce programlarını Java dilinde yazılmış ve bir jar dosyasına derlenir.

1. Geliştirilen, derlenmiş ve MapReduce programınızı test sonra kullanın `scp` baş düğüme jar dosyanızı karşıya yüklemek için komutu.

    ```bash
    scp mycustomprogram.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Değiştirin **kullanıcıadı** kümenizin SSH kullanıcı hesabı ile. Değiştirin **CLUSTERNAME** küme adı ile. SSH hesabının güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir sertifika kullanıyorsanız, kullanmanız gerekebilir `-i` parametresini kullanarak özel anahtar dosyası belirtin.

2. Kullanarak kümeye bağlanma [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. SSH oturumundan YARN ile MapReduce programını yürütün.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Bu komut YARN MapReduce işi gönderir. Giriş dosyası `/example/data/sample.log`, ve çıkış dizini `/example/data/logoutput`. Giriş dosyası ve herhangi bir çıktı dosyalarını, kümenin varsayılan depolama depolanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanım C# üzerindeki HDInsight, Apache Hadoop akışı MapReduce ile](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [HDInsight üzerinde Apache Hadoop için Java MapReduce programları geliştirme](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Seti'ni kullanma](../spark/apache-spark-eclipse-tool-plugin.md)
* [Apache Hive ve Apache Pig, HDInsight ile kullanmak Python kullanıcı tanımlı işlevler (UDF)](python-udf-hdinsight.md)
