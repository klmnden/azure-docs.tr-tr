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
ms.openlocfilehash: 9b86f2e05e2cb42470061bd6398b4200607f2418
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012294"
---
1. Yeni uygulama yapılandırma deposu oluşturmak için oturum açın [Azure portalında](https://aka.ms/azconfig/portal). Sayfanın sol üst köşesinde bulunan seçin **+ kaynak Oluştur**. İçinde **markette Ara** kutusuna **uygulama yapılandırması** ve Enter tuşuna basın.

    ![Uygulama yapılandırması için arama](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Seçin **uygulama yapılandırması** seçin ve arama sonuçlarını **Oluştur**.

3. Üzerinde **uygulama yapılandırması** > **Oluştur** sayfasında, aşağıdaki ayarları girin.

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Adın başında veya sonunda `-` karakter ve ardışık `-` karakterler geçerli değildir.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırması test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir aboneliğiniz varsa, otomatik olarak seçilir ve **abonelik** açılan görüntülenmiyorsa. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu oluşturun veya seçin. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebilirsiniz birden fazla kaynak düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Orta ABD* | Kullanım **konumu** yapılandırma mağazaya barındırılır coğrafi konumu belirtmek için. En iyi performans için kaynak, uygulamanızın diğer bileşenlerle aynı bölgede oluşturun. |

    ![Bir uygulama yapılandırma deposu kaynağı oluşturun](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. **Oluştur**’u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra seçin **ayarları** > **erişim anahtarlarını**. Bir ya da birincil salt okunur veya birincil salt okunur anahtar bağlantı dizesini not edin. Bu bağlantı dizesi daha sonra oluşturduğunuz uygulama yapılandırma deposu ile iletişim kurmak için uygulamanızı yapılandırmak için kullanırsınız.
