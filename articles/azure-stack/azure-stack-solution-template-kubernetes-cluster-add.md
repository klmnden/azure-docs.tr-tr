---
title: Azure yığın Market Kubernetes küme ekleme | Microsoft Docs
description: Azure yığın Marketinde Kubernetes küme eklemeyi öğrenin.
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
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: c66b0d7ea5ade90c6bb8f88006f2a09bd407deaa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Azure yığın Market Kubernetes küme ekleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

> [!note]  
> Azure kapsayıcı Hizmetleri (ACS) Kubernetes Azure yığında özel önizlemede değil. Bu makaledeki yönergeleri gerçekleştirmek için gereken Kubernetes Market öğesi için erişim istemek için [erişmek için bir istek göndermek](https://aka.ms/azsk8).

Kullanıcılarınız için bir Market öğesi olarak Kubernetes küme sunabilir. Kullanıcılarınızın Kubernetes içinde tek ve eşgüdümlü bir işlemle dağıtabilir.

Aşağıdaki makaleye bakın dağıtmak ve tek başına Kubernetes küme kaynaklarını sağlamak için bir Azure Resource Manager şablonu kullanarak. Başlamadan önce Azure yığını ve genel Azure Kiracı ayarlarınızı denetleyin. Azure yığın hakkında gerekli bilgileri toplayın. Gerekli kaynaklar, Kiracı ve Azure yığın Market ekleyin. Küme bir Ubuntu server, özel bir komut dosyası ve markette olmasını Kubernetes Küme öğelerini bağlıdır.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Bir plan, teklif ve abonelik oluşturma

Bir plan, teklif ve Kubernetes küme Market öğesi için bir abonelik oluşturun. Ayrıca, var olan bir planı kullanabilir ve sunar.

1. Oturum [Yönetim Portalı.](https://adminportal.local.azurestack.external)

2. Bir planı temel plan oluşturun. Yönergeler için bkz: [Azure yığınında bir plan oluşturmak](azure-stack-create-plan.md).

3. Bir teklif oluşturun. Yönergeler için bkz: [bir teklifi Azure yığınında oluşturma](azure-stack-create-offer.md).

4. Seçin **sunar**, oluşturduğunuz teklif bulun.

5. Seçin **genel bakış** teklif dikey penceresinde.

6. Seçin **değişiklik durumu**. Seçin **ortak**.

7. Seçin **+ yeni** > **sunar ve planları** > **abonelik** yeni bir abonelik oluşturmak için.

    a. Girin bir **görünen adı**.

    b. Girin bir **kullanıcı**. Kiracı ile ilişkilendirilen Azure AD hesabı kullanın.

    c. **Sağlayıcı Açıklaması**

    d. Ayarlama **Directory Kiracı** Azure yığın için Azure AD kiracısı için. 

    e. Seçin **teklif**. Oluşturduğunuz teklif adını seçin. Abonelik kimliği not edin

## <a name="add-an-ubuntu-server-image"></a>Ubuntu server görüntüsü ekleme

Aşağıdaki Ubuntu Server görüntüsü Market'te ekleyin:

1. Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim**.

3. Seçin **+ Azure'dan eklemek**.

4. Girin `UbuntuServer`.

5. Aşağıdaki profili sunucusuyla seçin:
    - **Yayımcı**: kurallı
    - **Teklif**: UbuntuServer
    - **SKU**: 16.04 LTS
    - **Sürüm**: 16.04.201802220

6. Seçin **indirin.**

## <a name="add-a-custom-script-for-linux"></a>Linux için özel bir komut dosyası Ekle

Kubernetes küme marketten ekleyin:

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim**.

3. Seçin **+ Azure'dan eklemek**.

4. Girin `Custom Script for Linux`.

5. Aşağıdaki profili ile komut dosyası seçin:
    - **Teklif**: Linux 2.0 için özel bir komut dosyası
    - **Sürüm**: 2.0.3
    - **Yayımcı**: Microsoft Corp

6. Seçin **indirin.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Market'te Kubernetes küme ekleme

1. Açık [Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim**.

3. Seçin **+ Azure'dan eklemek**.

4. Girin `Kubernetes Cluster`.

5. `Kubernetes Cluster` öğesini seçin.

6. Seçin **indirin.**

    > [!note]  
    > Market öğesi markette görünmesi için beş dakika sürebilir.

    ![Kubernetes küme](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Güncelleştirme veya Kubernetes küme kaldırma 

Kubernetes küme öğesi güncellenirken markette öğeyi kaldırmanız gerekir. Ardından Market'te Kubernetes eklemek için bu makaledeki yönerge izleyebilirsiniz.

Kubernetes küme öğesini kaldırmak için:

1. Geçerli öğenin adı gibi unutmayın `Microsoft.AzureStackKubernetesCluster.0.1.0`

2. PowerShell ile Azure yığın bağlayın.

3. Öğesini kaldırmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığınına Kubernetes kümesi dağıtma](/user/azure-stack-solution-template-kubernetes-deploy.md)

[Azure yığınında hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md)