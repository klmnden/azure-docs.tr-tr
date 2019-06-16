---
title: include dosyası
description: include dosyası
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 4ba5c8b69776b39d8a6640744b0c24600f3a0d5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157813"
---
#### <a name="to-create-a-new-service"></a>Yeni hizmet oluşturmak için

1.  Microsoft hesabı kimlik bilgilerinizi kullanarak, Azure portalında şu URL için oturum açın: <https://portal.azure.com/>. Kamu Portalı'nda cihaz dağıtıyorsanız, oturum açın: <https://portal.azure.us/>

2.  Azure portalında **+ kaynak Oluştur** &gt; **depolama** &gt; **StorSimple sanal seri**.

    ![Yeni bir hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  İçinde **StorSimple cihaz Yöneticisi** , açılan dikey penceresinde aşağıdakileri yapın:

    1.  Hizmetiniz için benzersiz bir **Kaynak adı** sağlayın. Kaynak adı hizmetinizi tanımlamak için kullanılan kolay bir addır. Ad harf, rakam ve tirelerden oluşan 2-50 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

    2.  Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Bu alan bir aboneliğiniz olmadığı sürece yoktur.

    3.  İçin **kaynak grubu**, var olan bir'ı seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Hizmetiniz için bir **Konum** sağlayın. Bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services) hangi hizmetler kullanılabilir hangi bölgede daha fazla bilgi için. Genel olarak, seçmek bir **konumu** , Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın. Aşağıdakilerin de etkili olmasını isteyebilirsiniz:

        -   Var olan iş yükleri de dağıtmak StorSimple cihazınızla istediğiniz Azure varsa, o veri merkezini kullanmanızı öneririz.

        -   StorSimple cihaz Yöneticisi'ni ve Azure depolama, iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir. Bir Azure depolama hesabı oluşturmak için Azure portalında Azure depolama birimine gidin ve açıklanan adımları [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). Bu hesabı oluşturduktan sonra, [Hizmet için yeni bir depolama hesabı yapılandırma](https://azure.microsoft.com/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service) konusundaki adımları uygulayarak bunu StorSimple Cihaz Yöneticisi hizmetine ekleyin.

        -   Kamu Portalı'nda sanal cihazını dağıtma, StorSimple cihaz Yöneticisi hizmeti ABD Iowa ve ABD Virginia konumlarda kullanılabilir.

    5.  Seçin **yeni bir Azure depolama hesabı oluşturma** otomatik olarak hizmeti ile bir depolama hesabı oluşturmak için. Belirtin bir **depolama hesabı adı**. Verilerinizin farklı bir konumda olması gerekiyorsa bu kutunun işaretini kaldırın.

    6.  Panonuzda bu hizmetin hızlı bağlantısının olmasını istiyorsanız **Panoya sabitle** seçeneğini işaretleyin.

    7.  StorSimple Cihaz Yöneticisi’ni oluşturmak için **Oluştur**’a tıklayın.

        ![Yeni bir hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Yönlendirilirsiniz **hizmet** giriş sayfası. Hizmetin oluşturulması birkaç dakika sürer. Hizmet sorunsuz oluşturulduktan sonra, uygun şekilde size bildirilir ve hizmetin durumu **Etkin** olarak değişir.


