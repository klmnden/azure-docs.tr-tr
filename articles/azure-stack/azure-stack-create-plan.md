---
title: "Azure yığınında bir plan oluşturun | Microsoft Docs"
description: "Bulut yönetici olarak aboneleri sanal makine sağlamak olanak sağlayan bir plan oluşturun."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 5eefca3541ae9f73514f80b0f8df9e5027600f87
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Azure Stack'te plan oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Planlar](azure-stack-key-features.md), bir veya birden fazla hizmetten oluşan gruplardır. Bir sağlayıcısı olarak, kullanıcılarınıza sunmak için planları oluşturabilirsiniz. Buna karşılık, kullanıcılarınızın planları ve içerirler Hizmetleri'ni kullanmak için Teklifleriniz için abone olun. Bu örnekte işlem, ağ ve depolama kaynak sağlayıcıları içeren bir planı oluşturulacağını gösterir. Bu plan aboneleri sanal makineler sağlamak için olanak sağlar.

1. Azure yığın Yönetici portalı'na (https://adminportal.local.azurestack.external) oturum açın. 5. adımından sırasında oluşturulan hesabının kimlik bilgilerini girin [PowerShell betiğini çalıştırmak](azure-stack-run-powershell-script.md) bölümü.

2. Bir planı ve kullanıcıların abone olabilirsiniz teklifi oluşturmak için tıklatın **yeni** > **Kiracı sunar + planları** > **planı**.

   ![](media/azure-stack-create-plan/image01.png)
3. İçinde **yeni Plan** dikey penceresinde, doldurun **görünen adı** ve **kaynak adı**. Görünen ad kullanıcıların gördüğü planın kolay addır. Kaynak Adını yalnızca yönetici görebilir. Bu yöneticileri plan bir Azure Resource Manager kaynak olarak çalışmak için kullandığınız adıdır.

   ![](media/azure-stack-create-plan/image02.png)
4. Yeni bir **kaynak grubu**, veya plan için bir kapsayıcı olarak varolan bir tanesini seçin.

   ![](media/azure-stack-create-plan/image02a.png)
5. Tıklatın **Hizmetleri**seçin **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**ve ardından **Seçin**.

   ![](media/azure-stack-create-plan/image03.png)
6. Tıklatın **kotaları**, tıklatın **Microsoft.Storage (yerel)**ve ardından varsayılan kota seçin veya tıklatın **yeni kota oluştur** kota özelleştirmek için.

   ![](media/azure-stack-create-plan/image04.png)
7. Yeni kota oluşturuyorsanız, kota için bir ad girin > kota değerlerini ayarlamak > tıklatın **Tamam** > Yeni kota adına tıklayın.

   ![](media/azure-stack-create-plan/image06.png)
8. Tıklatın **Microsoft.Network (yerel)**ve ardından varsayılan kota seçin veya tıklatın **yeni kota oluştur** kota özelleştirmek için.

    ![](media/azure-stack-create-plan/image07.png)
9. Yeni kota oluşturuyorsanız, kota için bir ad yazın > kota değerlerini ayarlamak > tıklatın **Tamam** > Yeni kota adına tıklayın.

    ![](media/azure-stack-create-plan/image08.png)
10. Tıklatın **Microsoft.Compute (yerel)**ve ardından varsayılan kota seçin veya tıklatın **yeni kota oluştur** kota özelleştirmek için.

    ![](media/azure-stack-create-plan/image09.png)
11. Yeni kota oluşturuyorsanız, kota için bir ad yazın > kota değerlerini ayarlamak > tıklatın **Tamam** > Yeni kota adına tıklayın.

    ![](media/azure-stack-create-plan/image10.png)
12. İçinde **kotaları** dikey penceresinde tıklatın **Tamam**ve ardından **yeni Plan** dikey penceresinde tıklatın **Oluştur** planı oluşturmak için.

    ![](media/azure-stack-create-plan/image11.png)
13. Yeni planınız görmek için tıklatın **tüm kaynakları**, plan için arama yapın ve adına tıklayın.

    ![](media/azure-stack-create-plan/image12.png)

### <a name="next-steps"></a>Sonraki adımlar
[Teklif oluşturma](azure-stack-create-offer.md)
