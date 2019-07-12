---
title: Azure Önizleme Gözcü Symantec AWS verilere | Microsoft Docs
description: Azure Gözcü için Symantec AWS veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 214269bc5c854aa4d3bfd508b0adb5a53ec096df
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673979"
---
# <a name="connect-azure-sentinel-to-aws"></a>Azure Sentinel'ı için AWS'yi bağlama

AWS CloudTrail olaylarınızı tüm Azure Gözcü akış için AWS Bağlayıcısı'nı kullanın. Bu bağlantı işlemi, AWS kaynak günlüklerinizi AWS CloudTrail ve Azure Gözcü arasında bir güven ilişkisi oluşturmak için Azure Gözcü için erişim atar. Azure, AWS günlüklerine erişmek için Gözcü izin veren bir rol oluşturarak bu AWS üzerinde gerçekleştirilir.

## <a name="prerequisites"></a>Önkoşullar

Azure Gözcü çalışma alanı yazma iznine sahip olmalıdır.

## <a name="connect-aws"></a>AWS’ye bağlanma 
 
1.  Amazon Web Hizmetleri konsolunda, altında **güvenlik, kimlik ve Uyumluluk**, tıklayarak **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

2.  Seçin **rolleri** tıklatıp **rol oluşturma**.

    ![AWS2](./media/connect-aws/aws-2.png)

3.  Seçin **başka bir AWS hesabı.** İçinde **hesap kimliği** alanına **Microsoft hesabı kimliği** (**123412341234**) Gözcü Azure Portalı'ndaki AWS bağlayıcı sayfasında bulunabilir.

    ![AWS3](./media/connect-aws/aws-3.png)

4.  Emin **gerektiren bir dış kimlik** seçilir ve ardından ve dış AWS bağlayıcı sayfasında Gözcü Azure Portalı'nda bulunabilecek Kimliğini (çalışma alanı kimliği) girin.

    ![AWS4](./media/connect-aws/aws-4.png)

5.  Altında **izinleri ilkesi ekleme** seçin **AWSCloudTrailReadOnlyAccess**.

    ![AWS5](./media/connect-aws/aws-5.png)

6.  Bir etiketi (isteğe bağlı) girin.

    ![AWS6](./media/connect-aws/aws-6.png)

7.  Ardından, girin bir **rol adı** tıklatıp **rol oluşturma** düğmesi.

    ![AWS7](./media/connect-aws/aws-7.png)

8.  Rolleri listesinde, oluşturduğunuz rolü seçin.

    ![AWS8](./media/connect-aws/aws-8.png)

9.  Kopyalama **rol ARN** yapıştırın **ekleneceği rolü** Gözcü Azure Portalı'nda alan.

    ![AWS9](./media/connect-aws/aws-9.png)

10. İlgili şema AWS olaylarını Log Analytics'e kullanmak için arama **AWSCloudTrail**.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için AWS CloudTrail bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

