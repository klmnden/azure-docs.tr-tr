---
title: Bir açık hizmet aracısı için Azure veritabanı ile Pivotal Cloud Foundry üzerinde bir uygulama kullanma
author: zr-msft
manager: jeconnoc
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 01/11/2019
ms.topic: tutorial
description: İçin Azure veritabanı kullanan bir açık hizmet aracısı için Pivotal Cloud Foundry uygulamasının nasıl yapılandırılacağını açıklar
keywords: Pivotal Cloud Foundry, Cloud Foundry açık hizmet Aracısı, Azure için açık hizmet Aracısı
tags: Cloud-Foundry
ms.openlocfilehash: d553cd09ef42e47e3a10fb96039063b8aae665cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60197230"
---
# <a name="tutorial-use-an-open-service-broker-for-azure-database-with-an-application-on-pivotal-cloud-foundry"></a>Öğretici: Bir açık hizmet aracısı için Azure veritabanı ile Pivotal Cloud Foundry üzerinde bir uygulama kullanma

Pivotal Cloud Foundry örneği ile Azure için açık hizmet aracısı kullanarak doğrudan azure'da Cloud Foundry CLI ve Pivotal Cloud Foundry örneğinizin veritabanları gibi sağlama hizmetleri sağlar. Bu hizmetler, Pivotal Cloud Foundry Örneğinizde çalışan bir uygulamaya da bağlayabilirsiniz. Bu şekilde uygulamaya bir hizmete bağladığınızda, herhangi bir kod veya yapılandırma uygulamanızda güncelleştirmek gerekmez. Bu makalede, bir uygulamada Pivotal Cloud Foundry ile bir veritabanı için Azure hizmeti için bir açık hizmet Aracısı kullanmayı açıklar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama alanınızda Pivotal Cloud Foundry hazırlayın.
> * Github'dan örnek bir uygulama kaynağını kopyalama.
> * Uygulama dağıtımı için hazırlayın.
> * Uygulamayı dağıtın.
> * Azure için açık hizmet aracısı kullanarak bir veritabanı oluşturun.
> * Veritabanı uygulamanıza bağlayın.

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce şunları yapmalısınız:

