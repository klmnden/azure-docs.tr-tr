---
title: "Azure Service Fabric uygulamanızı eclipse'te hata ayıklama | Microsoft Docs"
description: "Güvenilirliğini ve performansını hizmetlerinizi geliştirmek ve bunları Eclipse'te yerel geliştirme kümede hata ayıklama tarafından geliştirin."
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli;mikhegn
ms.openlocfilehash: 023b878706abf524b5a7939492937a92151f6035
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Java Service Fabric uygulamanızı Eclipse kullanarak hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. İçindeki adımları izleyerek yerel bir geliştirme kümesi Başlat [, Service Fabric geliştirme ortamını ayarlama](service-fabric-get-started-linux.md).

2. Uzaktan hata ayıklama parametrelerle java işlemi başlatır böylece hata ayıklamak için istediğiniz hizmet entryPoint.sh güncelleştirin. Bu dosya şu konumda bulunabilir: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Bu örnekte, hata ayıklama için bağlantı noktası 8001 ayarlanır.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Uygulama bildirimi örneği sayısını veya ayıklanacak hizmet için yineleme sayısı 1 olarak ayarlayarak güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmalarını önler. Örneğin, durum bilgisi olmayan hizmetler için ayarlar ``InstanceCount="1"`` ve durum bilgisi olan hizmetler hedef ayarlamak ve min çoğaltma boyutları gibi 1 olarak ayarlayın: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Uygulamayı dağıtın.

5. Eclipse IDE'de seçin **Çalıştır -> hata ayıklama yapılandırmaları uzak Java uygulaması -> ve giriş bağlantı özelliklerini** ve özelliklerini şu şekilde ayarlayın:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Uygulamada hata ayıklama ve istenen noktalarda kesme noktaları ayarlayın.

Uygulama kilitlenme, coredumps etkinleştirmek isteyebilirsiniz. Yürütme ``ulimit -c`` bir Kabuğu'nda ve 0 döndürür sonra coredumps etkin değil. Sınırsız coredumps etkinleştirmek için aşağıdaki komutu yürütün: ``ulimit -c unlimited``. Komutunu kullanarak durum da doğrulayabilirsiniz ``ulimit -a``.  Coredump oluşturma yolu güncelleştirmek istiyorsanız, yürütme ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Sonraki adımlar

* [Linux Azure Tanılama'yı kullanarak günlükleri toplamak](service-fabric-diagnostics-how-to-setup-lad.md).
* [İzleme ve Hizmetleri yerel olarak tanılamak](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
