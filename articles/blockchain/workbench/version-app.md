---
title: Azure Blockchain Workbench blok zinciri uygulaması sürüm oluşturma
description: Uygulama sürümleri Azure Blockchain Workbench uygulamasında kullanma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 63f18e3ee316b9791bb62bfcd20c07a30cbebb5e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896886"
---
# <a name="azure-blockchain-workbench-application-versioning"></a>Azure Blockchain Workbench uygulama sürümü

Oluşturun ve bir Azure Blockchain Workbench uygulamasını birden çok sürümünü kullanın. Aynı uygulamanın birden çok sürümünü yüklediyseniz, sürüm geçmişi ve kullanıcılar, kullanmak istediğiniz sürümü seçebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Blockchain Workbench'i dağıtımı. Daha fazla bilgi için [Azure Blockchain Workbench dağıtım](deploy.md) dağıtımı hakkında ayrıntılı bilgi için
* Blockchain Workbench'i içinde bir dağıtılan blok zinciri uygulaması. Bkz: [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](create-app.md)

## <a name="add-an-app-version"></a>Bir uygulama sürüm ekleme

Yeni bir sürüme eklemek için yeni yapılandırma ve akıllı sözleşme dosyaları Blockchain Workbench'i yükleyin.

1. Bir web tarayıcısında Blockchain Workbench'i web adresine gidin. Örneğin, `https://{workbench URL}.azurewebsites.net/` , Blockchain Workbench'i web adresini bulmak hakkında daha fazla bilgi için bkz. [Blockchain Workbench'i Web URL'si](deploy.md#blockchain-workbench-web-url)
2. Olarak oturum bir [Blockchain Workbench'i yönetici](manage-users.md#manage-blockchain-workbench-administrators).
3. Başka bir sürümü ile güncelleştirmek istediğiniz blok zinciri uygulaması'nı seçin.
4. Seçin **Ekle sürüm**. **Ekle sürüm** bölmesi görüntülenir.
5. Yeni sürüm sözleşme yapılandırmayı seçin ve karşıya yüklemek için kod dosyaları sözleşme. Yapılandırma dosyasını otomatik olarak doğrulanır. Uygulamayı dağıtmadan önce tüm doğrulama hatalarını düzeltin.
6. Seçin **Ekle sürüm** yeni blok zinciri uygulaması sürümü eklemek için.

    ![Yeni bir sürüm ekleme](media/version-app/add-version.png)

Blok zinciri uygulaması dağıtımını birkaç dakika sürebilir. Dağıtım tamamlandığında, uygulama sayfayı yenileyin. Uygulama seçme ve seçerek **sürüm geçmişi** düğmesi, uygulamanın sürüm geçmişini görüntüler.

> [!IMPORTANT]
> Uygulamanın önceki sürümlerini devre dışı bırakıldı. Geçmiş sürümleri yeniden ayrı ayrı etkinleştirebilirsiniz.
>
> Uygulama rollerine yeni sürümde değişiklik yapıldıysa üyeler uygulama rollerine yeniden eklemeniz gerekebilir.

## <a name="using-app-versions"></a>Uygulama sürümü kullanma

Varsayılan olarak, uygulamanın son etkin sürümü Blockchain Workbench uygulamasında kullanılır. Uygulamanın önceki bir sürümünü kullanmak istiyorsanız, sürümü uygulama sayfasından önce seçmeniz gerekir.

1. Blockchain Workbench'i uygulama bölümünde, kullanmak istediğiniz sözleşme içeren uygulama onay kutusunu işaretleyin. Önceki sürümler etkinleştirilirse, sürüm geçmişi düğmesi kullanılabilir.
2. Seçin **sürüm geçmişi** düğmesi.
3. Sürüm Geçmişi bölmesinde, uygulama sürümü bağlantıyı seçerek *değiştirilme tarihi* sütun.

    ![Bir önceki sürümü seçin](media/version-app/use-version.png)

    Yeni sözleşmeler oluşturun veya önceki sürüm sözleşmelerinde eylemleri gerçekleştirin. Uygulama adı aşağıdaki uygulama sürümü görüntülenir ve eski sürümü hakkında bir uyarı görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain Workbench ile ilgili sorunları giderme](troubleshooting.md)
