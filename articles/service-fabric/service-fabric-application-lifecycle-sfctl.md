---
title: Azure Service Fabric CLI (sfctl) kullanarak Azure Service Fabric uygulamaları yönetme
description: Dağıtma ve uygulamaları, Azure Service Fabric CLI kullanarak bir Azure Service Fabric kümesinden kaldırma hakkında bilgi edinin
services: service-fabric
author: Christina-Kang
manager: timlt
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: bikang
ms.openlocfilehash: 2cbc5778385a5a4af3f6dc0306e2b943482bf40c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642895"
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric uygulaması Azure Service Fabric CLI (sfctl) kullanarak yönetme

Oluşturma ve bir Azure Service Fabric kümede çalışan uygulamaları silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Service Fabric CLI yükleyin. Ardından, Service Fabric kümesi seçin. Daha fazla bilgi için bkz: [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md).

* Dağıtılmaya hazır bir Service Fabric uygulama paketine sahip. Yazar ve bir uygulama paketi hakkında daha fazla bilgi için bilgiyi [Service Fabric uygulama modeli](service-fabric-application-model.md).

## <a name="overview"></a>Genel Bakış

Yeni bir uygulama dağıtmak için aşağıdaki adımları tamamlayın:

1. Bir uygulama paketi Service Fabric görüntü deposuna karşıya yükleyin.
2. Uygulama türü sağlayın.
3. Görüntü deposu içeriğini silin.
4. Bir uygulama oluşturmak ve belirtin.
5. Hizmetleri oluşturmak ve belirtin.

Var olan bir uygulamayı kaldırmak için aşağıdaki adımları tamamlayın:

1. Uygulamayı siler.
2. İlişkili uygulama türü sağlama.

## <a name="deploy-a-new-application"></a>Yeni bir uygulama dağıtma

Yeni bir uygulama dağıtmak için aşağıdaki görevleri tamamlayın:

### <a name="upload-a-new-application-package-to-the-image-store"></a>Yeni bir uygulama paketi görüntü deposuna karşıya yükleme

Bir uygulamayı oluşturmadan önce uygulama paketi Service Fabric görüntü deposuna karşıya yükleyin.

Örneğin, uygulama paketinizi ise `app_package_dir` dizin, dizinde karşıya yüklemek için aşağıdaki komutları kullanın:

```azurecli
sfctl application upload --path ~/app_package_dir
```

Büyük uygulama paketlerini için belirttiğiniz `--show-progress` karşıya yükleme ilerlemesini görüntülemek için seçeneği.

### <a name="provision-the-application-type"></a>Uygulama türü sağlama

Karşıya yükleme tamamlandığında, uygulama sağlayın. Uygulama sağlamak için aşağıdaki komutu kullanın:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

Değeri `application-type-build-path` uygulama paketinizi karşıya burada dizin adıdır.

### <a name="delete-the-application-package"></a>Uygulama paketi silme

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketleri görüntü deposundan silme sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanı kullanır ve uygulama performans sorunlarına yol açar. 

Uygulama paketi görüntü deposundan silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path` Uygulama oluşturduğunuzda, karşıya yüklediğiniz dizin adı olmalıdır.

### <a name="create-an-application-from-an-application-type"></a>Bir uygulama türü ile uygulama oluşturma

Uygulama sağlama sonra ad ve uygulamanızı oluşturmak için aşağıdaki komutu kullanın:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name` Uygulama örneği için kullanmak istediğiniz addır. Daha önceden hazırlanan uygulama bildirimden ek parametreler elde edebilirsiniz.

Uygulama adı öneki ile başlamalıdır `fabric:/`.

### <a name="create-services-for-the-new-application"></a>Yeni uygulama hizmetleri oluşturma

Bir uygulama oluşturduktan sonra uygulama hizmetleri oluşturun. Aşağıdaki örnekte, bizim uygulamadan yeni bir durum bilgisiz hizmet oluşturuyoruz. Bir uygulama oluşturun Hizmetleri hizmet bildirimi daha önceden hazırlanan uygulama paketinde tanımlanır.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Uygulama dağıtımı ve durumunu doğrulayın

Her şeyi sağlıklı doğrulamak için aşağıdaki sistem durumu komutları kullanın:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

Hizmet sağlıklı olduğunu doğrulamak için hizmet ve uygulama durumunu almak için benzer komutları kullanın:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

Sağlıklı hizmetler ve uygulamalar sahip bir `HealthState` değerini `Ok`.

## <a name="remove-an-existing-application"></a>Varolan bir uygulamayı kaldırma

Bir uygulamayı kaldırmak için aşağıdaki görevleri tamamlayın:

### <a name="delete-the-application"></a>Uygulama silme

Uygulamayı silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Uygulama türü sağlama

Uygulama sildikten sonra artık ihtiyacınız varsa uygulama türü sağlamasını. Uygulama türü sağlamayı kaldırmak için aşağıdaki komutu kullanın:

```azurecli
sfctl application unprovision --application-type-name TestAppType --application-type-version 1.0
```

Tür adı ve türü sürümü adı ve sürümü daha önceden hazırlanan uygulama bildiriminde eşleşmelidir.

## <a name="upgrade-application"></a>Uygulama yükseltme

Uygulamanızı oluşturduktan sonra ikinci bir sürümü, uygulamanızın sağlayacak adımları aynı kümesini yineleyebilirsiniz. Ardından, Service Fabric uygulama yükseltme işlemine ikinci uygulama sürümü için geçiş yapabilir. Daha fazla bilgi için belgelere bakın [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md).

Bir yükseltme gerçekleştirmek için önce öncekiyle aynı komutları kullanarak uygulamayı bir sonraki sürümünün sağlayın:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
sfctl store delete --content-path app_package_dir_2
```

Ardından önerilir izlenen otomatik bir yükseltme gerçekleştirmek için aşağıdaki komutu çalıştırarak yükseltmeyi başlatın:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Yükseltmeler ne olursa olsun kümesi belirtilen ile varolan parametreler geçersiz. Uygulama parametreleri bağımsız değişken olarak yükseltme komutuna gerekirse geçirilmesi gerekir. Uygulama parametreleri bir JSON nesnesi olarak kodlanmış.

Daha önce belirtilen herhangi bir parametre almak için kullanabileceğiniz `sfctl application info` komutu.

Uygulama yükseltme devam ederken, durum kullanılarak alınabilir `sfctl application upgrade-status` komutu.

Son olarak, bir yükseltme ilerleme durumu ve iptal edilmesi gereksinimlerini ise, kullanabileceğiniz `sfctl application upgrade-rollback` yükseltmeyi geri alma için.

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI temelleri](service-fabric-cli.md)
* [Service Fabric Linux'ta ile çalışmaya başlama](service-fabric-get-started-linux.md)
* [Service Fabric uygulama yükseltme başlatma](service-fabric-application-upgrade.md)
