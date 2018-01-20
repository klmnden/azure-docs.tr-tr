---
title: "Azure Hdınsight özel MapReduce programları - çalıştırma | Microsoft Docs"
description: "Ne zaman ve nasıl Hdınsight'ta özel MapReduce programları çalıştırın."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: 8e65c946d2cfcc830a1b9fa59b3f7886857f4f7d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="run-custom-mapreduce-programs"></a>Özel MapReduce programlarını çalıştırma

Hdınsight gibi Hadoop tabanlı büyük veri sistemlerini veri işleme çok çeşitli araçlar ve teknolojiler kullanarak etkinleştirin. Aşağıdaki tabloda ana avantajları ve her biri için konuları açıklanmaktadır.

| Sorgu mekanizması | Avantajları | Dikkat edilmesi gerekenler |
| --- | --- | --- |
| **Hive HiveQL kullanma** | <ul><li>Toplu işleme ve analizi, büyük miktarlarda değişmez veri, veri özetleme ve için mükemmel bir çözüm isteğe bağlı sorgulama üzerinde. Tanıdık SQL benzeri bir sözdizimi kullanır.</li><li>Kalıcı tabloları kolayca bölümlenmiş ve dizinlenmiş veri üretmek için kullanılabilir.</li><li>Birden çok dış tablolar ve görünümler aynı verilerin üzerine oluşturulabilir.</li><li>Veri depolama ve işleme için yoğun genişleme ve hataya dayanıklılık olanakları sağlayan bir basit veri ambarı uygulamasını destekler.</li></ul> | <ul><li>En az bazı tanımlanabilen yapısı için kaynak verilerini gerektirir.</li><li>Gerçek zamanlı sorgular ve satır düzeyinde güncelleştirmeler için uygun değil. Büyük veri kümelerine yönelik toplu işlemler için en iyi şekilde kullanılır.</li><li>Karmaşık işleme görevlerini bazı türleri gerçekleştirmek mümkün olmayabilir.</li></ul> |
| **Pig Latin kullanarak pig** | <ul><li>Mükemmel çözümünü ayarlar gibi veri düzenleme, birleştirme ve işlevleri kayıtları veya kayıt gruplarını uygulama veri kümeleri, filtreleme ve veri sütunları tanımlayarak, gruplandırma değerlere göre veya sütunları satırlara dönüştürerek yeniden yapılandırma.</li><li>İş akışı tabanlı bir yaklaşım, veri işlemlerinin bir dizisi olarak kullanabilirsiniz.</li></ul> | <ul><li>SQL kullanıcılar, Pig Latin daha az bilinen ve HiveQL kullanmayı daha zor fark edebilirsiniz.</li><li>Varsayılan çıkış genellikle bir metin dosyasıdır ve bu nedenle Excel gibi görselleştirme araçları ile kullanılacak daha zor olabilir. Genellikle bir Hive tablosu çıkışı üzerinden Katman.</li></ul> |
| **Özel Harita/azaltın** | <ul><li>Harita üzerinde tam denetim sağlar ve aşamaları ve yürütme azaltın.</li><li>Kümeden en yüksek performans elde etmek veya sunucuları ve ağ üzerindeki yükü en aza indirmek için en iyi duruma getirilmesi sorguları sağlar.</li><li>Bileşenleri iyi bilinen dilleri aralığında yazılabilir.</li></ul> | <ul><li>Kendi harita oluşturmak ve bileşenleri azaltmak için Pig veya Hive kullanmaktan çok daha zordur.</li><li>Veri kümelerini birleştirme gerektiren işlemler uygulamak daha zordur.</li><li>Olsa bile test çerçevelerini kullanılabilir, kodda hata ayıklama kod Hadoop İş Zamanlayıcısı denetimi altında bir toplu iş olarak çalıştığından normal uygulamadan daha daha karmaşıktır.</li></ul> |
| **HCatalog** | <ul><li>Yönetim kolaylaştıran ve verilerin depolandığı bilmeniz kullanıcıların ortadan kaldırarak depolama alanı yolu ayrıntılarını soyutlar.</li><li>Bildirim işlemleri oluştuğunda algılamak için Oozie gibi başka araçlar sağlayan veri kullanılabilirliği gibi olayların sağlar.</li><li>Veri bölümlendirme anahtarı tarafından dahil olmak üzere, ilişkisel bir görünümünü sunan ve veri erişimi kolay hale getirir.</li></ul> | <ul><li>RCFile, CSV metin destekler, JSON metni, SequenceFile ve ORC varsayılan dosya biçimleri, ancak diğer biçimleri için özel bir SerDe yazma gerekebilir.</li><li>HCatalog iş parçacığı açısından güvenli değil.</li><li>HCatalog yükleyicisi Pig betikleri kullanırken sütun veri türleri hakkında bazı kısıtlamalar vardır. Daha fazla bilgi için bkz: [HCatLoader veri türleri](http://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) Apache HCatalog belgelerinde.</li></ul> |

Genellikle, ihtiyaç duyduğunuz sonuçlar sağlayan bu yaklaşım basit kullanın. Örneğin, yalnızca Hive kullanarak bu tür sonuçları elde edebilirsiniz olabilir, ancak daha karmaşık senaryolar için Pig, kullanma ya da hatta kendi harita yazma ve düşürün gerekebilir. Ayrıca, Hive veya Pig, özel eşlenen denedikten sonra karar ve azaltmak bileşenleri ince ayar ve işleme en iyi duruma olanak sağlayarak daha iyi performans sağlayabilir.

## <a name="custom-mapreduce-components"></a>Özel Harita/azaltmak bileşenleri

Kod Haritası ve azaltmak oluşur olarak uygulanan iki ayrı işlevlerin **harita** ve **azaltmak** bileşenleri. **Harita** bileşen çalıştırıldığında paralel olarak birden çok küme düğümlerinde eşleme düğümün kendi veri alt kümesi için uygulama her düğüm. **Azaltmak** bileşeni harmanlar ve tüm harita işlevlerden sonuçları özetlenmektedir. Bu iki bileşenler hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop içinde kullanım MapReduce](hdinsight-use-mapreduce.md).

