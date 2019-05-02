---
title: Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma
description: Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/17/2017
ms.openlocfilehash: d0d68263485c5ab6e57a349317b1975862470cc2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721505"
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma


## <a name="introduction"></a>Giriş

Derin öğrenme, sağlık hizmetleri için üretim nakliye kadar her şeyi ve daha fazlasını etkilenip. Şirketler için derin gibi sabit sorunlarını gidermek öğrenme kapatma [görüntü sınıflandırma](https://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [konuşma tanıma](https://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)nesne tanıma ve makine çevirisi. 

Vardır [birçok popüler çerçeveleri](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software)de dahil olmak üzere [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), [Apache MXNet](https://mxnet.apache.org/), Theano, vs. [Caffe](https://caffe.berkeleyvision.org/) En ünlü sembolik olmayan (zorunlu) sinir ağı çerçeveleri biridir ve görüntü işleme dahil birçok alanda yaygın olarak kullanılan. Ayrıca, [CaffeOnSpark](https://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) Caffe Apache Spark, bu durumda derin öğrenme ile var olan bir Hadoop kümesinde kolayca da kullanılabilir bir araya getirir. Derin öğrenme Spark ETL işlem hatları, azalan sistem karmaşıklığını ve gecikme süresi ile birlikte eksiksiz bir çözüm öğrenme için kullanabilirsiniz.

[HDInsight](https://azure.microsoft.com/services/hdinsight/) olan Apache Hadoop bulut sunan Apache Spark, Apache Hive, Apache Hadoop, Apache HBase, Apache Storm, Apache Kafka ve ML Hizmetleri için iyileştirilmiş açık kaynaklı analiz kümeleri sağlayan. HDInsight, % 99,9 SLA ile desteklenir. Her biri bu büyük veri teknolojilerini ve ISV uygulamalarını yönetilen kümeler güvenlik ve izleme kuruluşlara yönelik olarak kolayca dağıtılabilir.

Bu makalede nasıl yükleneceği gösterilmektedir [Spark üzerinde Caffe](https://github.com/yahoo/CaffeOnSpark) bir HDInsight kümesi için. Bu makalede yerleşik MNIST tanıtım dağıtılmış CPU üzerinde HDInsight Spark'ı kullanarak ayrıntılı öğrenme kullanmayı göstermek için de kullanır.

Bir görevi tamamlamak için dört adım vardır:

1. Tüm düğümlerde gerekli bağımlılıkları yükleyin
2. Caffe HDInsight için Spark üzerinde baş düğümünde oluşturun.
3. Tüm çalışan düğümleri için gerekli kitaplıkları dağıtın
4. Caffe modeli oluşturabilir ve dağıtılmış bir şekilde çalıştırın.

HDInsight bir PaaS çözümü olduğundan, bazı görevleri daha kolaydır - harika platform özelliklerini sunar. Bu blog gönderisinde kullanılan özelliklerden biri çağrıldığında [betik eylemi](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), hangi, yürütebilir küme düğümleri (baş düğüm, alt düğüm veya kenar düğümüne) özelleştirmek için Kabuk komutları.

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>1. Adım:  Tüm düğümlerde gerekli bağımlılıkları yükleyin

Başlamak için bağımlılıkları yüklemeniz gerekir. Caffe site ve [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) bağımlılıkları için Spark YARN modunu yüklemeye yönelik bazı kullanışlı wiki sunar. HDInsight Spark YARN modunu de kullanır. Ancak, HDInsight platformuna yönelik birkaç daha fazla bağımlılıkları eklemeniz gerekir. Bunu yapmak için betik eylemi kullanın ve tüm baş düğümü ve çalışan düğümleri üzerinde çalıştırın. Bu betik eylemi, bu bağımlılıkların diğer paketleri ayrıca bağlı olarak yaklaşık 20 dakika sürer. Bir GitHub konumu veya varsayılan BLOB Depolama hesabı gibi HDInsight kümenize erişilebilir olan bazı konumda koymalısınız.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in https://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
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


Betik eylemi iki adımı vardır. İlk adım, gerekli tüm kitaplıkların yüklemektir. Bu kitaplıkları (gflags aracına gibi glog) Caffe derleme hem de Caffe (numpy gibi) çalıştırmak için gerekli kitaplıkları içerir. CPU en iyi duruma getirme libatlas kullanıyorsunuz ancak MKL veya CUDA (için GPU) gibi diğer iyileştirme kitaplıkları yükleme CaffeOnSpark wiki her zaman izleyebilirsiniz.

İkinci adım, derleme, indirip protobuf 2.5.0 Caffe için çalışma zamanı sırasında dir. Protobuf 2.5.0 [gereklidir](https://github.com/yahoo/CaffeOnSpark/issues/87), ancak bu sürümü Kaynak Kodu derlemek gereken şekilde Ubuntu 16 üzerindeki bir paket olarak kullanılabilir değildir. Ayrıca birkaç kaynak yok Internet'te derlemeniz konusunda. Daha fazla bilgi için [burada](https://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html).

Başlamak için yalnızca bu betik eylemi kümenizi karşı tüm çalışan düğümleri ve baş düğümlerine (HDInsight 3.5 için) çalıştırabilirsiniz. Betik eylemleri var olan bir kümede çalıştırmak veya betik eylemleri küme oluşturma sırasında kullanabilirsiniz. Betik eylemleri hakkında daha fazla bilgi için belgelere bakın [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

![Bağımlılıkları yüklemek üzere betik eylemleri](./media/apache-spark-deep-learning-caffe/Script-Action-1.png)


## <a name="step-2-build-caffe-on-apache-spark-for-hdinsight-on-the-head-node"></a>2. Adım: Caffe HDInsight için Apache Spark, baş düğümünde oluşturun.

İkinci adım, baş düğüme Caffe oluşturun ve ardından tüm çalışan düğümleri için derlenmiş kitaplıkları dağıtmak sağlamaktır. Bu adımda, gerekir [ssh, baş içine](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix). Bundan sonra izlemeniz gereken [CaffeOnSpark yapı işlemi](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn). Aşağıda birkaç ek adım ile CaffeOnSpark oluşturmak için kullanabileceğiniz betiği verilmiştir. 

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
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occurred: exec returned: 2"
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

CaffeOnSpark belgelere ne diyor birden fazla yapmanız gerekebilir. Değişiklikler şöyledir:
- CPU-yalnızca değiştirme ve bu belirli bir amaç için libatlas kullanın.
- Veri kümeleri, daha sonra kullanmak için tüm çalışan düğümleri için erişilebilir olan bir paylaşılan konuma olan BLOB depolama alanına yerleştirin.
- Derlenmiş Caffe kitaplıkları, BLOB depolama alanına yerleştirin ve daha sonra ek derleme süresi önlemek için betik eylemlerini kullanarak tüm düğümleri bu kitaplıkları kopyalayın.


### <a name="troubleshooting-an-ant-buildexception-has-occurred-exec-returned-2"></a>Sorun giderme: Ant BuildException oluştu: exec döndürdü: 2

İlk CaffeOnSpark oluşturmaya çalışırken, bazen söylüyor

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occurred: exec returned: 2

Kod deposu tarafından Temizle "temiz olun" ve ardından "yapı çalışma yap" bağımlılıkları doğru olduğu sürece, bu sorunu çözmek için.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Sorun giderme: Maven deposu bağlantı zaman aşımı

Bazen aşağıdaki kod parçacığına benzer bir bağlantı zaman aşımı hatası, maven sunar:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Birkaç dakika sonra yeniden denemeniz gerekir.


### <a name="troubleshooting-test-failure-for-caffe"></a>Sorun giderme: Caffe için test hatası

Son onay için CaffeOnSpark yaparken büyük olasılıkla test hatası bakın. Bu büyük olasılıkla UTF-8 kodlaması ile ilişkilidir, ancak Caffe kullanımını etkilememesi gerekir

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>3. Adım: Tüm çalışan düğümleri için gerekli kitaplıkları dağıtın

Sonraki adım kitaplıkları dağıtmaktır (temel kitaplıklarında CaffeOnSpark/caffe-Genel/dağıtma/lib/ve CaffeOnSpark/caffe-dağıtım/dağıtma/lib /) tüm düğümlere. 2. adım, BLOB Depolama alanında bu kitaplıkları yerleştirin ve bu adımda, tüm baş düğümlerine ve çalışan düğümleri kopyalamak için betik eylemlerini kullanın.

Bunu yapmak için betik eylemi aşağıdaki kod parçacığında gösterildiği gibi çalıştırın:

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Doğru konumu noktasına kümenize belirli ihtiyacınız olduğundan emin olun)

2. adımda, tüm düğümler için erişilebilir olan BLOB Depolama alanında yerleştirdiğiniz çünkü bu adımda, yalnızca, tüm düğümlere kopyalayın.

## <a name="step-4-compose-a-caffe-model-and-run-it-in-a-distributed-manner"></a>4. Adım: Caffe modeli oluşturabilir ve dağıtılmış bir şekilde çalıştırın

Caffe, önceki adımlarda çalıştırdıktan sonra yüklenir. Sonraki adım, Caffe modeli yazmaktır. 

Caffe "bir model oluşturmak için yalnızca bir yapılandırma dosyası tanımlamak gereken bir ifadesel mimari", kullanma ve olmadan hiç (çoğu durumda) kodlama. Şimdi burada göz atın. 

Eğittiğiniz modeli MNIST eğitim için bir örnek modelidir. El yazısı basamak MNIST veritabanını 60.000 örnekler Eğitim kümesi ve bir sınama kümesi 10.000 örnekler vardır. Bu, daha büyük bir NIST kullanılabilir bir alt kümesidir. Basamak boyutu normalleştirilmiş ve sabit boyutlu görüntü ortalanmış olmuştur. Veri kümesini indirin ve doğru biçime dönüştürmek için bazı kodlar CaffeOnSpark sahiptir.

CaffeOnSpark bazı ağ topolojileri örnek MNIST eğitimi sağlar. En iyi duruma getirme ve ağ mimarisini (ağ topolojisi) bölme iyi bir tasarım var. Bu durumda, gerekli iki dosya vardır: 

"Solver" dosyası (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) en iyi duruma getirme gözlemledikten ve parametre güncelleştirmeleri oluşturmak için kullanılır. Örneğin, CPU veya GPU, kaç adet yineleme olan İtici Güç nedir kullanılıp kullanılmayacağını tanımlar vs. Ayrıca, programın (ikinci dosya ihtiyacınız olan) kullanmalıdır hangi neuron ağ topolojisi da tanımlar. Çözücü hakkında daha fazla bilgi için bkz: [Caffe belgeleri](https://caffe.berkeleyvision.org/tutorial/solver.html).

CPU, GPU yerine kullandığından, bu örnekte, en son satırına değiştirmelisiniz.

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe yapılandırma](./media/apache-spark-deep-learning-caffe/Caffe-1.png)

Diğer satırları gerektiği gibi değiştirebilirsiniz.

İkinci bir dosya (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) nasıl neuron ağ, benzer ve ilgili girdi ve çıktı dosyası tanımlar. Eğitim verileri konumu yansıtacak şekilde dosyasını güncelleştirmeniz gerekir. (Bu, kümeniz için belirli doğru konuma işaret edecek şekilde gerekir) lenet_memory_train_test.prototxt aşağıdaki bölümünde değiştirin:

- "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" Dönüştür "wasb: / / / Proje/machine_learning/image_dataset/mnist_train_lmdb"
- "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" Dönüştür "wasb: / / / Proje/machine_learning/image_dataset/mnist_test_lmdb"

![Caffe yapılandırma](./media/apache-spark-deep-learning-caffe/Caffe-2.png)

Ağ tanımlama hakkında daha fazla bilgi için denetleme [MNIST veri kümesinde Caffe belgeleri](https://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Bu makalede amacıyla bu MNIST örneği kullanın. Baş düğümünden aşağıdaki komutları çalıştırın:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

Yukarıdaki komut, her YARN kapsayıcı için gerekli dosyaları (lenet_memory_solver.prototxt ve lenet_memory_train_test.prototxt) dağıtır. Komutu, ayrıca LD_LIBRARY_PATH için her Spark sürücüsü/Yürütücü ilgili YOLUNU ayarlar. Önceki kod parçacığı ve CaffeOnSpark kitaplıkların bulunduğu konumu işaret LD_LIBRARY_PATH tanımlanır. 

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

YARN küme modunu kullandığından, rastgele bir kapsayıcı (ve bir rasgele çalışan düğümü) Spark sürücüsü zamanlanacak durumda yalnızca çıktısı aşağıdakine benzer konsolunda görmeniz gerekir:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Ne olduğunu bilmek istiyorsanız, genellikle daha fazla bilgi içeren sürücünün günlük Spark almanız gerekir. Bu durumda, ilgili YARN günlüklerini bulmak için YARN UI gitmeniz gerekiyor. Bu URL'ye göre YARN UI alabilirsiniz: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN UI](./media/apache-spark-deep-learning-caffe/YARN-UI-1.png)

Bu belirli bir uygulama için kaç kaynak ayrılmış bir göz atalım. "Zamanlayıcı" bağlantısına tıklayarak ve ardından bu uygulama için olduğunu çalışan dokuz kapsayıcılar görürsünüz. sekiz yürütücüler sağlamak için YARN isteyin ve sürücü işlemi için başka bir kapsayıcıdır. 

![YARN Zamanlayıcı](./media/apache-spark-deep-learning-caffe/YARN-Scheduler.png)

Hata varsa sürücü günlükleri veya kapsayıcı günlüklerini denetlemek isteyebilirsiniz. Sürücü günlükleri için uygulama kimliği YARN kullanıcı arabiriminde tıklayın ve ardından "Günlükleri" düğmesine tıklayın. Stderr sürücü günlüklere yazılır.

![YARN UI 2](./media/apache-spark-deep-learning-caffe/YARN-UI-2.png)

Örneğin, aşağıdaki hatayı sürücüsünü günlüğündeki bazıları çok fazla yürütücüler tahsis belirten görebilirsiniz.

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

Bazı durumlarda, sorunu sürücüleri yerine yürütücüler oluşabilir. Bu durumda, kapsayıcı günlüklerini denetlemeniz gerekir. Her zaman kapsayıcı günlüklerini Al ve ardından başarısız kapsayıcıya alın. Örneğin, Caffe çalıştırırken bu hatayla karşılayabilir.

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

Bu durumda, başarısız bir kapsayıcı kimliği için ihtiyaç duyduğunuz (Yukarıdaki durumda bu, container_1485916338528_0008_05_000005'dir). Ardından çalıştırmanız gerekir 

    yarn logs -containerId container_1485916338528_0008_03_000005

baş düğümüne. Kapsayıcı hatası denetledikten sonra GPU modunu (burada, CPU modunu kullanmanız) kullanarak lenet_memory_solver.prototxt neden olur.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Sonuçlar alınıyor

8 yürütücüler tahsis ve ağ topolojisini basit olduğundan, yaklaşık 30 dakika sonucunu çalıştırmak için yalnızca sürer. Komut satırından wasb:///mnist.model için model yerleştirin ve sonuçları wasb adlı bir klasöre yerleştirin görebilirsiniz: / / / mnist_features_result.

Çalıştırarak sonuçlar alabilirsiniz.

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

SampleID MNIST kümesindeki kimlik temsil eder ve etiket modeli tanımlayan sayısıdır.


## <a name="conclusion"></a>Sonuç

Bu belgede, basit bir örnek çalıştırmayla CaffeOnSpark yüklemeye çalıştınız. HDInsight, tam yönetilen bulut dağıtılmış bir işlem platformudur ve makine öğrenimi ve Gelişmiş analiz iş yükleri, büyük veri kümesinde çalıştırmak için en iyi yerdir ve dağıtılmış derin öğrenme için Caffe HDInsight Spark üzerinde derin öğrenme gerçekleştirmek için kullanabilirsiniz görevler.


## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)

