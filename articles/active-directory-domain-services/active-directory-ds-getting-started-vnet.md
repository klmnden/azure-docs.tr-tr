---
title: "Azure Active Directory Domain Services: Sanal ağ oluşturma veya seçme | Microsoft Docs"
description: "Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: cb372232492e8f98ff1543798b92b4b60fc25021
ms.lasthandoff: 04/12/2017


---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services için sanal ağ oluşturma veya seçme
## <a name="before-you-begin"></a>Başlamadan önce
[Azure Active Directory Domain Services için ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md) bölümüne bakın.

## <a name="task-2-create-an-azure-virtual-network"></a>Görev 2: Azure sanal ağı oluşturma
Sonraki yapılandırma görevi bir Azure sanal ağı ve onun içinde bir alt ağ oluşturulmasını içerir. Sanal ağınızdaki bu alt ağda Azure Active Directory Domain Services’ı etkinleştirin. Kullanmayı tercih ettiğiniz bir sanal ağınız varsa bu adımı atlayabilirsiniz.

> [!NOTE]
> Oluşturduğunuz veya Azure Active Directory Domain Services ile kullanmayı seçtiğiniz Azure sanal ağının, Azure Active Directory Domain Services tarafından desteklenen bir Azure bölgesine ait olduğundan emin olun. Azure Active Directory Domain Services’ın kullanılabildiği Azure bölgelerini öğrenmek için [Bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
>
>Sonraki bir yapılandırma adımında Azure Active Directory Domain Services etkinleştirilirken doğru sanal ağı seçebilmek için sanal ağın adını not edin.


Azure Active Directory Domain Services’ı etkinleştirmek istediğiniz bir Azure sanal ağı oluşturmak için aşağıdaki yapılandırma yönergelerini izleyin:

1. [Klasik Azure portalı](https://manage.windowsazure.com)'na gidin.
2. Sol bölmede **Ağlar**’ı seçin.

    ![Ağlar düğümü](./media/active-directory-domain-services-getting-started/networks-node.png)  
    **Sanal Ağlar** penceresi açılır.
3. Pencerenin altındaki görev bölmesinde **Yeni**’ye tıklayın.

    ![Sanal ağlar penceresi](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. **Ağ Hizmetleri** düğümüne tıklayıp **Sanal Ağ** öğesini seçin.
    
    ![Sanal ağ - hızlı oluştur](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. Sanal ağ oluşturmak için **Hızlı Oluştur**’a tıklayın.
    
6. Sanal ağınız için bir **Ad** belirtin ve aşağıdakileri yapın: 
    * Bu ağ için **Adres alanını** veya **Maksimum VM sayısını** yapılandırmayı seçebilirsiniz. 
    * Şu an için **DNS sunucusu** ayarını **Yok** olarak ayarlanmış şekilde bırakabilirsiniz. Azure Active Directory Domain Services’ı etkinleştirmenizin ardından ayarı güncelleştirebilirsiniz.
7. **Konum** açılır listesinde desteklenen bir Azure bölgesi seçin.  
    Azure Active Directory Domain Services’ın kullanılabildiği Azure bölgelerini öğrenmek için [Bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
8. Sanal ağınızı oluşturmak için **Sanal Ağ Oluştur**’a tıklayın.

    ![Azure Active Directory Domain Services için sanal ağ oluşturma](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Sanal ağ oluşturduktan sonra sanal ağın adını seçin ve **Yapılandır** sekmesine tıklayın.

    ![Alt ağ oluşturma](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. **Sanal ağ adres alanları** altında **alt ağ ekle**’ye tıklayın ve ardından **AaddsSubnet** adlı bir alt ağ belirtin. 

    ![Azure Active Directory Domain Services için alt ağ oluşturma](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. Alt ağı oluşturmak için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Görev 3: [Azure Active Directory Domain Services’ı etkinleştirme](active-directory-ds-getting-started-enableaadds.md)

