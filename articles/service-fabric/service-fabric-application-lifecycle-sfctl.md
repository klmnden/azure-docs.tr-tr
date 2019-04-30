---
title: Azure Service Fabric CLI (sfctl) kullanarak Azure Service Fabric uygulamaları yönetme
description: Dağıtma ve uygulamaları, Azure Service Fabric CLI kullanarak bir Azure Service Fabric kümesinden kaldırma hakkında bilgi edinin
services: service-fabric
author: rockboyfor
manager: digimobile
ms.service: service-fabric
ms.topic: conceptual
origin.date: 07/31/2018
ms.date: 04/29/2019
ms.author: v-yeche
ms.openlocfilehash: 9b0f785a6a43f984708645084a8a8036326d3d24
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621386"
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) kullanarak bir Azure Service Fabric uygulamasını Yönet

Oluşturma ve bir Azure Service Fabric kümesinde çalışan uygulamaların silme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* Service Fabric CLI'sını yükleyin. Ardından, Service Fabric kümenizi seçin. Daha fazla bilgi için [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md).

* Bir Service Fabric uygulama paketini dağıtılmaya hazır olması. Yazar ve bir uygulama paketi hakkında daha fazla bilgi için okuyun [Service Fabric uygulama modelini](service-fabric-application-model.md).

## <a name="overview"></a>Genel Bakış

Yeni bir uygulamayı dağıtmak için aşağıdaki adımları tamamlayın:

1. Bir uygulama paketi, Service Fabric görüntü deposuna yükleyin.
2. Uygulama türü sağlayın.
3. Görüntü deposu içeriğini silin.
4. Bir uygulama oluşturun ve belirtin.
5. Belirtin ve hizmetler oluşturun.

Mevcut bir uygulamayı kaldırmak için aşağıdaki adımları tamamlayın:

1. Uygulamayı silin.
2. İlişkili uygulama türünün sağlamasını kaldırma.

## <a name="deploy-a-new-application"></a>Yeni bir uygulama dağıtma

Yeni bir uygulamayı dağıtmak için aşağıdaki görevleri tamamlayın:

### <a name="upload-a-new-application-package-to-the-image-store"></a>Görüntü deposu için yeni bir uygulama paketini karşıya yükleyin

Bir uygulama oluşturmadan önce uygulama paketi Service Fabric görüntü deposuna yükleyin.

Örneğin, uygulama paketinizi ise `app_package_dir` dizin, dizin karşıya yüklemek için aşağıdaki komutları kullanın:

```azurecli
sfctl application upload --path ~/app_package_dir
```

Büyük uygulama paketleri için belirttiğiniz `--show-progress` karşıya yükleme ilerlemesini görüntülemek için seçeneği.

### <a name="provision-the-application-type"></a>Uygulama türü sağlama

Karşıya yükleme tamamlandığında, uygulama sağlayın. Uygulama sağlamak için aşağıdaki komutu kullanın:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

Değeri `application-type-build-path` uygulama paketinizi yüklediğiniz dizinin adı.

### <a name="delete-the-application-package"></a>Uygulama paketini Sil

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmak önerilir.  Uygulama paketleri görüntü deposundan silme, sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanını kullanan ve uygulama performası sorunlarını için yol açar. 

Görüntü deposundan uygulama paketini silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path` uygulamayı oluştururken yüklediğiniz dizinin adı olmalıdır.

### <a name="create-an-application-from-an-application-type"></a>Bir uygulama türünden bir uygulama oluşturun

Uygulama sağladıktan sonra adı ve uygulamanızı oluşturmak için aşağıdaki komutu kullanın:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name` Uygulama örneği için kullanmak istediğiniz addır. Ek parametreler daha önce sağlanan uygulama bildirimindeki alabilirsiniz.

Uygulama adı ön ekine sahip başlamalıdır `fabric:/`.

### <a name="create-services-for-the-new-application"></a>Hizmetleri için yeni uygulama oluşturma

Bir uygulama oluşturduktan sonra uygulamadan hizmetler oluşturun. Aşağıdaki örnekte, bizim uygulamadan yeni bir durum bilgisi olmayan hizmet oluşturacağız. Bir hizmet bildiriminde daha önce sağlanan uygulama paketi dosyasından bir uygulama oluşturabilirsiniz Hizmetleri tanımlanır.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Uygulama dağıtımı ve durumunu doğrulayın

Her şey iyi durumda olmadığını doğrulamak için aşağıdaki sistem durumu komutları kullanın:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

Hizmetin iyi durumda olduğunu doğrulamak için hem hizmet hem de uygulama durumunu almak için benzer komutları kullanın:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

Sağlıklı hizmetler ve uygulamalar olması bir `HealthState` değerini `Ok`.

## <a name="remove-an-existing-application"></a>Mevcut bir uygulamayı kaldırma

Bir uygulamayı kaldırmak için aşağıdaki görevleri tamamlayın:

### <a name="delete-the-application"></a>Uygulamayı Sil

Uygulamayı silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Uygulama türünün sağlamasını kaldırma

Uygulama sildikten sonra artık ihtiyacınız kalmadığında uygulama türünün sağlamasını kaldırma. Uygulama türünün sağlamasını kaldırma için aşağıdaki komutu kullanın:

```azurecli
sfctl application unprovision --application-type-name TestAppType --application-type-version 1.0
```

Tür adı ve türü sürümü adı ve sürümü daha önce sağlanan uygulama bildiriminde eşleşmelidir.

## <a name="upgrade-application"></a>Uygulama yükseltme

Uygulamanızı oluşturduktan sonra aynı kümesini uygulamanızın ikinci bir sürümünü sağlamak için adımları tekrarlayabilirsiniz. Ardından, bir Service Fabric uygulama yükseltme ile uygulamanın ikinci sürümü çalışan geçiş yapabilirsiniz. Daha fazla bilgi için şirket belgelerine bakın. [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md).

Bir yükseltme gerçekleştirmek için önce gibi aynı komutları kullanarak uygulama'nın sonraki sürümü sağlayın:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
sfctl store delete --content-path app_package_dir_2
```

Ardından önerilir izlenen otomatik bir yükseltme gerçekleştirmek için aşağıdaki komutu çalıştırarak yükseltmeyi başlatın:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Yükseltmeler ne olursa olsun kümesi belirtilen ile mevcut parametreler geçersiz. Uygulama parametreleri bağımsız değişken olarak Yükselt komutu için gerekirse geçirilmelidir. Uygulama parametreleri bir JSON nesnesi olarak kodlanmış olmalıdır.

Daha önce belirtilen tüm parametreler almak için kullanabileceğiniz `sfctl application info` komutu.

Uygulama yükseltme devam ederken, durum kullanılarak alınabilir `sfctl application upgrade-status` komutu.

Son olarak, yükseltme ilerleme durumu ve iptal edilmesi gerekiyor, kullanabileceğiniz `sfctl application upgrade-rollback` yükseltmeyi geri alma için.

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI'sını temel bilgileri](service-fabric-cli.md)
* [Linux üzerinde Service Fabric ile çalışmaya başlama](service-fabric-get-started-linux.md)
* [Bir Service Fabric uygulama yükseltmesi başlatılıyor](service-fabric-application-upgrade.md)

<!--Update_Description: update meta properties -->