---
title: Azure Data Box Edge ile veri dönüştürme | Microsoft Docs
description: Data Box Edge'deki işlem rolünü yapılandırmayı ve verileri Azure'a göndermeden önce dönüştürmek için kullanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 31911c124aeafecb8ee37d14e58d3a0bdc0d4955
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112620"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge"></a>Öğretici: Azure veri kutusu Edge ile verileri dönüştürün

Bu öğreticide, Azure veri kutusu Edge Cihazınızda bir işlem rolünü yapılandırma açıklanır. Bilgi işlem rolü yapılandırdıktan sonra veri kutusu Edge Azure'a göndermeden önce verileri dönüştürebilirsiniz.

Bu yordamı tamamlamak için yaklaşık 10-15 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşımları ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

 
## <a name="prerequisites"></a>Önkoşullar

Veri kutusu Edge Cihazınızda bir işlem rolünü kurmadan önce emin olun:

- Veri kutusu Edge cihazınıza açıklandığı etkinleştirildikten sonra [bağlanma, ayarlamak ve Azure veri kutusu Edge etkinleştirme](data-box-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>İşlemi yapılandırma

İşlem, veri kutusu Edge üzerinde yapılandırmak için bir IOT hub'ı kaynak oluşturacaksınız.

1. Veri kutusu Edge kaynağınızın Azure portalında genel bakış gidin. Sağ bölmede üzerinde **işlem** kutucuk seçin **başlama**.

    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute/configure-compute-1.png)

2. Üzerinde **yapılandırma Edge işlem** kutucuk seçin **işlem yapılandırma**.
3. Üzerinde **yapılandırma Edge işlem** dikey penceresinde aşağıdakileri girin:

   
    |Alan  |Değer  |
    |---------|---------|
    |IoT Hub     | Aralarından seçim **yeni** veya **mevcut**. <br> Varsayılan olarak, bir standart katman (S1), bir IOT kaynak oluşturmak için kullanılır. Ücretsiz katman IOT kaynağı kullanmak için oluşturun ve ardından mevcut kaynağı seçin. <br> Her durumda, aynı abonelikte ve kaynak grubu, veri kutusu Edge kaynak tarafından kullanılan IOT hub'ı kaynak kullanır.     |
    |Ad     |IOT hub'ı kaynağınız için bir ad girin.         |

    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute/configure-compute-2.png)

4. **Oluştur**’u seçin. IOT hub'ı kaynak oluşturulması birkaç dakika sürer. IOT hub'ı kaynak oluşturulduktan sonra **işlem yapılandırma** kutucuğuna güncelleştirmeleri işlem yapılandırmasını göster. Uç bilgi işlem rolü yapılandırıldığını doğrulamak için şunu seçin **görünümü işlem** üzerinde **işlem yapılandırma** Döşe.
    
    ![İşlem ile çalışmaya başlama](./media/data-box-edge-deploy-configure-compute/configure-compute-3.png)

    Uç bilgi işlem rolü Edge cihazında ayarlandığında, iki cihazı oluşturur: bir IOT cihaz ve bir IOT Edge cihazı. IOT hub'ı kaynak hem de görüntülenebilir. Bir IOT Edge çalışma zamanı, aynı zamanda bu IOT Edge cihaz üzerinde çalışıyor. Bu noktada, yalnızca Linux platformuna IOT Edge cihazınız için kullanılabilir.


## <a name="add-shares"></a>Paylaşımları ekleme

Bu öğreticide basit dağıtım için iki paylaşımını gerekir: bir kenar paylaşımı ve başka bir uç yerel paylaşımı.

1. Bir Edge paylaşımı aşağıdaki adımları uygulayarak cihazda ekleyin:

    1. Veri kutusu Edge kaynağınıza gidin **Edge işlem > başlama**.
    2. Üzerinde **paylaşımlar ekleme** kutucuk seçin **Ekle**.
    3. Üzerinde **Ekle paylaşımı** dikey penceresinde paylaşımı adı sağlayın ve paylaşım türü seçin.
    4. Edge paylaşımını bağlayabilmeniz için onay kutusunu seçin. **paylaşımı ile Edge işlem kullanmak**.
    5. Seçin **depolama hesabı**, **depolama hizmeti**, bir mevcut kullanıcı ve ardından **Oluştur**.

        ![Bir Edge paylaşım Ekle](./media/data-box-edge-deploy-configure-compute/add-edge-share-1.png) 

    Yerel bir NFS paylaşımına oluşturduysanız, dosya paylaşımına kopyalamak için aşağıdaki uzak eşitleme (rsync) komut seçeneği kullanın:

    `rsync <source file path> < destination file path>`

    Rsync komut hakkında daha fazla bilgi için Git [Rsync belgeleri](https://www.computerhope.com/unix/rsync.htm).

    Edge paylaşımı oluşturulur ve başarılı oluşturma bildirimi alırsınız. Paylaşım listesi güncelleştirildi, ancak tamamlanması paylaşımı oluşturmak için beklemeniz gerekir.

2. Önceki adımda yer alan tüm adımları yinelenen ve onay kutusunu seçerek, Edge cihazında Edge Yerel paylaşım Ekle **Edge Yerel paylaşım olarak yapılandırma**. Veriye yerel cihazda kalır.

    ![Bir Edge Yerel paylaşım Ekle](./media/data-box-edge-deploy-configure-compute/add-edge-share-2.png)

  
3. Seçin **paylaşımlar ekleme** paylaşımları güncelleştirilmiş listesini görmek için.

    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-configure-compute/add-edge-share-3.png) 
 

