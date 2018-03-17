---
title: "Azure'dan Azure yığın Market öğe ekleme | Microsoft Docs"
description: "Azure yığın Market'te Azure tabanlı Windows Server sanal makine görüntüyü eklemeyi açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 25f179f825526e20377c0e94ccb38a788cadd898
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>Öğretici: Azure yığın Market öğesi Azure'dan ekleyin.

Bir Azure yığın operatör olarak, kullanıcıların bir sanal makine (VM) dağıtmanıza olanak sağlamak için yapmanız gereken ilk şey bir VM görüntüsü Azure yığın Market eklemektir. Varsayılan olarak, hiçbir şey Azure yığın Market yayımlanır, ancak öğelerinden indirebilirsiniz [seçkin Azure Market öğelerinin listesini](.\.\azure-stack-marketplace-azure-items.md) , Azure yığında çalıştırmak için önceden sınanmış. Bağlı bir senaryoda işletim ve Azure yığın örneğinizi Azure ile kaydettiğiniz bu seçeneği kullanın.

Bu öğreticide, Windows Server 2016 VM görüntüsü Azure yığın Market'te Azure Marketi'nden ekleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Windows Server VM Azure yığın Market öğesi ekleme
> * VM görüntüsü kullanılabilir olduğunu doğrulayın 
> * Test VM görüntüsü

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](asdk-post-deploy.md#install-azure-stack-powershell)
- Karşıdan [Azure yığın araçları](asdk-post-deploy.md#download-the-azure-stack-tools)
- [ASDK kaydetmek](asdk-register.md) Azure aboneliğinizle

## <a name="add-a-windows-server-vm-image"></a>Windows Server VM görüntüsü ekleme
Azure Marketi'nden görüntü yükleyerek Azure yığın markete bir Windows Server 2016 görüntüsü ekleyebilirsiniz. Bağlı bir senaryoda işletim ve zaten varsa bu seçeneği kullanın [Azure yığın örneğinizi kayıtlı](asdk-register.md) Azure ile.

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim** > **azure'dan Ekle**. 

   ![Azure'dan Ekle](media/asdk-marketplace-item/azs-marketplace.png)

3. Bulma veya arama **Windows Server 2016 Datacenter** görüntü.

4. Seçin **Windows Server 2016 Datacenter** görüntü ve ardından **karşıdan**.

   ![Görüntü Azure'dan indirin](media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> VM görüntüsünü karşıdan yüklemek ve Azure yığın Market yayımlamak için yaklaşık bir saat sürer. 

Yükleme tamamlandığında, görüntünün yayımlanan ve altında kullanılabilir olacaktır **Market Yönetim**. Görüntü ayrıca altında kullanılabilir **işlem** yeni sanal makineler oluşturmak için.

## <a name="verify-the-marketplace-item-is-available"></a>Market öğesi kullanılabilir olduğunu doğrulayın
Yeni VM görüntüsü Azure yığın marketi'ndeki kullanılabilir olduğunu doğrulamak için aşağıdaki adımları kullanın.

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim**. 

3. Windows Server 2016 Datacenter VM görüntüsü başarıyla eklendiğini doğrulayın.

   ![Azure indirilen görüntüden](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Test VM görüntüsü
Bir Azure yığın operatör olarak kullanabileceğiniz [Yönetici portalı](https://adminportal.local.azurestack.external) bir test oluşturmak için görüntüyü doğrulamak için VM başarılı bir şekilde kullanıma sunulmuştur. 

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).

2. Tıklatın **yeni** > **işlem** > **Windows Server 2016 Datacenter** > **oluşturma**.  
 ![Market görüntüsünden VM oluşturma](media/asdk-marketplace-item/new-compute.png)

3. İçinde **Temelleri** dikey penceresinde, aşağıdaki bilgileri girin ve ardından **Tamam**:
  - **Ad**: test vm 1
  - **Kullanıcı adı**: AdminTestUser
  - **Parola**: AzS TestVM01
  - **Abonelik**: varsayılan sağlayıcı abonelik kabul et
  - **Resource group**: test-vm-rg
  - **Konum**: yerel

4. İçinde **bir boyutu seçin** dikey penceresinde tıklatın **A1 standart**ve ardından **seçin**.  

5. İçinde **ayarları** dikey penceresinde, Varsayılanları kabul edin ve tıklatın **Tamam**

6. Sanal makineyi oluşturmak için **Özet** dikey penceresinde **Tamam** seçeneğine tıklayın.  
> [!NOTE]
> Sanal makine dağıtımı tamamlanması birkaç dakika sürer.

7. Yeni VM görüntülemek için tıklatın **sanal makineleri**, ardından arama **test vm 1** ve onun adına tıklayın.
    ![Görüntülenen ilk test VM görüntüsü](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Başarıyla VM'ye açtığınız ve test görüntüsü düzgün çalıştığını doğruladıktan sonra test kaynak grubunu silmeniz gerekir. Bu sınırlı kaynakları tek bir düğüm ASDK yüklemeleri için kullanılabilir boş.

Artık gerekli olduğunda, aşağıdaki adımları izleyerek kaynak grubu, VM ve tüm ilgili kaynaklar Sil:

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).
2. Tıklatın **kaynak grupları** > **test vm rg** > **kaynak grubu Sil**.
3. Tür **test vm rg** 'ye tıklayın ve kaynak grubu adı olarak **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir Windows Server VM Azure yığın Market öğesi ekleme
> * VM görüntüsü kullanılabilir olduğunu doğrulayın 
> * Test VM görüntüsü

Bir Azure yığın teklif ve planı nasıl oluşturulacağını öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure yığın Iaas hizmetleri sunar](asdk-offer-services.md)