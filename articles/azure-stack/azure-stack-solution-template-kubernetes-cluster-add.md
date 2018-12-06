---
title: Kubernetes için Azure Stack Marketini ekleyin | Microsoft Docs
description: Kubernetes için Azure Stack Marketini eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 0cac5658d5f6f32795b5988008b3b895024ecc06
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960543"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Kubernetes için Azure Stack Marketini Ekle

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu.

Kullanıcılarınız için bir Market öğesi Kubernetes sunabilir. Kullanıcılarınızın Kubernetes tek ve eşgüdümlü bir işlemle dağıtabilir.

Şu makaleye bakın dağıtmak ve tek başına bir Kubernetes kümesi için kaynakları sağlamak için bir Azure Resource Manager şablonu kullanarak. Azure Stack sürüm 1808 0.3.0 Kubernetes küme Market öğesi gerektirir. Başlamadan önce Azure Stack ve Azure genel Kiracı ayarlarını kontrol edin. Azure Stack hakkında gerekli bilgileri toplayın. Gerekli kaynakları kiracınız ve Azure Stack Marketini ekleyin. Bir Ubuntu sunucusu, özel komut dosyası ve Kubernetes öğeleri Market'te olması küme bağlıdır.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Bir plan, teklif ve bir abonelik oluşturun

Bir plan, teklif ve Kubernetes Market öğesi için bir abonelik oluşturun. Ayrıca, var olan bir planı kullanın ve sunar.

1. Oturum [Yönetim Portalı.](https://adminportal.local.azurestack.external)

1. Temel plan bir plan oluşturun. Yönergeler için [Azure Stack'te plan oluşturma](azure-stack-create-plan.md).

1. Bir teklif oluşturun. Yönergeler için [Azure Stack'te teklif oluşturma](azure-stack-create-offer.md).

1. Seçin **sunar**ve oluşturduğunuz teklif bulun.

1. Seçin **genel bakış** teklif dikey penceresinde.

1. Seçin **durumunu değiştir**. Seçin **genel**.

1. Seçin **+ kaynak Oluştur** > **sunar ve planları** > **abonelik** yeni bir abonelik oluşturmak için.

    a. Girin bir **görünen ad**.

    b. Girin bir **kullanıcı**. Kiracınız ile ilişkili Azure AD hesabını kullanın.

    c. **Sağlayıcısı açıklaması**

    d. Ayarlama **dizin Kiracı** Azure AD kiracınız için Azure Stack için. 

    e. Seçin **teklif**. Oluşturduğunuz teklif adını seçin. Abonelik kimliği not edin

## <a name="add-an-ubuntu-server-image"></a>Ubuntu server resim ekleme

Ubuntu Server aşağıda Market'te ekleyin:

1. Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri**ve ardından altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `UbuntuServer` yazın.

1. Sunucu en yeni sürümünü seçin. Tam sürümünü denetleyin ve en yeni sürümüne sahip olduğunuzdan emin olun:
    - **Yayımcı**: Canonical
    - **Teklif**: UbuntuServer
    - **Sürüm**: 16.04.201806120
    - **SKU**: 16.04 LTS

1. Seçin **indirin.**

## <a name="add-a-custom-script-for-linux"></a>Linux için özel bir komut dosyası Ekle

Kubernetes marketten ekleyin:

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Custom Script for Linux` yazın.

1. Aşağıdaki profil komut dosyasını seçin:
    - **Teklif**: Linux 2.0 için özel betik
    - **Sürüm**: 2.0.6
    - **Yayımcı**: Microsoft Corp

    > [!Note]  
    > Linux için özel betik birden fazla sürümünü listelenebilir. Eşleşen sürümünü eklemeniz gerekir. Kubernetes öğesi'nün tam sürümünü gerektirir.

1. Seçin **indirin.**


## <a name="add-kubernetes-to-the-marketplace"></a>Kubernetes Market'te Ekle

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Kubernetes` yazın.

1. `Kubernetes Cluster` öğesini seçin.

1. Seçin **indirin.**

    > [!note]  
    > Bu Market öğesi Market'te görünmesi için beş dakika sürebilir.

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Güncelleştirme veya Kubernetes kaldırma 

Kubernetes öğesi güncelleştirilirken Market'te öğeyi kaldırmak gerekir. Ardından, Kubernetes Market'te eklemek için bu makaledeki yönerge takip edebilirsiniz.

Kubernetes öğeyi kaldırmak için:

1. PowerShell ile Azure Stack operatör bağlanın. Yönergeler için bkz [Azure Stack operatör olarak PowerShell ile bağlanma](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Galerideki geçerli Kubernetes kümesi öğeyi bulur.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Geçerli öğenin adını gibi unutmayın `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Öğeyi kaldırmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir Kubernetes için Azure Stack dağıtma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Azure stack'teki hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)