## <a name="add-a-module"></a>Modül Ekle

Özel bir ya da önceden oluşturulmuş modülüne ekleyebilirsiniz. Bu uç cihazda hiçbir özel modüller vardır. Özel bir modül oluşturma konusunda bilgi almak için Git [geliştirme bir C# veri kutusu Edge cihazınız için modül](data-box-edge-create-iot-edge-module.md).

Bu bölümde oluşturduğunuz IOT Edge cihazı için özel bir modül Ekle [geliştirme bir C# modülü, veri kutusu Edge için](data-box-edge-create-iot-edge-module.md). Bu özel modül sınır cihazı Edge yerel paylaşımından dosyaları alır ve bunları bir kenarı (bulut) paylaşımına cihaza taşır. Bulut paylaşımı, bulut paylaşımı ile ilişkili Azure depolama hesabına dosyaları ardından iter.

1. Git **Edge işlem > başlama**. Üzerinde **modül eklemek** olarak senaryo türünü seçin, döşeme **basit**. **Add (Ekle)** seçeneğini belirleyin.
2. İçinde **yapılandırma ve Modül Ekle** dikey penceresinde aşağıdaki değerleri girin:

    
    |Alan  |Değer  |
    |---------|---------|
    |Ad     | Modül için benzersiz bir ad. Veri kutusu Ucunuzdaki ile ilişkili IOT Edge cihazı dağıtabileceğiniz bir docker kapsayıcısı modülüdür.        |
    |Görüntü URI'si     | Görüntü URI'si modülü için karşılık gelen kapsayıcı görüntüsünün.        |
    |Kimlik bilgileri gerekli     | Bu onay kutusu işaretlendiğinde, kullanıcı adı ve parola modülleri ile eşleşen bir URL almak için kullanılır.        |
    |Giriş paylaşımı     | Bir giriş paylaşımı seçin. Edge yerel paylaşımı giriş bu durumda paylaşımıdır. Burada kullanılan modül dosyaları Edge yerel paylaşımından bir kenar paylaşımına buluta burada karşıya taşır.        |
    |Çıkış paylaşımı     | Bir çıkış paylaşımı seçin. Edge paylaşımı çıktı Bu örnekte paylaşımıdır.        |
    |Tetikleyici türü     | Arasından seçim **dosya** veya **zamanlama**. Bir dosya tetikleyici, giriş paylaşıma yazılan bir dosya gibi bir dosya olay oluştuğunda etkinleştirilir. Tanımladığınız bir zamanlamaya göre yukarı zamanlanan bir tetikleyici tetikler.         |
    |Tetikleyici adı     | Tetikleyici için benzersiz bir ad.         |
    |Ortam değişkenleri| Yardımcı olacak isteğe bağlı bilgiler modülünüzde çalıştırılacağı ortam tanımlayın.   |

    ![Ekleme ve modül yapılandırma](./media/data-box-edge-deploy-configure-compute/add-module-1.png)

3. **Add (Ekle)** seçeneğini belirleyin. Modül eklenen. **Ekle Modülü** kutucuğuna modülü dağıtılacağını gösterecek biçimde güncelleştirilir. 

    ![Dağıtılan Modülü](./media/data-box-edge-deploy-configure-compute/add-module-2.png)

### <a name="verify-data-transform-and-transfer"></a>Veri dönüştürme işlemini doğrulama ve verileri aktarma

Modül bağlı ve beklendiği gibi çalıştığından emin olmak için son adımdır bakın. IOT Edge cihazınızın IOT hub'ı kaynak modülü çalışma zamanı durumunu çalıştırıyor olmalıdır.

Modül çalıştığını doğrulamak için aşağıdakileri yapın:

1. Seçin **Ekle Modülü** Döşe. Sayfasına yönlendirileceksiniz **modülleri** dikey penceresi. Modüller listesinde dağıttığınız modülü belirleyin. Eklediğiniz modülü çalışma zamanı durumunu olmalıdır *çalıştıran*.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-1.png)
 
1.  Dosya Gezgini'nde, hem yerel Edge hem de daha önce oluşturduğunuz uç paylaşımları için bağlanın.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-2.png) 
 
1.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-3.png) 
 
    Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-4.png)  

    Veri, bulut paylaşımından sonra depolama hesabına gönderilir. Verileri görüntülemek için depolama Gezgini'ne gidin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-5.png) 
 
Doğrulama işlemini tamamladınız.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşımları ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Veri kutusu Edge Cihazınızı yönetme konusunda bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](data-box-edge-manage-access-power-connectivity-mode.md)
