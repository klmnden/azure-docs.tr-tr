---
title: "Sanal makineler Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs"
description: "Sanal makineler Azure yığında kullanılabilir hale getirmek Öğreticisi"
services: azure-stack
documentationcenter: 
author: vhorne
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f6fce4a3230c98295afb19e633bf2801c115831f
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="make-virtual-machines-available-to-your-azure-stack-users"></a>Sanal makineler Azure yığın kullanıcılarınızın kullanımına sunun

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın bulut yönetici olarak (bazen kiracılar adlandırılır), kullanıcılarınızın abone olabilirsiniz teklifleri oluşturabilirsiniz. Aboneliğini kullanarak, kullanıcılar sonra Azure yığın hizmetleri kullanmasını sağlayabilirsiniz.

Bu makalede bir teklifi oluşturmak ve ardından test gösterilmektedir. Test için portalı bir kullanıcı olarak oturum açın, teklif abone ve aboneliği kullanarak bir sanal makine oluşturun.

Ne şunları öğreneceksiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Bir görüntü ekleme
> * Teklif test


Azure yığınında services Abonelikleri, teklifleri ve planları kullanan kullanıcılar teslim edilir. Kullanıcıların birden çok teklifleri için abone olabilirsiniz. Bir veya daha fazla plan teklifleri olabilir ve bir veya daha fazla hizmet planları olabilir.

![Abonelikler, teklifleri ve planları](media/azure-stack-key-features/image4.png)

