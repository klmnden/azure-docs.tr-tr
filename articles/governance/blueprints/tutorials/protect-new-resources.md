---
title: Şema kaynak kilitleriyle yeni kaynakları koruma
description: Salt okunur Azure Blueprint kaynak kilitleri nasıl kullanacağınızı öğrenin ve dağıtılan kaynakları yeni korunacak silmeyin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b885a90728df8cb15c75141b7bce81aec3968359
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65779580"
---
# <a name="tutorial-protect-new-resources-with-azure-blueprints-resource-locks"></a>Öğretici: Yeni kaynaklar Azure Blueprint kaynak kilitleri ile koruma

Azure bir Blueprint'i [kaynak kilitleri](../concepts/resource-locking.md) , hatta sahip bir hesap tarafından değiştirilmiş gelen yeni dağıtılan kaynakları korumak mümkün kılar _sahibi_ rol. Bu koruma, bir Resource Manager şablonu yapıt şema tanımı tarafından oluşturulan kaynakları eklenebilir.

Aşağıdaki adımları ele alınmaktadır:

> [!div class="checklist"]
> - Yeni bir şema tanımını oluşturma
> - Blueprint tanımınızı olarak işaretlemek **yayımlandı**
> - Mevcut bir aboneliğe, şema tanımını atama
> - Yeni Kaynak Grup İnceleme
> - Kilitler kaldırmak için şema atamasını Kaldır

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-new-blueprint-definition"></a>Yeni şema tanımını oluşturma

İlk olarak, yeni şema tanımını oluşturun.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **boş Blueprint** şema örnek seçin ve sayfanın üst kısmındaki **Başlat ile boş blueprint**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: Şema örnek kopyası için bir ad sağlayın. Bu öğreticide, adı kullanacağız _kilitli storageaccount_.
   - **Blueprint açıklaması**: Şema tanımını açıklar. Kullanım "için dağıtılan kaynakları kilitleme test blueprint kaynak."
   - **Tanım konumu**: Üç nokta kullanın ve bir yönetim grubuna veya aboneliğe blueprint tanımınızı kaydetmek için'ı seçin.

1. Seçin _Yapıtları_ sayfanın üst kısmındaki sekme veya **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Kaynak grubu aboneliği ekleyin: Seçin **+ yapıt ekleme...**  altında satır **abonelik**.
   _Yapıt türü_ olarak 'Kaynak Grubu'nu seçin. Ayarlama _Yapıt görünen ad_ için **RGtoLock**.
   _Kaynak Grubu Adı_ ve _Konum_ alanlarını boş bırakın, diğer yandan **dinamik parametre** yapmak için her bir özelliğin onay kutusunun işaretli olduğundan emin olun. Bu yapıtı şemaya eklemek için **Ekle**’ye tıklayın.

