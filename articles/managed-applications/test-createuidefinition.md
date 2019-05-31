---
title: Azure yönetilen uygulamalar için kullanıcı Arabirimi tanımını test etme | Microsoft Docs
description: Azure yönetilen uygulamanızın portal aracılığıyla oluşturmak için kullanıcı deneyimi sınamak açıklar.
author: tfitzmac
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 99ca319910be2cb20214172826eb40361abe72f0
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257647"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Azure yönetilen uygulamalar için portal arabirimi testi

Sonra [createUiDefinition.json dosyası oluşturma](create-uidefinition-overview.md) yönetilen uygulamanız için kullanıcı deneyimi sınamak gerekir. Test etmeyi kolaylaştırmak için portal dosyanızda yükler ve korumalı bir ortamda kullanın. Aslında, yönetilen uygulamayı dağıtmak gerekmez. Korumalı alan geçerli, tam ekran portal deneyiminde, kullanıcı arabirimi sunar. Veya, arabirim, ancak test portal'ın eski bir görünüm kullanan bir PowerShell betiğini kullanabilirsiniz. Bu makalede her iki yaklaşım gösterilmektedir. Korumalı alan arabirimi önizlemek için önerilen yoldur.

## <a name="prerequisites"></a>Önkoşullar

* A **createUiDefinition.json** dosya. Bu dosya yoksa, kopyalama [örnek dosyası](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json).

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="use-sandbox"></a>Korumalı alanını kullanma

1. Açık [UI tanımı korumalı alanı oluşturun](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade).

   ![Korumalı alan göster](./media/test-createuidefinition/show-sandbox.png)

1. Boş tanımı createUiDefinition.json dosyanızı içeriğiyle değiştirin. Seçin **Önizleme**.

   ![Önizleme'yi seçin](./media/test-createuidefinition/select-preview.png)

1. Oluşturduğunuz formu görüntülenir. Bir kullanıcı deneyiminin adım ve değerleri doldurun.

   ![Form Göster](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>Sorun giderme

Formunuza seçtikten sonra görüntülenmiyorsa **Önizleme**, sözdizimi hatası olabilir. Bu sayfaya gidin ve sağ kaydırma çubuğundaki kırmızı göstergesi bulun.

![Sözdizimi hatası Göster](./media/test-createuidefinition/show-syntax-error.png)

Formunuza görüntülemez ve bunun yerine bir bulutun etiket bırakma ile bir simge görürsünüz, formunuzu özelliği eksik gibi bir hata var. Web geliştirici araçları, tarayıcınızda açın. **Konsol** Arabiriminizin hakkında önemli iletileri görüntüler.

![Hatayı Göster](./media/test-createuidefinition/show-error.png)

## <a name="use-test-script"></a>Test betiği kullanın

Portalda Arabiriminizin Test etmek için aşağıdaki komut dosyalarından birini yerel makinenize kopyalayın:

* [PowerShell dışarıdan yükleme betiği](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Azure CLI dışarıdan yükleme betiği](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

Portal arabirimi dosyanızda görmek için indirdiğiniz betiğin çalıştırın. Betik, Azure Aboneliğinize bir depolama hesabı oluşturur ve createUiDefinition.json dosyanızı depolama hesabına yükler. Depolama hesabı tarafından silindiği veya depolama hesabı, ilk kez çalıştırdığınızda, komut dosyası oluşturulur. Depolama hesabı, Azure aboneliğinizde zaten varsa bu betiği kullanır. Betik portalı açılır ve depolama hesabından dosyanızı yükler.

Depolama hesabı için bir konum sağlayın ve createUiDefinition.json dosyanızı içeren klasörü belirtin.

PowerShell için şunu kullanın:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory .\100-Marketplace-Sample
```

Azure CLI için şunu kullanın:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a .\100-Marketplace-Sample
```

Komut dosyasını aynı klasöre, createUiDefinition.json dosyasıdır ve depolama hesabı zaten oluşturdunuz, bu parametreleri belirtmeniz gerekmez.

PowerShell için şunu kullanın:

```powershell
.\SideLoad-CreateUIDefinition.ps1
```

Azure CLI için şunu kullanın:

```azurecli
./sideload-createuidef.sh
```

Betik, tarayıcınızda yeni bir sekmede açılır. Yönetilen uygulama oluşturma arabiriminize portalıyla gösterir.

![Portalı görüntüle](./media/test-createuidefinition/view-portal.png)

Alanlar için değerler sağlayın. İşiniz bittiğinde şablona geçirilen değerlerin bakın.

![Değerleri göster](./media/test-createuidefinition/show-json.png)

Bu değerler, dağıtım şablonu test parametre dosyası olarak kullanabilirsiniz.

Özet ekranında portalda yanıt vermemeye başlıyor, çıktı bölümünde bir hata olabilir. Örneğin, mevcut olmayan bir denetim başvurulan. Çıktıda bir parametre boş ise, parametre mevcut olmayan bir özelliğe başvurma. Örneğin, Denetim başvurusu geçerli, ancak özellik başvurusu geçerli değil.

## <a name="test-your-solution-files"></a>Çözüm dosyalarınızı test

Portal arabirimi beklendiği gibi çalıştığını doğruladıktan sonra createUiDefinition dosyanızı mainTemplate.json dosyanızla düzgün bir şekilde tümleşiktir doğrulamak için zaman var. CreateUiDefinition dosyası dahil olmak üzere çözüm dosyalarınızı içeriğini sınamak için bir doğrulama betiği çalıştırabilirsiniz. Betik JSON söz dizimi doğrular, regex ifadeler metin alanları için denetler ve portal arabirimi çıkış değerlerini şablonunuzun parametrelerle eşleştiğinden emin olur. Bu betik çalıştırma hakkında daha fazla bilgi için bkz. [şablonları için statik doğrulama denetimlerini çalıştırmak](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Sonraki adımlar

Portal arabirimi doğruladıktan sonra sağlama hakkında bilgi edinin, [Azure Marketi'nde uygulama yönetilen](publish-marketplace-app.md).