Daha fazla bilgi için bkz: [anahtar özelliklerinin ve kavramlarının Azure yığınında](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Teklif oluşturma

Şimdi, noktalar, kullanıcılarınızın hazır alabilirsiniz. İşlem başlattığınızda, ilk teklif sonra bir planı ve son olarak kotaları oluşturmanız istenir.

3. **Teklif oluşturma**

   Teklifleri satın almak veya abone olmak için kullanıcılara sağlayıcılardan bir veya daha fazla plan gruplarıdır.

   a. [Oturum](azure-stack-connect-azure-stack.md) bulut yönetici ve ardından olarak portalına **yeni** > **sunar + planları** > **teklif**.
   ![Yeni Teklif](media/azure-stack-tutorial-tenant-vm/image01.png)

   b. İçinde **yeni teklif** bölümünde, doldurmak **görünen adı** ve **kaynak adı**ve ardından yeni veya varolan bir seçin **kaynak grubu**. Görünen Ad, teklifin kolay adıdır. Yalnızca bulut operatörü, kaynak adı görebilirsiniz. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Görünen ad](media/azure-stack-tutorial-tenant-vm/image02.png)

   c. Tıklatın **temel planları**hem de **planı** 'yi tıklatın **Ekle** yeni bir plan için teklif eklemek için.

   ![Bir plan Ekle](media/azure-stack-tutorial-tenant-vm/image03.png)

   d. İçinde **yeni Plan** bölümünde, doldurmak **görünen adı** ve **kaynak adı**. Görünen ad kullanıcıların gördüğü planın kolay addır. Yalnızca bulut operatörü, kaynak adı görebilirsiniz. Bulut operatörleri plan bir Azure Resource Manager kaynak olarak çalışmak için kullandığınız addır.

   ![Plan görünen adı](media/azure-stack-tutorial-tenant-vm/image04.png)

   e. Tıklatın **Hizmetleri**seçin **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**ve ardından **Seçin**.

   ![Plan Hizmetleri](media/azure-stack-tutorial-tenant-vm/image05.png)

   f. Tıklatın **kotaları**ve ardından bir kota oluşturmak istediğiniz ilk hizmeti seçin. Bir Iaas kota için işlem, ağ ve depolama hizmetleri için şu adımları izleyin.

   Bu örnekte, ilk işlem hizmeti için bir kota oluşturuyoruz. Ad alanı listesinde **Microsoft.Compute** ad alanı ve ardından **yeni kota oluştur**.
   
   ![Yeni kota oluştur](media/azure-stack-tutorial-tenant-vm/image06.png)

   g. Üzerinde **kota oluştur** bölümünde, kota için bir ad yazın ve'ı tıklatın ve kota için istediğiniz parametreleri ayarlamak **Tamam**.

   ![Kota adı](media/azure-stack-tutorial-tenant-vm/image07.png)

   h. Şimdi, için **Microsoft.Compute**, oluşturduğunuz kota seçin.

   ![Kota seçin](media/azure-stack-tutorial-tenant-vm/image08.png)

   Ağ ve depolama hizmetleri için bu adımları yineleyin ve ardından **Tamam** üzerinde **kotaları** bölümü.

   ı. Tıklatın **Tamam** üzerinde **yeni plan** bölümü.

   j. Üzerinde **planı** bölümünde, yeni planı seçin ve tıklatın **seçin**.

   k. Üzerinde **yeni teklif** 'yi tıklatın **oluşturma**. Teklif oluşturduğunuzda, bir bildirim görürsünüz.

   l. Pano menüsünde **sunar** ve oluşturduğunuz teklif'ı tıklatın.

   m. **Change State** (Durumu Değiştir) öğesine ve **Public** (Genel) seçeneğine tıklayın.

   ![Genel durum](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Bir görüntü ekleme

Sanal makineler sağlamadan önce Azure yığın Market görüntü eklemeniz gerekir. Azure Marketi'nden Linux görüntüleri dahil olmak üzere seçiminizi görüntüsü ekleyebilirsiniz.

Bağlı bir senaryoda işletim ve Azure ile Azure yığın örneğinizi kaydolduysanız, sonra Windows Server 2016 VM görüntüsü Azure Marketi'nden açıklanan adımları kullanarak indirebilirsiniz [indirme Market Azure öğelerinden Azure yığınına](azure-stack-download-azure-marketplace-item.md) konu.

Market'te farklı öğeler ekleme hakkında daha fazla bilgi için bkz: [Azure yığın Market](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Teklif test

Bir teklif oluşturduğunuza göre test edebilirsiniz. Bir kullanıcı olarak oturum açın ve teklif abone ve bir sanal makine ekleyin.

1. **Bir teklife abone olma**

   Artık, portala teklife abone olmak için bir kullanıcı olarak oturum açabilirsiniz.

   a. Oturum açma Kullanıcı Portalı'na bir kullanıcı ve tıklatın **bir abonelik edinmeniz**.
   - Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve biçim https://portal olacaktır. &lt; *bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure yığın Geliştirme Seti kullanıyorsanız, portalı https://portal.local.azurestack.external adresidir.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. İçinde **görünen adı** alan, aboneliğiniz için bir ad yazın, tıklatın **teklif**, teklifleri birini tıklatın **bir teklif seçin** bölümünde ve ardından  **Oluşturma**.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Oluşturduğunuz abonelik görüntülemek için **daha fazla hizmet**, tıklatın **abonelikleri**, yeni aboneliğinizi'ye tıklayın.  

   Teklife abone olduktan sonra hangi hizmetlerin yeni abonelik parçası olan görmek için portal yenileyin.

2. **Sanal makine sağlama**

   Şimdi portalına abonelik kullanarak bir sanal makine sağlamak için bir kullanıcı olarak oturum. 

   a. Kullanıcı Portalı'na bir kullanıcı olarak oturum açın.
      - Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve biçim https://portal olacaktır. &lt; *bölge*&gt;.&lt; *FQDN*&gt;.
   - Azure yığın Geliştirme Seti kullanıyorsanız, portalı https://portal.local.azurestack.external adresidir.

   b.  Panoda tıklatın **yeni** > **işlem** > **Windows Server 2016 Datacenter Eval**ve ardından **Oluştur**.

   c. İçinde **Temelleri** bölümüne şunu yazın bir **adı**, **kullanıcı adı**, ve **parola**, seçin bir **abonelik**, oluşturma bir **kaynak grubu** (veya varolan bir tanesini seçin) ve ardından **Tamam**.

   d. İçinde **bir boyutu seçin** 'yi tıklatın **A1 standart**ve ardından **seçin**.  

   e. İçinde **ayarları** 'yi tıklatın **sanal ağ**. İçinde **Seç sanal ağ** 'yi tıklatın **Yeni Oluştur**. İçinde **sanal ağ oluştur** bölümünde, tüm Varsayılanları kabul edin ve tıklatın **Tamam**. İçinde **ayarları** 'yi tıklatın **Tamam**.

   ![Sanal ağ oluşturma](media/azure-stack-provision-vm/image04.png)

   f. İçinde **Özet** 'yi tıklatın **Tamam** sanal makine oluşturulamıyor.  

   g. Yeni bir sanal makine görmek için tıklatın **tüm kaynakları**, sanal makine için arama yapın ve adına tıklayın.

    ![Tüm kaynaklar](media/azure-stack-provision-vm/image06.png)

Bu öğreticide öğrendiklerinizi:

> [!div class="checklist"]
> * Teklif oluşturma
> * Bir görüntü ekleme
> * Teklif test

> [!div class="nextstepaction"]
> [Web, mobil ve API uygulamaları Azure yığın kullanıcılarınızın kullanımına sunun](azure-stack-tutorial-app-service.md)