---
title: Hizmet Kataloğu uygulaması dağıtmak için Azure portalını kullanma | Microsoft Docs
description: Yönetilen uygulama tüketicileri, Azure portalı üzerinden bir hizmet Kataloğu uygulaması dağıtma işlemi gösterilmektedir.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 4d2e8b442f70ee791fe65a32402e5272eda3f209
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60589046"
---
# <a name="deploy-service-catalog-app-through-azure-portal"></a>Azure Portalı aracılığıyla hizmet Kataloğu uygulaması dağıtma

İçinde [hızlı önceki](publish-managed-app-definition-quickstart.md), yönetilen uygulama tanımı yayımlandı. Bu hızlı başlangıçta, bu tanımından bir hizmet Kataloğu uygulaması oluşturun.

## <a name="create-service-catalog-app"></a>Hizmet Kataloğu uygulaması oluşturma

Azure portalında aşağıdaki adımları kullanın:

1. Seçin **kaynak Oluştur**.

   ![Kaynak oluşturma](./media/deploy-service-catalog-quickstart/create-new.png)

1. Arama **hizmet Kataloğu yönetilen uygulaması** ve kullanılabilir seçenekler arasından seçin.

   ![Hizmet Kataloğu uygulaması arayın](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. Yönetilen uygulama hizmeti bir açıklamasını görürsünüz. **Oluştur**’u seçin.

   ![Oluştur’u seçin](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. Portal erişimi olan bir yönetilen uygulama tanımlarını gösterir. Kullanılabilir tanımlarından dağıtmak istediğiniz birini seçin. Bu hızlı başlangıçta kullanacaksınız **yönetilen depolama hesabı** önceki hızlı başlangıçta oluşturduğunuz tanımı. **Oluştur**’u seçin.

   ![Dağıtılacak tanımı seçin](./media/deploy-service-catalog-quickstart/select-definition.png)

1. İçin değerler sağlayın **Temelleri** sekmesi. Hizmet Kataloğu uygulamanızı dağıtmak için Azure aboneliği seçin. Adlı yeni bir kaynak grubu oluşturma **applicationGroup**. Uygulamanız için bir konum seçin. İşiniz bittiğinde seçin **Tamam**.

   ![Basic için değerler sağlayın](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. Depolama hesabı adı için önek sağlar. Oluşturulacak depolama hesabı türünü seçin. İşiniz bittiğinde seçin **Tamam**.

   ![Depolama için değerler sağlama](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. Özeti gözden geçirin. Doğrulama başarılı olduktan sonra seçin **Tamam** dağıtımına başlamak için.

   ![Özeti görüntüle](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>Sonuçları görüntüleme

Hizmet Kataloğu uygulaması dağıtıldıktan sonra iki yeni kaynak grubunuz olur. Bir kaynak grubu, hizmet Kataloğu uygulaması barındırır. Bir kaynak grubu kaynaklarını hizmet Kataloğu uygulaması içerir.

1. Adlı kaynak grubunu görüntülemek **applicationGroup** hizmet Kataloğu uygulamasını görmek için.

   ![Uygulamayı görüntüleyin](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. Adlı kaynak grubunu görüntülemek **applicationGroup {characters karma}** hizmet Kataloğu uygulaması için kaynakları görmek için.

   ![Kaynakları görüntüle](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen bir uygulama için tanım dosyalarını oluşturmayı öğrenmek için bkz: [oluştur ve yönetilen uygulama tanımı yayımlama](publish-service-catalog-app.md).
* Azure CLI için bkz: [Azure CLI ile dağıtma hizmet Kataloğu uygulaması](./scripts/managed-application-cli-sample-create-application.md).
* PowerShell için bkz: [PowerShell ile dağıtma hizmet Kataloğu uygulaması](./scripts/managed-application-poweshell-sample-create-application.md).