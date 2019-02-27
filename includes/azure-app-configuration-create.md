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
ms.openlocfilehash: 71e63de247e05a4712687354ed548219b36e8f2f
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884865"
---
1. Yeni uygulama yapılandırma deposu oluşturmak için ilk kez oturum için [Azure portalında](https://aka.ms/azconfig/portal). Sayfanın sol üst kısmında **+ Kaynak oluştur**'a tıklayın. İçinde **markette Ara** metin kutusuna **uygulama yapılandırması** basın **Enter**.

    ![Uygulama yapılandırması için arama](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Tıklayın **uygulama yapılandırması** arama sonuçlarına ve ardından **Oluştur**.

3. İçinde **uygulama yapılandırması** > **Oluştur** sayfasında, aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Ad `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmaz.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırması test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa bu otomatik olarak seçilir ve **Abonelik** açılan penceresi görüntülenmez. |
    | **Kaynak Grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu oluşturun veya seçin. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden fazla kaynağı düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Orta ABD* | Kullanım **konumu** yapılandırma mağazaya barındırılır coğrafi konumu belirtmek için. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |

    ![Bir uygulama yapılandırma deposu kaynağı oluşturun](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. **Oluştur**’a tıklayın. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra tıklayın **ayarları** > **erişim anahtarlarını**. Her iki birincil salt okunur veya birincil salt okunur anahtar bağlantı dizesini not edin. Bu daha az önce oluşturduğunuz uygulama yapılandırma deposu ile iletişim kurmak için uygulamanızı yapılandırmak için kullanın.

6. Tıklayın **anahtar/değer Gezgini** ve **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Settings:BackgroundColor | Beyaz |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | Siyah |
    | TestApp:Settings:Message | Azure uygulama yapılandırma verileri |

    Size vermeyecek **etiket** ve **içerik türü** şimdilik boş.
