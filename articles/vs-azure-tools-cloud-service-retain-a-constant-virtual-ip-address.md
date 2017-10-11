---
title: "Bir Azure bulut hizmeti için sabit bir sanal IP adresi korumak nasıl | Microsoft Docs"
description: "Azure bulut hizmetinizin sanal IP adresi (VIP) değişmez emin olmak öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: a76bcba5ab4ca8e1a4899e4aa28f734c09af2aa9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Bir Azure bulut hizmeti için sabit bir sanal IP adresi koru
Azure üzerinde barındırılan bir bulut hizmeti güncelleştirdiğinizde, sanal IP adresi (VIP) hizmetinin değiştirmez emin olmak gerekebilir. Birçok etki alanı Yönetim Hizmetleri, etki alanı adlarını kaydetme için etki alanı adı sistemi (DNS) kullanın. VIP aynı kalırsa DNS çalışır. Kullanabileceğiniz **Yayımlama Sihirbazı** bulut hizmetinizin VIP ne zaman değiştirmez emin olmak için Azure araçlarında güncelleştirmeniz. DNS etki alanı yönetimi için bulut hizmetlerini kullanma hakkında daha fazla bilgi için bkz: [bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Bir bulut hizmeti, VIP değiştirmeden yayımlama
İlk kez, Azure'a üretim ortamı gibi belirli bir ortamda dağıttığınızda bir bulut hizmeti VIP tahsis edilir. VIP yalnızca dağıtımı açıkça silin veya dağıtım örtük olarak dağıtımı güncelleştirme işlemi tarafından silinmesi durumunda değiştirir. VIP korumak için dağıtımınızı silmemelisiniz ve Visual Studio, dağıtımınızı otomatik olarak silmez emin olmanız gerekir. 

Dağıtım ayarlarında belirttiğiniz **Yayımlama Sihirbazı**, çeşitli dağıtım seçenekleri destekler. Yeni bir dağıtımını veya artımlı veya eşzamanlı olabilir bir güncelleştirme dağıtımı belirtebilirsiniz. Her iki tür güncelleştirme dağıtımı VIP korur. Bu farklı dağıtım türleri tanımları için bkz: [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md). Ayrıca, bir hata oluşursa bir bulut hizmeti önceki dağıtımını silinip silinmediğini kontrol edebilirsiniz. Bu seçenek doğru şekilde ayarlamazsanız, VIP beklenmedik bir şekilde değiştirebilirsiniz.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Güncelleştirme, VIP değiştirmeden bir bulut hizmeti
1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın. 

2. İçinde **Çözüm Gezgini**, projeye sağ tıklayın. Kısayol menüsünden seçin **Yayımla**.

    ![Yayımla menüsü](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. İçinde **Azure uygulamasını Yayımla** iletişim kutusunda, dağıtmak istediğiniz Azure aboneliğini seçin. Gerekli ve select oturum **sonraki**.

    ![Azure uygulama oturum aç sayfasında yayımlama](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. Üzerinde **ortak ayarları** sekmesinde, doğrulayın bulut adını hizmet dağıtımı, **ortam**, **yapı yapılandırması**ve **Hizmet yapılandırmasını** doğru olduğunu.

    ![Azure uygulama ortak ayarlar sekmesinde yayımlama](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. Üzerinde **Gelişmiş ayarları** sekmesinde şunları doğrulayın **dağıtım etiketi** ve **depolama hesabı** doğrudur. Doğrulayın **hata durumunda dağıtımı Sil** onay kutusu işaretli değildir ve doğrulayın **dağıtım güncelleştirme** onay kutusu seçilidir. Temizleme tarafından **hata durumunda dağıtımı Sil** onay kutusunu olun dağıtımı sırasında bir hata meydana gelirse, VIP kayıp değil. Seçerek **dağıtım güncelleştirme** onay kutusu, dağıtımınızı silinmez ve uygulamanızı yayımladığınızda, VIP kaybı olmadığından emin olun. 

    ![Azure uygulama Gelişmiş Ayarlar sekmesi yayımlama](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. Daha fazla nasıl güncelleştirilmesi rolleri istediğinizi belirtmek için seçin **ayarları** yanına **dağıtım güncelleştirme**. Şunlardan birini seçin **Artımlı güncelleştirme** veya **eşzamanlı güncelleştirme**seçip **Tamam**. Seçin **Artımlı güncelleştirme** uygulama her zaman kullanılabilir olmasını sağlamak, uygulamanızın her örneği birbiri ardından güncelleştirilecek. Seçin **eşzamanlı güncelleştirme** uygulamanız aynı anda tüm örneklerini güncelleştirmek için. Eşzamanlı güncelleştirme hızlıdır ancak hizmetiniz güncelleştirme işlemi sırasında kullanılamayabilir. İşiniz bittiğinde, seçin **sonraki**.

    ![Azure uygulama dağıtım ayarları sayfasında yayımlama](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. İçinde **Azure uygulamasını Yayımla** iletişim kutusunda **sonraki** kadar **Özet** sayfası görüntülenir. Lütfen ayarlarınızı doğrulayın ve ardından **Yayımla**.
   
    ![Azure uygulama Özet sayfası yayımlama](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Visual Studio kullanarak yayımla Azure Uygulama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md)

