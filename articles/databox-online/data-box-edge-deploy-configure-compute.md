---
title: Azure Data Box Edge ile veri dönüştürme | Microsoft Docs
description: Data Box Edge'deki işlem rolünü yapılandırmayı ve verileri Azure'a göndermeden önce dönüştürmek için kullanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: c52c311f1e1cd1335ea5797eadacd0bc89e1b36c
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402124"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Öğretici: Azure veri kutusu Edge (Önizleme) ile verileri dönüştürün

Bu öğreticide, Azure veri kutusu Edge Cihazınızda bir işlem rolünü yapılandırma açıklanır. Bilgi işlem rolü yapılandırdıktan sonra veri kutusu Edge Azure'a göndermeden önce verileri dönüştürebilirsiniz.

Bu yordamı tamamlamak için yaklaşık 30-45 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure IOT hub'ı kaynak oluştur
> * İşlem rolünü ayarlama
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş ve bu çözümü dağıtın önce gözden [Azure Önizleme için hizmet koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="prerequisites"></a>Önkoşullar

Veri kutusu Edge Cihazınızda bir işlem rolünü kurmadan önce emin olun:

* Veri kutusu Edge cihazınıza açıklandığı etkinleştirildikten sonra [bağlanma, ayarlamak ve Azure veri kutusu Edge etkinleştirme](data-box-edge-deploy-connect-setup-activate.md).


## <a name="create-an-iot-hub-resource"></a>IoT Hub kaynağı oluşturma

Veri kutusu Edge üzerinde bir işlem rolü'kurmak ayarlamadan önce bir IOT hub'ı kaynak oluşturmanız gerekir.

Ayrıntılı yönergeler için [IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) sayfasına gidin. Veri kutusu Edge kaynağınız için kullanılan aynı abonelik ve kaynak grubunu kullanın.

![IoT Hub kaynağı oluşturma](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Bir uç bilgi işlem rolü ayarlanmadı, aşağıdaki uyarılar uygulayın:

- Herhangi bir Azure IOT cihazları veya Azure IOT Edge cihazlarının IOT hub'ı kaynak yok.
- Yerel Edge paylaşımları oluşturamazsınız. Paylaşım eklediğinize Edge işlemi için yerel paylaşım oluşturma seçeneği etkinleştirilmez.


## <a name="set-up-compute-role"></a>İşlem rolünü ayarlama

Uç bilgi işlem rolü Edge cihazında ayarlandığında, iki cihazı oluşturur: bir IOT cihaz ve bir IOT Edge cihazı. IOT hub'ı kaynak hem de görüntülenebilir.

Cihazda bilgi işlem rolü ayarlamak için aşağıdakileri yapın:

1. Select veri kutusu Edge kaynak Git **genel bakış**ve ardından **işlem rolünü kurmadan**. 

    ![Sol bölmede genel bağlantı](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    İsteğe bağlı olarak, gidebilirsiniz **modülleri** seçip **işlem yapılandırma**.

    !["Modülleri" ve "işlem yapılandırma" bağlantıları](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
1. Aşağı açılan listesinde seçin **IOT hub'ı kaynak** önceki adımda oluşturduğunuz.  
    Bu noktada, yalnızca Linux platformuna IOT Edge cihazınız için kullanılabilir. 
    
1. **Oluştur**’a tıklayın.

    ![Oluştur düğmesi](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
    İşlem rolünün oluşturulması birkaç dakika sürer. Bu sürümdeki bir hata nedeniyle işlem rolü oluşturulduktan sonra ekran yenilenmez. Uç bilgi işlem rolü yapılandırıldığını doğrulamak için Git **modülleri**.  

    !["Yapılandırma Edge işlem" cihaz listesi](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

1. Git **genel bakış** yeniden.  
    Ekrana bilgi işlem rolü yapılandırılmış olduğunu göstermek için güncelleştirilir.

    ![Bir işlem rolü ayarlayın](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
1. IOT Edge bilgi işlem rolü oluştururken kullandığınız hub'ı, Git **IOT cihazları**.  
    IOT cihaz şimdi etkinleştirildi. 

    !["IOT cihazları" sayfası](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

1. Sol bölmede seçin **IOT Edge**.  
    IOT Edge cihazı de etkinleştirilir.

    ![Bir işlem rolü ayarlayın](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
1. IoT Edge cihazını seçip tıklayın.  
    Bu IoT Edge cihazında bir Edge aracısı çalışmaktadır. 

    ![Cihaz Ayrıntıları sayfası](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

    Bu uç cihazda hiçbir özel modüller vardır, ancak artık özel bir modülü ekleyebilirsiniz. Özel bir modül oluşturma konusunda bilgi almak için Git [geliştirme bir C# veri kutusu Edge cihazınız için modül](data-box-edge-create-iot-edge-module.md).


## <a name="add-a-custom-module"></a>Özel modül ekleme

Bu bölümde oluşturduğunuz IOT Edge cihazı için özel bir modül Ekle [geliştirme bir C# modülü, veri kutusu Edge için](data-box-edge-create-iot-edge-module.md). 

Aşağıdaki yordam bir örnek burada Özel Modül Edge cihazdaki dosyaların yerel paylaşımından alır ve bunları bir bulut paylaşımı için cihazda taşır kullanır. Bulut paylaşımı, bulut paylaşımı ile ilişkili Azure depolama hesabına dosyaları ardından iter. 

1. Yerel bir paylaşımı, aşağıdakileri yaparak Edge cihazında ekleyin:

    a. Data Box Edge kaynağınızda **Paylaşımlar**'a gidin. 
    
    b. Seçin **Ekle paylaşımı**ve ardından paylaşımı adı sağlayın ve paylaşım türü seçin. 
    
    c. Yerel bir paylaşımı oluşturmak için Seç **Edge Yerel paylaşım olarak yapılandırma** onay kutusu. 
    
    d. Seçin **Yeni Oluştur** veya **var olanı kullan**ve ardından **Oluştur**.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Yerel bir NFS paylaşımına oluşturduysanız, dosya paylaşımına kopyalamak için aşağıdaki uzak eşitleme (rsync) komut seçeneği kullanın:

    `rsync --inplace <source file path> < destination file path>`

    Rsync komut hakkında daha fazla bilgi için Git [Rsync belgeleri](https://www.computerhope.com/unix/rsync.htm). 

    Yerel paylaşımı oluşturulur ve başarılı oluşturma bildirimi alırsınız. Paylaşım listesi güncelleştirildi, ancak tamamlanması paylaşımı oluşturmak için beklemeniz gerekir.
    
1. Paylaşımları listesine gidin. 

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
1. Yeni oluşturulan yerel paylaşım özelliklerini görüntülemek için seçin. 

1. İçinde **Edge için yerel bağlama noktası işlem modülleri** kutusunda, bu paylaşıma karşılık gelen değeri kopyalayın.  
    Modül dağıttığınızda bu yerel bağlama noktası kullanacaksınız.

    !["Yerel bağlama noktası Edge için işlem modülleri" kutusu](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
1. Veri kutusu Edge Cihazınızda oluşturulmuş mevcut bir bulut paylaşımı üzerinde **Edge için yerel bağlama noktası işlem modülleri** kutusunda, bu bulut paylaşımı için Edge işlem modüller için yerel bağlama noktası kopyalayın.  
    Modül dağıttığınızda bu yerel bağlama noktası kullanacaksınız.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

1. IOT Edge cihaza Özel Modül eklemek, IOT hub'ı kaynağınıza gidin ve ardından Git **IOT Edge cihazı**. 

1. Cihazı seçin ve sonra **cihaz ayrıntıları**seçin **modülleri ayarlama**. 

    ![Modülleri ayarlama bağlantı](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

1. Altında **Ekle modülleri**, aşağıdakileri yapın:

    a. Özel Modül için kapsayıcı kayıt defteri ayarları için ad, adres, kullanıcı adı ve parola girin.  
    Ad, adres ve listelenen kimlik bilgilerini modülleri ile eşleşen bir URL almak için kullanılır. Bu modülü dağıtmak için **Dağıtım modülleri** sayfasında **IoT Edge modülü**'nü seçin. Bu IOT Edge modülü, veri kutusu Edge cihazla ilişkilendirilmiş IOT Edge cihazı dağıtabileceğiniz bir docker kapsayıcısı var.

    ![Modülleri ayarlama sayfası](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    b. IOT Edge Özel Modül ayarlarını modülünüzde ve karşılık gelen kapsayıcı görüntüsünün resim URI'sini adını girerek belirtin. 
    
    ![IOT Edge özel modüller sayfası](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    c. İçinde **kapsayıcı oluşturma seçenekleri** kutusuna, Bulut ve Yerel paylaşım için önceki adımda kopyaladığınız Edge modülleri için yerel bağlama noktalarını girin.
    > [!IMPORTANT]
    > Kopyalanan yolları kullanın. yeni yollar oluşturabilirsiniz. Karşılık gelen yerel bağlama noktaları eşlenen **InputFolderPath** ve **OutputFolderPath** modülünde belirttiğiniz zaman, [modülü ile özel kodgüncelleştirildi](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
    İçinde **kapsayıcı oluşturma seçenekleri** kutusunda, aşağıdaki örnek yapıştırabilirsiniz: 
    
    ```
    {
        "HostConfig": {
        "Binds": [
        "/home/hcsshares/mysmblocalshare:/home/LocalShare",
        "/home/hcsshares/mysmbshare1:/home/CloudShare"
        ]
        }
    }
    ```

    Ayrıca tüm ortam değişkenleri burada modülünüzde için sağlar.

    ![Kapsayıcı oluşturma seçenekleri kutusu](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    d. Gerekirse, Gelişmiş Edge çalışma zamanı ayarlarını yapılandırın ve ardından **sonraki**.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
1.  Altında **yolları belirtin**, modüller arasında ayarlayın.  
    Bu örnekte, veri bulut paylaşımına gönderir ve yerel paylaşımının adını girin.

    Değiştirebilirsiniz *rota* aşağıdaki yol dizesi ile:       `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![Rota belirtme bölümü](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 

1. **İleri**’yi seçin. 

1.  Altında **gözden geçirin, dağıtım**tüm ayarları gözden geçirin ve ardından **Gönder** dağıtım için modül göndermek için.

    ![Modülleri ayarlama sayfası](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
    Bu eylem, aşağıdaki görüntüde gösterildiği gibi modülü dağıtımı başlar:

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Veri dönüştürme işlemini doğrulama ve verileri aktarma

Modül bağlı ve beklendiği gibi çalıştığından emin olmak için son adımdır bakın. IOT Edge cihazınızın IOT hub'ı kaynak modülü çalışma zamanı durumunu çalıştırıyor olmalıdır.

![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
Modül çalıştığını doğrulamak için aşağıdakileri yapın:

1. Modül seçin ve ardından kimlik modül İkizi görüntüleyin.  
    Sınır cihazı ve modülü için istemci durumu olarak görüntülenmesi gereken *bağlı*.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
    Modül çalışmaya başladıktan sonra veri kutusu uç kaynağında Edge modüllerinin listesi altında de görüntülenir. Eklenir modülü çalışma zamanı durumunu *çalıştıran*.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
1.  Hem yerel dosya Gezgini'nde, Bağlan ve bulut paylaşımları, önceden oluşturulmuş.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
1.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
    Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

    Veri, bulut paylaşımından sonra depolama hesabına gönderilir. Verileri görüntülemek için depolama Gezgini'ne gidin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Doğrulama işlemini tamamladınız.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * IoT Hub kaynağı oluşturma
> * İşlem rolünü ayarlama
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Veri kutusu Edge Cihazınızı yönetme konusunda bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](https://aka.ms/dbg-docs)


