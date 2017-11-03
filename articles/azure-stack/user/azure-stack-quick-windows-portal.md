---
title: "Azure yığın Hızlı Başlat - Windows sanal makine oluşturma"
description: "Azure yığın Hızlı Başlat - Portalı'nı kullanarak bir Windows VM oluşturma"
services: azure-stack
author: ErikjeMS
manager: byronr
ms.service: azure-stack
ms.topic: azure-stack
ms.date: 09/15/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: abca538f28bbc0a8f3f00311ca1a69d196f10272
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>Azure yığın portal ile bir Windows sanal makine oluşturma

Azure yığın Portalı'nı kullanarak bir Windows sanal makine oluşturabilirsiniz. Oluşturduğunuz, yapılandırmak ve kaynakları yönetmek için bir tarayıcı tabanlı kullanıcı arabirimi portalıdır.

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure yığın portalında oturum açın

Azure yığın portalında oturum açın. Azure yığın portal adresi hangi Azure yığın ürün bağlandığınız bağlıdır:

* Azure yığın Geliştirme Seti (ASDK) için gidin: https://portal.local.azurestack.external.
* Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan URL'sine gidin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Tıklatın **yeni** > **işlem** > **Windows Server 2016 Datacenter Eval** > **oluşturma**. Görmüyorsanız, **Windows Server 2016 Datacenter Eval** giriş, Azure yığın operatörünüze başvurun. Bunlar markete açıklandığı gibi eklemenizi isteyin [Azure yığın Market Windows Server 2016 VM görüntüsü ekleme](../azure-stack-add-default-image.md) makalesi. 
    ![](media/azure-stack-quick-windows-portal/image01.png)
2. Altında **Temelleri**, bir **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir tek seçin bir **konumu**ve ardından **Tamam**.

    ![](media/azure-stack-quick-windows-portal/image02.png)
3. Altında **bir boyutu seçin**, tıklatın **D1 standart** > **seçin**.
    ![](media/azure-stack-quick-windows-portal/image03.png)
4. Altında **ayarları**, Varsayılanları kabul edin ve tıklayın **Tamam**.
    ![](media/azure-stack-quick-windows-portal/image04.png)
5. Altında **Özet**, tıklatın **Tamam** sanal makine oluşturulamıyor. 
    ![](media/azure-stack-quick-windows-portal/image05.png)
6. Yeni bir sanal makine görmek için tıklatın **tüm kaynakları**, sanal makine için arama yapın ve adına tıklayın.
    ![](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makine artık ihtiyacınız olduğunda, kaynak grubu, sanal makine ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine sayfasından kaynak grubunu seçin ve **silmek**.

## <a name="next-steps"></a>Sonraki adımlar
Bu Hızlı Başlangıç, basit bir Windows sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
