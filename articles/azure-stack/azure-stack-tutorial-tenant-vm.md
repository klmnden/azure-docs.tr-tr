---
title: Sanal makineler Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs
description: Sanal makineler Azure yığında kullanılabilir yapma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 9329cb0dbfa24cf239b820573ef7f642cdca9103
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248168"
---
# <a name="tutorial-make-virtual-machines-available-to-your-azure-stack-users"></a>Öğretici: sanal makinelerin kullanılabilir Azure yığın kullanıcılarınızın kullanımına sunun

Azure yığın bulut yönetici olarak (bazen kiracılar adlandırılır), kullanıcılarınızın abone olabilirsiniz teklifleri oluşturabilirsiniz. Teklife abone olarak, kullanıcıların bir teklif sağlayan Azure yığın hizmetleri kullanabilir.

Bu öğretici, bir sanal makine için bir teklif oluşturun ve sonra bir kullanıcı olarak teklif test etmek için oturum aç gösterilmektedir.

Ne şunları öğreneceksiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Bir görüntü ekleme
> * Teklif test

Azure yığınında services Abonelikleri, teklifleri ve planları kullanan kullanıcılar teslim edilir. Kullanıcıların birden çok teklifleri için abone olabilirsiniz. Bir teklif bir veya daha fazla plan olabilir ve bir planı bir veya daha fazla hizmet olabilir.

![Abonelikler, teklifleri ve planları](media/azure-stack-key-features/image4.png)

Daha fazla bilgi için bkz: [anahtar özelliklerinin ve kavramlarının Azure yığınında](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Teklif oluşturma

Teklifleri satın almak veya abone olmak için kullanıcılara sağlayıcılardan bir veya daha fazla plan gruplarıdır. Bir teklif oluşturma işlemi birkaç adım vardır. İlk olarak, teklif sonra bir planı ve son olarak, kotalar oluşturmanız istenir.

1. [Oturum](azure-stack-connect-azure-stack.md) bulut yönetici ve ardından olarak portalına **yeni** > **sunar + planları** > **teklif**.

   ![Yeni teklif](media/azure-stack-tutorial-tenant-vm/image01.png)

2. İçinde **yeni teklif**, girin bir **görünen adı** ve **kaynak adı**ve ardından yeni veya varolan bir seçin **kaynak grubu**. Görünen Ad, teklifin kolay adıdır. Yalnızca bulut operatörü, kaynak adı görebilirsiniz. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Görünen ad](media/azure-stack-tutorial-tenant-vm/image02.png)

3. Seçin **temel planları**hem de **planı** bölümünde, select **Ekle** yeni bir plan için teklif eklemek için.

   ![Bir plan Ekle](media/azure-stack-tutorial-tenant-vm/image03.png)

4. İçinde **yeni Plan** bölümünde, doldurmak **görünen adı** ve **kaynak adı**. Görünen ad kullanıcıların gördüğü planın kolay addır. Yalnızca bulut operatörü, kaynak adı görebilirsiniz. Bulut operatörleri plan bir Azure Resource Manager kaynak olarak çalışmak için kullandığınız addır.

   ![Plan görünen adı](media/azure-stack-tutorial-tenant-vm/image04.png)

5. Seçin **Hizmetleri**. Hizmetler listesinden seçim **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**. Seçin **seçin** hizmetlerin plana eklemek için.

   ![Plan hizmetleri](media/azure-stack-tutorial-tenant-vm/image05.png)

6. Seçin **kotaları**ve ardından bir kota için oluşturmak istediğiniz ilk hizmeti seçin. Bir Iaas kota için aşağıdaki örnekte, işlem, ağ ve depolama hizmetleri için kotalarını yapılandırmak için bir kılavuz olarak kullanın.

   - İlk olarak bir kota için işlem hizmeti oluşturun. Ad alanı listesinde **Microsoft.Compute** ve ardından **yeni kota oluştur**.

     ![Yeni kota oluştur](media/azure-stack-tutorial-tenant-vm/image06.png)

   - İçinde **kota oluştur**, kota için bir ad girin. Değiştirme veya oluşturmakta olduğunuz kotasının gösterilen kota değerleri kabul edin. Bu örnekte, biz varsayılan ayarları kabul edin ve seçin **Tamam**.

     ![Kota adı](media/azure-stack-tutorial-tenant-vm/image07.png)

   - Çekme **Microsoft.Compute** ad alanı listesini ve oluşturduğunuz kota seçin. Bu kota işlem hizmetine bağlar.

     ![Kota seçin](media/azure-stack-tutorial-tenant-vm/image08.png)

      Ağ ve depolama hizmetleri için bu adımları yineleyin. İşiniz bittiğinde seçin **Tamam** içinde **kotaları** tüm kotalarını kaydetmek için.

