---
title: SAP S/4HANA veya BW/4HANA Azure VM'de dağıtma | Microsoft Docs
description: SAP S/4HANA veya BW/4HANA Azure VM'de dağıtın
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
ms.openlocfilehash: 10c5116afa46817a42834e0350937fde7ae0b927
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657351"
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA veya BW/4HANA azure'da dağıtmak
Bu makalede, SAP bulut Gereci kitaplığı (SAP CAL) 3.0 kullanılarak S/4HANA Azure üzerinde dağıtmayı açıklar. BW/4HANA gibi diğer SAP HANA tabanlı çözümlerini dağıtmak için aynı adımları izleyin.

> [!NOTE]
SAP CAL hakkında daha fazla bilgi için Git [SAP bulut Gereci Kitaplığı](https://cal.sap.com/) Web sitesi. SAP de sahip bir blog hakkında [SAP bulut Gereci kitaplığı 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

> [!NOTE]
29 Mayıs 2017 itibariyle SAP CAL dağıtmak için daha az tercih edilen Klasik dağıtım modeli yanı sıra Azure Resource Manager dağıtım modelini kullanabilirsiniz. Yeni Resource Manager dağıtım modeli kullanır ve klasik dağıtım modeli göz ardı öneririz.

## <a name="step-by-step-process-to-deploy-the-solution"></a>Çözümü dağıtmak için adım adım işlemi

Aşağıdaki ekran görüntüleri dizisi S/4HANA Azure üzerinde SAP CAL kullanarak dağıtmak nasıl gösterir. İşlem BW/4HANA gibi diğer çözümleri için aynı şekilde çalışır.

**Çözümleri** sayfası SAP CAL HANA tabanlı çözümler kullanılabilir bazıları Azure üzerinde gösterir. **SAP S/4HANA 1610 FPS01, Fully-Activated Gereci** Orta sırada:

![SAP CAL çözümleri](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL bir hesap oluşturun
1. SAP CAL ilk kez oturum açmak için SAP S kullanıcı veya SAP ile kayıtlı başka bir kullanıcı kullanın. Ardından tarafından SAP CAL cihazları azure'da dağıtmak için kullanılan bir SAP CAL hesap tanımlayın. Hesap tanımı'nda şunları yapmanız gerekir:

    a. Azure'da (Resource Manager veya Klasik) dağıtım modeli seçin.

    b. Azure aboneliğiniz girin. Bir SAP CAL hesabı yalnızca bir abonelik için atanmış olabilir. Birden fazla abonelik ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.

    c. Azure aboneliğinize dağıtmak için SAP CAL izin verin.

    > [!NOTE]
    Sonraki adımlar Resource Manager dağıtımları için bir SAP CAL hesabının nasıl oluşturulacağını gösterir. Klasik dağıtım modeline bağlı SAP CAL hesabınız zaten varsa, *gereksinim* yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izleyin. Resource Manager modelinde dağıtmak yeni SAP CAL hesabı gerekir.

2. Yeni bir SAP CAL hesabı oluşturun. **Hesapları** sayfa, Azure için üç seçeneğiniz gösterir: 

    a. **Microsoft Azure (Klasik)** Klasik dağıtım modeli ve artık tercih edilir.

    b. **Microsoft Azure** yeni Resource Manager dağıtım modeli.

    c. **Windows Azure 21Vianet tarafından işletilen** Çin'de klasik dağıtım modelini kullanan bir seçenektir.

    Resource Manager modelinde dağıtmak için seçin **Microsoft Azure**.

    ![SAP CAL hesap ayrıntıları](./media/cal-s4h/s4h-pic-2a.png)

3. Azure girin **abonelik kimliği** Azure Portalı'nda bulunabilir.

   ![SAP CAL hesapları](./media/cal-s4h/s4h-pic3c.png)

4. Tanımladığınız Azure aboneliğinize dağıtmak için SAP CAL yetkilendirmek için tıklatın **Authorize**. Bir tarayıcı sekmesinde aşağıdaki sayfası görünür:

   ![Oturum açma Internet Explorer bulut Hizmetleri](./media/cal-s4h/s4h-pic4c.png)

5. Birden fazla kullanıcı listeleniyorsa, Abonelikteki seçtiğiniz Azure olması için bağlantılı Microsoft hesabını seçin. Bir tarayıcı sekmesinde aşağıdaki sayfası görünür:

   ![Internet Explorer bulut Hizmetleri onayı](./media/cal-s4h/s4h-pic5a.png)

6. Tıklatın **kabul**. Yetkilendirme başarılı olursa, SAP CAL hesabı tanımı yeniden görüntüler. Kısa bir süre sonra bir ileti yetkilendirme işleminin başarılı olduğunu onaylar.

7. Yeni oluşturulan SAP CAL hesabı, kullanıcıya atamak için girin, **kullanıcı kimliği** metin kutusuna tıklayın ve sağ **Ekle**.

   ![Kullanıcı ilişkisi hesabı](./media/cal-s4h/s4h-pic8a.png)

8. SAP CAL oturum açmak için kullandığınız kullanıcı hesabınızın ilişkilendirmek için tıklatın **gözden**. 
 
9. Yeni oluşturulan SAP CAL hesap kullanıcı arasındaki ilişki oluşturmak için tıklatın **oluşturma**.

   ![SAP CAL hesabı ilişkisi kullanıcıya](./media/cal-s4h/s4h-pic9b.png)

Olduğu bir SAP CAL hesabı başarıyla oluşturuldu:

- Resource Manager dağıtım modeli kullanır.
- SAP sistemleri Azure aboneliğinize dağıtın.

Şimdi S/4HANA azure'da kullanıcı aboneliğinize dağıtmaya başlatabilirsiniz.

> [!NOTE]
Devam etmeden önce Azure H-serisi VM'ler için Azure vCPU kotaları yüklü olup olmadığını belirler. Şu anda bazı SAP HANA tabanlı çözümler dağıtmak için H-serisi VM'ler Azure SAP CAL kullanır. Azure aboneliğiniz H-seri için hiçbir H-serisi vCPU kota sahip olmayabilir. Bu durumda, en az 16 H-serisi Vcpu'lar kotası almak için Azure desteğine başvurun gerekebilir.

> [!NOTE]
SAP CAL Azure üzerinde bir çözümü dağıttığınızda, yalnızca bir Azure bölgesi seçebilirsiniz bulabilirsiniz. SAP CAL tarafından önerilen farklı Azure bölgeleri dağıtmak için SAP CAL abonelik satın gerekir. Ayrıca bir ileti CAL hesabınızda Başlangıçta önerilen dışındaki Azure bölgeleri sunmak için etkin olması için SAP ile açmanız gerekebilir.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma

Şimdi bir çözüm dağıtma **çözümleri** SAP CAL sayfası. SAP CAL dağıtmak için iki sıraları sahiptir:

- Dağıtılacak sistem tanımlamak için bir sayfa kullanır bir temel sıralı
- Gelişmiş dizisi VM boyutlarını belirli seçenekler sunar 

Biz, buradaki dağıtım temel yolunu gösterir.

1. Üzerinde **hesap ayrıntıları** sayfasında gerekir:

    a. SAP CAL hesabı seçin. (Resource Manager dağıtım modeliyle dağıtmak için ilişkili olan bir hesap kullanın.)

    b. Bir örnek girmek **adı**.

    c. Bir Azure seçin **bölge**. SAP CAL bir bölge önerir. Başka bir Azure bölgesi gerekir ve bir SAP CAL aboneliğiniz yoksa, SAP CAL abonelikle sipariş gerekir.

    d. Asıl girin **parola** sekiz veya dokuz karakter çözüm için. Parola farklı bileşenleri Yöneticiler için kullanılır.

   ![SAP CAL Basic modu: Örnek oluşturma](./media/cal-s4h/s4h-pic10a.png)

2. Tıklatın **oluşturma**, görüntülenen ileti kutusunda tıklatıp **Tamam**.

   ![SAP CAL VM boyutları desteklenir](./media/cal-s4h/s4h-pic10b.png)

3. İçinde **özel anahtar** iletişim kutusu, tıklatın **depolamak** SAP CAL özel anahtarı depolamak için. Özel anahtarı parola koruması kullanmak için tıklatın **karşıdan**. 

   ![SAP CAL özel anahtar](./media/cal-s4h/s4h-pic10c.png)

4. SAP CAL okuma **uyarı** tıklayın ve ileti **Tamam**.

   ![SAP CAL uyarı](./media/cal-s4h/s4h-pic10d.png)

    Artık dağıtım gerçekleşir. Bir süre sonra boyutu ve karmaşıklığı (SAP CAL tahmini sağlar) çözümün bağlı olarak durumu etkin ve kullanım için hazır olarak gösterilir.

5. Bir kaynak grubundaki diğer ilişkilendirilmiş kaynakları olan toplanan sanal makineleri bulmak için Azure portalına gidin: 

   ![Yeni Portalı'nda dağıtılan SAP CAL nesneler](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. SAP CAL portalında olarak durumu görünür **etkin**. Çözüme bağlanmak için **Bağlan**. Farklı bileşenler bağlanmak için farklı seçenekleri, bu çözüm içinde dağıtılır.

   ![SAP CAL örnekleri](./media/cal-s4h/active_solution.png)

7. Dağıtılan sistemlere bağlanmak için seçeneklerden birini kullanmadan önce tıklatın **Başlarken Kılavuzu**. 

   ![Örneğine bağlanın](./media/cal-s4h/connect_to_solution.png)

    Belge her bağlantı yöntemleri için kullanıcıların adları. Bu kullanıcıların parolalarını dağıtım işleminin başında tanımlanan ana parola ayarlanır. Belgelerde dağıtılan sisteme oturum açmak için kullanabileceğiniz parolalarını ile diğer daha işlevsel kullanıcılar listelenir. 

    Windows Uzak Masaüstü makinede önceden yüklenmiş olarak SAP GUI kullanırsanız, örneğin, S/4 sistem şuna benzeyebilir:

   ![Önceden yüklenmiş SAP GUI SM50](./media/cal-s4h/gui_sm50.png)

    Veya DBACockpit kullanırsanız, örnek şuna benzeyebilir:

   ![DBACockpit SAP GUI SM50](./media/cal-s4h/dbacockpit.png)

Birkaç saat içinde sağlıklı bir SAP S/4 Gereci Azure'da dağıtılır.

Bir SAP CAL abonelik satın aldıysanız, SAP dağıtımlarınızı SAP CAL aracılığıyla Azure üzerinde tam olarak destekler. Destek BC VCM CAL sırasıdır.







