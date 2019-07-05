---
title: Örnek - PCI-DSS v3.2.1 şema - dağıtma adımları
description: Adımlar ödeme kartı sektör veri güvenliği standardı v3.2.1 şema örnek dağıtın.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b4e9435e5f569a076fc5beb6441b9da935b87f3a
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561377"
---
# <a name="deploy-the-pci-dss-v321-blueprint-sample"></a>PCI-DSS v3.2.1 blueprint örneği dağıtma

Azure kavramsal tasarımlar PCI-DSS v3.2.1 şema örnek dağıtmak için aşağıdaki adım atılmalıdır:

> [!div class="checklist"]
> - Örnekten yeni blueprint oluşturma
> - Örnek olarak kopyanızın işaretlemek **yayımlandı**
> - Blueprint kopyasını mevcut bir aboneliğe atayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten Blueprint oluşturma

İlk olarak, şema örnek bir başlangıç örneğini kullanarak ortamınızda yeni bir şema oluşturarak uygulayın.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **PCI-DSS v3.2.1** şema örnek altında _diğer örnekleri_ seçip **Bu örneği kullanmak**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: PCI-DSS v3.2.1 şema örnek kopyası için bir ad sağlayın.
   - **Tanım konumu**: Üç nokta kullanın ve örneğe kopyasını kaydetmek için yönetim grubunu seçin.

1. Seçin _Yapıtları_ sayfanın üst kısmındaki sekme veya **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Şema örnek değişiklikleri yapıtların listesini gözden geçirin. Yapıtlar birçoğu, daha sonra tanımlarız parametrelere sahip. Seçin **Taslağı Kaydet** bittiğinde şema örnek gözden geçirme.

## <a name="publish-the-sample-copy"></a>Örnek kopyalama yayımlama

Şema örnek kopyanızın artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve olmalıdır **yayımlanan** , atanan ve dağıtılan kullanılmadan önce. Değişiklik standart PCI-DSS v3.2.1 uzağa taşıyabilir ancak bu şema kopyasını gereksinimlerine ve ortam için özelleştirilebilir.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni sayfa sağlayan bir **sürüm** şema örnek kopyası için. Bu özellik, daha sonra bir değişikliği yapmak için yararlıdır. Sağlamak **notları değiştirmek** "ilk sürüm PCI-DSS v3.2.1 blueprint örnekten yayımlanan gibi." Ardından **Yayımla** sayfanın alt kısmındaki.

## <a name="assign-the-sample-copy"></a>Örnek kopya atama

Blueprint kopyasını başarıyla silindikten sonra **yayımlanan**, yönetim grubu için kaydedildi dahilinde bir aboneliğe atanabilir. Bu adım, burada parametreler şema kopyasını, her dağıtım benzersiz olacak şekilde sağlanır.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: Bir veya daha fazla yönetim grubuna olduğunuz abonelikleri için şema örnek kopyanızın kaydedilen seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
     - **Ödev adı**: Şema adını temel alarak, önceden doldurulmuş adıdır.
       Gerektiği gibi değiştirin ya da olduğu gibi bırakın.
     - **Konum**: Yönetilen kimlikle oluşturulması için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Şema tanımı sürümü**: Çekme bir **yayımlanan** şema örnek kopyanızın sürümü.

   - Kilit atama

     Ortamınızı ayarlama şema kilidi seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan değeri bırakın _sistem tarafından atanan_ yönetilen kimlik seçeneği.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Tam bir liste veya yapıt parametrelerin ve Tanımlamaların için bkz. [Yapıt parametreleri tablo](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki. Şema atamasını oluşturulur ve yapıt dağıtımı başlar. Dağıtım gereken yaklaşık bir saat. Dağıtım durumunu denetlemek için şema atamasını açın.

> [!WARNING]
> Azure şemaları hizmet ve yerleşik şema örnekleri **ücretsiz olarak sunulmaktadır**. Azure kaynaklarıdır [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Kullanım [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) çalıştıran bu şema örnek tarafından dağıtılan kaynakların maliyetini tahmin etmek için.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri Tablo

Aşağıdaki tabloda, yapıt parametreleri şema listesi sağlar:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|[Önizleme]\: PCI v3.2.1:2018 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma|İlke Ataması|Kaynak türleri listesi | Seçilen kaynak türleri için tanılama ayarını denetle. Tüm kaynaklar seçili varsayılan değer:| 
|İzin verilen konumlar|İlke Ataması|İzin verilen konumların listesi|Veri Merkezi konumlarını uygulamasına dağıtılması herhangi bir kaynak için izin verilen listesi. Bu liste, küresel olarak istenen Azure konumları için özelleştirilebilir. İzin vermek istediğiniz konumları seçin.| 
|Kaynak grupları için izin verilen konumlar|İlke Ataması |İzin verilen konumu |Bu ilke, kuruluşunuzun içindeki kaynak grupları oluşturabilirsiniz konumları kısıtlamanıza olanak sağlar. Coğrafi uyumluluk gereksinimlerinizi zorlamak için bu seçeneği kullanın.| 
|SQL sunucularında denetim dağıtma|İlke Ataması|Saklanacağı gün sayısı|Verileri elde tutma gün sayısı. Varsayılan değer 180 ancak PCI 365 gerektirir.| 
|SQL sunucularında denetim dağıtma|İlke Ataması|Depolama hesabı için kaynak grubu adı|Denetim veritabanı olayları bir denetim (bir depolama hesabı, bu bölgedeki tüm sunucular tarafından paylaşılan bir SQL Server'ın oluşturulduğu her bölgede oluşturulur), Azure depolama hesabınızdaki günlüğüne yazar.| 

## <a name="next-steps"></a>Sonraki adımlar

PCI-DSS v3.2.1 şema örnek dağıtmak için adımları gözden geçirdikten sonra genel bakış ve denetimi eşleme hakkında bilgi edinmek için şu makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 şema - genel bakış](./index.md)
> [PCI-DSS v3.2.1 şema - denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
