---
title: Azure portalını Data Box Gateway dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Gateway'in dağıtımına yönelik ilk öğretici Azure portalının hazırlanmasını içerir.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6db713984b62ce3db48b2e72a4b117696bdd6add
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452981"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>Öğretici: Azure veri kutusu ağ geçidi (Önizleme) dağıtmaya hazırlanma


Bu, Azure Data Box Gateway'inizi tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide Data Box Gateway kaynağını dağıtmak için Azure portalının nasıl hazırlanacağı açıklanır. 

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


> [!IMPORTANT]
> - Data Box Gateway önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 

### <a name="get-started"></a>başlarken

Data Box Gateway'inizi dağıtmak için, aşağıdaki öğreticileri belirtilen sırada izleyin.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md)** |Data Box Gateway sanal cihazını sağlamadan önce Data Box Gateway kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Data Box Gateway'i Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway'i VMware'de sağlama](data-box-gateway-deploy-provision-vmware.md)**|Hyper-V için, Windows Server 2016 veya Windows Server 2012 R2 üzerinde Hyper-V çalıştıran konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlayın. <br><br><br> VMware için, VMware ESXi 6.0 veya 6.5 çalıştıran bir konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlayın.<br></br> |
| 3. |**[Data Box Gateway'i bağlama, kurma ve etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Ardından SMB paylaşımlarını sağlayabilirsiniz.  |
| 4. |**[Verileri Data Box Gateway ile aktarma](data-box-gateway-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Burada Data Box Gateway kaynağınız, Data Box Gateway cihazınız ve veri merkezi ağı için yapılandırma önkoşullarını bulabilirsiniz.

### <a name="for-the-data-box-gateway-resource"></a>Data Box Gateway kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz Data Box Gateway kaynağı için etkinleştirilmiş olmalı.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-gateway-device"></a>Data Box Gateway cihazı için

Sanal cihazı dağıtmadan önce şunlardan emin olun:

* Windows Server 2012 R2 veya sonraki bir sürümü üzerinde Hyper-V ya da VMware (ESXi 6.0 veya 6.5) çalıştıran ve cihaz sağlamak için kullanılabilecek bir konak sistemine erişiminiz var.
* Konak sistemi, Data Box sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabilir:
  
  * En az 4 çekirdek.
  * En az 8 GB RAM. 
  * Bir ağ arabirimi.
  * 250 GB işletim sistemi diski.
  * Sistem verileri için 2 TB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezinizdeki ağ, Data Box Gateway cihazınızın ağ gereksinimlerine uygun olarak yapılandırılmış. Daha fazla bilgi için bkz. [Data Box Gateway Sistem Gereksinimleri](data-box-gateway-system-requirements.md).

* Data Box Gateway'inize her zaman ayrılmış 20 Mb/sn İnternet bant genişliği (veya daha fazlası) sağlanıyor. Bu bant genişliği başka hiçbir uygulamayla paylaşılmamalıdır. Bant genişliği azaltma kullanılıyorsa, azaltmanın çalışması için 32 Mb/sn veya daha fazla İnternet bant genişliği kullanmanızı öneririz.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Aşağıdaki adımları izleyerek yeni bir Data Box Gateway kaynağı oluşturun. 

Sanal cihazlarınızı yönetmek için bir Data Box Gateway kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Azure portalında aşağıdaki adımları uygulayarak bir Data Box kaynağı oluşturun.
1. Microsoft Azure kimlik bilgilerinizi kullanarak şu URL'de Azure önizleme portalında oturum açın: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Data Box Edge önizlemesinde kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. **Data Box Gateway** seçeneğinde **Oluştur**'a tıklayın.

    ![Data Box Gateway hizmetini arama](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. Yeni kaynak için aşağıdaki bilgileri girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak adı   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Abonelik    |Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.     |
    |Konum     |Bu sürümde Doğu ABD, Batı ABD 2, Güneydoğu Asya ve Batı Avrupa kullanılabilir. <br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|
    
    ![Data Box Gateway kaynağını oluşturma](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. **Tamam** düğmesine tıklayın.
 
Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulduktan sonra, bu size uygun bir şekilde bildirilir.


## <a name="download-the-virtual-device-image"></a>Sanal cihaz görüntüsünü indirme

Data Box Gateway kaynağı oluşturulduktan sonra, konak sisteminizde sanal makineyi sağlamak için uygun sanal cihaz görüntüsünü indirin. Sanal cihaz görüntüleri işletim sistemine özgüdür ve Azure portalında kaynağınızın **Hızlı Başlangıç** dikey penceresinden indirilebilir.

> [!IMPORTANT]
> Data Box Gateway üzerinde çalıştırılan yazılım yalnızca Data Box Gateway kaynağıyla kullanılabilir.


[Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

1. Oluşturduğunuz kaynağa tıklayın ve sonra da **Genel Bakış**'a tıklayın. Zaten bir Azure Data Box Gateway kaynağınız varsa, kaynağın üzerine tıklayın ve **Genel Bakış**'a gidin.

    ![Yeni Data Box Gateway kaynağı](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. Sağ bölmedeki hızlı başlangıçta, indirmek istediğiniz görüntüye karşılık gelen bağlantıya tıklayın. Görüntü dosyaları yaklaşık 4,8 GB'tır.
   
   * [Windows Server 2012 R2 ve sonraki sürümleri üzerinde Hyper-V için VHDX](https://aka.ms/dbe-vhdx-2012).
   * [VMWare ESXi 6.0 veya 6.5 için VMDK](https://aka.ms/dbe-vmdk).

5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Data Box Gateway kaynağı çalışır duruma geldikten sonra, etkinleştirme anahtarını almanız gerekecektir. Bu anahtar Data Box Gateway cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır.

Etkinleştirme anahtarı, Data Box Gateway kaynağınızla etkinleştirilmesi gereken tüm Data Box Gateway cihazlarınızı kaydetmek için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağa tıklayın ve sonra da **Genel Bakış**'a tıklayın.

    ![Yeni Data Box Gateway kaynağı](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Etkinleştirme anahtarını oluşturmak için **Anahtar oluştur**'a tıklayın. Kopyala simgesine tıklayarak anahtarı kopyalayın ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarının kullanım süresi, oluşturulduktan 3 gün sonra sona erer. 
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


