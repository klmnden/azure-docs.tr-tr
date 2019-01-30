---
title: Azure Stack hızlı başlangıç - Windows sanal makine oluşturma
description: Azure Stack hızlı başlangıç - portal kullanarak bir Windows VM oluşturma
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: unknown
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 1dd5cb9bbd8e197e7654242861e97684f4f378c8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241993"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portal ile bir Windows server sanal makinesi oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturup, bu makaledeki adımları izleyin.

> [!NOTE]  
> Bu makaledeki ekran görüntüleri, Azure Stack 1808 sürümü ile sunulan kullanıcı arabirimini eşleşecek şekilde güncelleştirilir. 1808 kullanma desteği ekler *yönetilen diskler* yönetilmeyen diskler yanı sıra. Önceki bir sürümünü kullanıyorsanız, disk seçimi gibi bazı görüntülerini bu makalede gösterilen değerinden farklı olacaktır.  


## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalında oturum açın. Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* Azure Stack geliştirme Seti'ni (ASDK) için Git: https://portal.local.azurestack.external.
* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Tıklayın **+ kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter –-,-kullandıkça**  >   **Oluşturma**. Görmüyorsanız **Windows Server 2016 Datacenter –-,-kullandıkça** girişi, Azure Stack operatörü başvurun. Bunlar ve Market içinde anlatıldığı gibi eklemenizi isteyin [Windows Server 2016 VM görüntüsü eklemek için Azure Stack marketini](../azure-stack-add-default-image.md) makalesi.

    ![Portalında bir Windows sanal makinesi oluşturma adımları](media/azure-stack-quick-windows-portal/image01.png)
2. Altında **Temelleri**, tür a **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir adet seçin bir **konumu**ve ardından **Tamam**.

    ![Temel ayarları yapılandırma](media/azure-stack-quick-windows-portal/image02.png)
3. Altında **boyutu** seçin **standart D1**ve ardından **seçin**.  
    ![Sanal makine boyutunu seçin](media/azure-stack-quick-windows-portal/image03.png)

4. Üzerinde **ayarları** sayfasında, varsayılan olarak istediğiniz değişiklikleri yapın.
   - Yapılandırabileceğiniz 1808 Azure Stack sürümünden başlayarak **depolama** seçebileceğiniz kullanılacak *yönetilen diskler*. 1808 sürümünden önce yalnızca yönetilmeyen diskler kullanılabilir.  
   ![Sanal makine ayarlarını yapılandırma](media/azure-stack-quick-windows-portal/image04.png)  
   Yapılandırmalarınızı hazır olduğunuzda seçin **Tamam** devam etmek için.

5. Altında **özeti**, tıklayın **Tamam** sanal makine oluşturmak için.
    ![Özet görüntüleyin ve sanal makine oluşturma](media/azure-stack-quick-windows-portal/image05.png)

6. Yeni sanal makinenize görmek için tıklayın **tüm kaynakları**, sanal makine adı için arama yapın ve ardından arama sonuçlarında adına tıklayın.
    ![Sanal makine bakın](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makine kullanarak tamamladığınızda, sanal makineyi ve kaynaklarını silin. Bunu yapmak için sanal makine sayfasında kaynak grubunu seçin ve **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Windows Server sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [sanal Makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