Çoğu Hdınsight işleme senaryoda bu daha basit ve Pig veya Hive gibi daha üst düzey bir soyutlama kullanmak için daha verimli olur. Özel Harita oluşturmak ve daha karmaşık bir işlem gerçekleştirmek için Hive komut içinde kullanmak için düşürün.

Özel Harita/azaltmak bileşenleri genellikle Java'da yazılır. Hadoop ayrıca C#, F #, Visual Basic, Python ve JavaScript gibi diğer dillerde geliştirilen kullanılacak bileşenleri tanır akış bir arabirim sağlar.

* Özel Java MapReduce programları geliştirme hakkında kılavuz için bkz: [hdınsight'ta Hadoop için Java MapReduce geliştirmek programlar](apache-hadoop-develop-deploy-java-mapreduce-linux.md).
* Python kullanarak bir örnek için bkz [MapReduce programları Hdınsight için akış Python geliştirme](apache-hadoop-streaming-python.md).

Kendi harita oluşturmayı düşünün ve aşağıdaki koşulları için düşürün:

* Veri ayrıştırma ve ondan yapılandırılmış bilgi almak için Özel mantık kullanarak tamamen yapılandırılmamış verileri işlemek gerekir.
* Zor (veya olanaksız) karmaşık görevleri gerçekleştirmek istediğiniz Pig içinde ifade etmek için veya Hive bir UDF oluşturmaya başvurmadan. Örneğin, coğrafi konum adları için enlem ve boylam koordinatları veya veri kaynağını IP adresleri dönüştürmek için bir dış coğrafi kodlama hizmeti kullanmanız gerekebilir.
* Akış arabirimi Hadoop kullanarak var olan .NET, Python veya JavaScript kodu eşleme ve azaltma bileşenleri yeniden kullanmak istiyorum.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Karşıya yükleme ve özel MapReduce programınızı çalıştırma

En yaygın MapReduce programları Java'da yazılmış ve jar dosyasına derlenir.

1. Geliştirilmiş derlenmiş ve MapReduce programınızı test sonra kullanın `scp` headnode jar dosyanızı karşıya yüklemek için komutu.

    ```bash
    scp mycustomprogram.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Değiştir **kullanıcıadı** kümeniz için SSH kullanıcı hesabıyla. Değiştir **CLUSTERNAME** küme adı ile. SSH hesabını güvenli hale getirmek için parola kullandıysanız parolayı girmeniz istenir. Bir sertifika kullanılması durumunda kullanmanız gerekebilir `-i` parametresini kullanarak özel anahtar dosyası belirtin.

2. Küme bağlamak için [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. SSH oturumundan YARN ile MapReduce programını çalıştırın.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Bu komut MapReduce işi YARN gönderir. Giriş dosyası `/example/data/sample.log`, ve çıktı dizinine `/example/data/logoutput`. Giriş dosyası ve tüm çıkış dosyaları için kümenin varsayılan depolama alanına depolanır.

## <a name="next-steps"></a>Sonraki adımlar

* [C# MapReduce hdınsight'ta Hadoop akış ile kullanma](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Hdınsight'ta Hadoop için Java MapReduce programlar geliştirmek](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [MapReduce programları Hdınsight için akış Python geliştirme](apache-hadoop-streaming-python.md)
* [Eclipse için Azure Araç Seti Spark Hdınsight kümesi için uygulamalar oluşturmak için kullanın](../spark/apache-spark-eclipse-tool-plugin.md)
* [Hdınsight'ta kullanım Python kullanıcı tanımlı işlevler (UDF) Hive veya Pig ile](python-udf-hdinsight.md)
