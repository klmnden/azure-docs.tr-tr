---
title: Azure Data Box Edge ile veri dönüştürme | Microsoft Docs
description: Data Box Edge'deki işlem rolünü yapılandırmayı ve verileri Azure'a göndermeden önce dönüştürmek için kullanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 4729e08399132243543c6f4e1cadd537d185e9e3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166262"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Öğretici: Azure Data Box Edge ile veri dönüştürme (Önizleme)

Bu öğreticide Data Box Edge'deki işlem rolünü yapılandırma adımları açıklanmaktadır. İşlem rolü yapılandırıldıktan sonra Data Box Edge verileri Azure'a göndermeden önce dönüştürebilir.

Bu yordamın tamamlanması 30-45 dakika kadar sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IoT Hub kaynağı oluşturma
> * İşlem rolünü ayarlama
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
 
## <a name="prerequisites"></a>Ön koşullar

Data Box Edge'deki işlem rolünü ayarlamadan önce şunları yaptığınızdan emin olun:

* Data Box Edge cihazınız [Azure Data Box Edge cihazınızı bağlama ve etkinleştirme](data-box-edge-deploy-connect-setup-activate.md) sayfasında açıklanan şekilde etkinleştirildi.


## <a name="create-an-iot-hub-resource"></a>IoT Hub kaynağı oluşturma

Data Box Edge'deki işlem rolünü ayarlamadan önce bir IoT Hub kaynağı oluşturmanız gerekir.

Ayrıntılı yönergeler için [IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) sayfasına gidin. Data Box Edge kaynağınız için kullandığınız aboneliği ve kaynak grubunu seçin.

