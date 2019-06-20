---
title: Azure Kubernetes Service (AKS) ile bir Apache Spark işi çalıştırma
description: Bir Apache Spark işi çalıştırmak için Azure Kubernetes Service (AKS) kullanın
services: container-service
author: lenadroid
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: ddaff590fd493b430a72c30dd35cb1b891b80d84
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205343"
---
# <a name="running-apache-spark-jobs-on-aks"></a>AKS üzerinde Apache Spark işleri çalıştırma

[Apache Spark] [ apache-spark] büyük ölçekli veri işleme için hızlı bir altyapıdır. Sürümünden itibaren [Spark 2.3.0 yayın][spark-latest-release], Apache Spark, Kubernetes kümelerini ile yerel tümleştirme destekler. Azure Kubernetes Service (AKS), Azure'da çalışan yönetilen bir Kubernetes ortamıdır. Bu belge, hazırlama ve bir Azure Kubernetes Service (AKS) kümesi üzerinde Apache Spark işleri çalıştırma ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakilere ihtiyacınız vardır.

* Kubernetes ile ilgili temel bilgilere ve [Apache Spark][spark-quickstart].
* [Docker Hub] [ docker-hub] hesabını veya bir [Azure Container Registry][acr-create].
* Azure CLI [yüklü] [ azure-cli] geliştirme sisteminizde.
* [JDK 8] [ java-install] sisteminizde yüklü.
* SBT ([Scala derleme aracı][sbt-install]) sisteminizde yüklü.
* Git komut satırı araçları, sisteminizde yüklü.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Spark, büyük ölçekli veri işleme için kullanılır ve Spark kaynak gereksinimlerini karşılamak için Kubernetes düğümleri boyutlandırılır gerektirir. Boyutu en az öneririz `Standard_D3_v2` , Azure Kubernetes Service (AKS) düğümler için.

Bu en düşük öneri karşılayan bir AKS kümesi gerekirse, aşağıdaki komutları çalıştırın.

Küme için bir kaynak grubu oluşturun.

```azurecli
az group create --name mySparkCluster --location eastus
```

Boyutu düğümleri ile bir AKS kümesi oluşturma `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

AKS kümeye bağlanın.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Kapsayıcı görüntülerini depolamak için Azure Container Registry (ACR) kullanıyorsanız, AKS ACR arasındaki kimlik doğrulaması yapılandırın. Bkz: [ACR kimlik doğrulamasını belgeleri] [ acr-aks] adımları için.

## <a name="build-the-spark-source"></a>Spark kaynak derleme

Spark işleri bir AKS kümesinde çalıştırmadan önce Spark, kaynak kodu oluşturun ve bir kapsayıcı görüntüsüne paketlemek gerekir. Spark kaynak bu işlemi tamamlamak için kullanılan betikleri içerir.

Geliştirme sisteminizde Spark proje depoyu kopyalayın.

```bash
git clone -b branch-2.3 https://github.com/apache/spark
```

Kopyalanan deponun dizinine geçin ve Spark kaynağının yolunu bir değişkene kaydedin.

```bash
cd spark
sparkdir=$(pwd)
```

Yüklü birden fazla JDK sürüm varsa, ayarlama `JAVA_HOME` sürüm 8 geçerli oturum için kullanılacak.

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Spark ile Kubernetes desteği kaynak kodu oluşturmak için aşağıdaki komutu çalıştırın.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

Aşağıdaki komutlar, Spark kapsayıcı görüntüsü oluşturma ve kapsayıcı görüntüsünü kayıt defterine gönderin. Değiştirin `registry.example.com` kapsayıcı kayıt defterinizin adıyla ve `v1` kullanmayı tercih etiketi. Docker hub'ı kullanıyorsanız, bu değeri kayıt defteri adıdır. Azure Container Registry (ACR) kullanıyorsanız, bu ACR oturum açma sunucusu adını değerdir.

```bash
REGISTRY_NAME=registry.example.com
REGISTRY_TAG=v1
```

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG build
```

