---
title: Filtre, Azure veri kutusu Edge üzerinde işlem ile Gelişmiş dağıtımı için verileri çözümlemek için öğretici | Microsoft Docs
description: Veri kutusu Edge üzerinde işlem rolünü yapılandırma ve Azure'a göndermeden önce gelişmiş dağıtım akışı için verileri dönüştürmek için kullanma hakkında bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/13/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge for advanced deployment flow so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 836a3f3c17c4cf11ac972b7cc129fb3f83093024
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793956"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-for-advanced-deployment-flow"></a>Öğretici: Azure veri kutusu Edge gelişmiş dağıtım Flow ile verileri dönüştürün

Bu öğreticide, Azure veri kutusu Edge Cihazınızda bir gelişmiş dağıtım akışı için bir işlem rolü yapılandırma açıklanır. Bilgi işlem rolü yapılandırdıktan sonra veri kutusu Edge Azure'a göndermeden önce verileri dönüştürebilirsiniz.

İşlem, basit veya gelişmiş dağıtım akışı Cihazınızda için yapılandırılabilir.

|                  | Basit dağıtım                                | Gelişmiş dağıtım                   |
|------------------|--------------------------------------------------|---------------------------------------|
| Yönelik     | BT yöneticileri                                | Geliştiriciler                            |
| Tür             | Veri kutusu Edge hizmeti modüllerini dağıtmak için kullanın      | IOT Hub hizmeti modüllerini dağıtmak için kullanın |
| Dağıtılan modüller | Single                                           | Zincirleme veya birden çok modül           |


Bu yordamı tamamlamak için yaklaşık 20-30 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşımları ekleme
> * Tetikleyici ekleyin
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

 
## <a name="prerequisites"></a>Önkoşullar

Veri kutusu Edge Cihazınızda bir işlem rolünü kurmadan önce emin olun:

- Veri kutusu Edge cihazınıza açıklandığı etkinleştirildikten sonra [bağlanma, ayarlamak ve Azure veri kutusu Edge etkinleştirme](data-box-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>İşlemi yapılandırma

İşlem, veri kutusu Edge üzerinde yapılandırmak için bir IOT hub'ı kaynak oluşturacaksınız.

1. Azure portalında veri kutusu Edge kaynağınızın Git **genel bakış**. Sağ bölmede üzerinde **işlem** kutucuk seçin **başlama**.

    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-1.png)

2. Üzerinde **yapılandırma Edge işlem** kutucuk seçin **işlem yapılandırma**.

    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-2.png)

3. Üzerinde **yapılandırma Edge işlem** dikey penceresinde aşağıdakileri girin:

   
    |Alan  |Değer  |
    |---------|---------|
    |IoT Hub     | Aralarından seçim **yeni** veya **mevcut**. <br> Varsayılan olarak, bir standart katman (S1), bir IOT kaynak oluşturmak için kullanılır. Ücretsiz katman IOT kaynağı kullanmak için oluşturun ve ardından mevcut kaynağı seçin. <br> Her durumda, aynı abonelikte ve kaynak grubu, veri kutusu Edge kaynak tarafından kullanılan IOT hub'ı kaynak kullanır.     |
    |Ad     |IOT hub'ı kaynağınız için bir ad girin.         |

    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-3.png)

4. **Oluştur**’u seçin. IOT hub'ı kaynak oluşturulması birkaç dakika sürer. IOT hub'ı kaynak oluşturulduktan sonra **yapılandırma Edge işlem** kutucuğuna güncelleştirmeleri işlem yapılandırmasını göster. Uç bilgi işlem rolü yapılandırıldığını doğrulamak için şunu seçin **görünümü yapılandırma** üzerinde **işlem yapılandırma** Döşe.
    
    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-4.png)

    Uç bilgi işlem rolü Edge cihazında ayarlandığında, iki cihazı oluşturur: bir IOT cihaz ve bir IOT Edge cihazı. IOT hub'ı kaynak hem de görüntülenebilir. Bir IOT Edge çalışma zamanı, aynı zamanda bu IOT Edge cihaz üzerinde çalışıyor.

    Bu noktada, yalnızca Linux platformuna IOT Edge cihazınız için kullanılabilir.


