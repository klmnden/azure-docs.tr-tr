---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: ec0425ff2188ecf1816d5f5841394c8e32f301d2
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188168"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Visual Studio Code, işlevler projenizi doğrudan Azure’da dağıtmanıza olanak sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır.

Bu makalede, yeni bir işlev uygulaması oluşturduğunuz varsayılır. 

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. İçinde **Azure: İşlevleri** alan, işlev uygulaması simgesine Dağıt'ı seçin.

    ![İşlev uygulaması ayarları](./media/functions-publish-project-vscode/function-app-publish-project.png)

1. Değil oturum açma, siz istenirse **Azure'da oturum aç**. Ayrıca **ücretsiz bir Azure hesabı oluşturun**. Tarayıcıdan sonra başarılı oturum açma, Visual Studio Code için geri dönün. 

1. Birden fazla aboneliğiniz varsa **bir abonelik seçin** seçin işlev uygulaması için **+ oluştur yeni işlev uygulamanızı Azure'a**.

1. İşlev uygulamanızı tanımlayan bir genelde benzersiz olan bir ad yazın ve Enter tuşuna basın. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir.

1. **+ Yeni Kaynak Grubu Oluştur**’u seçin, `myResourceGroup` gibi bir kaynak grubu adı yazın ve Enter tuşuna basın. Ayrıca var olan bir kaynak grubunu kullanabilirsiniz.

1. Seçin **+ oluştur yeni depolama hesabı**, yeni depolama hesabı genel olarak benzersiz bir ad, Enter tuşuna basın ve işlev uygulaması tarafından kullanılan tür. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Var olan bir hesabı da kullanabilirsiniz.

1. Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir konum seçin.

    Aşağıdaki Azure kaynakları, Enter tuşuna bastığınızda, aboneliğinizde oluşturulur:

    * **[Kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md)** : Tüm oluşturulan Azure kaynaklarını içerir. Ad, işlev uygulamanızın adı üzerinde temel alır.
    * **[Depolama hesabı](../articles/storage/common/storage-quickstart-create-account.md)** : İşlev uygulamanızın adı alan benzersiz bir ada sahip bir standart depolama hesabı oluşturulur.
    * **[Barındırma planı](../articles/azure-functions/functions-scale.md)** : Tüketim planı, Batı ABD bölgesinde, sunucusuz bir işlev uygulamanızı barındırmak için oluşturulur.
    * **İşlev uygulaması**: Projenizi dağıtılır ve bu yeni işlev uygulamasında çalıştırır.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Seçin **görünümü çıkış** oluşturduğunuz Azure kaynaklarını oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimi dahil olmak üzere.

1. Geri **Azure: İşlevleri** alanında, aboneliğiniz kapsamındaki yeni işlev uygulaması'nı genişletin. Genişletin **işlevleri**, sağ **HttpTrigger**ve ardından **işlev URL'sini kopyalama**.

    ![Yeni HTTP tetikleyici işlevi URL'sini Kopyala](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
