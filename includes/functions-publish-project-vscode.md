---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cfa36331f8f4ad45f3bf8ff32eee7d89c7d8852
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608408"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Visual Studio Code, işlevler projenizi doğrudan Azure’da dağıtmanıza olanak sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır.

Varsayılan olarak, Visual Studio tüm işlev uygulamanızı oluşturmak için gereken Azure kaynakları oluşturur. Bu kaynakların adlarını seçtiğiniz işlev uygulamasının adı temel alır. Oluşturulan kaynakların tam denetim gerekiyorsa, bunun yerine yapabilecekleriniz [Gelişmiş seçenekleri kullanarak yayımlama](../articles/azure-functions/functions-develop-vs-code.md#enabled-publishing-with-advanced-create-options).

Bu bölümde, Azure'da yeni bir işlev uygulaması oluşturduğunuzu varsayar.

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. Visual Studio Code'da komut paletini açın için F1 tuşuna basın. Arayın ve seçin komut Paleti'nde `Azure Functions: Deploy to function app...`.

1. Değil oturum açma, siz istenirse **Azure'da oturum aç**. Ayrıca **ücretsiz bir Azure hesabı oluşturun**. Tarayıcıdan sonra başarılı oturum açma, Visual Studio Code için geri dönün. 

1. Birden fazla aboneliğiniz varsa **bir abonelik seçin** seçin işlev uygulaması için **+ oluştur yeni işlev uygulamanızı Azure'a**.

1. İşlev uygulamanızı tanımlayan bir genelde benzersiz olan bir ad yazın ve Enter tuşuna basın. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir.

    Aşağıdaki Azure kaynakları, Enter tuşuna bastığınızda, aboneliğinizde oluşturulur:

    * **[Kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md)** : Tüm oluşturulan Azure kaynaklarını içerir. Ad, işlev uygulamanızın adı üzerinde temel alır.
    * **[Depolama hesabı](../articles/storage/common/storage-quickstart-create-account.md)** : İşlev uygulamanızın adı alan benzersiz bir ada sahip bir standart depolama hesabı oluşturulur.
    * **[Barındırma planı](../articles/azure-functions/functions-scale.md)** : Tüketim planı, Batı ABD bölgesinde, sunucusuz bir işlev uygulamanızı barındırmak için oluşturulur.
    * **İşlev uygulaması**: Projenizi dağıtılır ve bu yeni işlev uygulamasında çalıştırır.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Seçin **görünümü çıkış** oluşturduğunuz Azure kaynaklarını oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimi dahil olmak üzere.

1. Geri **Azure: İşlevleri** alanında, aboneliğiniz kapsamındaki yeni işlev uygulaması'nı genişletin. Genişletin **işlevleri**, sağ **HttpTrigger**ve ardından **işlev URL'sini kopyalama**.

    ![Yeni HTTP tetikleyici işlevi URL'sini Kopyala](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
