---
title: "Azure kapsayıcı hizmeti (AKS) ile bir Apache Spark işini çalıştır"
description: "Azure kapsayıcı hizmeti (AKS) bir Apache Spark işi çalıştırmak için kullanın"
services: container-service
author: lenadroid
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: 9d57f572ba159191f5b634b5ea604563ac2f7801
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="running-apache-spark-jobs-on-aks"></a>AKS üzerinde Apache Spark işleri çalıştırma

[Apache Spark] [ apache-spark] büyük ölçekli veri işleme için hızlı bir altyapıdır. Sürümünden başlayarak [Spark 2.3.0 sürüm][spark-latest-release], Apache Spark Kubernetes kümeleri ile yerel tümleştirme destekler. Azure kapsayıcı hizmeti (AKS), Azure'da çalışan yönetilen bir Kubernetes ortamıdır. Bu belge hazırlama ve bir Azure kapsayıcı hizmeti (AKS) kümede çalışan Apache Spark iş ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede içindeki adımları tamamlamak için aşağıdakiler gerekir.

* Kubernetes ilgili temel bilgilere ve [Apache Spark][spark-quickstart].
* [Docker hub'a] [ docker-hub] hesabı veya bir [Azure kapsayıcı kayıt defteri][acr-create].
* Azure CLI [yüklü] [ azure-cli] geliştirme sisteminizde.
* [JDK 8] [ java-install] , sisteminizde yüklü.
* SBT ([Scala yapı aracı][sbt-install]), sisteminizde yüklü.
* Git komut satırı araçlarını, sisteminizde yüklü.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Spark büyük ölçekli veri işleme için kullanılır ve Kubernetes düğümleri Spark kaynak gereksinimlerini karşılamak için boyutlandırılır gerektirir. En az bir boyutunu öneririz `Standard_D3_v2` , Azure kapsayıcı hizmeti (AKS) düğümleri için.
 
Bu en az öneri karşılayan bir AKS küme gerekiyorsa, aşağıdaki komutları çalıştırın.

Küme için bir kaynak grubu oluşturun.

```azurecli
az group create --name mySparkCluster --location eastus
```

AKS küme boyutu düğümleriyle oluşturmak `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

AKS kümeye bağlanın.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Kapsayıcı görüntüleri saklamak için Azure kapsayıcı kayıt defteri (ACR) kullanıyorsanız, AKS ACR arasındaki kimlik doğrulamasını yapılandırın. Bkz: [ACR authentication belgeleri] [ acr-aks] adımları için.

## <a name="build-the-spark-source"></a>Spark kaynak derleme

Spark işlerinin AKS kümede çalıştırmadan önce Spark kaynak kodu oluşturma ve kapsayıcı görüntüsüne paketini gerekir. Spark kaynak bu işlemi tamamlamak için kullanılan komut dosyalarını içerir. 

Geliştirme sisteminizde Spark proje depoyu kopyalayın.

```bash
git clone https://github.com/apache/spark
```

Kopyalanan deposu dizinine değiştirin ve Spark kaynağının yolunu bir değişkene kaydedin.

```bash
cd spark
sparkdir=$(pwd)
```

Yüklü birden fazla JDK sürümleri varsa, ayarlamak `JAVA_HOME` sürüm 8 geçerli oturum için kullanılacak. 

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Kaynak kodu Kubernetes desteğiyle Spark oluşturmak için aşağıdaki komutu çalıştırın.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

Aşağıdaki komut, Spark kapsayıcı görüntülerini oluşturur ve bir kapsayıcı görüntü kayıt defterine iter. `registry.example.com` komutunu, kapsayıcı kayıt defterinizin adıyla değiştirin. Docker hub'a kullanıyorsanız, bu kayıt defteri adı değerdir. Azure kapsayıcı kayıt defteri (ACR) kullanıyorsanız, bu değer ACR oturum açma sunucusu adı olur.

```bash
./bin/docker-image-tool.sh -r registry.example.com -t v1 build
```

Kapsayıcı görüntü kapsayıcısı görüntü kaydınız iletin.

```bash
./bin/docker-image-tool.sh -r registry.example.com -t v1 push
```

## <a name="prepare-a-spark-job"></a>Spark işi hazırlama

Ardından, Spark iş hazırlayın. Jar dosyasını Spark iş tutmak için kullanılan ve çalıştırırken yardıma ihtiyaç duymanız `spark-submit` komutu. Jar genel bir URL ile erişilebilir hale veya içinde bir kapsayıcı görüntü önceden paketlenmiş. Bu örnekte, bir örnek jar Pi değerini hesaplamak için oluşturulur. Bu jar sonra Azure depolama alanına yüklenir. Varolan bir jar varsa, yedek çekinmeyin

Burada Spark iş projesi oluşturmak istediğiniz bir dizin oluşturun.

```bash
mkdir myprojects
cd myprojects
```

Bir şablondan yeni bir Scala projesi oluşturun.

```bash
sbt new sbt/scala-seed.g8
```

İstendiğinde, girin `SparkPi` proje adı.

```bash
name [Scala Seed Project]: SparkPi
```

Yeni oluşturulan proje dizinine gidin.

```bash
cd sparkpi
```

Proje jar dosyasını olarak paketleme sağlayan bir SBT eklenti eklemek için aşağıdaki komutları çalıştırın.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Yeni oluşturulan projeye örnek kodu kopyalayın ve tüm gerekli bağımlılıkları eklemek için şu komutları çalıştırın.

```bash
EXAMPLESDIR="src/main/scala/org/apache/spark/examples"
mkdir -p $EXAMPLESDIR
cp $sparkdir/examples/$EXAMPLESDIR/SparkPi.scala $EXAMPLESDIR/SparkPi.scala

cat <<EOT >> build.sbt
// https://mvnrepository.com/artifact/org.apache.spark/spark-sql
libraryDependencies += "org.apache.spark" %% "spark-sql" % "2.3.0" % "provided"
EOT

sed -ie 's/scalaVersion.*/scalaVersion := "2.11.11",/' build.sbt
sed -ie 's/name.*/name := "SparkPi",/' build.sbt
```

Proje jar paketlemek için aşağıdaki komutu çalıştırın.

```bash
sbt assembly
```

Başarılı paketleme sonra aşağıdakine benzer bir çıktı görmeniz gerekir.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>Depolama alanına kopyalama işi

Bir Azure depolama hesabı ve jar dosyasını tutmak için kapsayıcı oluşturun.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Azure depolama hesabı aşağıdaki komutlarla jar dosyasını yükleyin.

```bash
CONTAINER_NAME=jars
BLOB_NAME=SparkPi-assembly-0.1.0-SNAPSHOT.jar
FILE_TO_UPLOAD=target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar

echo "Creating the container..."
az storage container create --name $CONTAINER_NAME
az storage container set-permission --name $CONTAINER_NAME --public-access blob

echo "Uploading the file..."
az storage blob upload --container-name $CONTAINER_NAME --file $FILE_TO_UPLOAD --name $BLOB_NAME

jarUrl=$(az storage blob url --container-name $CONTAINER_NAME --name $BLOB_NAME | tr -d '"')
```

Değişken `jarUrl` şimdi jar dosyasını genel olarak erişilebilir yolunu içerir.

## <a name="submit-a-spark-job"></a>Bir Spark iş gönderme

Spark iş göndermeden önce Kubernetes API sunucu adresi gerekir. Kullanım `kubectl cluster-info` bu adresi almak için komutu.

Kubernetes API sunucunuz çalıştığı URL bulur.

```bash
kubectl cluster-info
```

Adresini ve bağlantı noktasını not edin.

```bash
Kubernetes master is running at https://<your api server>:443
```

Spark depo kök geri gidin.

```bash
cd $sparkdir
```

İş kullanarak Gönder `spark-submit`. 

Değeri değiştirme `<kubernetes-api-server>` API sunucu adresi ve bağlantı noktası. Değiştir `<spark-image>` kapsayıcı görüntünüzü biçiminde adıyla `<your container registry name>/spark:<tag>`.

```bash
./bin/spark-submit \
  --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.container.image=<spark-image> \
  $jarUrl
```

Bu işlem, kabuk oturumunuz için iş durumu akışları Spark iş başlatır. İş çalışırken, Spark sürücü pod görebilir ve kubectl kullanarak Yürütücü pod'ları get pod'ları komutu. Bu komutları çalıştırmak üzere ikinci terminal oturumu açın.

```console
$ kubectl get pods

NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

İş çalışırken, Spark UI de erişebilirsiniz. İkinci terminal oturumunda kullanmak `kubectl port-forward` komutu Spark UI erişim sağlar.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Open adresi Spark UI erişmek için `127.0.0.1:4040` bir tarayıcıda.

![Spark UI](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>İş sonuçları ve günlükleri alma

İş tamamlandıktan sonra sürücü pod "Tamamlandı" durumda olacaktır. Aşağıdaki komutla pod adını alın.

```bash
kubectl get pods --show-all
```

Çıktı:

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Kullanım `kubectl logs` günlükleri spark sürücü pod ' alabilirsiniz. Pod adı sürücü pod'ın adıyla değiştirin.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

Bu günlükler içinde Pi değerinin Spark iş sonuçlarını görebilirsiniz.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Kapsayıcı görüntüsüyle paket jar

Yukarıdaki örnekte, Spark jar dosyası Azure depolama alanına karşıya yüklenmedi. Özel olarak geliştirilmiş Docker görüntülere jar dosyasını paketini başka bir seçenektir.

Bunu yapmak için bulma `dockerfile` Spark görüntü konumunda bulunan için `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` dizin. Ekleme 'M `ADD` Spark iş bildirimi `jar` yere arasında `WORKDIR` ve `ENTRYPOINT` bildirimleri.

Jar yolu konumuna güncelleştirme `SparkPi-assembly-0.1.0-SNAPSHOT.jar` geliştirme sisteminizde dosya. Kendi özel jar dosyasını da kullanabilirsiniz.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Derleme ve görüntünün dahil Spark kodlarıyla gönderme.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Bir uzak jar URL belirten yerine iş çalışırken `local://` düzeni, Docker görüntüsündeki jar dosyasını yoluyla kullanılabilir.

```bash
./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=3 \
    --conf spark.kubernetes.container.image=<spark-image> \
    local:///opt/spark/work-dir/<your-jar-name>.jar
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı için Spark belgelerine göz atın.

> [!div class="nextstepaction"]
> [Spark belgeleri][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/en-us/azure/aks/
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/en-us/azure/storage/common/storage-azure-cli
