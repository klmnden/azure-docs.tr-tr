---
title: SAP S/4HANA veya BW/4hana'yı Azure VM üzerinde dağıtın | Microsoft Docs
description: SAP S/4HANA veya BW/4hana'yı Azure VM üzerinde dağıtın
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: c59fcf43cb4767f1d95d769dfce4d5c8755e45ee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60836884"
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA veya BW/4hana'yı azure'da dağıtın
Bu makalede, S/4hana'yı Azure üzerindeki SAP Cloud Appliance Library (SAP CAL) 3.0 kullanarak dağıtmayı açıklar. BW/4HANA gibi diğer SAP HANA tabanlı çözümleri dağıtmak için aynı adımları izleyin.

> [!NOTE]
> SAP CAL hakkında daha fazla bilgi için Git [SAP Cloud Appliance Library](https://cal.sap.com/) Web sitesi. SAP de sahip bir blog hakkında [SAP bulut Gereci kitaplığı 3.0](https://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).
> 
> [!NOTE]
> 29 Mayıs 2017'den itibaren SAP CAL'ı dağıtmak için daha az tercih edilen Klasik dağıtım modeli yanı sıra Azure Resource Manager dağıtım modelini kullanabilirsiniz. Yeni Resource Manager dağıtım modelini kullanan ve klasik dağıtım modeli dikkate öneririz.

## <a name="step-by-step-process-to-deploy-the-solution"></a>Çözümü dağıtmak için adım adım işlemi

Aşağıdaki ekran görüntüleri dizisi S/4hana'yı Azure üzerindeki SAP CAL'ı kullanarak dağıtma gösterilmektedir. İşlem, BW/4HANA gibi diğer çözümler için aynı şekilde çalışır.

**Çözümleri** sayfası SAP CAL HANA tabanlı çözümler kullanılabilir bazıları Azure üzerinde gösterir. **SAP S/4HANA 1610 FPS01, Fully-Activated Gereci** Orta satırında:

![SAP CAL çözümleri](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL bir hesap oluşturun
1. SAP CAL için ilk kez oturum açmak için SAP S kullanıcı veya başka bir kullanıcı ile SAP kayıtlı kullanın. Ardından tarafından SAP CAL gereçler azure'da dağıtmak için kullanılan bir SAP CAL hesabı tanımlayın. Hesap tanımında yapmanız gerekir:

    a. Azure'da (Resource Manager veya Klasik) dağıtım modeli seçin.

    b. Azure aboneliğinizi girin. SAP CAL hesabı yalnızca bir aboneliğe atanabilir. Birden fazla aboneliğine ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.

    c. Azure aboneliğinize dağıtmak için SAP CAL izin verin.

   > [!NOTE]
   >  Sonraki adımlar Resource Manager dağıtımları için bir SAP CAL hesabının nasıl oluşturulacağını gösterir. Klasik dağıtım modeline bağlı bir SAP CAL hesabı zaten varsa, *gereken* yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izleyin. Resource Manager modelinde dağıtmak yeni SAP CAL hesabı gerekir.

1. Yeni bir SAP CAL hesabı oluşturun. **Hesapları** sayfa, Azure için üç seçeneğiniz gösterir: 

    a. **Microsoft Azure (Klasik)** Klasik dağıtım modeli ve artık tercih edilir.

    b. **Microsoft Azure** yeni Resource Manager dağıtım modeli.

    c. **Windows Azure 21Vianet tarafından işletilen** Klasik dağıtım modelini kullanan Çin'de bir seçenektir.

    Resource Manager modelinde dağıtmak için seçebileceğiniz **Microsoft Azure**.

    ![SAP CAL hesabı ayrıntıları](./media/cal-s4h/s4h-pic-2a.png)

1. Azure girin **abonelik kimliği** Azure portalında bulunabilir.

   ![SAP CAL hesapları](./media/cal-s4h/s4h-pic3c.png)

1. Tanımladığınız Azure aboneliğinize dağıtmak için SAP CAL yetkilendirmek için tıklatın **Authorize**. Tarayıcı sekmesinde aşağıdaki sayfa açılır:

   ![Oturum açma Internet Explorer bulut Hizmetleri](./media/cal-s4h/s4h-pic4c.png)

1. Birden fazla kullanıcı listede yoksa, Abonelikteki seçtiğiniz Azure aboneliğinde olması bağlantılı bir Microsoft hesabı seçin. Tarayıcı sekmesinde aşağıdaki sayfa açılır:

   ![Internet Explorer bulut Hizmetleri onayı](./media/cal-s4h/s4h-pic5a.png)

1. Tıklayın **kabul**. Yetkilendirme başarılı olursa, SAP CAL hesabı tanımı yeniden görüntüler. Kısa bir süre sonra bir ileti, Yetkilendirme işlemi başarılı olduğunu onaylar.

1. Kullanıcı için yeni oluşturulan SAP CAL hesabı atamak için girin, **kullanıcı kimliği** sağ tıklayıp metin kutusundaki **Ekle**.

   ![Kullanıcı ilişkisi hesap](./media/cal-s4h/s4h-pic8a.png)

1. Hesabınız için SAP CAL oturum açmak için kullandığınız kullanıcı ilişkilendirmek için tıklayın **gözden geçirme**. 
 
1. Yeni oluşturulan SAP CAL hesabı kullanıcı arasındaki ilişkiyi oluşturmak için tıklayın **Oluştur**.

   ![Kullanıcı için SAP CAL hesabı ilişkisi](./media/cal-s4h/s4h-pic9b.png)

Sizin için bir SAP CAL hesabı başarıyla oluşturuldu:

- Resource Manager dağıtım modelini kullanın.
- SAP sistemlerini Azure aboneliğinize dağıtın.

Artık S/4hana'yı azure'da kullanıcı aboneliğinize dağıtmaya başlayabilirsiniz.

> [!NOTE]
> Devam etmeden önce Azure H serisi VM'ler için Azure vCPU kotaları sahip olup olmadığını belirler. Şu anda SAP CAL bazı SAP HANA tabanlı çözümleri dağıtmak için H-serisi Azure Vm'leri kullanır. Azure aboneliğiniz için H-serisi herhangi H serisi, vCPU kotaları olmayabilir. Bu durumda, en az 16 H serisi, Vcpu kotası almak için Azure desteğine başvurun gerekebilir.
> 
> [!NOTE]
> Azure'da SAP CAL üzerinde bir çözümü dağıttığınızda, yalnızca bir Azure bölgesine seçebileceğiniz bulabilirsiniz. SAP CAL tarafından önerilen farklı Azure bölgelerinde uygulamasına dağıtmak için SAP'den CAL aboneliği satın almanız gerekir. SAP CAL hesabınızın Başlangıçta önerilen olanlar dışındaki Azure bölgeleri sunmak için etkin olması için bir ileti açmak gerekebilir.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma

Bir çözüm dağıtalım **çözümleri** SAP CAL sayfası. SAP CAL dağıtmak için iki diziyi sahiptir:

- Dağıtılacak sistem tanımlamak için bir sayfa kullanan temel bir dizisi
- Belirli seçenekler VM boyutlarına göre size gelişmiş bir dizisi 

Biz burada dağıtım temel yolunu gösterir.

1. Üzerinde **hesap ayrıntılarını** sayfasında gerekir:

    a. SAP CAL hesabı seçin. (Resource Manager dağıtım modeliyle dağıtmak için ilişkili olan bir hesap kullanın.)

    b. Bir örneği girmeniz **adı**.

    c. Azure'ı seçin **bölge**. Bir bölgede SAP CAL önerir. Başka bir Azure bölgesine gerekir ve bir SAP CAL aboneliğiniz yoksa, SAP CAL abonelikle sipariş gerekir.

    d. Bir ana girin **parola** çözümü sekiz veya dokuz karakter. Parola yöneticileri farklı bileşenleri için kullanılır.

   ![SAP CAL temel modu: Örnek Oluştur](./media/cal-s4h/s4h-pic10a.png)

1. Tıklayın **Oluştur**, görüntülenen ileti kutusunda tıklatıp **Tamam**.

   ![SAP CAL desteklenen VM boyutları](./media/cal-s4h/s4h-pic10b.png)

1. İçinde **özel anahtar** iletişim kutusu, tıklayın **Store** SAP CAL özel anahtarı depolamak için. İçin özel anahtar parola koruması kullanmak için **indirme**. 

   ![SAP CAL özel anahtar](./media/cal-s4h/s4h-pic10c.png)

1. SAP CAL okuma **uyarı** iletisi ve tıklayın **Tamam**.

   ![SAP CAL Uyarısı](./media/cal-s4h/s4h-pic10d.png)

    Artık dağıtım gerçekleşir. Bir süre sonra büyüklüğü ve karmaşıklığı (SAP CAL bir tahmin sağlar), çözümün bağlı olarak durumu etkin ve kullanıma hazır olarak gösterilir.

1. Toplanan diğer kaynaklarla ilişkili bir kaynak grubundaki sanal makineleri bulmak için Azure portalına gidin: 

   ![Yeni Portalı'nda dağıtılan SAP CAL nesneler](./media/cal-s4h/sapcaldeplyment_portalview.png)

1. SAP CAL Portalı'nda, durum olarak görünür. **etkin**. Çözüme bağlanmak için **Connect**. Farklı bileşenlere bağlanmak için farklı seçenekleri, bu çözüm içinde dağıtılır.

   ![SAP CAL örnekleri](./media/cal-s4h/active_solution.png)

1. Dağıtılmış sisteme bağlanmak için seçeneklerden birini kullanmadan önce tıklayın **Başlarken Kılavuzu**. 

   ![Örneğine bağlanın](./media/cal-s4h/connect_to_solution.png)

    Belgeleri yöntemlerin her biri bağlantı için kullanıcılar adları. Kullanıcılar için parolalar, dağıtım işleminin başında tanımladığınız ana parola ayarlanır. Belgelerde, dağıtılmış sisteme oturum açmak için kullanabileceğiniz parolalarını ile daha işlevsel diğer kullanıcılar listelenir. 

    Örneğin, Windows Uzak Masaüstü makinede önceden SAP GUI kullanırsanız S/4 sistem şuna benzeyebilir:

   ![Önceden yüklenmiş SAP GUI'de SM50](./media/cal-s4h/gui_sm50.png)

    Veya DBACockpit kullanıyorsanız, örneği şuna benzeyebilir:

   ![SM50 DBACockpit SAP GUI'de](./media/cal-s4h/dbacockpit.png)

Birkaç saat içinde sağlıklı bir SAP S/4 Gereci Azure'da dağıtılır.

SAP CAL aboneliği satın aldıysanız, SAP dağıtımları SAP CAL aracılığıyla Azure'da tam olarak destekler. Destek, BC VCM CAL kuyruğudur.







