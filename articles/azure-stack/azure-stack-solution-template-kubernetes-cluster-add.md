---
title: Bir Kubernetes kümesi için Azure Stack Marketini ekleyin | Microsoft Docs
description: Bir Kubernetes kümesi için Azure Stack Marketini eklemeyi öğrenin.
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
ms.date: 09/12/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: ded2aa17fe9b8de2d8c8f662f5d99b1ce33a2b25
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634218"
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Bir Kubernetes kümesi için Azure Stack Marketini Ekle

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!note]  
> Azure Stack'te AKS (Azure Kubernetes hizmeti) altyapısı, özel Önizleme aşamasındadır. Bu makaledeki yönergeleri gerçekleştirmek için gereken Kubernetes Market öğesi erişim istemek için [erişim elde etmek için talebinizi](https://aka.ms/azsk8).

Kullanıcılarınız için bir Market öğesi bir Kubernetes kümesi sunabilir. Kullanıcılarınızın Kubernetes tek ve eşgüdümlü bir işlemle dağıtabilir.

Şu makaleye bakın dağıtmak ve tek başına bir Kubernetes kümesi için kaynakları sağlamak için bir Azure Resource Manager şablonu kullanarak. Başlamadan önce Azure Stack ve Azure genel Kiracı ayarlarını kontrol edin. Azure Stack hakkında gerekli bilgileri toplayın. Gerekli kaynakları kiracınız ve Azure Stack Marketini ekleyin. Bir Ubuntu sunucusu, özel bir betik ve Market'te olması için Kubernetes kümesi öğeleri kümesi bağlıdır.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Bir plan, teklif ve bir abonelik oluşturun

Bir plan, teklif ve Kubernetes küme Market öğesi için bir abonelik oluşturun. Ayrıca, var olan bir planı kullanın ve sunar.

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

1. Aşağıdaki profil ile bir sunucu seçin:
    - **Yayımcı**: Canonical
    - **Teklif**: UbuntuServer
    - **SKU**: 16.04 LTS
    - **Sürüm**: 16.04.201802220

    > [!Note]  
    > Birden fazla sürümünü Ubuntu Server 16.04 LTS listelenebilir. Eşleşen sürümünü eklemeniz gerekir. Kubernetes kümesini öğesi'nün tam sürümünü gerektirir.

1. Seçin **indirin.**

## <a name="add-a-custom-script-for-linux"></a>Linux için özel bir komut dosyası Ekle

Kubernetes kümesini marketten ekleyin:

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **tüm hizmetleri** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Custom Script for Linux` yazın.

1. Aşağıdaki profil komut dosyasını seçin:
    - **Teklif**: Linux 2.0 için özel betik
    - **Sürüm**: 2.0.3 sürümünü
    - **Yayımcı**: Microsoft Corp

    > [!Note]  
    > Linux için özel betik birden fazla sürümünü listelenebilir. Eşleşen sürümünü eklemeniz gerekir. Kubernetes kümesini öğesi'nün tam sürümünü gerektirir.

1. Seçin **indirin.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Market'te Kubernetes kümenize ekleyin

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **hizmetlerini ekleyin** altındaki **Yönetim** kategorisi, select **Market Yönetim**.

1. Seçin **+ Azure'dan Ekle**.

1. `Kubernetes Cluster` yazın.

1. `Kubernetes Cluster` öğesini seçin.

1. Seçin **indirin.**

    > [!note]  
    > Bu Market öğesi Market'te görünmesi için beş dakika sürebilir.

    ![Kubernetes kümesi](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Güncelleştirme veya Kubernetes kümesi kaldırma 

Kubernetes kümesi öğesi güncelleştirilirken Market'te öğeyi kaldırmak gerekir. Ardından, Kubernetes kümesini Market'te eklemek için bu makaledeki yönerge takip edebilirsiniz.

Kubernetes kümesi öğeyi kaldırmak için:

1. Geçerli öğenin adını gibi unutmayın `Microsoft.AzureStackKubernetesCluster.0.1.0`

1. PowerShell ile Azure stack'e bağlanma.

1. Öğeyi kaldırmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack için bir Kubernetes kümesi dağıtma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Azure stack'teki hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)