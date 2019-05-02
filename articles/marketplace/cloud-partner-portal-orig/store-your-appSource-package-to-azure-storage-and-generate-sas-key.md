---
title: Azure depolama için AppSource paketinizi Store ve SAS anahtarıyla bir URL oluşturun
description: Karşıya yükleme ve AppSource paket güvenliğini sağlamak için gerekli adımlar ayrıntılı olarak açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pabutler
ms.openlocfilehash: ac77767aee2dcde33f4266e1d2d09c49dcf5f8a3
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943293"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Azure depolama için AppSource paketinizi Store ve SAS anahtarıyla bir URL oluşturun
=============================================================================

Dosyalarınızın güvenliğini sağlamak için tüm iş ortakları kendi AppSource paket dosyası, bir Azure blob depolama hesabında depolamak ve paylaşabilmesi için bir SAS anahtarı kullanmanız gerekir. Azure depolama konumunuz için sertifika ve AppSource deneme için kullanmak için şu paket dosyası alır.

Blob depolama alanına paketinizi karşıya yüklemek için aşağıdaki adımları kullanın:

1. Git <https://azure.microsoft.com> ve ücretsiz deneme sürümü ya da faturalandırılan hesabı oluşturun.

2. [Azure Portal](https://portal.azure.com/)’da oturum açın.

3. Tıklayarak yeni bir depolama hesabı oluşturma **+ yeni** ve **veri + depolama** hesabı.

   ![Veri + depolama dikey Microsoft Azure Portal](media/CRMScreenShot7.png)

4. Girin bir **adı** ve **kaynak grubu** tıklayın ve ad **Oluştur** düğmesi.

   ![Microsoft Azure Portal'da depolama hesabı oluşturma](media/CRMScreenShot8.png)

5. Yeni oluşturulan kaynak grubuna gidin ve yeni bir blob kapsayıcı oluşturun.

   ![Paket, Microsoft Azure portalı kullanarak blob karşıya yükleme](media/CRMScreenShot9.png)

6. Zaten yapmadıysanız, karşıdan yüklenip Microsoft [Azure Depolama Gezgini](https://storageexplorer.com/).

7. Depolama Gezgini'ni açın ve Azure depolama hesabınıza bağlanmak simge kullanın.

8. Oluşturduğunuz blob kapsayıcısına gelin ve tıklayın **karşıya** , paket zip dosyası eklemek için.

   ![Microsoft Depolama Gezgini'ni kullanarak paketini karşıya yükleyin](media/CRMScreenShot10.png)

9. Sağ tıklayın, dosya ve select **paylaşılan erişim imzası Al**.

   ![Bir Azure dosya paylaşılan erişim imzası Al](media/CRMScreenShot11.png)

10. Değiştirme **süre sonu** SAS bir ay için etkin hale getirmek için ardından **Oluştur**.

    ![Bir Azure dosya SAS sona erme tarihini değiştir](media/CRMScreenShot12.png)

11. URL alanını kopyalayın ve daha sonra kullanmak üzere kaydedin. İlişkili teklif oluşturduğunuzda, bu URL girmeniz gerekir. 

    ![Bir Azure dosya SAS URL'sini kopyalayın](media/CRMScreenShot13.png)

