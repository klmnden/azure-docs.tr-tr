---
title: Azure yönetilen uygulamalar için kullanıcı Arabirimi tanımını test etme | Microsoft Docs
description: Azure yönetilen uygulamanızın portal aracılığıyla oluşturmak için kullanıcı deneyimi sınamak açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2018
ms.author: tomfitz
ms.openlocfilehash: 2c313538e297c5781b48fcfe9d0d5390f94c97f5
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043969"
---
# <a name="test-azure-portal-interface-for-your-managed-application"></a>Yönetilen uygulamanız için Azure portal arabirimi testi
Sonra [createUiDefinition.json dosyası oluşturma](create-uidefinition-overview.md) Azure yönetilen uygulama için kullanıcı deneyimini test gerekir. Test etmeyi kolaylaştırmak için portal dosyanızda yükleyen bir betik kullanın. Aslında, yönetilen uygulamayı dağıtmak gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* A **createUiDefinition.json** dosya. Bu dosya yoksa, kopyalama [örnek dosyası](https://github.com/Azure/azure-quickstart-templates/blob/master/test/template-validation-tests/sample-template/createUIDefinition.json) ve yerel olarak kaydedin.

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="download-test-script"></a>Test betiği indirin

Portalda Arabiriminizin Test etmek için aşağıdaki komut dosyalarından birini yerel makinenize kopyalayın:

* [PowerShell dışarıdan yükleme betiği](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Azure CLI dışarıdan yükleme betiği](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

## <a name="run-script"></a>Betiği çalıştırın

Portal arabirimi dosyanızda görmek için indirdiğiniz betiğin çalıştırın. Betik, Azure Aboneliğinize bir depolama hesabı oluşturur ve createUiDefinition.json dosyanızı depolama hesabına yükler. Ardından, portal açar ve depolama hesabından dosyanızı yükler.

Depolama hesabı için bir konum sağlayın ve createUiDefinition.json dosyanızı içeren klasörü belirtin. Depolama hesabı konumu ilk zaman betiği çalıştırın veya depolama hesabı silindiyse sağlamak yeterlidir.

PowerShell için şunu kullanın:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory <path-to-folder-with-createuidefinition>
```

Azure CLI için şunu kullanın:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a <path-to-folder-with-createuidefinition>
```

## <a name="test-your-interface"></a>Test, arabirimi

Betik, tarayıcınızda yeni bir sekmede açılır. Yönetilen uygulama oluşturma arabiriminize portalıyla gösterir.

![Portalı görüntüle](./media/test-createuidefinition/view-portal.png)

Web geliştirici araçları, alanı doldurarak önce tarayıcınızda açın. **Konsol** Arabiriminizin hakkında önemli iletileri görüntüler.

![Konsol seçin](./media/test-createuidefinition/select-console.png)

Arabirim tanımı, bir hata varsa, konsolu açıklamaya bakın.

![Hatayı göster](./media/test-createuidefinition/show-error.png)

Alanlar için değerler sağlayın. İşiniz bittiğinde şablona geçirilen değerlerin bakın.

![Değerleri göster](./media/test-createuidefinition/show-json.png)

## <a name="test-your-solution-files"></a>Çözüm dosyalarınızı test

Portal arabirimi beklendiği gibi çalıştığını doğruladıktan sonra createUiDefinition dosyanızı mainTemplate.json dosyanızla düzgün bir şekilde tümleşiktir doğrulamak için zaman var. CreateUiDefinition dosyası dahil olmak üzere çözüm dosyalarınızı içeriğini sınamak için bir doğrulama betiği çalıştırabilirsiniz. Betik JSON söz dizimi doğrular, regex ifadeler metin alanları için denetler ve portal arabirimi çıkış değerlerini şablonunuzun parametrelerle eşleştiğinden emin olur. Bu betik çalıştırma hakkında daha fazla bilgi için bkz. [şablonları için statik doğrulama denetimlerini çalıştırmak](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Sonraki adımlar

Portal arabirimi doğruladıktan sonra sağlama hakkında bilgi edinin, [Azure Marketi'nde uygulama yönetilen](publish-marketplace-app.md).
