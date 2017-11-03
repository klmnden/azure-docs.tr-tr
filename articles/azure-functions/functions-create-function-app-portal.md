---
title: "Azure Portalı'ndan bir işlev uygulaması oluşturma | Microsoft Docs"
description: "Yeni bir işlev uygulaması portaldan Azure App Service'te oluştur."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 3e4eef12c1d19be6e0f1051caaa5cf2e98626aef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Azure portalından bir işlev uygulaması oluşturma

Azure işlev uygulamalarının Azure App Service altyapısı kullanır. Bu konu Azure portalında bir işlev uygulaması oluşturulacağını gösterir. Bir işlev uygulaması tekil işlevler yürütülmesi barındıran kapsayıcıdır. Barındırma planı App Service'te bir işlev uygulaması oluşturduğunuzda işlevi uygulamanızı uygulama hizmetin tüm özelliklerini kullanabilirsiniz.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Bir işlev uygulaması oluşturduğunuzda, geçerli bir tedarik **uygulama adı**, hangi içerebilir yalnızca harf, rakam ve kısa çizgi. Alt çizgi (**_**) izin verilen bir karakter değildir.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. 

İşlev uygulaması oluşturulduktan sonra bir veya daha fazla farklı dillerde tekil işlevler oluşturabilirsiniz. İşlevler Oluştur [portalını kullanarak](functions-create-first-azure-function.md#create-function), [sürekli dağıtım](functions-continuous-deployment.md), ya da [FTP ile karşıya](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Hizmet planları

Azure işlevleri sahip iki farklı hizmet planları: Tüketim planlama ve uygulama hizmeti planı. Kodunuz çalışan, Ölçek genişletme yükü işlemek gereken ve ardından ölçekler-kod çalışmadığı zaman zaman tüketimi planı otomatik olarak işlem gücü ayırır. Uygulama hizmeti planı uygulama hizmeti tüm özellikleri, işlev uygulama erişim sağlar. İşlev uygulamanızı oluşturulur ve şu anda değiştirilemez, hizmeti planınızı seçmeniz gerekir. Daha fazla bilgi için bkz: [barındırma planı bir Azure işlevleri arasından seçim](functions-scale.md).

Bir uygulama hizmeti planı JavaScript işlevleri çalıştırmayı planlıyorsanız, daha az çekirdek planla seçmeniz gerekir. Daha fazla bilgi için bkz: [işlevleri için JavaScript başvurusu](functions-reference-node.md#choose-single-core-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması App Service'te oluştururken oluşturmanız veya Blob, kuyruk ve tablo depolama destekleyen genel amaçlı bir Azure depolama hesabına bağlamak gerekir. Dahili olarak, işlevlerini kullanan depolama Tetikleyicileri yönetme ve işlev yürütmeleri günlüğü gibi işlemler için. Bazı depolama hesapları, kuyruklar ve tablolar, yalnızca blob storage hesapları, Azure Premium Storage ve ZRS çoğaltma genel amaçlı depolama hesaplarıyla gibi desteklemez. Bu hesapları işyeri dışında depolama hesabı dikey penceresinden bir işlev uygulaması oluştururken filtrelenir.

>[!NOTE]
>Barındırma planı tüketim kullanırken işlevi kod ve bağlama yapılandırma dosyaları Azure File storage ana depolama hesabında depolanır. Ana depolama hesabı sildiğinizde, bu içeriği silinir ve kurtarılamaz.

Depolama hesabı türleri hakkında daha fazla bilgi için bkz: [Azure Storage hizmetlerine giriş](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



