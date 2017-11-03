---
title: "Bulut hizmetinin (Klasik portalı) nasıl yapılandırılacağı | Microsoft Docs"
description: "Bulut Hizmetleri ile Azure yapılandırmayı öğrenin. Bulut hizmeti yapılandırmasını güncelleştirin ve rol örnekleri için uzaktan erişim yapılandırma hakkında bilgi edinme."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-cloud-services"></a>Bulut Hizmetleri Yapılandırma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-how-to-configure-portal.md)
> * [Klasik Azure Portalı](cloud-services-how-to-configure.md)
> 
> 

Klasik Azure portalında bir bulut hizmeti için en yaygın olarak kullanılan ayarları yapılandırabilirsiniz. Ya da yapılandırma dosyalarınızı doğrudan güncelleştirmek isterseniz güncelleştirilecek hizmet yapılandırma dosyasını indirin ve ardından güncelleştirilmiş dosyayı yükleyin ve bulut hizmetini yapılandırma değişiklikleriyle güncelleştirin. Her iki şekilde de yapılandırma güncelleştirmeleri tüm rol örneklerine atılır.

Klasik Azure portalı de olanak tanır [bir rolde Azure bulut Hizmetleri için Uzak Masaüstü Bağlantısı etkinleştir](cloud-services-role-enable-remote-desktop.md)

Her rol için en az iki rol örneği varsa azure yapılandırma güncelleştirmeleri sırasında yalnızca 99,95 yüzde hizmet kullanılabilirlik sağlayabilirsiniz. Diğer güncelleştirilirken istemci isteklerini işlemek bir sanal makine sağlar. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Bir bulut hizmeti değiştirme
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**, bulut hizmeti adına tıklayın ve ardından **yapılandırma**.
   
    ![Yapılandırma sayfası](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    Üzerinde **yapılandırma** sayfasında, izleme, güncelleştirme rol ayarlarını yapılandırmak ve konuk işletim sistemi ve rol örnekleri için ailesi seçin. 
2. İçinde **izleme**ayrıntılı veya minimum izleme düzeyini ayarlayın ve ayrıntılı izleme için gerekli olan tanılama bağlantı dizelerini yapılandırın.
3. (Role göre gruplandırılmış) hizmeti roller için aşağıdaki ayarları güncelleştirebilirsiniz:
   
    * **Ayarları** belirtilen çeşitli yapılandırma ayarlarının değerleri değiştirmek *ConfigurationSettings* öğeleri hizmetin yapılandırma (.cscfg) dosyası.

    * **Sertifikalar**  
        SSL şifreleme bir rol için kullanılan sertifika parmak izi değiştirin. Bir sertifika değiştirmek için önce yeni sertifikayı yüklemeniz gerekir (üzerinde **sertifikaları** sayfası). Ardından rol ayarlarında görüntülendiği sertifika dizesinde parmak izi güncelleştirin.
4. İçinde **işletim sistemi**, işletim sistemi ailesi veya rol örnekleri için sürüm değiştirme veya seçme **otomatik** geçerli işletim sistemi sürümü otomatik güncelleştirmeleri etkinleştirmek için. İşletim sistemi ayarlarını, web rolleri ve çalışan rolleri için geçerlidir, ancak sanal makineleri etkilemez.
   
    Dağıtım işlemi sırasında tüm rol örneklerinde en son işletim sistemi sürümü yüklenir ve işletim sistemleri, varsayılan olarak otomatik olarak güncelleştirilir. 
   
    Bulut hizmetinizin kodunuzda uyumluluk gereksinimleri nedeniyle farklı bir işletim sistemi sürümü üzerinde çalıştırmak ihtiyacınız varsa, bir işletim sistemi ailesi ve sürümü seçebilirsiniz. Belirli işletim sistemi sürümü seçtiğinizde, bulut hizmeti için otomatik işletim sistemi güncelleştirmelerini askıya alınır. İşletim sistemi güncelleştirmeleri aldığından emin olmak gerekir.
   
    Uygulamalarınızı en son işletim sistemi sürümü ile yüklü tüm uyumluluk sorunları gidermek, işletim sistemi sürümü ayarlayarak otomatik işletim sistemi güncelleştirmeleri etkinleştirebilir **otomatik**. 
   
    ![İşletim sistemi ayarları](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. Yapılandırma ayarlarınızı kaydetmek ve bunları rol örneklerine gönderme için tıklatın **kaydetmek**. (Tıklatın **atmak** değişiklikleri iptal etmek için.) **Kaydet** ve **atmak** bir ayarı değiştirdikten sonra Komut çubuğuna eklenir.

## <a name="update-a-cloud-service-configuration-file"></a>Bir bulut hizmeti yapılandırma dosyasını güncelleştir
1. Bir bulut hizmeti yapılandırma dosyasının (.cscfg) geçerli yapılandırmasıyla indirin. Üzerinde **yapılandırma** sayfasında bulut hizmeti için **karşıdan**. Ardından **kaydetmek**, veya **Kaydet** dosyayı kaydetmek için.
2. Hizmet yapılandırma dosyası güncelleştirdikten sonra karşıya yükleme ve yapılandırma güncelleştirmeleri uygulayın:
   
   1. Üzerinde **yapılandırma** sayfasında, **karşıya**.
      
       ![Yapılandırma karşıya yükle](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. İçinde **yapılandırma dosyası**, kullanın **Gözat** güncelleştirilmiş .cscfg dosyasını seçin.
   3. Bulut hizmetinizi yalnızca bir örneğine sahip herhangi bir rol içeriyorsa seçin **bir veya daha fazla rol tek bir örnek içeriyor olsa bile yapılandırma uygulamak** devam etmek rolleri için yapılandırma güncelleştirmeleri etkinleştirmek için onay kutusunu.
      
       Her rolün en az iki örneğinin tanımlamadığınız sürece Azure en az 99,95 yüzde garanti edemez, bulut hizmeti kullanılabilirlik hizmet yapılandırma güncelleştirmeleri sırasında. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).
   4. Tıklatın **Tamam** (onay işareti). 

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage.md).
* [Azure bulut Hizmetleri rolünde için Uzak Masaüstü Bağlantısı etkinleştir](cloud-services-role-enable-remote-desktop.md)
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate.md).

