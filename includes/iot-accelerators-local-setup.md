---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/17/2019
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: d4da1597ebed6c27cf6c12bab4a4e59be742c577
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54383112"
---
## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Uzaktan izleme kaynak kodu depoları, kaynak kodu ve mikro Hizmetleri Docker görüntülerini çalıştırmak için gereken Docker yapılandırma dosyalarını içerir.

Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gitmek için komut satırı ortamı kullanın. Daha sonra .NET deposu komutların ya da kopyalamak için aşağıdaki adımlardan birini çalıştırın:

.NET mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

Bu makalede .NET mikro Hizmetleri kullandığınız varsayılır. Ayrıca Java uygulamaları kullanılabilir vardır. Java mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Bu komutlar, mikro Hizmetleri yerel olarak çalıştırmak için kullandığınız komut dosyalarının yanı sıra tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro hizmetler Docker'da çalıştırma için kaynak kodu gerekmez ancak daha sonra çözüm Hızlandırıcısını değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Azure hizmetlerini dağıtmak için aşağıdaki betiği kullanın. Aşağıdaki betik örnekleri, bir Windows makinede .NET deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın.

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturma

Gerekli Azure kaynakları henüz oluşturduysanız, şu adımları izleyin:

1. Komut satırı ortamınızda gidin **\services\scripts\local\launch** kopyaladığınız deponun klasöründe.

1. Yüklemek için aşağıdaki komutları çalıştırın **bilgisayarları** CLI aracını kullanarak ve Azure hesabınızda oturum açın:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Çalıştırma **start.cmd** betiği. Betik için aşağıdaki bilgileri ister:
    * Bir çözüm adı.
    * Kullanılacak Azure aboneliği.
    * Kullanmak için Azure veri merkezi konumu.

    Betik, çözümünüzün adına ile Azure'da kaynak grubu oluşturur. Bu kaynak grubu, çözüm Hızlandırıcısını Azure kaynaklarını içerir. İlgili kaynaklara artık ihtiyacınız sonra bu kaynak grubunu silebilirsiniz.

    Betik ayrıca bir ön ek ortam değişkenlerini kümesi ekler **bilgisayarları** yerel makinenize. Docker kapsayıcıları ve mikro hizmet projeleri yerel olarak başlatıldığında, bu ortam değişkenlerinden yapılandırma değerlerine okuyun.

    > [!TIP]
    > Betik tamamlandığında, bu da adlı bir dosyaya ortam değişkenlerini kaydeder  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** . Gelecekteki çözüm Hızlandırıcı dağıtımları için bunları kullanabilirsiniz. Yerel makinenizde, herhangi bir ortam değişkenini değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.

1. Komut satırı ortamınızdan çıkın.

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynakları

Gerekli Azure kaynakları zaten oluşturduysanız, yerel makinenizde karşılık gelen ortam değişkenleri oluşturun. Bunlar, kaydedilebilir  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** dağıtım dosyasından. Yerel makinenizde ortam değişkenleri değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.