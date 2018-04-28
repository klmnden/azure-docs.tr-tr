---
title: Caffe Azure Hdınsight Spark üzerinde dağıtılmış derin learning için kullanın. | Microsoft Docs
description: Caffe Azure Hdınsight Spark üzerinde dağıtılmış derin learning için kullanın.
services: hdinsight
documentationcenter: ''
author: xiaoyongzhu
manager: asadk
editor: cgronlun
tags: azure-portal
ms.assetid: 71dcd1ad-4cad-47ad-8a9d-dcb7fa3c2ff9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/17/2017
ms.author: xiaoyzhu
ms.openlocfilehash: 27ce89f205efa6b8f2d29e034c6e5002065879fc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Caffe Azure Hdınsight Spark üzerinde dağıtılmış derin learning için kullanın.


## <a name="introduction"></a>Giriş

Derin öğrenme, her şeyi üretim taşıma sağlık ve daha fazlasını etkileyen. Şirketler gibi sabit sorunları çözmek öğrenme derin kapatma [görüntü sınıflandırma](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [konuşma tanıma](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)nesne tanıma ve makine çevirisi. 

Vardır [birçok popüler uygulamayı](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software)dahil [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, vs. Caffe En ünlü simgesel olmayan (kesinlik temelli) sinir ağı çerçeveleri biridir ve bilgisayar görme dahil birçok alanda yaygın olarak kullanılır. Ayrıca, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) birleştirir; bu durumda derin öğrenme Apache Spark Caffe var olan bir Hadoop kümesine üzerinde kolayca da kullanılabilir. Eksiksiz bir çözüm learning için derin öğrenme Spark ETL ardışık düzen, azalan sistem karmaşıklık ve gecikme süresi ile birlikte kullanabilirsiniz.

[Hdınsight](https://azure.microsoft.com/services/hdinsight/) olan bir bulutta Hadoop sunumu Spark, Hive, Hadoop, HBase, Storm, Kafka ve R Server için en iyi duruma getirilmiş açık kaynak analitik kümeleri sağlar. Hdınsight % 99,9 SLA ile yedeklenir. Bu büyük veri teknolojileri ve ISV uygulamaların her güvenlik ve kuruluşlar için izleme yönetilen kümeleriyle kolayca dağıtılabilir.

Bu makalede nasıl yükleneceği gösterilmektedir [Spark üzerinde Caffe](https://github.com/yahoo/CaffeOnSpark) Hdınsight kümesi için. Bu makalede yerleşik MNIST demo nasıl derin CPU'larda Hdınsight Spark kullanarak öğrenme dağıtılmış kullanılacağını göstermek için de kullanır.

Görevi gerçekleştirmek için dört adım vardır:

1. Gereken bağımlılıklardan tüm düğümlere yükleyin
2. Baş düğümünde Hdınsight için Spark üzerinde Caffe derleme
3. Tüm çalışan düğümleri için gerekli kitaplıklar Dağıt
4. Caffe model oluşturmak ve dağıtılmış bir şekilde çalıştırın.

Hdınsight bir PaaS çözümü olduğundan, bazı görevleri gerçekleştirmek kolay olması için kullanışlı bir platform özellikleri - sunar. Bu blog gönderisinde kullanılan özellikler çağrılır [betik eylemi](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), hangi, yürütebilir küme düğümleri (baş düğüm, alt düğüm veya kenar düğümüne) özelleştirmek için Kabuk komutları.

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>1. adım: gerekli bağımlılıkları tüm düğümlere yükleyin.

Başlamak için bağımlılıkları yüklemeniz gerekir. Caffe site ve [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) bağımlılıkları için Spark YARN modunu yüklemeye yönelik bazı kullanışlı wiki sunar. Hdınsight Spark YARN modunu de kullanır. Ancak, Hdınsight platform için birkaç daha fazla bağımlılıkları eklemeniz gerekir. Bunu yapmak için bir betik eylemi kullanın ve tüm baş düğümler ve çalışan düğümleri üzerinde çalıştırın. Bu bağımlılıklar ayrıca diğer paketlere bağımlı gibi bu betik eylemi yaklaşık 20 dakika sürer. GitHub konum veya varsayılan BLOB storage hesabı gibi Hdınsight kümenize erişilebilen bazı konumda girmelisiniz.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose you install them on all the nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


Betik eylemi iki adımı vardır. İlk adım, tüm gerekli kitaplıkları yüklemektir. Bu kitaplıklar hem Caffe (gflags aracına gibi glog) derleme ve Caffe (numpy gibi) çalıştırmak için gerekli kitaplıkları içerir. libatlas CPU iyileştirme için kullanmakta olduğunuz ancak MKL veya (GPU için) CUDA gibi diğer en iyi duruma getirme kitaplıkları yükleme her zaman CaffeOnSpark wiki izleyebilirsiniz.

İkinci adım, derleme, yükleyip protobuf 2.5.0 Caffe için çalışma zamanı sırasında almaktır. Protobuf 2.5.0 [gereklidir](https://github.com/yahoo/CaffeOnSpark/issues/87), ancak kaynak kodu derleme gerekir böylece bu sürümü Ubuntu 16 üzerinde bir paketi olarak kullanılabilir değildir. Ayrıca birkaç kaynak yok Internet'te derlemeniz nasıl. Daha fazla bilgi için bkz: [burada](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html).

Başlamak için yalnızca bu betik eylemi kümenizi karşı tüm çalışan düğümleri ve baş düğümler için (Hdınsight 3.5 için) çalıştırabilirsiniz. Betik eylemleri olan bir kümede çalıştırabilir veya küme oluşturma sırasında betik eylemleri kullanın. Betik eylemleri hakkında daha fazla bilgi için belgelere bakın [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions).

![Bağımlılıklar yüklemek için betik eylemleri](./media/apache-spark-deep-learning-caffe/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a>2. adım: Caffe Hdınsight için Spark baş düğümünde oluşturmak

İkinci Caffe üzerinde headnode oluşturmak ve tüm çalışan düğümlerinin derlenmiş kitaplıklara dağıtmak için bir adımdır. Bu adımda, şunları yapmalısınız [ssh, headnode içine](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix). Bundan sonra izlemeniz gereken [CaffeOnSpark yapı işlemi](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn). Aşağıda birkaç ek adımlarla CaffeOnSpark oluşturmak için kullanabileceğiniz komut dosyası yer almaktadır. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Ne CaffeOnSpark belgeleri diyor birden fazla yapmanız gerekebilir. Değişiklikleri şunlardır:
- CPU-yalnızca değiştirin ve belirli bu amaç için libatlas kullanın.
- Veri kümeleri, daha sonra kullanmak için tüm çalışan düğümlerinin erişebileceği paylaşılan bir konum BLOB Depolama birimine yerleştirin.
- BLOB depolama alanına derlenmiş Caffe kitaplıklarının koyun ve daha sonra bu kitaplıkları ek derleme süresini önlemek için betik eylemleri kullanılarak tüm düğümlere kopyalayın.


### <a name="troubleshooting-an-ant-buildexception-has-occurred-exec-returned-2"></a>Sorun giderme: Ant BuildException oluştu: döndürülen exec: 2

İlk CaffeOnSpark oluşturmaya çalışırken, bazen yazacaktır

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Kod depo tarafından temizleme "temiz make" ve ardından "yapı çalışma yapma" doğru bağımlılıklara sahip olduğu sürece bu sorunu çözmek için.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Sorunlarını giderme: Maven depo bağlantı zaman aşımı

Bazen maven bağlantı zaman aşımı hatası, aşağıdaki kod parçacığını benzer sağlar:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Birkaç dakika sonra yeniden denemeniz gerekir.


### <a name="troubleshooting-test-failure-for-caffe"></a>Sorun giderme: hata Caffe için test etme

CaffeOnSpark son denetleme yaparken büyük olasılıkla bir sınama hatası bakın. Bu büyük olasılıkla UTF-8 kodlamasını ile ilişkilidir, ancak Caffe kullanımını etkileyen değil

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>3. adım: tüm çalışan düğümleri için gerekli kitaplıklar Dağıt

Sonraki adım kitaplıkları dağıtmaktır (temelde CaffeOnSpark/caffe-genel/dağıtmak/lib kitaplıklarında/ve CaffeOnSpark/caffe-atandığında/dağıtmak/lib /) tüm düğümlere. Adım 2'de, bu kitaplıkları BLOB Depolama alanında koyabilir ve bu adımda, tüm baş düğümler ve çalışan düğümleri kopyalamak için betik eylemleri kullanın.

Bunu yapmak için aşağıdaki kod parçacığında gösterildiği gibi bir betik eylemi çalıştırın:

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Doğru konuma noktasına kümenize belirli ihtiyacı olmadığından emin olun)

2. adımda, tüm düğümler için erişilebilir olan BLOB Depolama yerleştirdiğiniz çünkü bu adımda, yalnızca tüm düğümlere kopyalayın.

## <a name="step-4-compose-a-caffe-model-and-run-it-in-a-distributed-manner"></a>4. adım: bir Caffe model oluşturmak ve dağıtılmış bir şekilde çalıştırın

Yukarıdaki adımları çalıştırdıktan sonra Caffe yüklenir. Sonraki adım, bir Caffe modeli yazmaktır. 

Caffe "Burada bir model oluşturmak için bir yapılandırma dosyası tanımlamak için yeterlidir bir açıklayıcı mimari", kullanma ve olmadan hiç (çoğu durumda) kodlama. Bu nedenle gelin var. bir göz atalım. 

Eğittiğiniz modeli MNIST eğitim için örnek modelidir. El yazısı basamak MNIST veritabanı 60.000 örnekler Eğitim kümesi ve bir sınama kümesi 10.000 örnekler vardır. NIST kullanılabilir daha büyük bir alt kümesidir. Rakamları boyutu normalleştirilmiş ve sabit boyutlu bir resmi ortalanmış olmuştur. CaffeOnSpark dataset indirmek ve doğru biçime dönüştürmek için bazı betikler yok.

CaffeOnSpark MNIST eğitim için bazı ağ topolojileri örnek sağlar. En iyi duruma getirme ve ağ mimarisini (ağ topolojisi) bölme iyi bir tasarıma sahiptir. Bu durumda, gerekli iki dosya vardır: 

"Solver" dosyası (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) en iyi duruma getirme gözlemledikten ve parametre güncelleştirmeleri oluşturmak için kullanılır. Örneğin, CPU veya GPU, kaç yineleme olan satışlarının nedir mı kullanıldığını tanımlar vs. Ayrıca, hangi neuron ağ topolojisi'nin (ihtiyacınız ikinci dosyası olan) programın kullanması gereken tanımlar. Çözücü hakkında daha fazla bilgi için bkz: [Caffe belgelerine](http://caffe.berkeleyvision.org/tutorial/solver.html).

Bu örnekte, GPU yerine CPU kullandığından son satırın değiştirmeniz gerekir:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe yapılandırma](./media/apache-spark-deep-learning-caffe/Caffe-1.png)

Diğer satırlar gerektiği gibi değiştirebilirsiniz.

İkinci dosyası (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) nasıl neuron ağ, benzer ve ilgili girdi ve çıktı dosyası tanımlar. Ayrıca dosyanın eğitim veri konumu yansıtacak şekilde güncelleştirmeniz gerekir. (Kümenize belirli doğru konuma işaret etmeniz) lenet_memory_train_test.prototxt aşağıdaki bölümünde değiştirin:

- "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" değiştirmek "wasb: / / / projeleri/machine_learning/image_dataset/mnist_train_lmdb"
- "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" değiştirmek "wasb: / / / projeleri/machine_learning/image_dataset/mnist_test_lmdb"

![Caffe yapılandırma](./media/apache-spark-deep-learning-caffe/Caffe-2.png)

Ağ tanımlama hakkında daha fazla bilgi için kontrol [MNIST veri kümesi üzerinde Caffe belgeleri](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Bu makalede amacıyla bu MNIST örneği kullanın. Baş düğümünden aşağıdaki komutları çalıştırın:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

Yukarıdaki komut, gerekli dosyaları (lenet_memory_solver.prototxt ve lenet_memory_train_test.prototxt) her YARN kapsayıcıya dağıtır. Komut ayrıca LD_LIBRARY_PATH için her Spark sürücü/Yürütücü ilgili YOLUNU ayarlar. LD_LIBRARY_PATH CaffeOnSpark kitaplıkların bulunduğu konuma noktaları ve önceki kod parçacığını tanımlanır. 

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

YARN küme modu kullandığından, Spark sürücüsü rasgele bir kapsayıcı (ve rasgele çalışan düğümüne) zamanlanacak durumda yalnızca aşağıdakine benzer çıktısı konsolunda görmeniz gerekir:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Neler olduğunu bilmek istiyorsanız, genellikle daha fazla bilgiye sahip sürücünün günlük Spark almanız gerekir. Bu durumda, ilgili YARN günlüklerini bulmak için YARN UI gitmeniz gerekir. Bu URL tarafından YARN kullanıcı Arabiriminde alabilirsiniz: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN KULLANICI ARABİRİMİ](./media/apache-spark-deep-learning-caffe/YARN-UI-1.png)

Bu belirli bir uygulama için kaç tane kaynaklar bir göz atalım. "Zamanlayıcı" bağlantısını tıklatın ve sonra bu uygulama için olduğunu çalıştıran dokuz kapsayıcıları görürsünüz. sekiz yürütücüler sağlamak için YARN isteyin ve sürücü işlemi için başka bir kapsayıcıdır. 

![YARN Zamanlayıcı](./media/apache-spark-deep-learning-caffe/YARN-Scheduler.png)

Hata varsa sürücüsü günlükleri veya kapsayıcı günlükleri denetlemek isteyebilirsiniz. Sürücüsü günlükleri için YARN kullanıcı arabiriminde uygulama kimliği'ni tıklatın, ardından "Günlükleri" düğmesini tıklatın. Sürücü günlükleri stderr yazılır.

![YARN KULLANICI ARABİRİMİNDE 2](./media/apache-spark-deep-learning-caffe/YARN-UI-2.png)

Örneğin, bazı sürücü günlükleri, hata çok fazla yürütücüler tahsis belirten görebilirsiniz.

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

Bazı durumlarda, sorunu sürücüleri yerine yürütücüler oluşabilir. Bu durumda, kapsayıcı günlükleri denetlemeniz gerekir. Her zaman kapsayıcı günlükleri alın ve başarısız kapsayıcı alın. Örneğin, Caffe çalıştırırken bu hatayı karşılamayabilir.

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

Bu durumda, başarısız kapsayıcı kimliği almanız gerekir (Yukarıdaki durumda bu, container_1485916338528_0008_05_000005'dir). Ardından çalıştırmanız gerekir 

    yarn logs -containerId container_1485916338528_0008_03_000005

headnode. Kapsayıcı hatası denetledikten sonra (burada, CPU modunu kullanmanız) GPU modunu kullanarak içinde lenet_memory_solver.prototxt neden olur.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Sonuçlar alınıyor

8 yürütücüler ayırma ve ağ topolojisini basit olduğundan, yaklaşık 30 dakika sonucunu çalıştırmak için yalnızca almanız gerekir. Komut satırından wasb:///mnist.model için model yerleştirin ve sonuçları wasb adlı bir klasöre yerleştirin görebilirsiniz: / / / mnist_features_result.

Çalıştırarak sonuçlar alabilirsiniz

    hadoop fs -cat hdfs:///mnist_features_result/*

ve sonuç aşağıdaki gibi görünür:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

SampleID MNIST kümesindeki Kimliğini temsil eder ve etiket modeli tanımlayan bir sayıdır.


## <a name="conclusion"></a>Sonuç

Bu belgede, basit bir örnek çalıştırmaya CaffeOnSpark yüklemeye çalıştınız. Hdınsight tam yönetilen bir bulut dağıtılmış bir işlem platformudur ve makine öğrenme ve Gelişmiş analitik iş yükleri, büyük veri kümesi üzerinde çalıştırmak için en iyi yerdir ve dağıtılmış derin öğrenme için Caffe Hdınsight Spark üzerinde derin öğrenme görevleri gerçekleştirmek için kullanabilirsiniz.


## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)

