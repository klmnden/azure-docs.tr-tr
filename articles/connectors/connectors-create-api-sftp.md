---
title: Logic apps içinde SFTP Bağlayıcısı'nı kullanmayı öğrenin | Microsoft Docs
description: Logic apps ile Azure uygulama hizmeti oluşturun. Dosya gönderme ve alma için SFTP API'sine bağlayın. Gibi oluştururken, güncelleştirme, almak veya dosyaları silme çeşitli işlemler gerçekleştirebilirsiniz.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 28ea02082903f71f619a52672ba41ce65557b0c7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296011"
---
# <a name="get-started-with-the-sftp-connector"></a>SFTP Bağlayıcısı ile çalışmaya başlama
Dosya gönderme ve alma için bir SFTP hesaba erişmek için SFTP Bağlayıcısı'nı kullanın. Gibi oluştururken, güncelleştirme, almak veya dosyaları silme çeşitli işlemler gerçekleştirebilirsiniz.  

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-sftp"></a>SFTP için Bağlan
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

### <a name="create-a-connection-to-sftp"></a>SFTP bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>SFTP tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Bu örnekte, **bir dosya eklendiğinde veya SFTP -** tetikleyici için bir dosya eklendiğinde mantığı uygulama iş akışını başlatmak için kullanılan veya bir SFTP sunucusunda, değiştirilemiyor. Ayrıca, yeni veya değiştirilmiş dosyasının içeriğini denetler ve içeriğini, içeriği kullanmadan önce ayıklanması gereken olduğunu gösteriyorsa dosyasını ayıklamak için bir karar verir bir koşul de ekleyin. Son olarak, bir dosyanın içeriğini ayıklamak için bir eylem ekleyin ve ayıklanan içeriğin SFTP sunucu üzerindeki bir klasöre yerleştirin. 

Bir kurumsal örnekte, bu tetikleyici müşteri siparişleri temsil eden yeni dosyalar için bir SFTP klasörü izlemek için kullanabilirsiniz.  Ardından bir SFTP bağlayıcı eylem gibi kullanabilir **alma dosya içeriğini**, siparişler veritabanında daha fazla işleme ve depolama sipariş içeriğini almak için.

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Koşul ekle
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>SFTP eylemini kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/sftpconnector/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).