1. Kaynak grubu altında şablon ekleyin: Seçin **+ yapıt ekleme...** altında satır **RGtoLock** girişi. _Yapıt türü olarak_ 'Azure Resource Manager şablonu'nu seçin, _Yapıt görünen adı_’nı 'StorageAccount' olarak ayarlayın ve _Açıklama_ alanını boş bırakın. Düzenleyici kutusundaki **Şablon** sekmesinde aşağıdaki Resource Manager şablonunu yapıştırın. Şablon yapıştırılan seçin **Ekle** blueprint'e yapıt bu eklemek için.

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "storageAccountType": {
               "type": "string",
               "defaultValue": "Standard_LRS",
               "allowedValues": [
                   "Standard_LRS",
                   "Standard_GRS",
                   "Standard_ZRS",
                   "Premium_LRS"
               ],
               "metadata": {
                   "description": "Storage Account type"
               }
           }
       },
       "variables": {
           "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
       },
       "resources": [{
           "type": "Microsoft.Storage/storageAccounts",
           "name": "[variables('storageAccountName')]",
           "location": "[resourceGroup().location]",
           "apiVersion": "2018-07-01",
           "sku": {
               "name": "[parameters('storageAccountType')]"
           },
           "kind": "StorageV2",
           "properties": {}
       }],
       "outputs": {
           "storageAccountName": {
               "type": "string",
               "value": "[variables('storageAccountName')]"
           }
       }
   }
   ```

1. Seçin **Taslağı Kaydet** sayfanın alt kısmındaki.

Bu adım, seçilen yönetim grubu veya abonelik içinde şema tanımını oluşturur.

Bir kez **başarılı şema tanımını kaydetme** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="publish-the-blueprint-definition"></a>Şema tanımını yayımlama

Şema tanımı, artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve olmalıdır **yayımlanan** , atanan ve dağıtılan kullanılmadan önce.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; _kilitli storageaccount_ blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni bölmede sağlamak **sürüm** olarak _1.0_. Bu özellik, daha sonra bir değişikliği yapmak için yararlıdır. Sağlamak **notları değiştirmek** "ilk sürüm şema dağıtılan kaynakları kilitlemek için yayımlanan gibi." Ardından **Yayımla** sayfanın alt kısmındaki.

Bu adım, bir abonelik için şema atamak mümkün kılar. Bir kez yayımlandıktan sonra değişiklikleri yine de yapılabilir. Ek bir değişiklik yapılması yeni bir yayımlama **sürüm** aynı şema tanımını farklı sürümleri arasında farklar izlemek için değer.

Bir kez **yayımlama başarılı tanımı blueprint** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="assign-the-blueprint-definition"></a>Şema tanımını atama

Şema tanımını başarıyla silindikten sonra **yayımlanan**, yönetim grubu için kaydedildi dahilinde bir aboneliğe atanabilir. Bu adım, blueprint tanımının her dağıtım benzersiz hale getirmek için parametreler burada sağlanır.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; _kilitli storageaccount_ blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - Temel

     - **Abonelikler**: Bir veya daha fazla yönetim grubuna olduğunuz Abonelikleri, şema tanımına kaydedilmiş seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
     - **Ödev adı**: Şema tanımını adını temel alarak, önceden doldurulmuş adıdır. Bu atama kilitleme yeni kaynak grubunu temsil etmek için bu nedenle atama adı değiştirmek istiyoruz _atama kilitli storageaccount TestingBPLocks_.
     - **Konum**: Yönetilen kimlikle oluşturulması için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
       Bu öğreticide, seçin _Doğu ABD 2_.
     - **Şema tanımı sürümü**: Çekme **yayımlanan** sürüm _1.0_ blueprint tanımının.

   - Atamayı Kilitle

     Seçin _salt okunur_ blueprint kilit modu. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan değeri bırakın _sistem tarafından atanan_ seçeneği. Daha fazla bilgi için [yönetilen kimlikleri](../../../active-directory/managed-identities-azure-resources/overview.md).

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Her bir yapıt ne tanımlanan için parametre değeri ayarlamak **değer** sütun.

     |Yapıt adı|Yapıt türü|Parametre adı|Değer|Açıklama|
     |-|-|-|-|-|
     |RGtoLock kaynak grubu|Kaynak grubu|Ad|TestingBPLocks|Blueprint kilitleri uygulamak için yeni kaynak grubunun adını tanımlar.|
     |RGtoLock kaynak grubu|Kaynak grubu|Location|Batı ABD 2|Blueprint kilitleri uygulamak için yeni kaynak grubunun konumunu tanımlar.|
     |StorageAccount|Resource Manager şablonu|storageAccountType (depolama hesabı)|Standard_GRS|Depolama SKU'SU'ı seçin. Varsayılan değer _Standard_LRS_.|

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki.

Bu adım tanımlanmış kaynakları dağıtır ve seçili yapılandırır **kilit atama**. Blueprint kilitleri uygulamak için en fazla 30 dakika sürebilir.

Bir kez **başarılı atama şema tanımını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Atamaya göre dağıtılan kaynakları inceleyin

Atama oluşturduğunuz kaynak grubunu _TestingBPLocks_ ve Resource Manager şablonu yapıt tarafından dağıtılan bir depolama hesabı. Atama Ayrıntıları sayfasında, yeni kaynak grubu ve seçili kilit durumu görüntülenir.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; _atama kilitli storageaccount TestingBPLocks_ blueprint ataması ve ardından bu seçeneği belirleyin.

   Bu sayfadan başarılı atama görebiliriz ve kaynakları, yeni şema kilitleme durumu ile dağıtıldı. Atama güncelleştirdiyseniz, **atama işlemi** açılan, dağıtım her tanım sürümüne ayrıntılarını gösterir. Kaynak grubu, doğrudan özellik sayfasını açmak üzere tıklanabilecek.

1. Seçin **TestingBPLocks** kaynak grubu.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ardından **rol atamaları** sekmesi.

   Olduğunu görebiliriz burada _atama kilitli storageaccount TestingBPLocks_ blueprint ataması _sahibi_ dağıtmak ve kaynak grubu kilitlemek için rol olarak kullanıldı.

1. Seçin **atamaları Reddet** sekmesi.

   Oluşturulan şema atamasını bir [atamasını Reddet](../../../role-based-access-control/deny-assignments.md) zorlamak için dağıtılan kaynak grubunda _salt okunur_ blueprint kilit modu. Reddetme atama uygun haklara sahip biri üzerinde engeller _rol atamaları_ belirli eylemleri gelen sekmesi. Reddetme atama etkiler _tüm ilkeleri_.

   Sorumlu bir reddetme atamasından dışlama hakkında daha fazla bilgi için bkz: [Blueprint kaynak kilitleme](../concepts/resource-locking.md#exclude-a-principal-from-a-deny-assignment).

1. Reddet Ataması'nı seçin ve ardından **izinler reddedildi** soldaki sayfası.

   Reddetme atama ile yapılan tüm işlemlerde engelliyor **\*** ve **eylem** yapılandırma, ancak hariç tutarak okuma erişimi verir  **\* /Okuma**aracılığıyla **NotActions**.

1. Azure portal Kırıntı seçin **TestingBPLocks - erişim denetimi (IAM)**. Ardından **genel bakış** sayfasında soldaki ardından **kaynak grubunu Sil** düğmesi. Bir ad girin _TestingBPLocks_ seçin ve silmeyi onaylamak için **Sil** bölmesinin alt kısmındaki.

   Portal bildiriminden **TestingBPLocks başarısız oldu. kaynak grubunu Sil** görüntülenir. Hesabınızı kaynak grubunu silme izni sahipken hatayı bildiren, şema atamasını tarafından erişim reddedildi. Biz seçildiğini unutmayın _salt okunur_ blueprint ataması sırasında şema kilit modu. Şema kilidi iznine sahip bir hesap da engeller _sahibi_, kaynak silmelerini. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

Bu adımlar, dağıtılan kaynaklarımızın iznine sahip bir hesaptan bile istenmeyen silmeyi önleyen blueprint kilit ile artık korunduğunu gösterir.

## <a name="unassign-the-blueprint"></a>Şema atamasını Kaldır

Son adım, şema tanımını atama kaldırmaktır. Atama kaldırılıyor bilgiler yapıtlar kaldırmaz.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; _atama kilitli storageaccount TestingBPLocks_ blueprint ataması ve ardından bu seçeneği belirleyin.

1. Seçin **Atamayı Kaldır blueprint** sayfanın üstünde düğme. Onay iletişim kutusunda uyarı okuyun ve ardından **Tamam**.

   Blueprint ataması kaldırıldı ile şema kilitler de kaldırılır. Oluşturulan kaynakları yeniden izinlerine sahip bir hesap tarafından silinebilir.

1. Seçin **kaynak grupları** Azure ardından menüden **TestingBPLocks**.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ardından **rol atamaları** sekmesi.

Şema atamasını artık olduğu kaynak grubu için güvenlik görünür _sahibi_ erişim.

Bir kez **başarılı kaldırma şema atamasını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle tamamlandığında, aşağıdaki kaynakları silin:

- Kaynak grubu _TestingBPLocks_
- Blueprint tanımının _kilitli storageaccount_

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.