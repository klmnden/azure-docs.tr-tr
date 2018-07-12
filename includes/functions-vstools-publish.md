---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 6cbf24c56458ab8b3b6c7b44bedbd8b48d4677b3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38739233"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

2. **Azure İşlev Uygulaması**'nı, **Yeni Oluştur**'u ve sonra da **Yayımla**'yı seçin.

    ![Yayımlama hedefi seçme](./media/functions-vstools-publish/functions-vstools-create-new-function-app.png)

2. Henüz Visual Studio'yu Azure hesabınıza bağlamadıysanız **Hesap ekle...** öğesini seçin.

3. **Uygulama Hizmetini Oluştur** iletişim kutusunda, **Barındırma** ayarlarını resmin altındaki tabloda belirtildiği gibi kullanın:

    ![App Service iletişim kutusu oluşturma](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[App Service Planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Yeni bir plan oluşturmak için **Yeni**'ye tıkladıktan sonra **Boyut**'un altında **Tüketim**'i seçtiğinizden emin olun. Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir **Konum** seçin.  |
    | **[Depolama Hesabı](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** | Genel amaçlı depolama hesabı | İşlevler çalışma zamanı için bir Azure depolama hesabı gereklidir. **Yeni**'ye tıklayarak genel amaçlı bir depolama hesabı oluşturun veya mevcut hesabı kullanın.  |

4. **Oluştur**'a tıklayarak Azure'da bu ayarlarla bir işlev uygulaması ve ilgili kaynaklar oluşturun ve işlev proje kodunuzu dağıtın. 

5. Dağıtım tamamlandığında, **Site URL'si** değerini (Azure'daki işlev uygulamanızın adresi) not edin.

    ![Başarı iletisi yayımlama](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