7. İçinde **yeni plan**seçin **Tamam**.

8. Altında **planı**, yeni planı seçin ve ardından **seçin**.

9. İçinde **yeni teklif**seçin **oluşturma**. Teklif oluşturulduğunda bir bildirim görürsünüz.

10. Pano menüsünde seçin **sunar** ve oluşturduğunuz teklif seçin.

11. Seçin **durum değiştirme**ve ardından seçerseniz **ortak**.

    ![Genel durum](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Bir görüntü ekleme

Sanal makineler sağlamadan önce Azure yığın Market görüntü eklemeniz gerekir. Azure Marketi'nden Linux görüntüleri dahil olmak üzere seçiminizi görüntüsü ekleyebilirsiniz.

Bağlı bir senaryoda işletim ve Azure ile Azure yığın örneğinizi kaydolduysanız, sonra Windows Server 2016 VM görüntüsü Azure Marketi'nden açıklanan adımları kullanarak indirebilirsiniz [indirme Market Azure öğelerinden Azure yığınına](azure-stack-download-azure-marketplace-item.md) konu.

Market'te farklı öğeler ekleme hakkında daha fazla bilgi için bkz: [Azure yığın Market](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Teklif test

Bir teklif oluşturduğunuza göre test edebilirsiniz. Bir kullanıcı olarak oturum açın, teklif abone ve bir sanal makine Ekle.

1. **Bir teklife abone olma**

   a. Kullanıcı Portalı bir kullanıcı hesabıyla oturum açın ve seçin **bir abonelik edinmeniz** döşeme.
   - Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve şu biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure yığın Geliştirme Seti kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. İçinde **bir abonelik edinmeniz**, aboneliğinizde için bir ad girin **görünen adı** alan. Seçin **teklif**ve teklifleri birini seçin **bir teklif seçin** listesi. **Oluştur**’u seçin.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Abonelik görüntülemek için seçin **daha fazla hizmet**ve ardından **abonelikleri**. Hangi hizmetlerin abonelik parçası olan görmek için yeni bir abonelik seçin.

   >[!NOTE]
   >Teklife abone olduktan sonra hangi hizmetlerin yeni abonelik parçası olan görmek için portal yenilemek olabilir.

2. **Sanal makine sağlama**

   Kullanıcı Portalı'ndan Yeni Abonelik kullanarak bir sanal makine sağlayabilirsiniz.

   a. Kullanıcı Portalı bir kullanıcı hesabıyla oturum açın.
      - Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve şu biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure yığın Geliştirme Seti kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

   b.  Panoda seçin **yeni** > **işlem** > **Windows Server 2016 Datacenter Eval**ve ardından **Oluştur**.

   c. İçinde **Temelleri**, aşağıdaki bilgileri sağlayın:
      - Girin bir **adı**
      - Girin bir **kullanıcı adı**
      - Girin bir **parola**
      - Seçin bir **abonelik**
      - Oluşturma bir **kaynak grubu** (veya varolan bir tanesini seçin.) 
      - Seçin **Tamam** bu bilgileri kaydetmek için.

   d. İçinde **bir boyutu seçin**seçin **A1 standart**ve ardından **seçin**.  

   e. İçinde **ayarları**seçin **sanal ağ**.

   f. İçinde **Seç sanal ağ**seçin **Yeni Oluştur**.

   g. İçinde **sanal ağ oluştur**tüm Varsayılanları kabul edin ve seçin **Tamam**.

   h. Seçin **Tamam** içinde **ayarları** ağ yapılandırmasını kaydetmek için.

   ![Sanal ağ oluşturma](media/azure-stack-provision-vm/image04.png)

   i. İçinde **Özet**seçin **Tamam** sanal makine oluşturulamıyor.  

   j. Yeni bir sanal makine görmek için seçin **tüm kaynakları**. Sanal makine için arama ve Arama sonuçlarından adını seçin.

   ![Tüm kaynaklar](media/azure-stack-provision-vm/image06.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Bir görüntü ekleme
> * Teklif test

Bilgi edinmek için sonraki öğretici İlerlet nasıl yapılır:
> [!div class="nextstepaction"]
> [SQL veritabanları Azure yığın kullanıcılarınızın kullanımına sunun](azure-stack-tutorial-sql-server.md)