---
title: include dosyası
description: include dosyası
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: c98a17be394887ef4e008b079467c85d4ded7e09
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393333"
---
1. Yeni bir uygulama yapılandırma deposu oluşturmak için oturum açın [Azure portalında](https://portal.azure.com). Bölmesinin sol üst köşesinde seçin **+ kaynak Oluştur**. İçinde **markette Ara** kutusuna **uygulama yapılandırması** ve Enter tuşuna basın.

    ![Uygulama yapılandırması için arama](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

1. Seçin **uygulama yapılandırması** seçin ve arama sonuçlarını **Oluştur**.

1. Üzerinde **uygulama yapılandırması** > **Oluştur** bölmesinde, aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma kaynağı saklamak için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Adın başında veya sonunda `-` karakter ve ardışık `-` karakterler geçerli değildir.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırması test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir aboneliğiniz varsa, otomatik olarak seçilir ve **abonelik** değil listesi görüntülenir. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu oluşturun veya seçin. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebilirsiniz birden fazla kaynak düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Orta ABD* | Kullanım **konumu** yapılandırma mağazaya barındırılır coğrafi konumu belirtmek için. En iyi performans için kaynak, uygulamanızın diğer bileşenlerle aynı bölgede oluşturun. |

    ![Bir uygulama yapılandırma deposu kaynağı oluşturun](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

1. **Oluştur**’u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra seçin **ayarları** > **erişim anahtarlarını**. Bir ya da birincil salt okunur veya birincil salt okunur anahtar bağlantı dizesini not edin. Oluşturduğunuz uygulama yapılandırma deposu ile iletişim kurmak için uygulamanızı yapılandırmak için bu bağlantı dizesi daha sonra kullanacaksınız.
