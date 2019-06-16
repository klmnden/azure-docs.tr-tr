---
title: Azure Service Fabric uygulamanızı eclipse'teki hata ayıklama | Microsoft Docs
description: Geliştirme ve bunları Eclipse'te yerel geliştirme kümesinde hata ayıklama güvenilirliğini ve performansını hizmetlerinizi geliştirin.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 2f00636da2b29e7815569a683fdf51c6a4e3b0e0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393597"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Eclipse kullanarak Service Fabric Java uygulamanızı hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. İçindeki adımları izleyerek bir yerel geliştirme kümesi Başlat [Service Fabric geliştirme ortamınızı ayarlama](service-fabric-get-started-linux.md).

2. Uzaktan hata ayıklama parametreleriyle java işlemini başlatır, hata ayıklamak için istediğiniz hizmetin entryPoint.sh güncelleştirin. Bu dosya şu konumda bulunabilir: `ApplicationName\ServiceNamePkg\Code\entrypoint.sh`. Bu örnekte, hata ayıklama için 8001 numaralı bağlantı noktası ayarlanmıştır.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Hatası ayıklanmakta olan hizmet için çoğaltma sayısını veya örnek sayısını 1 olarak ayarlayarak uygulama bildirimini güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmaları önler. Örneğin, şu işlemleri izleyerek durum bilgisi olmayan hizmetler için `InstanceCount="1"` ayarını yapın ve durum bilgisi olan hizmetler için hedefi ve en az çoğaltma kümesi boyutlarını 1 olarak ayarlayın: `TargetReplicaSetSize="1" MinReplicaSetSize="1"`.

4. Uygulamayı dağıtın.

5. Eclipse IDE'de seçin **Çalıştır -> hata ayıklama yapılandırmaları -> Uzak Java uygulaması ve giriş bağlantı özellikleri** ve özelliklerini aşağıdaki gibi ayarlayın:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  İstenen noktalarda kesme noktaları ayarlayın ve uygulamanın hatasını ayıklama.

Uygulama kilitlenmesi durumunda coredumps etkinleştirmek isteyebilirsiniz. Yürütme `ulimit -c` bir Kabuğu'nda ve 0 döndürür sonra coredumps etkin değil. Sınırsız coredumps etkinleştirmek için aşağıdaki komutu yürütün: `ulimit -c unlimited`. Komutunu kullanarak durum da doğrulayabilirsiniz `ulimit -a`.  Coredump oluşturma yolu güncelleştirmek isterseniz, yürütme `echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern`. 

### <a name="next-steps"></a>Sonraki adımlar

* [Linux Azure Tanılama'yı kullanarak günlük toplama](service-fabric-diagnostics-how-to-setup-lad.md).
* [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
