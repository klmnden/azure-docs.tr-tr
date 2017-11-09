---
title: Klasik Azure VM oturumunu | Microsoft Docs
description: "Klasik dağıtım modeli kullanılarak oluşturulmuş bir Windows sanal makine oturum açmak için Azure portalını kullanın."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 510b394256bbe86a5eb5bfbc3af4681670b89de3
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Azure Portal kullanarak bir Windows sanal makinesinde oturum açma
Azure portalında kullandığınız **Bağlan** bir Uzak Masaüstü oturumu ve bir Windows VM oturumunu Başlat düğmesi.

Bir Linux VM bağlanmak istiyor? Bkz: [Linux çalıştıran bir sanal makine için oturum açma](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak bir VM'de oturum açma hakkında daha fazla bilgi için bkz: [burada](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
1. Azure Portal’da oturum açın.
2. Erişmek istediğiniz sanal makineye tıklayın. Ad içinde listelenen **tüm kaynakları** bölmesi.

    ![Sanal makine konumları](./media/connect-logon/azureportaldashboard.png)

3. Tıklatın **Bağlan** sanal makine Pano üzerinde komut çubuğunda.

    ![Sanal makine için simge Bağlan](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## <a name="log-on-to-the-virtual-machine"></a>Sanal makinede oturum açma
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Varsa **Bağlan** düğmesi etkin değil veya Uzak Masaüstü Bağlantısı ile ilgili başka sorununuz, yapılandırmayı sıfırlamayı deneyin. tıklatın **sıfırlama uzaktan erişim** sanal makine panosundan.

    ![Sıfırlama uzaktan erişim](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Parolanızı sorunlar için onu sıfırlamayı deneyin. Tıklatın **parola sıfırlama** sol kenarına sanal makine panoyu altında **destek + sorun giderme**.

    ![Parola sıfırlama](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Bu ipuçları çalışmıyor veya ne bkz ihtiyacınız olmayan [sorun giderme Uzak Masaüstü bağlantıları için Windows tabanlı Azure sanal makine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu makale ortak sorunları tanılama ve çözme boyunca size yol gösterecektir.
