---
title: Azure Stack hızlı başlangıç - Windows sanal makine oluşturma
description: Azure Stack hızlı başlangıç - portal kullanarak bir Windows VM oluşturma
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: efe6213e5c0261fb26ac40e74c2b0f6e0c9252dd
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139635"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portal ile bir Windows server sanal makinesi oluşturma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturup, bu makaledeki adımları izleyin.

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalında oturum açın. Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* Azure Stack geliştirme Seti'ni (ASDK) için Git: https://portal.local.azurestack.external.
* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Tıklayın **yeni** > **işlem** > **Windows Server 2016 Datacenter değerlendirme** > **oluşturma**. Görmüyorsanız **Windows Server 2016 Datacenter değerlendirme** girişi, Azure Stack operatörü başvurun. Bunlar ve Market içinde anlatıldığı gibi eklemenizi isteyin [Windows Server 2016 VM görüntüsü eklemek için Azure Stack marketini](../azure-stack-add-default-image.md) makalesi.

    ![Portalında bir Windows sanal makinesi oluşturma adımları](media/azure-stack-quick-windows-portal/image01.png)
2. Altında **Temelleri**, tür a **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir adet seçin bir **konumu**ve ardından **Tamam**.

    ![Temel ayarları yapılandırma](media/azure-stack-quick-windows-portal/image02.png)
3. Altında **bir boyut seçin**, tıklayın **standart D1** > **seçin**.
    ![Sanal makine boyutunu seçin](media/azure-stack-quick-windows-portal/image03.png)
4. Altında **ayarları**, Varsayılanları kabul edin ve tıklayın **Tamam**.
    ![Sanal makine ayarlarını yapılandırma](media/azure-stack-quick-windows-portal/image04.png)
5. Altında **özeti**, tıklayın **Tamam** sanal makine oluşturmak için.
    ![Özet görüntüleyin ve sanal makine oluşturma](media/azure-stack-quick-windows-portal/image05.png)
6. Yeni sanal makinenize görmek için tıklayın **tüm kaynakları**, sanal makine adı için arama yapın ve ardından arama sonuçlarında adına tıklayın.
    ![Sanal makine bakın](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makine kullanarak tamamladığınızda, sanal makineyi ve kaynaklarını silin. Bunu yapmak için sanal makine sayfasında kaynak grubunu seçin ve **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Windows Server sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [sanal Makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