![IoT Hub kaynağı oluşturma](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Edge işlem rolü ayarlanmadığında: 
- IoT Hub kaynağına IoT cihazı veya IoT Edge cihazı eklenemez.
- Yerel Edge paylaşımları oluşturamazsınız. Paylaşım eklediğinize Edge işlemi için yerel paylaşım oluşturma seçeneği etkinleştirilmez.


## <a name="set-up-compute-role"></a>İşlem rolünü ayarlama

Edge cihazında Edge işlem rolü ayarlandığında bir IoT cihazı ve bir IoT Edge cihazı olmak üzere iki cihaz oluşturulur. Bu cihazların ikisi de IoT Hub kaynağında görüntülenebilir.

Cihazda işlem rolünü ayarlamak için aşağıdaki adımları gerçekleştirin.

1. Data Box Edge kaynağına gidip **Genel Bakış** ve **İşlem rolünü ayarla**'ya tıklayın. 

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    **Modüller** sayfasına gidip **İşlemi yapılandırma**'ya da tıklayabilirsiniz.

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
2. Açılan listeden bir önceki adımda oluşturduğunuz **IoT Hub kaynağını** seçin. Şu an için IoT Edge cihazınızda yalnızca Linux platformunu kullanabilirsiniz. **Oluştur**’a tıklayın.

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
3. İşlem rolünün oluşturulması birkaç dakika sürer. Bu sürümdeki bir hata nedeniyle işlem rolü oluşturulduktan sonra ekran yenilenmez. **Modüller**'e gittiğinizde Edge işleminin yapılandırılmış olduğunu görebilirsiniz.  

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

4. Yeniden **Genel Bakış** sayfasına gittiğinizde ekranın işlem rolünün yapılandırılmış olduğunu gösterecek şekilde güncelleştirildiğini görebilirsiniz.

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
5. Edge işlem rolünü oluştururken kullandığınız IoT Hub'a gidin. **IoT cihazları** sayfasına gidin. Etkin bir IoT cihazı olduğunu görebilirsiniz. 

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

6. **IoT Edge** sayfasına gittiğinizde bir IoT Edge cihazının da etkinleştirilmiş olduğunu görebilirsiniz.

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
7. IoT Edge cihazını seçip tıklayın. Bu IoT Edge cihazında bir Edge aracısı çalışmaktadır. 

    ![İşlem rolünü ayarlama](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

Ancak bu Edge cihazında özel modül yoktur. Şimdi bu cihaza özele modül ekleyebilirsiniz.


## <a name="add-a-custom-module"></a>Özel modül ekleme

Bu bölümde IoT Edge cihazına özel modül ekleyeceksiniz. 

Bu yordamda kullanılan örnekte özel modül Edge cihazındaki yerel paylaşımdan dosya alıp bunları cihazdaki bir bulut paylaşımına taşımaktadır. Bulut paylaşımı ardından dosyaları bulut paylaşımıyla ilişkilendirilmiş olan Azure depolama hesabına göndermektedir. 

1. İlk adım, Edge cihazına yerel paylaşım eklemektir. Data Box Edge kaynağınızda **Paylaşımlar**'a gidin. **+ Paylaşım ekle**'ye tıklayın. Paylaşım adını girin ve paylaşım türünü seçin. Yerel paylaşım oluşturmak için **Edge yerel paylaşımı olarak yapılandır** seçeneğini işaretleyin. **Var olan kullanıcı** veya **yeni oluştur** seçeneğini belirleyin. **Oluştur**’a tıklayın.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Yerel NFS paylaşımı oluşturduysanız paylaşıma dosya kopyalamak için aşağıdaki rsync komutu seçeneğini kullanın:

    `rsync --inplace <source file path> < destination file path>`

     Rsync komutu hakkında daha fazla bilgi için bkz. [Rsync belgeleri](https://www.computerhope.com/unix/rsync.htm). 

 
2. Yerel paylaşım oluşturulduktan ve oluşturma işleminin başarılı olduğunu gösteren bildirimi gördükten sonra (paylaşım listesi güncelleştirilebilir ancak oluşturma işleminin tamamlanmasını beklemeniz gerekir) paylaşım listesine gidin. 

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
3. Yeni oluşturulan yerel paylaşımı seçip tıklayın ve paylaşımın özelliklerini görüntüleyin. Bu paylaşıma ait olan **Edge modülleri için yerel bağlama noktasını** kopyalayıp kaydedin.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
    Data Box Edge cihazınızda oluşturulmuş olan mevcut bulut paylaşımına gidin. Bu bulut paylaşımı için de yerel Edge işlemi bağlama noktasını kopyalayıp kaydedin. Bu yerel bağlama noktaları modülün dağıtılması sırasında kullanılacaktır.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

4. IoT Edge cihazına özel modül eklemek için IoT Hub kaynağınıza ve ardından **IoT Edge cihazına** gidin. Cihazı seçip tıklayın. **Cihaz ayrıntıları** bölümünde üstteki komut çubuğundan **Modülleri Ayarla**'ya tıklayın. 

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

5. **Modül Ekle** bölümünde aşağıdaki adımları izleyin:

    1. Özel modülün **Kapsayıcı kayıt defteri ayarları** için **ad**, **adres**, **kullanıcı adı** ve **parola** bilgilerini girin. Ad, adres ve listelenen kimlik bilgileri, eşleşen URL'ye sahip olan modülleri almak için kullanılır. Bu modülü dağıtmak için **Dağıtım modülleri** sayfasında **IoT Edge modülü**'nü seçin. Bu IoT Edge modülü, Data Box Edge cihazınızla ilişkilendirilmiş olan IoT Edge cihazınıza dağıtabileceğiniz bir Docker kapsayıcısıdır.

        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    2. IoT Edge özel modül ayarlarını belirtin. Modülünüz için **ad** ve **Görüntü URI'si** bilgilerini girin. 
    
        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    3. **Kapsayıcı oluşturma seçenekleri** bölümünde önceki adımlarda bulut ve yerel paylaşımlar için kopyalanan Edge modülü yerel bağlama noktalarını girin (yeni yol oluşturmak yerine bu yolların kullanılması önemlidir). Bu paylaşımlar ilgili kapsayıcı bağlama noktalarıyla eşlenir. Ayrıca modülünüze ait ortam değişkenlerini de girin.

        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    4. Gerekirse **Gelişmiş Edge çalışma zamanı ayarlarını** yapılandırın ve **İleri**'ye tıklayın.

        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
6.  **Rota belirtme** bölümünde modüller arasındaki rotayı ayarlayın. Bu durumda bulut paylaşımına veri gönderecek olan yerel paylaşımın adını girin. **İleri**’ye tıklayın.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 
 
7.  **Dağıtımı gözden geçirme** bölümünde tüm ayarları gözden geçirin ve uygunsa modülü dağıtım için **gönderin**.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
Bu işlem **Modüller** bölümünde **IoT Edge Özel modülü** ile gösterilen modül dağıtımını başlatır.

![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Veri dönüştürme işlemini doğrulama ve verileri aktarma

Son adım, modülün bağlı olduğundan ve beklenen şekilde çalıştığından emin olmaktır. Modülün çalıştığını doğrulamak için aşağıdaki adımları gerçekleştirin.

1. IoT Hub kaynağındaki IoT Edge cihazınız için modülün "Çalışıyor" durumda olması gerekir.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
2. Modülü seçip tıklayın ve **Modül Kimliği İkizi** bölümüne bakın. Edge cihazı ve modülü istemci durumunun **Bağlı** olması gerekir.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
3.  Modül çalışmaya başladıktan sonra Data Box Edge kaynağınızın Edge modülleri listesinde de görüntülenir. Eklediğiniz modülün **çalışma zamanı durumu**, **Çalışıyor** şeklindedir.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
4.  Dosya Gezgini aracılığıyla oluşturduğunuz yerel paylaşıma ve bulut paylaşımına bağlanın.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
5.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
6.  Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

7.  Veriler ardından bulut paylaşımından depolama hesabına gönderilir. Verileri görüntülemek için Depolama Gezgini'ne gidin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Bu işlemle doğrulama adımları tamamlanmış olur.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Data Box Edge konuları hakkında bilgi edindiniz:

> [!div class="checklist"]
> * IoT Hub kaynağı oluşturma
> * İşlem rolünü ayarlama
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Data Box Edge cihazınızı yönetmeyi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](http://aka.ms/dbg-docs)


