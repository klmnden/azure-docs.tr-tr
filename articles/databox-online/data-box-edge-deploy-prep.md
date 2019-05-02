---
title: Azure veri kutusu Edge dağıtmak için Azure portal, veri merkezi ortamındaki hazırlamak için öğretici | Microsoft Docs
description: Azure veri kutusu Edge dağıtma hakkında daha fazla ilk öğreticide, Azure portalında hazırlanmasını içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: d7e66970db3397531c798bc37bf7c1f346e999bf
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924766"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Öğretici: Azure veri kutusu Edge dağıtmaya hazırlanma  


Bu, Azure veri kutusu Edge tamamen dağıtmak için gerekli olan dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide, Azure portalında bir veri kutusu Edge kaynak dağıtmaya hazırlanmak açıklar.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


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

- Microsoft Azure aboneliğiniz için bir veri kutusu Edge kaynak etkinleştirilir. Kullandıkça Öde abonelikleri desteklenmez.
- Sahibi veya katkıda bulunan aboneliğinize erişimi var.
- Yönetici veya kullanıcı Azure Active Directory Graph API'si için erişimi var. Daha fazla bilgi için [Azure Active Directory Graph API'si](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-edge-device"></a>Data Box Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

- Sevkiyat paketine dahil güvenlik bilgileri gözden geçirdim.
- Cihazı rafa için veri merkezinizde bir standart 19" rafa kullanılabilir bir 1 U yuvası sahip. 
- Düz, kararlı ve düzeyi çalışma yüzeyine burada cihaz güvenli bir şekilde tuttuğunuzda erişebilirsiniz.
- Burada cihazınızın kurulumunun yapılabilmesi için istediğinize site bağımsız bir kaynak veya bir raf güç dağıtım birimi (PDU) bir kesintisiz güç kaynağı (UPS) ile standart AC gücü sahiptir.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri kutusu Edge cihazınız için ağ gereksinimleri başına, veri merkezinizdeki ağ yapılandırılır. Daha fazla bilgi için [veri kutusu Edge sistem gereksinimleri](data-box-edge-system-requirements.md).

- Aşağıdakiler için normal çalışma koşullarına, veri kutusu Edge:

    - En az 10 MB / cihaz güncelleştirildiği emin olmak için bant genişliği indirin.
    - En az 20 MB/sn'lik adanmış karşıya yükleyin ve dosya aktarımı için bant genişliği'ni indirin.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Fiziksel cihazlarınızı yönetmek için bir Data Box Edge kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Veri kutusu Edge kaynak oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın. 
    
    - Azure portalında şu URL: [ https://portal.azure.com ](https://portal.azure.com).
    - Veya, Azure kamu portalında şu URL: [ https://portal.azure.us ](https://portal.azure.us). Daha fazla ayrıntı için [portalı kullanarak Azure kamu Bağlan](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

2. Sol bölmede seçin **+ kaynak Oluştur**. Arama **veri kutusu Edge / veri ağ geçidi kutusunda**. Seçin **veri kutusu Edge / veri ağ geçidi kutusunda**. **Oluştur**’u seçin.
3. Veri kutusu Edge cihazı için kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. Bu sürümde, Doğu ABD, Güneydoğu Asya ve Batı Avrupa'da kullanılabilir. 

    Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. Bölge, yalnızca cihaz yönetimi için meta verileri depolar. Gerçek veriler herhangi bir depolama hesabında depolanır. 
    
    İçinde **veri kutusu Edge** seçeneği için **Oluştur**.

    ![Data Box Edge hizmetini arama](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Üzerinde **Temelleri** sekmesinde girin veya aşağıdaki seçin **proje ayrıntıları**.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu önceki seçimi temel alınarak otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.     |

4. Aşağıdaki seçin veya girin **örnek ayrıntıları**.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Bölge     |Bu sürümde, Doğu ABD, Güneydoğu Asya ve Batı Avrupa, kaynak dağıtmak kullanılabilir. Azure kamu kullanıyorsanız, tüm kamu bölgeleri gösterildiği gibi kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/).<br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|

    ![Proje ve örnek ayrıntıları](media/data-box-edge-deploy-prep/data-box-edge-resource.png)

5. Seçin **sonraki: Teslimat adresi**.

    - Bir cihaz zaten varsa, birleşik giriş kutusunu seçin **bir veri kutusu Edge cihazı sahibim**.
    - Bu, sipariş yeni cihaz ise, cihaz göndermeye ilgili kişi adı, şirket, adresi girin ve kişi bilgilerini.

    ![Yeni cihaz için teslimat adresi](media/data-box-edge-deploy-prep/data-box-edge-resource1.png)

6. Seçin **sonraki: Gözden geçir + Oluştur**.

7. Üzerinde **gözden + Oluştur** sekmesinde, gözden **fiyatlandırma ayrıntıları**, **kullanım koşullarını**, kaynağınızın ayrıntıları. Birleşik giriş kutusunu **gizlilik koşullarını gözden geçirdikten**.

    ![Veri kutusu Edge kaynak ayrıntıları ve gizlilik koşulları gözden geçirin](media/data-box-edge-deploy-prep/data-box-edge-resource2.png)

8. **Oluştur**’u seçin.

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldığında sonra bildirim alırsınız. Seçin **kaynağa Git**.

![Veri kutusu Edge kaynağa Git](media/data-box-edge-deploy-prep/data-box-edge-resource3.png)

Siparişi veren sonra Microsoft sırasını inceler ve size (aracılığıyla e-posta) Sevkiyat ayrıntıları ulaşır.

![Veri kutusu Edge siparişi gözden geçirme için bildirim](media/data-box-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Veri kutusu Edge kaynak çalışır duruma geldikten sonra etkinleştirme anahtarı almanız gerekir. Bu anahtar Data Box Edge cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynak seçin. Seçin **genel bakış** seçip **cihaz Kurulumu**.

    ![Cihaz kurulumu seçin](media/data-box-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. Üzerinde **etkinleştirme** kutucuk seçin **anahtar üret** etkinleştirme anahtarı oluşturmak için. Anahtarı kopyalayın ve daha sonra kullanmak üzere kaydetmek için Kopyala simgesini seçin.

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



