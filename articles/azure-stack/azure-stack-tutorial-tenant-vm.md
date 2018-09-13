---
title: Sanal makineler, Azure Stack kullanıcılarına kullandırmak | Microsoft Docs
description: Azure Stack üzerinde sanal makineleri kullanılabilir hale getirmeyi öğrenin
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
ms.date: 09/11/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: d106d9f79498678f08142f952e09c5125c6e5d6c
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721526"
---
# <a name="tutorial-make-virtual-machines-available-to-your-azure-stack-users"></a>Öğretici: sanal makineler, Azure Stack kullanıcılar için kullanılabilir yap

Azure Stack bulut yönetici olarak (bazen kiracıları olarak adlandırılır), kullanıcıların abone olabileceği teklifleri oluşturabilirsiniz. Bir teklife abone olarak, kullanıcılar bir teklif sunduğu Azure Stack hizmetleri kullanabilir.

Bu öğreticide, bir sanal makine için bir teklif oluşturun ve test teklifini için daha sonra bir kullanıcı olarak oturum gösterilmektedir.

Ne öğreneceksiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Resim ekleme
> * Test teklifini

Azure Stack'te hizmetler abonelikler, teklifleri ve planları kullanan kullanıcılara dağıtılır. Kullanıcılar için birden çok teklife abone olabilirsiniz. Bir veya daha fazla plan Teklife sahip olabilir ve bir veya daha fazla hizmet bir plan olabilir.

![Abonelikler, teklifler ve planlar](media/azure-stack-key-features/image4.png)

