---
title: Azure portalı Data Box Edge dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure veri kutusu Edge dağıtma hakkında daha fazla ilk öğreticide, Azure portalında hazırlanmasını içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 7764b0ceee1b540e9650d232b7087811d7376f28
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452097"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Öğretici: Azure veri kutusu Edge dağıtmaya hazırlanma  


Bu, Azure veri kutusu Edge tamamen dağıtmak için gerekli olan dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide, Azure portalında bir veri kutusu Edge kaynak dağıtmaya hazırlanmak açıklar. 

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş ve bu çözümü dağıtın önce gözden [Azure Önizleme için hizmet koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).  

### <a name="get-started"></a>başlarken

Veri kutusu Edge dağıtmak için önceden belirlenmiş bir sırada aşağıdaki öğreticilere bakın.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Edge için hazırlama](data-box-edge-deploy-prep.md)** |Data Box Edge fiziksel cihazını kurmadan önce Data Box Edge kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Azure Data Box Edge'i kurma](data-box-edge-deploy-install.md)**|Data Box Edge fiziksel cihazını kutusundan çıkarın, rafa yerleştirin ve kablolarını bağlayın.  |
| 3. |**[Bağlanma, ayarlamak ve veri kutusu Edge etkinleştirme](data-box-edge-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Cihaz SMB veya NFS paylaşımlarının kurulması için hazırdır.  |
| 4. |**[Verileri Data Box Edge ile aktarma](data-box-edge-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
| 5. |**[Data Box Edge ile veri dönüştürme](data-box-edge-deploy-configure-compute.md)** |Azure'a taşınan verilerin dönüştürülmesi için cihazdaki Edge modüllerini yapılandırın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Veri kutusu Edge kaynağınızı, veri kutusu Edge cihazınıza ve veri merkezi ağı için yapılandırma önkoşulları aşağıda verilmiştir.

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz için bir veri kutusu Edge kaynak etkinleştirilir.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-edge-device"></a>Data Box Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:
- Cihazı rafa için veri merkezinizde bir standart 19" rafa kullanılabilir bir 1 U yuvası sahip. 
- Düz, kararlı ve düzeyi çalışma yüzeyine burada cihaz güvenli bir şekilde tuttuğunuzda erişebilirsiniz.
- Burada cihazınızın kurulumunun yapılabilmesi için istediğinize site bağımsız bir kaynak veya bir raf güç dağıtım birimi (PDU) bir kesintisiz güç kaynağı (UPS) ile standart AC gücü sahiptir.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri kutusu Edge cihazınız için ağ gereksinimleri başına, veri merkezinizdeki ağ yapılandırılır. Daha fazla bilgi için [veri kutusu Edge sistem gereksinimleri](data-box-gateway-system-requirements.md).

* Veri kutusu Edge adanmış 20 MB/sn Internet bant genişliği (veya daha fazlası) her zaman kullanılabilir. Bu bant genişliği ile diğer uygulamaları paylaşılmamalıdır. Ağ kapasitesi azaltma kullanıyorsanız, ardından çalışmak için azaltma için 32 MB/sn Internet bant genişliği veya daha fazla kullanmanızı öneririz.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Aşağıdaki adımları izleyerek yeni bir Data Box Edge kaynağı oluşturun. 

Fiziksel cihazlarınızı yönetmek için bir Data Box Edge kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Veri kutusu Edge kaynak oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Azure Önizleme portalı bu URL'de oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın: [ https://aka.ms/databox-edge ](https://aka.ms/databox-edge). 

2. Veri kutusu Edge önizlemesi için kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. İçinde **veri kutusu Edge** seçeneği için **Oluştur**.

    ![Data Box Edge hizmetini arama](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Yeni kaynak için aşağıdaki bilgileri girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak adı   | Hangi kaynağı tanımlamak bir kolay ad.<br>Kaynak adı harf, rakam ve kısa çizgi içeren 2 ila 50 karakter uzunluğunda olabilir.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Abonelik    |Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>Daha fazla bilgi edinin [Azure kaynak grupları](../azure-resource-manager/resource-group-overview.md).     |
    |Konum     |Bu sürümde Doğu ABD, Batı ABD 2, Güneydoğu Asya ve Batı Avrupa kullanılabilir. <br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|
    
    ![Veri kutusu Edge kaynak oluştur](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. **Tamam**’ı seçin.
 
Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulduktan sonra uygun şekilde bildirim alırsınız.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Veri kutusu Edge kaynak çalışır duruma geldikten sonra etkinleştirme anahtarı almanız gerekir. Bu anahtar Data Box Edge cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz ve ardından kaynak Seç **genel bakış**.

2. Seçin **anahtar üret** etkinleştirme anahtarı oluşturmak için. Anahtarı kopyalayın ve daha sonra kullanmak üzere kaydetmek için Kopyala simgesini seçin.

    ![Etkinleştirme anahtarını alma](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı, üç gün üretildikten sonra sona erer. 
> - Anahtarın süresi varsa, yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Veri kutusu Edge yükleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Veri kutusu Edge yükleyin](./data-box-edge-deploy-install.md)



