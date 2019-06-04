---
title: Öğretici - yeni kaynaklar blueprint kaynak kilitleri ile koruma
description: Bu öğreticide, Azure Blueprint kaynak kilitleri seçenekleri salt okunur öğreneceksiniz ve dağıtılan kaynakları yeni korunacak silmeyin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 274c437acd8df50d631727fc352c4b9ebecead18
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479980"
---
# <a name="tutorial-protect-new-resources-with-azure-blueprints-resource-locks"></a>Öğretici: Yeni kaynaklar Azure Blueprint kaynak kilitleri ile koruma

Azure şemaları ile [kaynak kilitleri](../concepts/resource-locking.md), yeni dağıtılan kaynaklar, hatta sahip bir hesap tarafından değiştirilmiş gelen Koruyabileceğiniz _sahibi_ rol. Bu koruma, bir Resource Manager şablonu yapıt tarafından oluşturulan kaynakları şema tanımlarında ekleyebilirsiniz.

Bu öğreticide, aşağıdaki adımları tamamlamanız:

> [!div class="checklist"]
> - Şema tanımını oluşturma
> - Blueprint tanımınızı olarak işaretlemek **yayımlandı**
> - Mevcut bir aboneliğe, şema tanımını atama
> - Yeni Kaynak Grup İnceleme
> - Kilitler kaldırmak için şema atamasını Kaldır

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-blueprint-definition"></a>Şema tanımını oluşturma

İlk olarak, şema tanımını oluşturun.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Üzerinde **Başlarken** seçin sol taraftaki sayfasında **Oluştur** altında **blueprint oluşturma**.

1. Bulma **boş Blueprint** sayfanın üstündeki şema örnek. Seçin **Başlat ile boş blueprint**.

1. Bu bilgileri girin **Temelleri** sekmesinde:

   - **Blueprint adı**: Şema örnek kopyası için bir ad sağlayın. Bu öğreticide, adı kullanacağız **kilitli storageaccount**.
   - **Blueprint açıklaması**: Şema tanımı için bir açıklama ekleyin. Kullanım **test blueprint kaynak üzerinde kilitlemek için dağıtılan kaynakların**.
   - **Tanım konumu**: Üç nokta düğmesini (…) seçin ve ardından şema tanımınızı kaydetmek için yönetim grubu veya abonelik seçin.

1. Seçin **Yapıtları** sekmesinde sayfanın üstünde ya da seçin **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Bir kaynak grubu, abonelik düzeyinde ekleyin:
   1. Seçin **yapıt ekleme** altında satır **abonelik**.
   1. Seçin **kaynak grubu** altında **Yapıt türü**.
   1. Ayarlama **Yapıt görünen ad** için **RGtoLock**.
   1. Bırakın **kaynak grubu adı** ve **konumu** kutularını boş, ancak bunları yapmak için her bir özellik onay kutusunun seçildiğinden emin olun **dinamik parametreleri**.
   1. Seçin **Ekle** blueprint'e yapıt eklemek için.

1. Kaynak grubu altında bir şablon ekleyin:
   1. Seçin **yapıt ekleme** altında satır **RGtoLock** girişi. 
   1. Seçin **Azure Resource Manager şablonu** altında **Yapıt türü**ayarlayın **Yapıt görünen ad** için **StorageAccount**ve bırakın **Açıklama** boş. 
   1. Üzerinde **şablon** sekmesinde, aşağıdaki Resource Manager şablonu Düzenleyicisi kutuya yapıştırın. Şablonda yapıştırdıktan sonra seçin **Ekle** blueprint'e yapıt eklemek için.

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

Sonra **başarılı şema tanımını kaydetme** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="publish-the-blueprint-definition"></a>Şema tanımını yayımlama

Şema tanımı, artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve bu atanan ve dağıtılan kullanılmadan önce yayımlanması gerekir.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; **kilitli storageaccount** blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni bölmede girin **1.0** olarak **sürüm**. Daha sonra bir değişiklik yaparsanız, bu özellik yararlıdır. Girin **notları değiştirmek**, gibi **ilk sürüm yayımlanan blueprint dağıtılan kaynakları kilitlemek için**. Ardından **Yayımla** sayfanın alt kısmındaki.

