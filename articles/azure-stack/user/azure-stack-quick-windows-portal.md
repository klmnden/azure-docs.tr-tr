---
title: Azure yığın Hızlı Başlat - Windows sanal makine oluşturma
description: Azure yığın Hızlı Başlat - Portalı'nı kullanarak bir Windows VM oluşturma
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 5776fc472483018eb2c9e4f8962d0b1e8bce8081
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure yığın portal ile bir Windows server sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın Portalı'nı kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturmak için bu makaledeki adımları izleyin.

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure yığın portalında oturum açın

Azure yığın portalında oturum açın. Azure yığın portal adresi hangi Azure yığın ürün bağlandığınız bağlıdır:

* Azure yığın Geliştirme Seti (ASDK) için gidin: https://portal.local.azurestack.external.
* Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan URL'sine gidin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Tıklatın **yeni** > **işlem** > **Windows Server 2016 Datacenter Eval** > **oluşturma**. Görmüyorsanız, **Windows Server 2016 Datacenter Eval** giriş, Azure yığın operatörünüze başvurun. Bunlar markete açıklandığı gibi eklemenizi isteyin [Azure yığın Market Windows Server 2016 VM görüntüsü ekleme](../azure-stack-add-default-image.md) makalesi.

    ![Portal'da Windows sanal makine oluşturma adımları](media/azure-stack-quick-windows-portal/image01.png)
2. Altında **Temelleri**, bir **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir tek seçin bir **konumu**ve ardından **Tamam**.

    ![Temel ayarları yapılandırma](media/azure-stack-quick-windows-portal/image02.png)
3. Altında **bir boyutu seçin**, tıklatın **D1 standart** > **seçin**.
    ![Sanal makine boyutunu seçin](media/azure-stack-quick-windows-portal/image03.png)
4. Altında **ayarları**, Varsayılanları kabul edin ve tıklayın **Tamam**.
    ![Sanal makine ayarlarını yapılandırma](media/azure-stack-quick-windows-portal/image04.png)
5. Altında **Özet**, tıklatın **Tamam** sanal makine oluşturulamıyor.
    ![Özet görüntüleyin ve sanal makine oluşturun](media/azure-stack-quick-windows-portal/image05.png)
6. Yeni bir sanal makine görmek için tıklatın **tüm kaynakları**aramak için sanal makine adı ve ardından arama sonuçlarında adına tıklayın.
    ![Sanal makine bakın](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makine kullanarak tamamladığınızda, sanal makine ve kaynaklarının silin. Bunu yapmak için sanal makine sayfasında kaynak grubunu seçin ve **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, temel bir Windows Server sanal makine dağıtılabilir. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