## <a name="add-shares"></a>Paylaşımları ekleme

Bu öğreticide Gelişmiş dağıtımı için iki paylaşımını gerekir: bir kenar paylaşımı ve başka bir uç yerel paylaşımı.

1. Bir Edge paylaşımı aşağıdaki adımları uygulayarak cihazda ekleyin:

    1. Veri kutusu Edge kaynağınıza gidin **Edge işlem > başlama**.
    2. Üzerinde **paylaşımlar ekleme** kutucuk seçin **Ekle**.
    3. Üzerinde **Ekle paylaşımı** dikey penceresinde paylaşımı adı sağlayın ve paylaşım türü seçin.
    4. Edge paylaşımını bağlayabilmeniz için onay kutusunu seçin. **paylaşımı ile Edge işlem kullanmak**.
    5. Seçin **depolama hesabı**, **depolama hizmeti**, bir mevcut kullanıcı ve ardından **Oluştur**.

        ![Bir Edge paylaşım Ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-1.png)

    <!--If you created a local NFS share, use the following remote sync (rsync) command option to copy files onto the share:

    `rsync <source file path> < destination file path>`

    For more information about the rsync command, go to [Rsync documentation](https://www.computerhope.com/unix/rsync.htm).-->

    Edge paylaşım oluşturulduktan sonra bir oluşturma işlemi başarılı bildirim alacaksınız. Paylaşım listesi, yeni bir paylaşım yansıtacak şekilde güncelleştirilir.

2. Önceki adımda yer alan tüm adımları yinelenen ve onay kutusunu seçerek, Edge cihazında Edge Yerel paylaşım Ekle **Edge Yerel paylaşım olarak yapılandırma**. Veriye yerel cihazda kalır.

    ![Bir Edge Yerel paylaşım Ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-2.png)

3. Üzerinde **paylaşımları** dikey penceresinde, paylaşımları güncelleştirilmiş listesine bakın.

    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-3.png)

4. Yeni oluşturulan yerel paylaşım özelliklerini görüntülemek için listeden paylaşımı seçin. İçinde **Edge için yerel bağlama noktası işlem modülleri** kutusunda, bu paylaşıma karşılık gelen değeri kopyalayın.

    Modül dağıttığınızda bu yerel bağlama noktası kullanacaksınız.

    !["Yerel bağlama noktası Edge için işlem modülleri" kutusu](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-4.png)
 
5. Oluşturduğunuz Edge paylaşımının özelliklerini görüntülemek için listeden paylaşımı seçin. İçinde **Edge için yerel bağlama noktası işlem modülleri** kutusunda, bu paylaşıma karşılık gelen değeri kopyalayın.

    Modül dağıttığınızda bu yerel bağlama noktası kullanacaksınız.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-5.png)


## <a name="add-a-trigger"></a>Tetikleyici ekleyin

1. Git **Edge işlem > Tetikleyicileri**. Seçin **+ tetikleyicisi Ekle**.

    ![Tetikleyici ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-1.png)

2. İçinde **tetikleyicisi Ekle** dikey penceresinde aşağıdaki değerleri girin.

    |Alan  |Değer  |
    |---------|---------|
    |Tetikleyici adı     | Tetikleyici için benzersiz bir ad.         |
    |Tetikleyici türü     | Seçin **dosya** tetikleyici. Bir dosya tetikleyici, giriş paylaşıma yazılan bir dosya gibi bir dosya olay oluştuğunda etkinleştirilir. Zamanlanan bir tetikleyici Öte yandan, Yukarı sizin tanımladığınız bir zamanlamaya göre tetikler. Bu örnekte, bir dosya tetikleyici ihtiyacımız var.    |
    |Giriş paylaşımı     | Bir giriş paylaşımı seçin. Edge yerel paylaşımı giriş bu durumda paylaşımıdır. Burada kullanılan modül dosyaları Edge yerel paylaşımından bir kenar paylaşımına buluta burada karşıya taşır.        |

    ![Tetikleyici ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-2.png)

