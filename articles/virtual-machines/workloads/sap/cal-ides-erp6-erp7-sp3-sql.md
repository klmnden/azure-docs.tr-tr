---
title: Azure'da SAP ERP 6.0 için SAP IDES EHP7 SP3 dağıtma | Microsoft Docs
description: Azure'da SAP ERP 6.0 için SAP IDES EHP7 SP3'ı dağıtma
services: virtual-machines-windows
documentationcenter: ''
author: hermanndms
manager: gwallace
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 1b2b3d46d0352f72b1ffb513a96c1ab5dc25ad54
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707490"
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>Azure'da SAP ERP 6.0 için SAP IDES EHP7 SP3'ı dağıtma
Bu makalede, SQL Server ve Windows işletim sistemi ile Azure üzerinde SAP Cloud Appliance Library (SAP CAL) 3.0 çalışan bir SAP IDES sistemi dağıtmayı açıklar. Ekran görüntüleri işlemi adım adım gösterir. Farklı bir çözümü dağıtmak için aynı adımları izleyin.

SAP CAL ile başlatmak için Git [SAP Cloud Appliance Library](https://cal.sap.com/) Web sitesi. SAP de sahip hakkında yeni bir blog [SAP bulut Gereci kitaplığı 3.0](https://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 

> [!NOTE]
> 29 Mayıs 2017'den itibaren SAP CAL'ı dağıtmak için daha az tercih edilen Klasik dağıtım modeli yanı sıra Azure Resource Manager dağıtım modelini kullanabilirsiniz. Yeni Resource Manager dağıtım modelini kullanan ve klasik dağıtım modeli dikkate öneririz.

Klasik modeli kullanan bir SAP CAL hesabı zaten oluşturduysanız *başka bir SAP CAL hesabı oluşturmak için ihtiyacınız*. Yalnızca Resource Manager modelini kullanarak Azure'a dağıtmak bu hesabı gerekir.

SAP CAL için oturum açtıktan sonra ilk sayfa genellikle, müşteri adayları **çözümleri** sayfası. İstediğiniz çözüm bulmak için bir bit kaydırmanız gerekebilir şekilde SAP CAL üzerinde sağlanan çözümlerin sürekli olarak artmaktadır. Vurgulanan özel olarak Azure üzerinde kullanıma hazır SAP IDES Windows tabanlı çözüm, dağıtım işlemini gösterir:

![SAP CAL çözümleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL bir hesap oluşturun
1. SAP CAL için ilk kez oturum açmak için SAP S kullanıcı veya başka bir kullanıcı ile SAP kayıtlı kullanın. Ardından tarafından SAP CAL gereçler azure'da dağıtmak için kullanılan bir SAP CAL hesabı tanımlayın. Hesap tanımında yapmanız gerekir:

    a. Azure'da (Resource Manager veya Klasik) dağıtım modeli seçin.

    b. Azure aboneliğinizi girin. SAP CAL hesabı yalnızca bir aboneliğe atanabilir. Birden fazla aboneliğine ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.
    
    c. Azure aboneliğinize dağıtmak için SAP CAL izin verin.

   > [!NOTE]
   >  Sonraki adımlar Resource Manager dağıtımları için bir SAP CAL hesabının nasıl oluşturulacağını gösterir. Klasik dağıtım modeline bağlı bir SAP CAL hesabı zaten varsa, *gereken* yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izleyin. Resource Manager modelinde dağıtmak yeni SAP CAL hesabı gerekir.

1. Yeni bir SAP CAL hesabı oluşturmak için **hesapları** sayfa, Azure için iki seçenek gösterir: 

    a. **Microsoft Azure (Klasik)** Klasik dağıtım modeli ve artık tercih edilir.

    b. **Microsoft Azure** yeni Resource Manager dağıtım modeli.

    ![SAP CAL hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    Resource Manager modelinde dağıtmak için seçebileceğiniz **Microsoft Azure**.

    ![SAP CAL hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

1. Azure girin **abonelik kimliği** Azure portalında bulunabilir. 

    ![SAP CAL abonelik kimliği](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

1. Tanımladığınız Azure aboneliğinize dağıtmak için SAP CAL yetkilendirmek için tıklatın **Authorize**. Tarayıcı sekmesinde aşağıdaki sayfa açılır:

    ![Oturum açma Internet Explorer bulut Hizmetleri](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

1. Birden fazla kullanıcı listede yoksa, Abonelikteki seçtiğiniz Azure aboneliğinde olması bağlantılı bir Microsoft hesabı seçin. Tarayıcı sekmesinde aşağıdaki sayfa açılır:

    ![Internet Explorer bulut Hizmetleri onayı](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

1. Tıklayın **kabul**. Yetkilendirme başarılı olursa, SAP CAL hesabı tanımı yeniden görüntüler. Kısa bir süre sonra bir ileti, Yetkilendirme işlemi başarılı olduğunu onaylar.

1. Kullanıcı için yeni oluşturulan SAP CAL hesabı atamak için girin, **kullanıcı kimliği** sağ tıklayıp metin kutusundaki **Ekle**. 

    ![Kullanıcı ilişkisi hesap](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

1. Hesabınız için SAP CAL oturum açmak için kullandığınız kullanıcı ilişkilendirmek için tıklayın **gözden geçirme**. 

1. Yeni oluşturulan SAP CAL hesabı kullanıcı arasındaki ilişkiyi oluşturmak için tıklayın **Oluştur**.

    ![Kullanıcı hesabı ilişkilendirmesi](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Sizin için bir SAP CAL hesabı başarıyla oluşturuldu:

- Resource Manager dağıtım modelini kullanın.
- SAP sistemlerini Azure aboneliğinize dağıtın.

> [!NOTE]
> Windows ve SQL Server üzerinde SAP IDES çözüm dağıtabilmeniz için önce bir SAP CAL aboneliğine kaydolma gerekebilir. Aksi takdirde, çözümü olarak gösterebilir **kilitli** genel bakış sayfasında.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma
1. SAP CAL hesabınızı ayarladıktan sonra seçin **Windows ve SQL Server üzerinde SAP IDES çözüm** çözüm. Tıklayın **oluşturma örneği**, kullanım ve koşulları koşullar onaylayın. 

1. Üzerinde **temel modu: Örneği oluşturma** sayfasında gerekir:

    a. Bir örneği girmeniz **adı**.

    b. Azure'ı seçin **bölge**. Sunulan birden fazla Azure bölgesini almak için bir SAP CAL aboneliğine ihtiyacınız.

    c.  Ana girin **parola** gösterildiği gibi çözüm için:

    ![SAP CAL temel modu: Örneği oluşturma](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

1.           **Oluştur**'a tıklayın. Bir süre sonra büyüklüğü ve karmaşıklığı (SAP CAL bir tahmin sağlar), çözümün bağlı olarak durumu etkin ve kullanıma hazır olarak gösterilir: 

    ![SAP CAL örnekleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

1. Kaynak grubu ve SAP CAL tarafından oluşturulan tüm nesneleri bulmak için Azure portalına gidin. Sanal makine, SAP CAL verilen aynı örnek adını başlayarak bulunabilir.

    ![Kaynak grubu nesneleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

1. SAP CAL portalında dağıtılan örneklerine gidin ve **Connect**. Aşağıdaki açılır pencere görünür: 

    ![Örneğine bağlanın](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

1. Dağıtılmış sisteme bağlanmak için seçeneklerden birini kullanmadan önce tıklayın **Başlarken Kılavuzu**. Belgeleri yöntemlerin her biri bağlantı için kullanıcılar adları. Kullanıcılar için parolalar, dağıtım işleminin başında tanımladığınız ana parola ayarlanır. Belgelerde, dağıtılmış sisteme oturum açmak için kullanabileceğiniz parolalarını ile daha işlevsel diğer kullanıcılar listelenir.

    ![SAP Hoş Geldiniz belgeleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Birkaç saat içinde iyi durumda bir SAP IDES sistem Azure'da dağıtılır.

SAP CAL aboneliği satın aldıysanız, SAP dağıtımları SAP CAL aracılığıyla Azure'da tam olarak destekler. Destek, BC VCM CAL kuyruğudur.

