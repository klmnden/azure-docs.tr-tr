---
title: Azure Stack'te plan oluşturma | Microsoft Docs
description: Bulut Yöneticisi olarak, abonelerin sanal makine sağlamasına olanak tanıyan bir plan oluşturun.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: a2ac138228b7b54f486acb0f42975748136a8da7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759478"
---
# <a name="create-a-plan-in-azure-stack"></a>Azure Stack'te plan oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Planları](azure-stack-key-features.md) bir veya daha fazla hizmet ve kotalarını gruplandırmaları. Bir sağlayıcı, kullanıcılarınıza sunabileceğiniz planlar oluşturabilirsiniz. Buna karşılık, kullanıcılarınızın planları, hizmetleri ve kotalar içerirler kullanılacak Teklifleriniz için abone olun. Bu örnekte işlem, ağ ve depolama kaynağı sağlayıcılarını içeren bir plan oluşturma işlemini gösterir. Bu plan aboneleri sanal makineler sağlama olanağı sağlar.

## <a name="create-a-plan-1902-and-later"></a>(1902 ve üzeri) bir plan oluşturun

1. Oturum [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external).

2. Bir plan ve kullanıcıların abone olabileceği teklifi oluşturmak için seçin **+ kaynak Oluştur**, ardından **sunar + planlar**, ardından **planı**.
  
   ![Bir plan seçin](media/azure-stack-create-plan/select-plan.png)

3. Plan adı belirtin, hizmetlerini ekleyin ve kotalar Seçili hizmetlerin her biri için tanımlamanızı sağlayan bir sekmeli kullanıcı arabirimi görüntülenir. En önemlisi de oluşturmak karar vermeden önce oluşturduğunuz teklif ayrıntılarını gözden geçirebilirsiniz.

   Altında **Temelleri** sekmesinde **yeni plan** penceresinde girin bir **görünen ad** ve **kaynak adı**. Görünen ad işleçleri görebilirsiniz planın kolay adıdır. Yönetici portalı'nda, plan ayrıntılarını yalnızca işleçler için görünür olduğunu unutmayın.

   ![Ayrıntılarını belirtin](media/azure-stack-create-plan/plan-name.png)

4. Yeni bir **kaynak grubu**, veya varolan bir plan için kapsayıcı olarak seçin.

   ![Kaynak grubunu belirtin](media/azure-stack-create-plan/resource-group.png)

5. Seçin **Hizmetleri** sekmesine tıklayın ve ardından onay kutusunu **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage** .
  
   ![Hizmetleri seçin](media/azure-stack-create-plan/services.png)

6. Seçin **kotalar** sekmesi. Yanındaki **Microsoft.Storage**açılan kutusundan varsayılan kota seçin veya seçin **Yeni Oluştur** özelleştirilmiş bir kota oluşturmak için.
  
   ![Kotalar](media/azure-stack-create-plan/quotas.png)

7. Yeni kota oluşturuyorsanız girin bir **adı** kotası için kota değerlerini belirtin. Seçin **Tamam** kota oluşturmak için.

   ![Yeni kota](media/azure-stack-create-plan/new-quota.png)

8. 6 ve 7 oluşturmak ve atamak için kotalar için adımları yineleyin **Microsoft.Network** ve **Microsoft.Compute**. Tüm üç hizmeti atanan kota olduğunda, sonraki örnekte olduğu gibi göz atacağız.

   ![Tam kota atamaları](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Seçin **gözden + Oluştur** planın incelenmesi için. Tüm değerleri ve doğru olduklarından emin olmak için kotaları gözden geçirin. Her hizmet/kota çifti solundaki genişletme oklar unutmayın. Yeni bir özellik seçili planlara, gerekli tüm düzenlemeleri yapın dönün ve bir plandaki her kota ayrıntılarını görüntülemek için teker teker kotaları genişletmenize olanak sağlar.

   ![Plan oluşturma](media/azure-stack-create-plan/create.png)

10. Hazır olduğunuzda seçin **Oluştur** planı oluşturun.

11. Yeni plan görmek için seçin **planları**adını seçin ve ardından planlama arayın. Kaynakları listesini uzunsa kullanın **arama** adıyla planınızı bulunacak.

## <a name="create-a-plan-1901-and-earlier"></a>(1901 ve öncesi) bir plan oluşturun

1. Oturum [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external).

2. Bir plan ve kullanıcıların abone olabileceği teklifi oluşturmak için seçin **+ kaynak Oluştur**, ardından **sunar + planlar**, ardından **planı**.
  
   ![Bir plan seçin](media/azure-stack-create-plan/select-plan1901.png)

3. Altında **yeni plan**, girin bir **görünen ad** ve **kaynak adı**. Görünen ad kullanıcılar görebilir planın kolay adıdır. Yalnızca yönetici Azure Resource Manager kaynağı olarak planla çalışmak için hangi yöneticileri kullanın kaynak adını görebilirsiniz.

   ![Ayrıntılarını belirtin](media/azure-stack-create-plan/plan-name1901.png)

4. Yeni bir **kaynak grubu**, veya varolan bir plan için kapsayıcı olarak seçin.

   ![Kaynak grubunu belirtin](media/azure-stack-create-plan/resource-group1901.png)

5. Seçin **Hizmetleri** ve onay kutusunu seçip **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**. Ardından, **seçin** yapılandırmayı kaydetmek için. Onay kutularını her seçenek üzerine fare geldiğinde görüntülenir.
  
   ![Hizmetleri seçin](media/azure-stack-create-plan/services1901.png)

6. Seçin **kotalar**, **Microsoft.Storage (yerel)**, seçin veya varsayılan kota seçin **yeni kota oluştur** özelleştirilmiş bir kota oluşturmak için.
  
   ![Kotalar](media/azure-stack-create-plan/quotas1901.png)

7. Yeni kota oluşturuyorsanız girin bir **adı** kotası için > kota değerlerini belirtin > seçin **Tamam**. **Kota oluştur** iletişim kutusunu kapatır.

   ![Yeni kota](media/azure-stack-create-plan/new-quota1901.png)

   Ardından, oluşturduğunuz yeni kota de seçin. Kota seçerek atar ve seçimi iletişim kutusunu kapatır.
  
   ![Kota atayın](media/azure-stack-create-plan/assign-quota1901.png)

8. 6 ve 7 oluşturmak ve atamak için kotalar için adımları yineleyin **Microsoft.Network (yerel)** ve **Microsoft.Compute (yerel)**. Tüm üç hizmeti atanan kota olduğunda, sonraki örnekte olduğu gibi göz atacağız.

   ![Tam kota atamaları](media/azure-stack-create-plan/all-quotas-assigned1901.png)

9. Altında **kotalar**, seçin **Tamam**ve ardından altındaki **yeni plan**, seçin **Oluştur** planı oluşturun.

    ![Plan oluşturma](media/azure-stack-create-plan/create1901.png)

10. Yeni planınızı görmek için seçin **tüm kaynakları**adını seçin ve ardından planlama arayın. Kaynakları listesini uzunsa kullanın **arama** adıyla planınızı bulunacak.

    ![Planını gözden geçirin](media/azure-stack-create-plan/plan-overview1901.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Teklif oluşturma](azure-stack-create-offer.md)