Bu adım, bir abonelik için şema atamak mümkün kılar. Şema tanımını yayımlandıktan sonra hala değişiklik yapabilirsiniz. Değişiklik yaparsanız, aynı şema tanımını sürümleri arasındaki farklar izlemek için yeni bir sürüm değeri tanımıyla yayımlamanız gerekir.

Sonra **yayımlama başarılı tanımı blueprint** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="assign-the-blueprint-definition"></a>Şema tanımını atama

Şema tanımını yayımlandıktan sonra kaydettiğiniz yönetim grubu dahilinde bir aboneliğe atayabilirsiniz. Bu adımda, blueprint tanımının her dağıtım benzersiz olacak şekilde parametreleri sağlayın.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; **kilitli storageaccount** blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - **Temel Bilgiler**

     - **Abonelikler**: Bir veya daha fazla, blueprint tanımının kaydedildiği yönetim grubundaki abonelikleri seçin. Birden fazla aboneliğiniz seçerseniz, girdiğiniz parametreleri kullanarak her abonelik için bir atama oluşturulur.
     - **Ödev adı**: Ad blueprint tanımının ada göre önceden doldurulmuştur. Bu atama kilitleme yeni kaynak grubunu temsil etmek için bu nedenle atama adı değiştirmek istiyoruz **atama kilitli storageaccount TestingBPLocks**.
     - **Konum**: Yönetilen kimlik oluşturulacağı bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
       Bu öğreticide, seçin **Doğu ABD 2**.
     - **Şema tanımı sürümü**: Yayımlanan sürümü **1.0** blueprint tanımının.

   - **Kilit atama**

     Seçin **salt okunur** blueprint kilit modu. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - **Yönetilen kimlik**

     Varsayılan seçeneği kullanın: **Sistem tarafından atanan**. Daha fazla bilgi için [yönetilen kimlikleri](../../../active-directory/managed-identities-azure-resources/overview.md).

   - **Yapıt parametreleri**

     Bu bölümde tanımlanan parametrelerin altında tanımlandıkları yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan çünkü. Her bir yapıt olarak gördükleri için parametre değeri ayarlamak **değer** sütun.

     |Yapıt adı|Yapıt türü|Parametre adı|Değer|Açıklama|
     |-|-|-|-|-|
     |RGtoLock kaynak grubu|Kaynak grubu|Ad|TestingBPLocks|Blueprint kilitleri uygulamak için yeni kaynak grubunun adını tanımlar.|
     |RGtoLock kaynak grubu|Kaynak grubu|Location|Batı ABD 2|Blueprint kilitleri uygulamak için yeni kaynak grubunun konumunu tanımlar.|
     |Depolama hesabı|Resource Manager şablonu|storageAccountType (depolama hesabı)|Standard_GRS|Depolama SKU'su. Varsayılan değer _Standard_LRS_.|

1. Tüm parametreler girdikten sonra seçin **atama** sayfanın alt kısmındaki.

Bu adım tanımlanmış kaynakları dağıtır ve seçili yapılandırır **kilit atama**. Bu, blueprint kilitleri uygulamak için en fazla 30 dakika sürebilir.

Sonra **başarılı atama şema tanımını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Atamaya göre dağıtılan kaynakları inceleyin

Atama kaynak grubu oluşturulur _TestingBPLocks_ ve Resource Manager şablonu yapıt tarafından dağıtılan bir depolama hesabı. Atama Ayrıntıları sayfasında, yeni kaynak grubu ve seçili kilit durumu gösterilir.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; **atama kilitli storageaccount TestingBPLocks** blueprint ataması ve ardından bu seçeneği belirleyin.

   Bu sayfadan kaynakları yeni şema kilitleme durumu ile dağıtıldı ve atama başarılı olduğunu görebiliriz. Atama güncelleştirdiyseniz, **atama işlemi** açılan, dağıtım her tanım sürümüne ayrıntılarını gösterir. Özellik sayfasını açmak için kaynak grubunu seçebilirsiniz.

