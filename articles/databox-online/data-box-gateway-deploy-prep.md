---
title: Azure portalını Data Box Gateway dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Gateway'in dağıtımına yönelik ilk öğretici Azure portalının hazırlanmasını içerir.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: f9650cdb6935fb45f0c59e8a114a9ce1c8e2d809
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686529"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway"></a>Öğretici: Azure veri kutusu ağ geçidi dağıtmaya hazırlanma


Bu, Azure Data Box Gateway'inizi tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide Data Box Gateway kaynağını dağıtmak için Azure portalının nasıl hazırlanacağı açıklanır. 

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

## <a name="get-started"></a>başlarken

Data Box Gateway'inizi dağıtmak için, aşağıdaki öğreticileri belirtilen sırada izleyin.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md)** |Data Box Gateway sanal cihazını sağlamadan önce Data Box Gateway kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Data Box Gateway'i Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway'i VMware'de sağlama](data-box-gateway-deploy-provision-vmware.md)**|Hyper-V için, Windows Server 2016 veya Windows Server 2012 R2 üzerinde Hyper-V çalıştıran konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlayın. <br><br><br> VMware için sağlayın ve VMware ESXi 6.0 veya 6.5 6.7 çalıştıran bir konak sisteminde bir veri kutusu ağ geçidi sanal cihaza bağlanın.<br></br> |
| 3. |**[Data Box Gateway'i bağlama, kurma ve etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Ardından SMB paylaşımlarını sağlayabilirsiniz.  |
| 4. |**[Verileri Data Box Gateway ile aktarma](data-box-gateway-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Burada Data Box Gateway kaynağınız, Data Box Gateway cihazınız ve veri merkezi ağı için yapılandırma önkoşullarını bulabilirsiniz.

### <a name="for-the-data-box-gateway-resource"></a>Data Box Gateway kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri kutusu ağ geçidi kaynağı için Microsoft Azure aboneliğiniz desteklenmelidir. Kullandıkça Öde abonelikleri desteklenmez.
- Sahibi veya katkıda bulunan aboneliğinize erişimi var.
- Yönetici veya kullanıcı Azure Active Directory Graph API'si için erişimi var. Daha fazla bilgi için [Azure Active Directory Graph API'si](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-gateway-device"></a>Data Box Gateway cihazı için

Sanal cihazı dağıtmadan önce şunlardan emin olun:

- Bir Windows Server 2012 R2 veya sonraki sürümlerde Hyper-V çalıştıran bir konak sistemi ya da olabilir (ESXi 6.0 veya 6.5 6.7) VMware erişiminiz bir sağlamak için kullanılan bir cihaz.
- Konak sistemi, Data Box sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabilir:
  
  - En az 4 sanal işlemci.
  - En az 8 GB RAM.
  - Bir ağ arabirimi.
  - 250 GB işletim sistemi diski.
  - Sistem verileri için 2 TB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Data Box Gateway cihazınızın ağ gereksinimlerine uygun olarak yapılandırılmış. Daha fazla bilgi için [veri kutusu ağ geçidi sistem gereksinimleri](data-box-gateway-system-requirements.md).

- Y: için normal çalışma koşullarına veri kutusu geçidinizin olmalıdır

    - En az 10 MB / cihaz güncelleştirildiği emin olmak için bant genişliği indirin.
    - En az 20 MB/sn'lik adanmış karşıya yükleyin ve dosya aktarımı için bant genişliği'ni indirin.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Sanal cihazlarınızı yönetmek için bir Data Box Gateway kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Bir veri kutusu ağ geçidi kaynağı oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın:

    - Azure portalında şu URL: [ https://portal.azure.com ](https://portal.azure.com).
    - Veya, Azure kamu portalında şu URL: [ https://portal.azure.us ](https://portal.azure.us). Daha fazla ayrıntı için [portalı kullanarak Azure kamu Bağlan](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

2. Sol bölmede seçin **+ kaynak Oluştur**. Arama **veri kutusu Edge / veri ağ geçidi kutusunda**. Veri kutusu kenar Seç / veri ağ geçidi kutusu. **Oluştur**’u seçin.
3. Veri kutusu ağ geçidi cihazı için kullanmak istediğiniz aboneliği seçin. Veri kutusu ağ geçidi kaynak dağıtmak istediğiniz bölgeyi seçin. Bu sürümde, Doğu ABD, Güneydoğu Asya ve Batı Avrupa'da kullanılabilir. Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. İçinde **veri kutusu ağ geçidi** seçeneği için **Oluştur**.

    ![Data Box Gateway hizmetini arama](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

4. Üzerinde **Temelleri** sekmesinde girin veya aşağıdaki seçin **proje ayrıntıları**.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu önceki seçimi temel alınarak otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.     |

5. Aşağıdaki seçin veya girin **örnek ayrıntıları**.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |   
    |Bölge     |Bu sürümde, Doğu ABD, Güneydoğu Asya ve Batı Avrupa, kaynak dağıtmak kullanılabilir. Azure kamu, kamu bölgeleri listelenen [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) kullanılabilir. <br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|
    
    ![Data Box Gateway kaynağını oluşturma](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
6. **İncele ve oluştur**’u seçin.
 
7. Üzerinde **gözden + Oluştur** sekmesinde, gözden **fiyatlandırma ayrıntıları**, **kullanım koşullarını**, kaynağınızın ayrıntıları. **Oluştur**’u seçin.

    ![Veri kutusu ağ geçidi kaynak ayrıntıları gözden geçirin](media/data-box-gateway-deploy-prep/data-box-gateway-resource1.png)

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldığında sonra bildirim alırsınız. Seçin **kaynağa Git**.

![Veri kutusu ağ geçidi kaynak ayrıntıları gözden geçirin](media/data-box-gateway-deploy-prep/data-box-gateway-resource2.png)

## <a name="download-the-virtual-device-image"></a>Sanal cihaz görüntüsünü indirme

Data Box Gateway kaynağı oluşturulduktan sonra, konak sisteminizde sanal makineyi sağlamak için uygun sanal cihaz görüntüsünü indirin. Belirli bir işletim sistemi için sanal cihaz görüntüleridir.

> [!IMPORTANT]
> Data Box Gateway üzerinde çalıştırılan yazılım yalnızca Data Box Gateway kaynağıyla kullanılabilir.

Aşağıdaki adımları izleyin [Azure portalında](https://portal.azure.com/) sanal cihaz görüntüsü indirilemedi.

1. Oluşturduğunuz ve ardından kaynak **genel bakış**. Mevcut bir Azure veri kutusu ağ geçidi kaynağı varsa, kaynağı seçin ve Git **genel bakış**. Seçin **cihaz Kurulumu**.

    ![Yeni Data Box Gateway kaynağı](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Üzerinde **indirme görüntü** kutucuğunda, sanal makine sağlamak için kullanılan ana bilgisayar sunucusunda işletim sistemine karşılık gelen sanal cihaz görüntüsü seçin. Görüntü, yaklaşık 5.6 GB dosyalarıdır.
   
   * [Windows Server 2012 R2 ve sonraki sürümleri üzerinde Hyper-V için VHDX](https://aka.ms/dbe-vhdx-2012).
   * [VMWare ESXi 6.0 veya 6.5 6.7 için VMDK](https://aka.ms/dbe-vmdk).

    ![Veri kutusu ağ geçidi sanal cihaz görüntüsünü indir](media/data-box-gateway-deploy-prep/data-box-gateway-download-image.png)

5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Veri kutusu ağ geçidi kaynak çalışır duruma geldikten sonra etkinleştirme anahtarı almanız gerekir. Bu anahtar Data Box Gateway cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz ve ardından kaynak Seç **genel bakış**. İçinde **cihaz Kurulumu**gidin **yapılandırma ve etkinleştirme** Döşe.

    ![Yapılandırma ve kutucuğu etkinleştirin](media/data-box-gateway-deploy-prep/data-box-gateway-configure-activate.png)

2. Seçin **anahtar üret** etkinleştirme anahtarı oluşturmak için. Anahtarı kopyalayın ve daha sonra kullanmak üzere kaydetmek için Kopyala simgesini seçin.

    ![Etkinleştirme anahtarını alma](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı, üç gün üretildikten sonra sona erer.
> - Anahtarın süresi varsa, yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

Data Box Gateway'inize sanal makine sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. Konak işletim sisteminize bağlı olarak, şuradaki ayrıntılı yönergelere bakın:

> [!div class="nextstepaction"]
> [Data Box Gateway'i Hyper-V'de sağlama](./data-box-gateway-deploy-provision-hyperv.md)

OR

> [!div class="nextstepaction"]
> [Data Box Gateway'i VMware'de sağlama](./data-box-gateway-deploy-provision-vmware.md)


