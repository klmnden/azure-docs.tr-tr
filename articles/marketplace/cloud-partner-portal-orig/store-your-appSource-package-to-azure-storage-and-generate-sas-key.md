---
title: Azure depolama için AppSource paketinizi Store ve SAS anahtarıyla bir URL oluşturun | Microsoft Docs
description: Karşıya yükleme ve AppSource paket güvenliğini sağlamak için gerekli adımlar ayrıntılı olarak açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 335590cc616035020f66cfc5208e21d4c5128fe6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811703"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Azure depolama için AppSource paketinizi Store ve SAS anahtarıyla bir URL oluşturun
=============================================================================

Dosyalarınızın güvenliğini sağlamak için tüm iş ortakları kendi AppSource paket dosyası, bir Azure blob depolama hesabında depolamak ve paylaşabilmesi için bir SAS anahtarı kullanmanız gerekir. Azure depolama konumunuz için sertifika ve AppSource deneme için kullanmak için şu paket dosyası alır.

Blob depolama alanına paketinizi karşıya yüklemek için aşağıdaki adımları kullanın:

1.  Git <http://azure.microsoft.com> ve ücretsiz deneme sürümü ya da faturalandırılan hesabı oluşturun.

2.  [Azure Portal](http://portal.azure.com/)’da oturum açın.

3.  Tıklayarak yeni bir depolama hesabı oluşturma **+ yeni** ve **veri + depolama** hesabı.

  ![Veri + depolama dikey Microsoft Azure Portal](media/CRMScreenShot7.png)

4. Girin bir **adı** ve **kaynak grubu** tıklayın ve ad **Oluştur** düğmesi.

  ![Microsoft Azure Portal'da depolama hesabı oluşturma](media/CRMScreenShot8.png)

5. Yeni oluşturulan kaynak grubuna gidin ve yeni bir blob kapsayıcı oluşturun.

  ![Paket, Microsoft Azure portalı kullanarak blob karşıya yükleme](media/CRMScreenShot9.png)

6.  Zaten yapmadıysanız, karşıdan yüklenip Microsoft [Azure Depolama Gezgini](http://storageexplorer.com/).

7.  Depolama Gezgini'ni açın ve Azure depolama hesabınıza bağlanmak simge kullanın.

8.  Oluşturduğunuz blob kapsayıcısına gelin ve tıklayın **karşıya** , paket zip dosyası eklemek için.

  ![Microsoft Depolama Gezgini'ni kullanarak paketini karşıya yükleyin](media/CRMScreenShot10.png)

9.  Sağ tıklayın, dosya ve select **paylaşılan erişim imzası Al**.

  ![Bir Azure dosya paylaşılan erişim imzası Al](media/CRMScreenShot11.png)

10.  Değiştirme **süre sonu** SAS bir ay için etkin hale getirmek için ardından **Oluştur**.

  ![Bir Azure dosya SAS sona erme tarihini değiştir](media/CRMScreenShot12.png)

11.  URL alanını kopyalayın ve daha sonra kullanmak üzere kaydedin. İlişkili teklif oluşturduğunuzda, bu URL girmeniz gerekir. 

  ![Bir Azure dosya SAS URL'sini kopyalayın](media/CRMScreenShot13.png)

