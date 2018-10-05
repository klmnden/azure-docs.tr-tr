---
title: IOT Edge modülü teklif oluşturma | Microsoft Docs
description: Yeni bir IOT Edge modülü markette yayımlama yapma.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: d3cc1a09963c5f7fee613af24c63fd15b1cfffee
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811296"
---
# <a name="how-to-publish-a-new-iot-edge-module-in-the-cloud-partner-portal"></a>Yeni bir IOT Edge modülü bulut iş ortağı Portalı'nda yayımlama

Bu makalede yeni bir IOT Edge modülü teklif yayımlamak için gereken adımlar açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları, bir IOT Edge modülü Azure Market yayımlama için geçerlidir.

-   Erişim [bulut iş ortağı Portalı'nı (CPP)](https://cloudpartner.azure.com/#alloffers). Daha fazla bilgi için [yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   Bir Azure Container Registry'de barındırılan, IOT Edge modülü vardır.

-   IOT Edge modülü meta verilerinizi (kapsamlı olmayan listesi dahil) hazır vardır:

    -   Unvan

    -   Açıklama (temel HTML biçiminde)

    -   Logo aşağıdaki boyutları ve png görüntü biçimi: 40 piksel x 40 piksel, 90 piksel x 90 piksel, 115 piksel x 115 piksel, 255 piksel x 115 piksel.

    -   Kullanım ve gizlilik ilkesi koşulları

<a name="prepare-your-iot-edge-module-listing-in-cpp"></a>IOT Edge modülü listenizi CPP hazırlama
-------------------------------------------

### <a name="create-a-new-offer-of-the-type-iot-edge-module"></a>IOT Edge modülü türü yeni bir teklif oluşturun 

IOT Edge modülü listenizi hazırlamak için aşağıdaki adımları izleyin:

-   Oturum, [CPP hesabı](https://cloudpartner.azure.com/).

>[!Note]
>Bulut iş ortağı Portalı hakkında genel bilgi için kontrol edebilirsiniz [belgeleri edinin](https://cloudpartner.azure.com/#learn)

-   Seçin **yeni teklif**ve ardından **IOT Edge Modülü**.

>[!NOTE]
>Bir IOT Edge modülü IOT Edge üzerinde çalıştırmak için özel olarak yapılan bir kapsayıcıdır. Bir IOT Edge modülü tarafından ele alınan senaryolar, bir IOT Edge bağlamda anlamlı olmalıdır. Ayrıca, bir IOT Edge cihaz modellemeniz için dağıtım yapmak için varsayılan yapılandırma ayarlarını içerir. Kapsayıcı edgeHub ve IOT Hub ile iletişim sağlamak için IOT Edge modülü SDK'sı de.

### <a name="define-your-offer-settings"></a>Teklif ayarlarınızı tanımlayın

Teklif ayarları sekmesinde, teklifiniz için bilgileri girin.

![Teklif kimliği](./media/cloud-partner-portal-create-iot-edge-module-offer/offer-identity.png)


-   **Teklif kimliği** benzersiz olarak CPP teklife tanımlar ve müşterilere yönelik URL'LERİNDE kullanılabilir.

-   **Adı** yalnızca bu teklifte CPP başvurmak için sizin tarafınızdan görünür olur.

### <a name="create-one-or-more-skus"></a>Bir veya daha fazla SKU'ları oluşturma

Her SKU, bir kapsayıcı görüntüsüne karşılık gelir. Birden fazla ekleyebilirsiniz ve en az bir SKU gerekir. Bir SKU iki bölümü vardır:

-   SKU meta verileri

-   Kapsayıcı meta verileri

**Bir SKU oluşturmak için:**

Seçin **SKU'ları** sekmesine tıklayın ve ardından **yeni SKU**.

![Yeni SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKUs.png)

SKU gereklidir aşağıdaki alanları içerir:
- SKU kimliği - benzersiz bir tanımlayıcı.
- Başlık - SKU başlık, en çok 50 karakter.
- Özet - en fazla 100 karakter, kısa bir açıklaması.
- Açıklama - uzun açıklaması.
- Bu SKU Gizle - varsayılan **Hayır**.
   
![SKU ayrıntıları](./media/cloud-partner-portal-create-iot-edge-module-offer/SKU-settings.png)

#### <a name="iot-edge-module-metadata-and-the-container-registry"></a>IOT Edge modülü meta veri ve kapsayıcı kayıt defteri

IOT Edge modülünün meta verileri, Azure container Registry'de (ACR) depolanmış görüntü başvuru bilgilerini içerir. Azure marketi, genel Market kayıt defterine görüntü kopyalar ve sonra sertifika müşteriler için kullanılabilir. IOT Edge modülü görüntüyü kullanmak için tüm kullanıcı isteği Market kapsayıcı kayıt defterinden sunulur.

![Kapsayıcı görüntüleri](./media/cloud-partner-portal-create-iot-edge-module-offer/container-images.png)

**Kapsayıcı görüntüleri**

Görüntü deposu ayrıntılarını şu alanlar vardır:

-   **Abonelik kimliği** ACR kayıt defteri olduğu mevcut Azure abonelik kimliği.

-   **Kaynak grubu adı** – ACR kayıt defterinin kaynak grubu adı.

-   **Kayıt defteri adı** – ACR kayıt defteri adı.

-   **Depo adı** – depo adı. Bir kez ayarlandıktan sonra bu değer daha sonra değiştirilemez. Diğer herhangi bir teklif hesabınız kapsamında aynı ada sahip olacak şekilde adı benzersiz olmalıdır.

-   **Kullanıcı adı** – (yönetici kullanıcı adı) ACR ile ilişkili kullanıcı adı.

-   **Parola** – ACR ile ilişkili parola.

    >[!Note]
    >Kullanıcı adı ve parola iş ortakları yayımlama işleminde açıklanan ACR erişiminiz olduğundan emin olmak için gereklidir.

IOT Edge modülü görüntüsü yayımlarken bir veya daha fazla görüntü etiketleri sağlayabilir. Test sırasında modül kolayca tanımlayabilirsiniz böylece bir 'Son' etiketi (varsayılan), modülüne eklediğinizden emin olun.

Aşağıdaki IOT Edge modülü özelliklerini belirtebilirsiniz:

-   **createOptions** -varsayılan createOptions bu IOT Edge modülü yepyeni başlatılabilmesi geçirilecek.

-   **ikiz:** -bu IOT Edge modülü IOT modülü SDK'sı kullanırken yepyeni başlatılabilmesi geçirmek için varsayılan çifti.

-   **yollar:** -varsayılan yollar, böylece bu IOT Edge modülü yepyeni modülü IOT SDK'sı kullanıyorsanız başlatılabilir geçirilecek.

### <a name="describe-your-iot-edge-module-for-your-customers"></a>Müşterileriniz için IOT Edge modülü açıklayın

Pazarlama özgü içeriğinizi Marketi sekmesinden ekleyin. Bu bilgiler, hangi Azure Market'te genel olarak görünür ve listelenen olacağını olur.

![IOT Edge modülü genel bakış](./media/cloud-partner-portal-create-iot-edge-module-offer/overview.png)

-   **Başlık** -genel kullanıma yönelik, IOT Edge modülü başlığı.

-   **Özet** -gözatma gibi çoğu sayfalarındaki herkese, IOT Edge modülü özetini sayfaları.

-   **Uzun Özet** -modül öne çıkan zaman herkese, IOT Edge modülü özeti. 

-   **Açıklama** -Ürün Ayrıntıları sayfası üzerinde herkese, IOT Edge modülü açıklaması. (Temel HTML etiketleri, açıklama Biçimlendirilecek kullanabilirsiniz.)

-   **Pazarlama tanımlayıcısı** -ürün URL'nizi oluşturmak için kullanılan benzersiz bir tanımlayıcı. Bu URL aşağıdaki biçimdedir: *azuremarketplace.microsoft.com/enus/marketplace/apps/yourpublisherid.youriotedgemodulemarketingidentifier*.

-   **Abonelik kimlikleri Önizleme** - bu abonelikler için erişime sahip kullanıcılar, sertifika adımından sonra IOT Edge modülü görmeye olacaktır ve önce bu hizmet de gider Canlı.

-   **Faydalı bağlantılar** -Ürün Ayrıntıları sayfanızda gösterilecek en fazla 10 bağlantılar ekleyebilirsiniz.

-   **Kategorileri önerilen** -en fazla beş kategorilerini seçin. Bunlar, Ürün Ayrıntıları sayfasında gösterilir. Şu anda, tüm IOT Edge modülleri altında gösterilen *nesnelerin interneti \> IOT Edge Modülü* Gözat sayfalarında kategorileri.

-   **Logo** -IOT Edge modülü logosu görüntülerinizi PNG biçiminde karşıya yükleyin. Aşağıdaki boyutları kullanın: ve tam olarak aşağıdaki boyutları: 40 piksel x 40 piksel, 90 piksel x 90 piksel, 115 piksel x 115 piksel, 255 piksel x 115 piksel.

-   **Ekran görüntüleri** -ekran görüntüleri, Ürün Ayrıntıları sayfasında görüntülenir. Bunlar, görsel olarak, IOT Edge modülü yaptığı ve nasıl çalıştığı iletişim kurmak için iyi bir yoldur. Mimari diyagramları gösterebilir veya büyük/küçük harf çizimler örneği için kullanın.

-   **Videoları** -videoları, Ürün Ayrıntıları sayfasında görüntülenir. Bunlar, görsel olarak, IOT Edge modülü yaptığı ve nasıl çalıştığı iletişim kurmak için iyi bir yoldur.

-   **Müşteri adayı Yönetimi** -ilgi ürününüzde gösteren tüm müşteri adaylarını toplamak için bir sistem seçebilirsiniz.

-   **Gizlilik** -gizlilik ilkeniz işaret eden bir URL olmalıdır.

-   **Kullanım koşullarını** -kullanım koşullarını olması gerekir. Bu sayfa biçimlendirmek veya diğer sayfalarınızı birini işaret edecek HTML etiketlerini kullanabilirsiniz.

### <a name="enter-your-support-contact-information"></a>Destek iletişim bilgilerinizi girin

Destek sekmesinde belirtin **mühendislik birimi ilgili kişisi** ve **müşteri desteği** bilgileri.

![Destek kişileri](./media/cloud-partner-portal-create-iot-edge-module-offer/support-contact-info.png)


## <a name="certify-your-iot-edge-module"></a>Sertifika, IOT Edge Modülü

Gerekli tüm bilgileri verdikten sonra seçin **Yayımla** , IOT Edge modülü sertifikasyona göndermek için. Sertifika işlemi adımları gösteren bir zaman çizelgesi kullanıcının görürsünüz.

**Modül doğrulama**

Modülünüzün sertifika ekibimiz tarafından doğrulanır. Modül sertifikalı sonra modülünüzde test etmek için özel bir bağlantı elde edersiniz. Test edildikten sonra değişiklik yapmak gerekiyorsa, ürününüzün meta verilerini düzenleme ve sertifika ekibi modülü yeniden gönderin. 

## <a name="publish-your-iot-edge-module"></a>Yayımlama, IOT Edge Modülü

Sınama ve yayımlama için hazır olan tamamladıktan sonra seçin **Go Live** , IOT Edge modülü yayımlamak için.

>[!Important]
>Ignite etkinliğini sırasında duyurulan, IOT Edge modülü olmasını istiyorsanız, modülünüzde 23/09/2018 tarafından ortak olmalıdır.