1. Seçin **TestingBPLocks** kaynak grubu.

1. Seçin **erişim denetimi (IAM)** soldaki sayfası. Ardından **rol atamaları** sekmesi.

   Olduğunu görebiliriz burada _atama kilitli storageaccount TestingBPLocks_ blueprint ataması _sahibi_ rol. Bu rol, dağıtmak ve kaynak grubu kilitlemek için kullanıldığından bu rolü var.

1. Seçin **atamaları Reddet** sekmesi.

   Oluşturulan şema atamasını bir [atamasını Reddet](../../../role-based-access-control/deny-assignments.md) zorlamak için dağıtılan kaynak grubunda **salt okunur** blueprint kilit modu. Reddetme atama uygun haklara sahip biri üzerinde engeller **rol atamaları** belirli eylemleri gelen sekmesi. Reddetme atama etkiler _tüm ilkeleri_.

   Sorumlu bir reddetme atamasından dışlama hakkında daha fazla bilgi için bkz: [Blueprint kaynak kilitleme](../concepts/resource-locking.md#exclude-a-principal-from-a-deny-assignment).

1. Reddetme Ataması'nı seçin ve ardından **izinler reddedildi** soldaki sayfası.

   Reddetme atama ile yapılan tüm işlemlerde engelliyor **\*** ve **eylem** yapılandırma da izin verir ancak okuma erişimi hariç tutarak  **\* /Okuma**aracılığıyla **NotActions**.

1. Azure portal içerik haritasındaki, seçin **TestingBPLocks - erişim denetimi (IAM)** . Ardından **genel bakış** sayfasında soldaki ardından **kaynak grubunu Sil** düğmesi. Bir ad girin **TestingBPLocks** silme işlemini onaylayın ve ardından **Sil** bölmesinin alt kısmındaki.

   Portal bildiriminden **TestingBPLocks başarısız oldu. kaynak grubunu Sil** görünür. Kaynak grubunu silme izni olsa da hesabınızı hatayı bildiren, şema atamasını tarafından erişim reddedildi. Biz seçildiğini unutmayın **salt okunur** blueprint ataması sırasında şema kilit modu. Şema kilidi iznine sahip bir hesap da engeller _sahibi_, kaynak silmelerini. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

Bu adımlar, dağıtılan kaynaklarımızın bile kaynakları silmek için izni olan hesabın istenmeyen silinmesini engellemek şema kilit ile artık korunduğunu gösterir.

## <a name="unassign-the-blueprint"></a>Şema atamasını Kaldır

Son adım, şema tanımını atama kaldırmaktır. Atama kaldırılıyor, ilişkilendirilmiş yapılarının kaldırmaz.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; **atama kilitli storageaccount TestingBPLocks** blueprint ataması ve ardından bu seçeneği belirleyin.

1. Seçin **Atamayı Kaldır blueprint** sayfanın üstünde. Onay iletişim kutusunda uyarı okuyun ve ardından **Tamam**.

   Şema atamasını kaldırıldığında, şemayı kilitler de kaldırılır. Kaynakları bir kez daha uygun izinlere sahip bir hesap tarafından silinebilir.

1. Seçin **kaynak grupları** Azure menüsünden ve ardından **TestingBPLocks**.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ve ardından **rol atamaları** sekmesi.

Şema atamasını artık olduğu kaynak grubu için güvenlik görünür _sahibi_ erişim.

Sonra **başarılı kaldırma şema atamasını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle tamamladığınızda, bu kaynakları silin:

- Kaynak grubu _TestingBPLocks_
- Blueprint tanımının _kilitli storageaccount_

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [yaşam döngüsü blueprint](../concepts/lifecycle.md).
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- Nasıl kullanılacağını öğrenmek [blueprint kaynak kilitleme](../concepts/resource-locking.md).
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Sorunlarını giderme](../troubleshoot/general.md) blueprint ataması sırasında.
