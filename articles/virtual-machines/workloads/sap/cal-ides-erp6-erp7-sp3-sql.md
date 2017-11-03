---
title: "Azure üzerinde SAP ERP 6.0 için SAP IDE EHP7 SP3 dağıtma | Microsoft Docs"
description: "Azure üzerinde SAP ERP 6.0 için SAP IDE EHP7 SP3 dağıtma"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 91eed294077ff72d0760018b10c98f32db88f3be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>Azure üzerinde SAP ERP 6.0 için SAP IDE EHP7 SP3 dağıtma
Bu makalede, SQL Server ve Windows işletim sistemi ile SAP bulut Gereci kitaplığı (SAP CAL) 3.0 Azure üzerinde çalışan bir SAP IDE sistemi dağıtmayı açıklar. Ekran görüntüleri işlemi adım adım göstermektedir. Farklı bir çözümü dağıtmak için aynı adımları izleyin.

SAP CAL ile başlatmak için Git [SAP bulut Gereci Kitaplığı](https://cal.sap.com/) Web sitesi. SAP ayrıca yeni hakkında bir blog sahip [SAP bulut Gereci kitaplığı 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 

> [!NOTE]
29 Mayıs 2017 itibariyle SAP CAL dağıtmak için daha az tercih edilen Klasik dağıtım modeli yanı sıra Azure Resource Manager dağıtım modelini kullanabilirsiniz. Yeni Resource Manager dağıtım modeli kullanır ve klasik dağıtım modeli göz ardı öneririz.

Klasik modeli kullanan bir SAP CAL hesabı zaten oluşturduysanız *başka bir SAP CAL hesabı oluşturmak gereken*. Yalnızca Resource Manager modelini kullanarak Azure'da dağıtmak bu hesabı gerekir.

SAP CAL oturum açtıktan sonra ilk sayfa genellikle için sizi **çözümleri** sayfası. İstediğiniz çözüm bulmak için oldukça biraz kaydırmanız gerekebilir şekilde SAP CAL sunulan çözümleri sürekli olarak artmaktadır. Vurgulanan özel olarak Azure üzerinde kullanılabilir SAP IDE Windows tabanlı çözüm dağıtım işlemi gösterilmektedir:

![SAP CAL çözümleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL bir hesap oluşturun
1. SAP CAL ilk kez oturum açmak için SAP S kullanıcı veya SAP ile kayıtlı başka bir kullanıcı kullanın. Ardından tarafından SAP CAL cihazları azure'da dağıtmak için kullanılan bir SAP CAL hesap tanımlayın. Hesap tanımı'nda şunları yapmanız gerekir:

    a. Azure'da (Resource Manager veya Klasik) dağıtım modeli seçin.

    b. Azure aboneliğiniz girin. Bir SAP CAL hesabı yalnızca bir abonelik için atanmış olabilir. Birden fazla abonelik ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.
    
    c. Azure aboneliğinize dağıtmak için SAP CAL izin verin.

    > [!NOTE]
    Sonraki adımlar Resource Manager dağıtımları için bir SAP CAL hesabının nasıl oluşturulacağını gösterir. Klasik dağıtım modeline bağlı SAP CAL hesabınız zaten varsa, *gereksinim* yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izleyin. Resource Manager modelinde dağıtmak yeni SAP CAL hesabı gerekir.

2. Yeni bir SAP CAL hesabı oluşturmak için **hesapları** sayfa, Azure için iki seçenek gösterir: 

    a. **Microsoft Azure (Klasik)** Klasik dağıtım modeli ve artık tercih edilir.

    b. **Microsoft Azure** yeni Resource Manager dağıtım modeli.

    ![SAP CAL hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    Resource Manager modelinde dağıtmak için seçin **Microsoft Azure**.

    ![SAP CAL hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. Azure girin **abonelik kimliği** Azure Portalı'nda bulunabilir. 

    ![SAP CAL abonelik kimliği](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. Tanımladığınız Azure aboneliğinize dağıtmak için SAP CAL yetkilendirmek için tıklatın **Authorize**. Bir tarayıcı sekmesinde aşağıdaki sayfası görünür:

    ![Oturum açma Internet Explorer bulut Hizmetleri](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. Birden fazla kullanıcı listeleniyorsa, Abonelikteki seçtiğiniz Azure olması için bağlantılı Microsoft hesabını seçin. Bir tarayıcı sekmesinde aşağıdaki sayfası görünür:

    ![Internet Explorer bulut Hizmetleri onayı](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. Tıklatın **kabul**. Yetkilendirme başarılı olursa, SAP CAL hesabı tanımı yeniden görüntüler. Kısa bir süre sonra bir ileti yetkilendirme işleminin başarılı olduğunu onaylar.

7. Yeni oluşturulan SAP CAL hesabı, kullanıcıya atamak için girin, **kullanıcı kimliği** metin kutusuna tıklayın ve sağ **Ekle**. 

    ![Kullanıcı ilişkisi hesabı](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. SAP CAL oturum açmak için kullandığınız kullanıcı hesabınızın ilişkilendirmek için tıklatın **gözden**. 

9. Yeni oluşturulan SAP CAL hesap kullanıcı arasındaki ilişki oluşturmak için tıklatın **oluşturma**.

    ![Kullanıcı hesabı ilişkilendirmesi](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Olduğu bir SAP CAL hesabı başarıyla oluşturuldu:

- Resource Manager dağıtım modeli kullanır.
- SAP sistemleri Azure aboneliğinize dağıtın.

> [!NOTE]
Windows ve SQL Server göre SAP IDE çözüm dağıtabilmeniz için önce bir SAP CAL aboneliğine kaydolma gerekebilir. Aksi takdirde, çözümü olarak gösterebilir **kilitli** genel bakış sayfasında.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma
1. Bir SAP CAL hesabı kurduktan sonra seçin **SAP IDE çözüm Windows ve SQL Server üzerinde** çözümü. Tıklatın **örnek Oluştur**ve kullanım ve koşulları koşullar onaylayın. 

2. Üzerinde **temel mod: Örnek Oluştur** sayfasında gerekir:

    a. Bir örnek girmek **adı**.

    b. Bir Azure seçin **bölge**. Sunulan birden fazla Azure bölgesine almak için bir SAP CAL abonelik gerekebilir.

    c.  Asıl girin **parola** gösterildiği gibi çözüm için:

    ![SAP CAL Basic modu: Örnek oluşturma](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. **Oluştur**'a tıklayın. Bir süre sonra boyutu ve karmaşıklığı (SAP CAL tahmini sağlar) çözümün bağlı olarak durumu etkin ve kullanım için hazır olarak gösterilir: 

    ![SAP CAL örnekleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. Kaynak grubu ve SAP CAL tarafından oluşturulan tüm nesneleri bulmak için Azure portalına gidin. Sanal makine SAP CAL verilen aynı örnek adını başlayarak bulunabilir.

    ![Kaynak grubu nesneleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. SAP CAL Portal'daki dağıtılan örneklerine gidin ve tıklayın **Bağlan**. Aşağıdaki açılır pencere görünür: 

    ![Örneğine bağlanın](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. Dağıtılan sistemlere bağlanmak için seçeneklerden birini kullanmadan önce tıklatın **Başlarken Kılavuzu**. Belge her bağlantı yöntemleri için kullanıcıların adları. Bu kullanıcıların parolalarını dağıtım işleminin başında tanımlanan ana parola ayarlanır. Belgelerde dağıtılan sisteme oturum açmak için kullanabileceğiniz parolalarını ile diğer daha işlevsel kullanıcılar listelenir.

    ![SAP Hoş Geldiniz belgeleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Birkaç saat içinde sağlıklı bir SAP IDE sistem Azure'da dağıtılır.

Bir SAP CAL abonelik satın aldıysanız, SAP dağıtımlarınızı SAP CAL aracılığıyla Azure üzerinde tam olarak destekler. Destek BC VCM CAL sırasıdır.

