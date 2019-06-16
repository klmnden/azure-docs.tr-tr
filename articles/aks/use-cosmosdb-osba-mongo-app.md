---
title: Mevcut MongoDB uygulamasını Azure Cosmos DB API ile MongoDB ile açık hizmet Aracısı (OSBA) Azure için tümleştirme
description: Bu makalede, Azure Cosmos DB API'si ile mevcut bir Java ve MongoDB uygulamasını (OSBA) Azure için açık hizmet aracısı kullanarak MongoDB tümleştirme öğrenin.
services: azure-dev-spaces
author: zr-msft
manager: jeconnoc
ms.service: azure-dev-spaces
ms.topic: article
ms.date: 01/25/2019
ms.author: zarhoads
ms.custom: mvc
keywords: Cosmos DB, açık hizmet Aracısı, Azure için açık hizmet Aracısı
ms.openlocfilehash: 46fa5564e5dd3429f812b263295044d867a8511c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61028431"
---
# <a name="integrate-existing-mongodb-application-with-azure-cosmos-db-api-for-mongodb-and-open-service-broker-for-azure-osba"></a>Mevcut MongoDB uygulamasını Azure Cosmos DB API ile MongoDB ile açık hizmet Aracısı (OSBA) Azure için tümleştirme

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Ayrıca, MongoDB için de dahil olmak üzere birkaç NoSQL API kablo protokolü uyum sağlar. Cosmos DB MongoDB API'si, Cosmos DB, uygulamanızın veritabanı sürücüleri ya da uygulanmasını değiştirmek zorunda kalmadan mevcut MongoDB uygulamanızı kullanmanıza olanak sağlar. Azure için açık hizmet aracısı kullanarak bir Cosmos DB hizmetini de sağlayabilirsiniz.

Bu makalede, bir MongoDB veritabanı kullanan var olan bir Java uygulama katılın ve Azure için açık hizmet aracısı kullanarak bir Cosmos DB veritabanını kullanacak şekilde güncelleştirin.

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce şunları yapmalısınız:
    