Kapsayıcı görüntüsünü, kapsayıcı görüntüsünü kayıt defterinize gönderin.

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG push
```

## <a name="prepare-a-spark-job"></a>Bir Spark işi hazırlama

Ardından, bir Spark işi hazırlayın. Bir jar dosyasını Spark işi tutmak için kullanılır ve ihtiyaç duyduğunuzda `spark-submit` komutu. Jar genel bir URL erişilebilir duruma veya önceden içinde bir kapsayıcı görüntüsüne paketlendi. Bu örnekte, bir örnek jar Pi değerini hesaplamak için oluşturulur. Bu jar dosyası, ardından Azure depolama alanına yüklenir. Mevcut bir jar varsa alternatif çekinmeyin

Bir Spark işi için bir proje oluşturmak için istediğiniz bir dizin oluşturun.

```bash
mkdir myprojects
cd myprojects
```

Bir şablondan yeni bir Scala projesi oluşturun.

```bash
sbt new sbt/scala-seed.g8
```

İstendiğinde girin `SparkPi` proje adı.

```bash
name [Scala Seed Project]: SparkPi
```

Yeni oluşturulan proje dizinine gidin.

```bash
cd sparkpi
```

Paketleme projesi bir jar dosyası olarak izin veren bir SBT eklentisini eklemek için aşağıdaki komutları çalıştırın.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Örnek kodu kopyalayıp yeni oluşturulan projeye ve tüm gerekli bağımlılıkları eklemek için şu komutları çalıştırın.

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

Projenin bir jar paketi için aşağıdaki komutu çalıştırın.

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

Bir Azure depolama hesabını ve jar dosyasını tutacak bir kapsayıcı oluşturun.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Jar dosyasını aşağıdaki komutları kullanarak Azure depolama hesabına yükleyin.

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

Değişken `jarUrl` artık genel olarak erişilebilir yolunu jar dosyasını içerir.

## <a name="submit-a-spark-job"></a>Bir Spark işi gönderme

Kube-proxy ayrı bir komut satırı aşağıdaki kodla başlayın.

```bash
kubectl proxy
```

Geri Spark deposunun kök dizinine gidin.

```bash
cd $sparkdir
```

Gönderme işlemi kullanılarak `spark-submit`.

```bash
./bin/spark-submit \
  --master k8s://http://127.0.0.1:8001 \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.container.image=$REGISTRY_NAME/spark:$REGISTRY_TAG \
  $jarUrl
```

Bu işlem, kabuk oturumu için iş durumunu akışları Spark işi başlatır. İş çalışırken Spark sürücüsü pod görebilir ve yürütücü pod'ları kullanarak kubectl get pod'ların komutu. Bu komutları çalıştırmak için ikinci terminal oturumu açın.

```console
$ kubectl get pods

NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

İş çalışırken Spark kullanıcı arabirimini de erişebilirsiniz. İkinci terminal oturumunda kullanın `kubectl port-forward` komut Spark Arabirimine erişim sağlar.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Spark kullanıcı ARABİRİMİ'ne erişmek için open adresi `127.0.0.1:4040` bir tarayıcıda.

![Spark kullanıcı Arabirimi](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>İş sonuçlarını ve günlüklerini alma

İş tamamlandıktan sonra sürücü pod "Completed" durumda olacaktır. Aşağıdaki komutla pod adını alın.

```bash
kubectl get pods --show-all
```

Çıktı:

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Kullanım `kubectl logs` spark sürücüsü pod ' günlüklerini almak için komutu. Pod sürücü pod's adıyla değiştirin.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

Bu günlüklere Pi değeridir Spark işinin sonucunu görebilirsiniz.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Kapsayıcı görüntüsünü içeren paketi jar

Yukarıdaki örnekte, Azure depolama için Spark jar dosyasını karşıya yüklendi. Jar dosyasını ürettikleri Docker görüntüleri paketi başka bir seçenektir.

Bunu yapmak için bulma `dockerfile` Spark görüntü raporu için `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` dizin. Ekleme 'M `ADD` Spark işi bildirimi `jar` arasında bir yerde `WORKDIR` ve `ENTRYPOINT` bildirimleri.

Jar yolu konumuna güncelleştirme `SparkPi-assembly-0.1.0-SNAPSHOT.jar` geliştirme sisteminizde dosyanın. Ayrıca, kendi özel jar dosyasını da kullanabilirsiniz.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Oluşturun ve dahil edilen Spark betiklerle görüntüyü gönderin.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Bir uzak jar URL belirten yerine iş çalıştırılırken `local://` düzeni, Docker görüntüsünü jar dosyasında yoluyla kullanılabilir.

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

> [!WARNING]
> Spark'tan [belgeleri][spark-docs]: "Kubernetes Zamanlayıcısı şu anda Deneysel'dır. Gelecek sürümlerde olabilir yapılandırması, kapsayıcı görüntüleri ve giriş noktaları davranış değişiklikleri".

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı için Spark belgelerine göz atın.

> [!div class="nextstepaction"]
> [Spark belgeleri][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: https://aka.ms/azure-jdks
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/azure/aks/
[azure-cli]: https://docs.microsoft.com/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/azure/storage/common/storage-azure-cli