3. Tetikleyici oluşturulduktan sonra size bildirilir. Tetikleyiciler listesinde yeni oluşturulan tetikleyici görüntülemek için güncelleştirilir. Yeni oluşturduğunuz tetikleyicisini seçin.

    ![Tetikleyici ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-3.png)

4. Kopyalamak ve örnek yol kaydedin. Bu örnek yolu değiştirmek ve daha sonra IOT hub'ı kullanın.

    `"sampleroute": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/modulename/inputs/input1\")"`

    ![Tetikleyici ekle](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-4.png)

## <a name="add-a-module"></a>Modül Ekle

Bu uç cihazda hiçbir özel modüller vardır. Özel bir ya da önceden oluşturulmuş modülüne ekleyebilirsiniz. Özel bir modül oluşturma konusunda bilgi almak için Git [geliştirme bir C# veri kutusu Edge cihazınız için modül](data-box-edge-create-iot-edge-module.md).

Bu bölümde oluşturduğunuz IOT Edge cihazı için özel bir modül Ekle [geliştirme bir C# modülü, veri kutusu Edge için](data-box-edge-create-iot-edge-module.md). Bu özel modül sınır cihazı Edge yerel paylaşımından dosyaları alır ve bunları bir kenarı (bulut) paylaşımına cihaza taşır. Bulut paylaşımı, bulut paylaşımı ile ilişkili Azure depolama hesabına dosyaları ardından iter.

1. Git **Edge işlem > başlama**. Üzerinde **modül eklemek** olarak senaryo türünü seçin, döşeme **Gelişmiş**. Seçin **IOT Hub'ına gidin**.

    ![Gelişmiş dağıtım seçin](./media/data-box-edge-deploy-configure-compute-advanced/add-module-1.png)

<!--2. In the **Configure and add module** blade, input the following values:  

    |Input share     | Select an input share. The Edge local share is the input share in this case. The module used here moves files from the Edge local share to an Edge share where they are uploaded into the cloud.        |
    |Output share     | Select an output share. The Edge share is the output share in this case.        |
-->

2. IOT hub'ı kaynağınıza gidin **IOT Edge cihazı** ve IOT Edge Cihazınızı seçin.

    ![IOT Edge cihazı IOT hub'ında gidin](./media/data-box-edge-deploy-configure-compute-advanced/add-module-2.png)

3. Üzerinde **cihaz ayrıntıları**seçin **modülleri ayarlama**.

    ![Modülleri ayarlama bağlantı](./media/data-box-edge-deploy-configure-compute-advanced/add-module-3.png)

4. Altında **Ekle modülleri**, aşağıdakileri yapın:

    1. Özel Modül için kapsayıcı kayıt defteri ayarları için ad, adres, kullanıcı adı ve parola girin.
    Ad, adres ve listelenen kimlik bilgilerini modülleri ile eşleşen bir URL almak için kullanılır. Bu modülü dağıtmak için **Dağıtım modülleri** sayfasında **IoT Edge modülü**'nü seçin. Bu IOT Edge modülü, veri kutusu Edge cihazla ilişkilendirilmiş IOT Edge cihazı dağıtabileceğiniz bir docker kapsayıcısı var.

        ![Modülleri ayarlama sayfası](./media/data-box-edge-deploy-configure-compute-advanced/add-module-4.png) 
 
    2. IoT Edge özel modül ayarlarını belirtin. Aşağıdaki değerleri girin.
     
        |Alan  |Değer  |
        |---------|---------|
        |Ad     | Modül için benzersiz bir ad. Veri kutusu Ucunuzdaki ile ilişkili IOT Edge cihazı dağıtabileceğiniz bir docker kapsayıcısı modülüdür.        |
        |Görüntü URI'si     | Görüntü URI'si modülü için karşılık gelen kapsayıcı görüntüsünün.        |
        |Kimlik bilgileri gerekli     | Bu onay kutusu işaretlendiğinde, kullanıcı adı ve parola modülleri ile eşleşen bir URL almak için kullanılır.        |
    
        İçinde **kapsayıcı oluşturma seçenekleri** kutusuna, Edge Paylaş ve Edge Yerel paylaşım için önceki adımda kopyaladığınız Edge modülleri için yerel bağlama noktalarını girin.

        > [!IMPORTANT]
        > Hangi işlevleri kapsayıcınızda bekliyor eşleşmelidir için burada kullanılan yolların, kapsayıcıya bağlanır. Sizi takip ediyorsanız [özel bir modül oluşturma](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code), kopyalanan yolları, modülü belirtilen kodu bekliyor. Bu yolları değiştirmeyin.
    
        İçinde **kapsayıcı oluşturma seçenekleri** kutusunda, aşağıdaki örnek yapıştırabilirsiniz:
    
        ```
        {
            "HostConfig": {
            "Binds": [
            "/home/hcsshares/mydbesmbedgelocalshare1:/home/input",
            "/home/hcsshares/mydbesmbedgeshare1:/home/output"
            ]
            }
        }
        ```

        Modülünüzün için kullanılan herhangi bir ortam değişkenlerini belirtin. Ortam değişkenlerini sağlayan isteğe bağlı bilgileri Yardım modülünüzde çalıştığı ortamı tanımlar.

        ![Kapsayıcı oluşturma seçenekleri kutusu](./media/data-box-edge-deploy-configure-compute-advanced/add-module-5.png) 
 
    4. Gerekirse, Gelişmiş Edge çalışma zamanı ayarlarını yapılandırın ve ardından **sonraki**.

        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-module-6.png)
 
5.  Altında **yolları belirtin**, modüller arasında ayarlayın.  
    
    ![Yolları belirtin](./media/data-box-edge-deploy-configure-compute-advanced/add-module-7.png)

    Değiştirebilirsiniz *rota* ile daha önce kopyaladığınız aşağıdaki yol dizesi. Bu örnekte, veri bulut paylaşımına gönderir ve yerel paylaşımının adını girin. Değiştirin `modulename` modülü adı. **İleri**’yi seçin.
        
    ```
    "route": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/filemove/inputs/input1\")"
    ```

    ![Rota belirtme bölümü](./media/data-box-edge-deploy-configure-compute-advanced/add-module-8.png)

6.  Altında **gözden geçirin, dağıtım**tüm ayarları gözden geçirin ve ardından **Gönder** dağıtım için modül göndermek için.

    ![Modülleri ayarlama sayfası](./media/data-box-edge-deploy-configure-compute-advanced/add-module-9.png)
 
    Bu eylem modülü dağıtımı başlatır. Dağıtım tamamlandıktan sonra **çalışma zamanı durumu** modül **çalıştıran**.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-module-10.png)

## <a name="verify-data-transform-transfer"></a>Veri doğrulama dönüştürme, Aktarım

Modül bağlı ve beklendiği gibi çalıştığından emin olmak için son adımdır bakın. IOT Edge cihazınızın IOT hub'ı kaynak modülü çalışma zamanı durumunu çalıştırıyor olmalıdır.

Veri dönüştürme ve azure'a aktarım doğrulamak için aşağıdaki adımları uygulayın.
 
1.  Dosya Gezgini'nde, hem yerel Edge hem de daha önce oluşturduğunuz uç paylaşımları için bağlanın.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-2.png)
 
1.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-3.png)
 
    Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-4.png)  

    Veri, bulut paylaşımından sonra depolama hesabına gönderilir. Verileri görüntülemek için depolama hesabınıza gidin ve ardından **Depolama Gezgini**. Karşıya yüklenen veriler, depolama hesabınızdaki görüntüleyebilirsiniz.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-5.png)
 
Doğrulama işlemini tamamladınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşımları ekleme
> * Tetikleyici ekleyin
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Veri kutusu Edge Cihazınızı yönetme konusunda bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](data-box-edge-manage-access-power-connectivity-mode.md)
