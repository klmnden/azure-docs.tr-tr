---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: v-krghan
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/25/2019
ms.author: v-krghan
ms.custom: include file
ms.openlocfilehash: 79fcadc75876af39d65dcfce88dac6802d55efd4
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59630587"
---
## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Uzaktan izleme kaynak kodu depoları, kaynak kodu ve mikro Hizmetleri Docker görüntülerini çalıştırmak için gereken Docker yapılandırma dosyalarını içerir.

Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gitmek için komut satırı ortamı kullanın. Ardından komutların java depoyu kopyalamak için aşağıdaki adımlardan birini çalıştırın:

Java mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Bu komutlar, mikro Hizmetleri yerel olarak çalıştırmak için kullandığınız komut dosyalarının yanı sıra tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro hizmetler Docker'da çalıştırma için kaynak kodu gerekmez ancak daha sonra çözüm Hızlandırıcısını değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Azure hizmetlerini dağıtmak için aşağıdaki betiği kullanın. Aşağıdaki betik örnekleri, bir Windows makinede java deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın.

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

     Betik ayrıca bir ön ek ortam değişkenlerini kümesi ekler **bilgisayarları** yerel makinenize. Bu ortam değişkenleri, bir Azure Key Vault kaynaktan okumak, Uzaktan izleme ayrıntılarını sağlayın. Uzaktan izleme kendi yapılandırma değerlerinden burada okuyacaksa bu Key Vault kaynaktır.

     > [!TIP]
     > Betik tamamlandığında, bu da adlı bir dosyaya ortam değişkenlerini kaydeder  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** . Gelecekteki çözüm Hızlandırıcı dağıtımları için bunları kullanabilirsiniz. Yerel makinenizde, herhangi bir ortam değişkenini değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.

1. Komut satırı ortamınızdan çıkın.

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynakları

Gerekli Azure kaynakları zaten oluşturduysanız, yerel makinenizde karşılık gelen ortam değişkenleri oluşturun.
Aşağıdaki ortam değişkenlerini ayarlayın:
* **PCS_KEYVAULT_NAME** -Azure Key Vault kaynak adı
* **PCS_AAD_APPID** -AAD uygulama kimliği
* **PCS_AAD_APPSECRET** -AAD uygulama gizli anahtarı

Bu Azure anahtar kasası kaynak yapılandırma değerlerini okur. Bu ortam değişkenlerini de kaydedilebilir  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** dağıtım dosyasından. Yerel makinenizde ortam değişkenleri değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.

Mikro hizmet tarafından gereken yapılandırma bazıları örneğinde depolanır **Key Vault** ilk dağıtımı oluşturuldu. Karşılık gelen anahtar kasası değişkenleri gerektiği şekilde değiştirilmesi gerekir.