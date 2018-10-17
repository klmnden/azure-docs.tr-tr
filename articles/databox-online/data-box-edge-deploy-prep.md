---
title: Azure portalı Data Box Edge dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Edge'in dağıtımına yönelik ilk öğretici Azure portalın hazırlanmasını içerir.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 79061caac3d598df79f383b9b13458ab9537cd81
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833072"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge-preview"></a>Öğretici: Azure Data Box Edge (Önizleme) dağıtımına hazırlanma


Bu, Azure Data Box Edge'inizi tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide Data Box Edge kaynağını dağıtmak için Azure portalının nasıl hazırlanacağı açıklanır. 

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 

### <a name="get-started"></a>başlarken

Data Box Edge'inizi dağıtmak için, aşağıdaki öğreticileri belirtilen sırada izleyin.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Edge için hazırlama](data-box-edge-deploy-prep.md)** |Data Box Edge fiziksel cihazını kurmadan önce Data Box Edge kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Azure Data Box Edge'i kurma](data-box-edge-deploy-install.md)**|Data Box Edge fiziksel cihazını kutusundan çıkarın, rafa yerleştirin ve kablolarını bağlayın.  |
| 3. |**[Data Box Edge'i bağlama, kurma ve etkinleştirme](data-box-edge-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Cihaz SMB veya NFS paylaşımlarının kurulması için hazırdır.  |
| 4. |**[Verileri Data Box Edge ile aktarma](data-box-edge-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
| 5. |**[Data Box Edge ile veri dönüştürme](data-box-edge-deploy-configure-compute.md)** |Azure'a taşınan verilerin dönüştürülmesi için cihazdaki Edge modüllerini yapılandırın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Burada Data Box Edge kaynağınız, Data Box Edge cihazınız ve veri merkezi ağı için yapılandırma önkoşullarını bulabilirsiniz.

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz Data Box Edge kaynağı için etkinleştirilmiş olmalı.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-edge-device"></a>Data Box Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

- Cihazı yerleştirmek için veri merkezinizdeki 19 inçlik standart bir rafta 1 U büyüklüğünde boş yuva mevcut. 
- Cihazın güvenli bir şekilde yerleştirilebileceği düz, sabit ve dengeli bir çalışma yüzeyine sahip olduğunuzdan emin olun.
- Cihazı kurmayı planladığınız konumda bağımsız bir kaynaktan gelen standart AC güç beslemesi veya kesintisiz güç kaynağına (UPS) sahip bir raf güç dağıtım birimi (PDU) olduğunu doğrulayın.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezinizdeki ağ, Data Box Edge cihazınızın ağ gereksinimlerine uygun olarak yapılandırılmış. Daha fazla bilgi için bkz. [Data Box Edge Sistem Gereksinimleri](data-box-gateway-system-requirements.md).

* Data Box Edge'inize her zaman ayrılmış 20 Mb/sn İnternet bant genişliği (veya daha fazlası) sağlanıyor. Bu bant genişliği başka hiçbir uygulamayla paylaşılmamalıdır. Bant genişliği azaltma kullanılıyorsa, azaltmanın çalışması için 32 Mb/sn veya daha fazla İnternet bant genişliği kullanmanızı öneririz.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Aşağıdaki adımları izleyerek yeni bir Data Box Edge kaynağı oluşturun. 

Fiziksel cihazlarınızı yönetmek için bir Data Box Edge kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Azure portalında aşağıdaki adımları uygulayarak bir Data Box kaynağı oluşturun.

1. Microsoft Azure kimlik bilgilerinizi kullanarak şu URL'de Azure önizleme portalında oturum açın: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Data Box Edge önizlemesinde kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. **Data Box Edge** seçeneğinde **Oluştur**'a tıklayın.

    ![Data Box Edge hizmetini arama](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Yeni kaynak için aşağıdaki bilgileri girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak adı   | Kaynağı tanımlamak için kolay bir ad.<br>Kaynak adı 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Abonelik    |Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.     |
    |Konum     |Bu sürümde Doğu ABD, Batı ABD 2, Güneydoğu Asya ve Batı Avrupa kullanılabilir. <br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|
    
    ![Data Box Edge kaynağı oluşturma](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. **Tamam** düğmesine tıklayın.
 
Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulduktan sonra, bu size uygun bir şekilde bildirilir.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Data Box Edge kaynağı çalışır duruma geldikten sonra, etkinleştirme anahtarını almanız gerekecektir. Bu anahtar Data Box Edge cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağa tıklayın ve sonra da **Genel Bakış**'a tıklayın.

2. Etkinleştirme anahtarını oluşturmak için **Anahtar oluştur**'a tıklayın. Kopyala simgesine tıklayarak anahtarı kopyalayın ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarının kullanım süresi, oluşturulduktan 3 gün sonra sona erer. 
> - Anahtarın kullanım süresi dolarsa, yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Data Box Edge'inizi kurma hakkında bilgi almak için bir sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [Data Box Edge cihazını kurma](./data-box-edge-deploy-install.md)



