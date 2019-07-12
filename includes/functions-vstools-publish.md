---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/02/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1a8ecd0e53d28aed66546c41f95cc3855d1783a
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608407"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

2. **Azure İşlev Uygulaması**'nı, **Yeni Oluştur**'u ve sonra da **Yayımla**'yı seçin.

    ![Yayımlama hedefi seçme](./media/functions-vstools-publish/functions-visual-studio-publish-profile.png) 

    Tıkladığınızda **Çalıştır (önerilen) bir paket dosyasından**, işlev uygulamanızı kullanarak dağıtılacak [Zip dağıtma](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) ile [çalışma alanından paket](../articles/azure-functions/run-functions-from-deployment-package.md) modu etkin. Bu, çalışan işlevlerinizi önerilen yöntem ve daha iyi performansa neden.

    >[!CAUTION]
    >**Varolanı Seç**'i seçtiğinizde, yerel projedeki dosyalar Azure'da mevcut işlev uygulamasındaki tüm dosyaların üzerine yazılır. Bu seçeneği yalnızca mevcut işlev uygulamasına yeniden güncelleştirme yayımlarken kullanın.

3. Henüz Visual Studio'yu Azure hesabınıza bağlamadıysanız **Hesap ekle...** öğesini seçin.

4. **Uygulama Hizmetini Oluştur** iletişim kutusunda, **Barındırma** ayarlarını resmin altındaki tabloda belirtildiği gibi kullanın:

    ![App Service iletişim kutusu oluşturma](./media/functions-vstools-publish/functions-visual-studio-publish.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[App Service Planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Yeni bir sunucusuz plan oluşturmak için **Yeni**’ye tıkladıktan sonra **Boyut**’un altında **Tüketim**’i seçtiğinizden emin olun. Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir **Konum** seçin. **Tüketim** dışında bir planda çalıştırdığınızda, [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md) yönetmelisiniz.  |
    | **[Depolama Hesabı](../articles/storage/common/storage-quickstart-create-account.md)** | Genel amaçlı depolama hesabı | İşlevler çalışma zamanı için bir Azure depolama hesabı gereklidir. Tıklayın **yeni** genel amaçlı depolama hesabı oluşturmak için. Dilerseniz [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz.  |

5. **Oluştur**'a tıklayarak Azure'da bu ayarlarla bir işlev uygulaması ve ilgili kaynaklar oluşturun ve işlev proje kodunuzu dağıtın. 

6. Dağıtım tamamlandığında, **Site URL'si** değerini (Azure'daki işlev uygulamanızın adresi) not edin.

    ![Başarı iletisi yayımlama](./media/functions-vstools-publish/functions-visual-studio-publish-complete.png)