* [Pivotal Cloud Foundry yüklenmiş ve yapılandırılmış](create-cloud-foundry-on-azure.md)
* [Azure'nın yüklü ve yapılandırılmış için açık hizmet Aracısı sahip](https://network.pivotal.io/products/azure-open-service-broker-pcf) Cloud Foundry örneğinizle

Yüklenmiş ve yapılandırılmış Azure kutucuğu için açık hizmet Aracısı ile Pivotal Cloud Foundry Ops Manager ekranın bir örnek aşağıda verilmiştir:

![Yüklü Azure için açık hizmet Aracısı ile Pivotal Cloud Foundry](media/pcf-ops-manager.png)

## <a name="prepare-your-application-space-in-pivotal-cloud-foundry"></a>Uygulama alanınızda Pivotal Cloud Foundry hazırlama

Pivotal Cloud Foundry Örneğinize uygulamanızı dağıtmak için ile oturum açmış olmanız gerekir `cf` komut satırı aracı. Ayrıca, istenen kuruluşunuz ve hedeflenen alanı olmalıdır.

Oturum açtınız ve görüntüleme alanı hedefleme doğrulamak için `cf target`. Aşağıdaki örnekte zaten olarak oturum açmış kullanıcının gösterir *yönetici*kullanarak *myorg* kuruluş ve hedefleme *geliştirme* alanı:

```cmd
$ cf target

api endpoint:   https://api.system.40.85.111.222.cf.pcfazure.com
api version:    2.120.0
user:           admin
org:            myorg
space:          dev
```

Oturum açmak için kullandığınız `cf login`:

```cmd
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Yeni bir kuruluş ve alanı oluşturmak için kullanın `cf create-org` ve `cf create-space`:

```cmd 
cf create-org myorg
cf create-space dev -o myorg
```

Bir alanı hedeflemek için kullanın `cf target`:

```cmd
cf target -o myorg -s dev
```

## <a name="get-application-code"></a>Uygulama kodunu alma

Azure için açık hizmet aracısı kullanmak bir uygulama için uygulama bir veritabanı gibi bir veri kaynağı olarak kullanmak gerekir. Bu makalede, kullanacağız [müzik örnek uygulama Cloud Foundry spring](https://github.com/cloudfoundry-samples/spring-music) bir veri kaynağı kullanan bir uygulamayı göstermek için.

Uygulamasını github'dan kopyalayın ve alt dizinine gidin:

```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring music
```

> [!TIP]
> Bu uygulama için veri kaynaklarını görmek için [src/main/resources/application.yml](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/application.yml) dosya.


## <a name="prepare-the-application-for-deployment"></a>Uygulama dağıtımı için hazırlama

Pivotal Cloud Foundry Örneğinize uygulamayı dağıtmadan önce oluşturmanız gerekir. Gösterim amaçlı için de bazı etkinleştirdik *hata ayıklama* veri kaynağı bağlantı bilgileri başlar.SSH günlüğe kaydetme.

Etkinleştirmek için *hata ayıklama* eklemek için veritabanı bağlantı ayrıntıları günlüğe kaydetme, yaml özelliği sonuna aşağıdaki *application.yml*:

```yaml
---
logging:
  level:
    com.zaxxer.hikari: DEBUG
```

Örnek uygulama, uygulamaya bir Spring Boot çalıştırılabilir jar dosyasını derlemek için gradle kullanır. Çalıştırılabilir jar Pivotal Cloud Foundry Örneğinize dağıtılır. Uygulamayı oluşturmak için:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```


## <a name="deploy-your-application"></a>Uygulamanızı dağıtma

Kullanım `cf push` Pivotal Cloud Foundry Örneğinize uygulamayı dağıtmak için komut:

```cmd
cf push

Pushing from manifest to org myorg / space dev as admin...
Using manifest file /path/to/spring-music/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:       spring-music
...
Waiting for app to start...

name:              spring-music
requested state:   started
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
     state     since                  cpu    memory         disk           details
#0   running   2018-12-06T21:24:06Z   0.0%   313.3M of 1G   170.7M of 1G
```

Değeri Şuradan Kopyala: *yollar* out görüntülenen `cf push`. Çalışan uygulamaya erişmek için kullanacağı URL'yi yoldur. Örneğin:

```cmd
...
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
```


Uygulamanız dağıtıldıktan sonra uygulama tarafından kullanılan bağlantı URL'sini görmek için uygulama günlüklerini görüntüleyebilirsiniz. Komut uygulama günlükleri görüntüler ve kullanır `grep` aranacak *jdbcUrl* yapılandırma.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
```

Uygulamayı kullanarak fark *h2:mem:testdb*, bellek içi veritabanı. Spring uygulaması bir bellek içi veritabanına bağımlılık sınıf üzerinde bir bellek içi veritabanı kullanmak için otomatik olarak yapılandırılır ve [otomatik yapılandırma](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-auto-configuration.html) etkinleştirilir. Örnek uygulama [yapılandırılmış h2 bellek içi veritabanına bağımlılık](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L49) ve otomatik yapılandırma etkin [src/main/java/org/cloudfoundry/samples/music/Application.java](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/java/org/cloudfoundry/samples/music/Application.java#L8).

Bir tarayıcıda gitmek için uygulamanın yolu kullanın. Çıktıda yol ya da URL görüntülenen `cf push` komutu.

> [!TIP]
> Çalıştırarak uygulamanın URL'si görüntüleyebilirsiniz `cf apps`.

Tarayıcınızı kullanarak uygulamayı gittikten sonra silerek etkileşim birkaç mevcut Albümler ve birkaç yenilerini oluşturma. Örnek uygulama, değişikliklerinizi kaydetmek için bellek içi veritabanına kullanıyor. Bazı uygulama doldurulduğunda göreceksiniz [varsayılan veri](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). 

![Varsayılan verilerle Spring Music uygulaması](media/music-app.png)

Uygulamanızı bir bellek içi veritabanı kullandığından, uygulamanızı yeniden ya da yeniden değişiklikleriniz kalıcı olmaz. Bazı değişiklikler yaptıktan sonra değişiklikleri, kalıcı değil olduğunu göstermek için kullanarak uygulamanızı restage `cf restage`:

```cmd
cf restage spring-music
```

Aynı URL'yi kullanarak bir tarayıcıda, uygulama hazırlığı yapılan sonra ona gidin. Yaptığınız değişiklikleri kalkar dikkat edin ve varsayılan veriler görüntülenir.

![Varsayılan verilerle Spring Music uygulaması](media/music-app.png)

## <a name="create-a-database"></a>Veritabanı oluşturma

Azure'da açık hizmet Aracısı'nı kullanarak kalıcı bir veritabanı oluşturmak için kullanın `cf create-service` komutu. Azure'da PostgreSQL veritabanı kaynak grubunda aşağıdaki komutunu sağlar *MyResourceGroup* içinde *eastus* bölge. Daha fazla bilgi *resourceGroup*, *konumu*, ve diğer Azure özgü JSON parametreleri kullanılabilir [PostgreSQL modülünü başvuru belgeleri](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md#provision):

```cmd
cf create-service azure-postgresql-9-6 general-purpose mypgsql -c '{"resourceGroup":"MyResourceGroup", "location":"eastus"}'
```

Adlı bir hizmet olarak sunulan veritabanı Pivotal Cloud Foundry Örneğinizde *mypgsql*. Veritabanınızı sağlama, bu işlem birkaç dakika içinde tamamlandıktan sonra uygulamanızı bağlayabilirsiniz. Veritabanınızı sağlama tamamlandığını doğrulamak için `cf services` komutu:

```cmd
cf services

Getting services in org myorg / space dev as admin...

name      service                plan              bound apps     last operation
mypgsql   azure-postgresql-9-6   general-purpose                  create succeeded
```

*Son işlem* hizmetiniz için değer *başarılı oluşturma* ne zaman tamamlandığının sağlama.

## <a name="bind-the-database-to-your-application"></a>Uygulamanızı veritabanına bağlama

Kullanım `bind-service` hizmet uygulamanızı bağlamak için komutu.

```cmd
cf bind-service spring-music mypgsql
```

Uygulamanız için hizmet bağladıktan sonra değişikliklerin etkili olması uygulamanın restage gerekir.

```cmd
cf restage spring-music
```

Uygulama [Spring Cloud bağlayıcıları kullanmak üzere yapılandırılmış](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L45...L46), kullanılacak Pivotal Cloud Foundry örneğinizin sağlayan [otomatik yeniden yapılandırma](https://docs.cloudfoundry.org/buildpacks/java/configuring-service-connections/spring-service-bindings.html#auto) uygulamanızın veri kaynağında. Bu durumda, uygulamanızı hazırlığı yapılan zaman Pivotal Cloud Foundry Örneğinize bir hizmeti kullanmak için uygulamanızı otomatik olarak bir veri kaynağı için kendi bağlanan yeniden.

Uygulamanızı hazırlığı yapılan sonra kullanacağınız *mypgsql* bellek içi veritabanı yerine veri depolamak için.

Yaptığınız tüm değişiklikler artık yeniden başlatmaları arasında kalıcıdır ve yeniden dağıtır. Ayrıca, uygulama bağlantı URL'sini yeniden uygulama günlüklerini görüntüleyerek kullanarak da görebilirsiniz.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
2018-12-07T14:48:58.10-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:48:58.107 DEBUG 16 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:postgresql://12345678-aaaa-bbbb-cccc-1234567890ab.postgres.database.azure.com:5432/123456789?user=aabbcc1122@12345678-aaaa-bbbb-cccc-1234567890ab&password=<masked>&&sslmode=require
```

İki giriş olduğuna dikkat edin:

* Özgün değeri *h2:mem:testdb*.
* Uygulamanızı hazırlığı yapılan, PostgreSQL veritabanınız için güncelleştirilmiş değeri.

PostgreSQL veritabanı'na kalıcı verileri doğrulamak için uygulamayı tarayıcınızda giderek, bazı değişiklikler yapmanız ve uygulamanızı restage:

```cmd
cf restage spring-music
```

Aynı URL'yi kullanarak bir tarayıcıda, uygulama hazırlığı yapılan sonra ona gidin. Yaptığınız değişiklikleri yine de olduğuna dikkat edin.

## <a name="cleanup"></a>Temizleme

Uygulamanızı veritabanı bağlantısını kesmek istiyorsanız, bu kullanarak bağlantısını `cf unbind-service` komutu.

```cmd
cf unbind-service spring-music mypgsql
```

Benzer şekilde, uygulamanız bir hizmete bağlama, uygulamanız bu değişikliklerin etkili olabilmesi için restage gerekir. Bu eylem, her ikisi de bırakır *mypgsql* ve uygulamanızı olduğu gibi ancak uygulamanızı başlayacak bellek içi veritabanı yerine kullanarak *mypgsql*.

Veritabanınızı silmek için kullanabileceğiniz `cf delete-service` komutu. *Bu geri alamazsınız eylem kadar devam etmeden önce veritabanınızı silmek istediğinizden emin olun.*

```cmd
cf delete-service mypgsql
```

Uygulamanız, Pivotal Cloud Foundry örneğinden kaldırmak için:
 
```cmd
cf delete spring-music
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Pivotal Cloud Foundry için bir uygulama dağıtımının yanı sıra Azure için açık hizmet aracısı kullanarak veritabanı oluşturma kapsamında. Pivotal Cloud Foundry örneğinizin uygulamanızdaki veritabanınıza bağlama de kapsar. Azure'da Cloud Foundry uygulamaları dağıtma hakkında daha fazla bilgi için bkz:

* [Azure'da cloud Foundry](../virtual-machines/linux/cloudfoundry-get-started.md)
* [Microsoft Azure üzerinde Cloud Foundry için ilk uygulamanızı dağıtma](../virtual-machines/linux/cloudfoundry-deploy-your-first-app.md)