* Sahip bir [Azure Kubernetes hizmeti küme](kubernetes-walkthrough.md) oluşturulur.
* Sahip [Azure için açık hizmet Aracısı yüklenir ve yapılandırılır. AKS kümenizde](integrate-azure.md). 
* Sahip [hizmet Kataloğu CLI](https://svc-cat.io/docs/install/) yüklenir ve çalıştırılmak için yapılandırılan `svcat` komutları.
* Var olan bir sahip [MongoDB](https://www.mongodb.com/) veritabanı. Örneğin, MongoDB üzerinde çalışan olabilir, [geliştirme makinesi](https://docs.mongodb.com/manual/administration/install-community/) veya bir [Azure VM](../virtual-machines/linux/install-mongodb.md).
* Bağlama ve MongoDB veritabanını sorgulama, gibi bir yöntemden faydalanabilir [mongo kabuğunu](https://docs.mongodb.com/manual/mongo/).

## <a name="get-application-code"></a>Uygulama kodunu alma
    
Bu makalede, kullandığınız [müzik örnek uygulama Cloud Foundry spring](https://github.com/cloudfoundry-samples/spring-music) bir MongoDB veritabanı kullanan bir uygulamayı göstermek için.
    
Uygulamasını github'dan kopyalayın ve alt dizinine gidin:
    
```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring-music
```

## <a name="prepare-the-application-to-use-your-mongodb-database"></a>MongoDB veritabanınız kullanmak üzere uygulamayı hazırlayın

Spring müzik örnek uygulama, veri kaynakları için çok sayıda seçenek sunuyor. Bu makalede, mevcut bir MongoDB veritabanını kullanacak şekilde yapılandırın. 

YAML aşağıdaki sonuna ekleyin *src/main/resources/application.yml*. Bu ek olarak adlandırılan bir profil oluşturur *mongodb* ve bir URI ve veritabanı adını yapılandırır. Mevcut MongoDB veritabanınız ile bağlantı bilgilerini URI değiştirin. Bir kullanıcı adı ve parola içeren URI ekleme doğrudan bu dosyaya içindir **yalnızca Geliştirme amaçlı kullanılır** ve **hiçbir zaman sürüm denetimine eklensin mi**.

```yaml
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://user:password@serverAddress:port/musicdb"
      database: musicdb
```



Ne zaman uygulamanızı başlatın ve kullanmasını söylemeniz *mongodb* profili, MongoDB veritabanına bağlanır ve uygulama verilerini depolamak için kullanın.

Uygulamanızı oluşturmak için:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```

Uygulamanızı başlatmak ve kullanmak üzere bilgi *mongodb* profili:

```cmd
java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Gidin `http://localhost:8080` tarayıcınızda.

![Varsayılan verilerle Spring Music uygulaması](media/music-app.png)

Bazı uygulama doldurulduğunda fark [varsayılan veri](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). Silerek onunla etkileşim kurabilir birkaç mevcut Albümler ve birkaç yenilerini oluşturma.

Uygulamanızı, MongoDB veritabanınız kullanıyor, buna bağlanma ve sorgulama doğrulayabilirsiniz *musicdb* veritabanı:

```cmd
mongo serverAddress:port/musicdb -u user -p password
use musicdb
db.album.find()

{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446d"), "title" : "Nevermind", "artist" : "Nirvana", "releaseYear" : "1991", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446e"), "title" : "Pet Sounds", "artist" : "The Beach Boys", "releaseYear" : "1966", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446f"), "title" : "What's Going On", "artist" : "Marvin Gaye", "releaseYear" : "1971", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
...
```

Önceki örnekte [mongo kabuğunu](https://docs.mongodb.com/manual/mongo/) MongoDB veritabanına bağlanın ve sorgulayın. Uygulamanızı durdurma, yeniden başlatmayı ve tarayıcınızda sayfasına dönüp değişikliklerinizi kalıcı da doğrulayabilirsiniz. Yaptığınız değişiklikleri yine de olduğuna dikkat edin.


## <a name="create-a-cosmos-db-database"></a>Bir Cosmos DB veritabanı oluşturma

Açık hizmet Aracısı'nı kullanarak Azure Cosmos DB veritabanı oluşturmak için kullanın `svcat provision` komutu:

```cmd
svcat provision musicdb --class azure-cosmosdb-mongo-account --plan account  --params-json '{
  "location": "eastus",
  "resourceGroup": "MyResourceGroup",
  "ipFilters" : {
    "allowedIPRanges" : ["0.0.0.0/0"]
  }
}'
```

Yukarıdaki komut kaynak grubundaki Azure Cosmos DB veritabanı hazırlar *MyResourceGroup* içinde *eastus* bölge. Daha fazla bilgi *resourceGroup*, *konumu*, ve diğer Azure özgü JSON parametreleri kullanılabilir [Cosmos DB modülü başvuru belgeleri](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/cosmosdb.md#provision-3).

Veritabanınızı sağlama tamamlandığını doğrulamak için `svcat get instance` komutu:

```cmd
$ svcat get instance musicdb

   NAME     NAMESPACE              CLASS                PLAN     STATUS
+---------+-----------+------------------------------+---------+--------+
  musicdb   default     azure-cosmosdb-mongo-account   account   Ready
```

Veritabanınız hazır olduğunda olup *hazır* altında *durumu*.

Veritabanınızı Sağlama tamamlandıktan sonra meta verileri için bağlamak gereken bir [Kubernetes gizli](https://kubernetes.io/docs/concepts/configuration/secret/). Bir gizli dizi için bağlı sonra diğer uygulamalar sonra bu verilere erişebilir. Veritabanınızın meta veriler için bir gizli dizi bağlamak için kullanın `svcat bind` komutu:

```cmd
$ svcat bind musicdb

  Name:        musicdb
  Namespace:   default
  Status:
  Secret:      musicdb
  Instance:    musicdb

Parameters:
  No parameters defined
```


## <a name="use-the-cosmos-db-database-with-your-application"></a>Cosmos DB veritabanı, uygulama ile kullanma

Cosmos DB veritabanı uygulamanızla kullanmak için bağlanmak için URI bilmeniz gerekir. Bu bilgileri almak için kullanın `kubectl get secret` komutu:

```cmd
$ kubectl get secret musicdb -o=jsonpath='{.data.uri}' | base64 --decode

mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
```

Önceki komutu alır *musicdb* gizli ve yalnızca URI görüntüler. Yukarıdaki komut ayrıca kodunu çözer için gizli dizileri base64 biçiminde depolanır.

Cosmos DB veritabanı URI'sini kullanarak güncelleştirme *src/main/resources/application.yml* kullanmak için:

```yaml
...
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
      database: musicdb
```

Bir kullanıcı adı ve parola içeren, URI, güncelleştirmeyi doğrudan bu dosyaya içindir **yalnızca Geliştirme amaçlı kullanılır** ve **hiçbir zaman sürüm denetimine eklensin mi**.

Yeniden oluşturma ve Cosmos DB veritabanı kullanmaya başlamak için uygulamanızı başlatın:

```cmd
./gradlew clean assemble

java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Hala uygulamanızın kullandığı fark *mongodb* profili ve ile başlayan URI *mongodb: / /* Cosmos DB veritabanına bağlanmak için. [Azure Cosmos DB MongoDB API'si](../cosmos-db/mongodb-introduction.md) Bu uyumluluk sağlar. Bu bir MongoDB veritabanını kullanıyor, ancak gerçekte Cosmos DB kullanarak yokmuş gibi çalışmaya devam etmesini sağlar.

Gidin `http://localhost:8080` tarayıcınızda. Varsayılan veri geri dikkat edin. Silerek onunla etkileşim kurabilir birkaç mevcut Albümler ve birkaç yenilerini oluşturma. Uygulamanızı durdurma, yeniden başlatmayı ve tarayıcınızda sayfasına dönüp değişikliklerinizi kalıcı doğrulayabilirsiniz. Yaptığınız değişiklikleri yine de olduğuna dikkat edin. Azure için açık hizmet aracısı kullanarak oluşturduğunuz Cosmos DB'ye değişiklikleri kalıcı.


## <a name="run-your-application-on-your-aks-cluster"></a>AKS kümenizde uygulamanızı çalıştırın

Kullanabileceğiniz [Azure geliştirme alanları](../dev-spaces/azure-dev-spaces.md) AKS kümenizi uygulamayı dağıtmak için. Azure geliştirme alanları dockerfile'ları ve Helm grafiklerini gibi yapılar oluşturmak, AKS içinde bir uygulamayı çalıştırmak ve ve dağıtmanıza yardımcı olur.

Azure geliştirme alanları AKS kümenizin etkinleştirmek için:

```cmd
az aks enable-addons --addons http_application_routing -g MyResourceGroup -n MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

Uygulamanız, AKS içinde çalıştırmak için hazırlamak üzere Azure geliştirme alanları araçları kullanın:

```cmd
azds prep --public
```

Bu komut dahil olmak üzere çeşitli yapıtların oluşturur. bir *grafikleri /* , Helm grafiği, projenin kök klasörü. Bu komutu üretilemiyor bir *Dockerfile* bu belirli proje oluşturmanız gerekmez.

Adlı projenize kökünde bir dosya oluşturun *Dockerfile* bu içeriğe sahip:

```Dockerfile
FROM openjdk:8-jdk-alpine
EXPOSE 8080
WORKDIR /app
COPY build/libs/spring-music-1.0.jar .
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
```

Ayrıca, güncelleştirmeye gerek duyduğunuz *configurations.develop.build* özelliğinde *azds.yaml* için *false*:
```yaml
...
configurations:
  develop:
    build:
      useGitIgnore: false
```

Ayrıca güncelleştirmeye gerek duyduğunuz *containerPort* özniteliğini *8080* içinde *charts/spring-music/templates/deployment.yaml*:

```yaml
...
spec:
  ...
  template:
    ...
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
```

AKS, uygulamayı dağıtmak için:

```cmd
$ azds up

Using dev space 'default' with target 'MyAKS'
Synchronizing files...1m 18s
Installing Helm chart...5s
Waiting for container image build...23s
Building container image...
Step 1/5 : FROM openjdk:8-jdk-alpine
Step 2/5 : EXPOSE 8080
Step 3/5 : WORKDIR /app
Step 4/5 : COPY build/libs/spring-music-1.0.jar .
Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
Built container image in 21s
Waiting for container...8s
Service 'spring-music' port 'http' is available at http://spring-music.1234567890abcdef1234.eastus.aksapp.io/
Service 'spring-music' port 8080 (TCP) is available at http://localhost:57892
press Ctrl+C to detach
...
```

Günlüklerde gösterilen URL'sine gidin. Önceki örnekte, kullanacağınız *http://spring-music.1234567890abcdef1234.eastus.aksapp.io/* . 

Değişikliklerinizi yanı sıra uygulama gördüğünüz doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Cosmos DB MongoDB kullanarak için MongoDB kullanarak mevcut bir uygulamayı güncelleştirme açıklanmıştır. Bu makalede, bu uygulamaya Azure geliştirme alanları ile AKS dağıtma ve Azure için açık hizmet aracısı kullanarak bir Cosmos DB hizmetini sağlamak yöntemleri de ele alınmıştır.

Cosmos DB, Azure ve Azure Dev alanları için açık hizmet Aracısı hakkında daha fazla bilgi için bkz:
* [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
* [Azure için açık hizmet Aracısı](https://osba.sh)
* [Geliştirme alanları ile geliştirin](../dev-spaces/azure-dev-spaces.md)