Daha fazla bilgi için bkz. [anahtar özellikler ve kavramlar Azure Stack'te](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Teklif oluşturma

Teklifler sağlayıcıları satın alan veya abone olmak için kullanıcılara sunmak bir veya daha fazla plan gruplarıdır. Teklif oluşturma işlemi birkaç adım vardır. İlk olarak, teklif, sonra bir plan ve son olarak, kotalar oluşturmanız istenir.

1. [Oturum](azure-stack-connect-azure-stack.md) portalda bir bulut Yöneticisi ve ardından olarak **+ kaynak Oluştur** > **sunar + planlar** > **teklif**.

   ![Yeni teklif](media/azure-stack-tutorial-tenant-vm/image01.png)

1. İçinde **yeni teklif**, girin bir **görünen ad** ve **kaynak adı**ve ardından yeni veya mevcut bir seçin **kaynak grubu**. Görünen ad teklifin kolay adıdır. Yalnızca bulut işleci yöneticileri teklif bir Azure Resource Manager kaynağı olarak çalışmak için kullandığınız adı kaynak adını görebilirsiniz.

   ![Görünen ad](media/azure-stack-tutorial-tenant-vm/image02.png)

1. Seçin **temel planlar**hem de **planı** bölümünden **Ekle** yeni bir plan teklife eklenecek.

   ![Bir plan Ekle](media/azure-stack-tutorial-tenant-vm/image03.png)

1. İçinde **yeni plan** bölümünde, doldurun **görünen ad** ve **kaynak adı**. Görünen ad kullanıcıların göreceği planın kolay addır. Yalnızca bulut operatörü, bulut operatörlerinin bir Azure Resource Manager kaynağı olarak planla çalışmak için kullandığınız adı olan kaynak adı görebilirsiniz.

   ![Plan görünen adı](media/azure-stack-tutorial-tenant-vm/image04.png)

1. Seçin **Hizmetleri**. Hizmetler listesinden seçim **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**. Seçin **seçin** hizmetlerin plana eklenecek.

   ![Plan hizmetleri](media/azure-stack-tutorial-tenant-vm/image05.png)

1. Seçin **kotalar**ve ardından bir kota için oluşturmak istediğiniz ilk hizmeti seçin. Bir Iaas kota için işlem, ağ ve depolama hizmetleri için kotalar yapılandırmak için kılavuz olarak aşağıdaki örneği kullanın.

   - İlk olarak bir kota için işlem hizmeti oluşturun. Ad alanı listesinde **Microsoft.Compute** seçip **yeni kota oluştur**.

     ![Yeni kota oluştur](media/azure-stack-tutorial-tenant-vm/image06.png)

   - İçinde **kota oluştur**, kota için bir ad girin. Değiştirin veya herhangi bir kota oluşturmakta olduğunuz için gösterilen kota değeri kabul edin. Bu örnekte, biz varsayılan ayarları kabul edin ve seçin **Tamam**.

     ![Kota adı](media/azure-stack-tutorial-tenant-vm/image07.png)

   - Çekme **Microsoft.Compute** ad alanı listesini ve oluşturduğunuz kota seçin. Bu kota işlem hizmetine bağlar.

     ![Kota seçin](media/azure-stack-tutorial-tenant-vm/image08.png)

      Ağ ve depolama hizmetleri için bu adımları yineleyin. İşlemi tamamladığınızda, seçin **Tamam** içinde **kotalar** kotalarını kaydetmek için.

1. İçinde **yeni plan**seçin **Tamam**.

1. Altında **planı**, yeni bir plan seçin ve ardından **seçin**.

1. İçinde **yeni teklif**seçin **Oluştur**. Teklif oluşturulduğunda bir bildirim görürsünüz.

1. Pano menüsünde **sunar** ve oluşturduğunuz teklif'ı seçin.

1. Seçin **durumunu değiştir**ve ardından seçtiğiniz **genel**.

    ![Genel durum](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Resim ekleme

Sanal makineler sağlamak önce bir görüntü için Azure Stack marketini eklemeniz gerekir. Azure Market'teki Linux görüntüleri dahil olmak üzere tercih ettiğiniz görüntüsü ekleyebilirsiniz.

Bağlı bir senaryoda çalışıyor ve Azure ile Azure Stack örneğinizin kayıtlıysanız, sonra Windows Server 2016 VM görüntüsü Azure Market'te açıklanan adımları kullanarak indirebilirsiniz [indirme Market Azure Stack azure'dan öğelerine](azure-stack-download-azure-marketplace-item.md) konu.

Market'te farklı öğeleri eklemek hakkında daha fazla bilgi için bkz: [Azure Stack Marketini](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Test teklifini

Teklif oluşturduğunuza göre bunu test edebilirsiniz. Bir kullanıcı olarak oturum açın, teklife abone olun ve sonra bir sanal makine ekleyin.

1. **Bir teklife abone olma**

   a. Bir kullanıcı hesabı ile kullanıcı portalında oturum açın ve seçin **bir abonelik edinmeniz** Döşe.
   - Tümleşik bir sistem için URL, işlecin bölge ve dış etki alanı adına göre değişir ve biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure Stack geliştirme Seti'ni kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

   ![Abonelik edinin](media/azure-stack-tutorial-tenant-vm/image10.png)

   b. İçinde **bir abonelik edinmeniz**, aboneliğinizde için bir ad girin **görünen ad** alan. Seçin **teklif**, bir teklif seçin **bir teklif seçin** listesi. **Oluştur**’u seçin.

   ![Teklif oluşturma](media/azure-stack-tutorial-tenant-vm/image11.png)

   c. Aboneliği görüntülemek için seçin **tüm hizmetleri**ve ardından altındaki **genel** kategorisi seçin **abonelikleri**. Hangi hizmetlerin abonelik parçası olduğunu görmek için yeni aboneliğinizi seçin.

   >[!NOTE]
   >Bir teklife abone olduktan sonra hangi hizmetlerin yeni aboneliğinin bir parçası olduğunu görmek için portalı yenilemeniz gerekebilir.

1. **Sanal makine sağlama**

   Kullanıcı Portalı'ndan yeni aboneliği kullanarak bir sanal makine sağlayabilirsiniz.

   a. Kullanıcı Portalı bir kullanıcı hesabıyla oturum açın.
      - Tümleşik bir sistem için URL, işlecin bölge ve dış etki alanı adına göre değişir ve biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure Stack geliştirme Seti'ni kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

   b.  Panoda seçin **+ kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter değerlendirme**seçip **Oluşturma**.

   c. İçinde **Temelleri**, aşağıdaki bilgileri sağlayın:
      - Girin bir **adı**
      - Girin bir **kullanıcı adı**
      - Girin bir **parola**
      - Seçin bir **abonelik**
      - Oluşturma bir **kaynak grubu** (veya varolan bir tanesini seçin.) 
      - Seçin **Tamam** bu bilgileri kaydedin.

   d. İçinde **bir boyut seçin**seçin **standart A1**, ardından **seçin**.  

   e. İçinde **ayarları**seçin **sanal ağ**.

   f. İçinde **sanal ağ Seç**seçin **Yeni Oluştur**.

   g. İçinde **sanal ağ oluştur**tüm Varsayılanları kabul edin ve seçin **Tamam**.

   h. Seçin **Tamam** içinde **ayarları** ağ yapılandırmasını kaydetmek için.

      i. İçinde **özeti**seçin **Tamam** sanal makine oluşturmak için.  

   j. Yeni bir sanal makine görmek için seçin **tüm kaynakları**. Sanal makine için arama yapın ve Arama sonuçlarından adını seçin.

   
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Resim ekleme
> * Test teklifini

Bilgi edinmek için sonraki öğreticiye ilerleyin nasıl yapılır:
> [!div class="nextstepaction"]
> [SQL veritabanları, Azure Stack kullanıcıları için kullanılabilir yap](azure-stack-tutorial-sql-server.md)