---
title: "Oluşturma ve bir bulut hizmeti dağıtma | Microsoft Docs"
description: "Oluşturmak ve Azure'da hızlı oluşturma yöntemini kullanarak bir bulut hizmeti dağıtma hakkında bilgi edinin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Oluşturma ve bir bulut hizmeti dağıtma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klasik Azure Portalı](cloud-services-how-to-create-deploy.md)
> 
> 

Klasik Azure portalı oluşturmak ve bir bulut hizmeti dağıtmak iki yol sunar: **hızlı Oluştur** ve **özel Oluştur**.

Bu konuda, yeni bir bulut hizmeti oluşturun ve ardından Hızlı oluşturma yönteminin kullanılacağını açıklanmaktadır **karşıya** karşıya yükleyin ve Azure bulut hizmet paketinde dağıtın. Bu yöntemi kullandığınızda, Klasik Azure portalı gittiğiniz gibi tüm gereksinimleri tamamlamak için kullanılabilir uygun bağlantılar sağlar. Oluşturduğunuzda, bulut hizmeti dağıtmak hazırsanız kullanarak aynı anda hem yapabilirsiniz **özel Oluştur**.

> [!NOTE]
> Bulut hizmeti Visual Studio Team Services (VSTS) gelen yayımlamayı düşünüyorsanız kullanmak **hızlı Oluştur**ve ardından VSTS yayımlama ayarlama **Hızlı Başlangıç** veya panoyu.
> 
> 

## <a name="concepts"></a>Kavramlar
Bir uygulamayı Azure bulut hizmeti olarak dağıtmak için üç bileşenler gereklidir:

* **Hizmet tanımı**  
  Bulut Hizmet tanım dosyası (.csdef) rol sayısı dahil olmak üzere hizmet modeli tanımlar.
* **Hizmet yapılandırması**  
  Bulut hizmet yapılandırma dosyasının (.cscfg) rol örneği sayısı dahil olmak üzere, hizmet ve bireysel rolleri bulut için yapılandırma ayarlarını sağlar.
* **Hizmet paketi**  
  Hizmet Paketi (.cspkg) uygulama kodu ve yapılandırmaları ve hizmet tanımı dosyası içerir.

Bunlar ve bir paketinin nasıl oluşturulacağı hakkında daha fazla bilgiyi [burada](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Uygulamanızı hazırlama
Bir bulut hizmeti dağıtmadan önce bulut hizmet paketi (.cspkg) uygulama kodunuz ve bir bulut hizmet yapılandırma dosyasının (.cscfg) oluşturmanız gerekir. Azure SDK'sı, bu gerekli dağıtım dosyaları hazırlamak için araçlar sağlar. SDK'dan gelen yükleyebilirsiniz [Azure indirmeleri](https://azure.microsoft.com/downloads/) sayfasında, tercih ettiğiniz uygulama kodunuz geliştirmek dili.

Bir hizmet paketi vermeden önce üç bulut hizmeti özellikleri özel yapılandırmalar gerektirir:

* Veri şifreleme için Güvenli Yuva Katmanı (SSL) kullanan bir bulut hizmet dağıtmak istiyorsanız [uygulamanızı yapılandırma](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) SSL için.
* Rol örneği Uzak Masaüstü bağlantılarını yapılandırmak istiyorsanız, [rollerini yapılandırma](cloud-services-role-enable-remote-desktop.md) Uzak Masaüstü için.
* Ayrıntılı bulut hizmetiniz için izleme yapılandırmak istiyorsanız, Azure tanılama bulut hizmeti için etkinleştirin. *Minimum izleme* rol örnekleri (sanal makineler) için ana bilgisayar işletim sistemlerden topladığı performans sayaçlarını (varsayılan düzeyi izleme) kullanır. "Ayrıntılı izleme * uygulama işlem sırasında oluşan sorunları daha yakından Analizine olanak sağlamak için rol örnekleri içinde performans verilerine göre ek ölçümleri toplar. Azure Tanılama'yı etkinleştirme konusunda bilgi edinmek için bkz: [Azure etkinleştirme tanılamada](cloud-services-dotnet-diagnostics.md).

Web rolleri veya çalışan rolleri dağıtımlarında bir bulut hizmeti oluşturmak için şunları yapmanız gerekir [hizmet paketi oluşturma](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Başlamadan önce
* Azure SDK'sını yüklemediyseniz tıklatın **Azure SDK Yükle** açmak için [Azure indirmeler sayfası](https://azure.microsoft.com/downloads/)ve ardından, tercih ettiğiniz kodunuzu geliştirmek dil için SDK'sını indirin. (Daha sonra bunu fırsatına sahip olacaksınız.)
* Tüm rol örneklerinin bir sertifika gerektiriyorsa, sertifikaları oluşturun. Bulut Hizmetleri bir .pfx dosyası özel bir anahtarla gerektirir. Yapabilecekleriniz [sertifikaları Azure'a yükleyin](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) oluşturur ve bulut hizmeti dağıtın.
* Bulut hizmeti bir benzeşim grubuna dağıtmayı planlıyorsanız, benzeşim grubu oluşturun. Bir benzeşim grubu bir bölgede aynı konuma bulut hizmetiniz ve diğer Azure hizmetleriyle dağıtmak için kullanabilirsiniz. Benzeşim grubu oluşturabilirsiniz **ağlar** alanı Azure Klasik portalı üzerinde **benzeşim grupları** sayfası.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Nasıl yapılır: Hızlı oluşturma yöntemini kullanarak bir bulut hizmeti oluştur
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **yeni**>**işlem**>**bulut hizmeti** > **Hızlı Oluştur**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. İçinde **URL**, üretim dağıtımlarında bulut hizmetinize erişmek için genel URL'de kullanılacak bir alt etki alanı adı girin. Üretim dağıtımları için URL biçimi: http://*myURL*. cloudapp.net.
3. İçinde **bölge veya benzeşim grubunda**, coğrafi bölge veya benzeşim grubu bulut hizmeti dağıtmak için seçin. Bulut hizmetiniz bir bölge içindeki diğer Azure hizmetleriyle aynı konuma dağıtmak istiyorsanız, bir benzeşim grubu seçin.
4. **Bulut Hizmeti Oluştur**’a tıklayın.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    İşlemin durumunu pencerenin altındaki ileti alanında izleyebilirsiniz.
   
    **Bulut Hizmetleri** alanı açılır ve yeni bulut hizmeti görüntülenir. Durumu Oluşturuldu olarak değiştiğinde, bulut hizmeti oluşturma başarıyla tamamlandı.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Nasıl yapılır: bir bulut hizmeti için bir sertifika karşıya yükle
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**, bulut hizmeti adına tıklayın ve ardından **Sertifikalar**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Ya da tıklatın **bir sertifika karşıya** veya **karşıya**.
3. İçinde **dosya**, kullanın **Gözat** sertifikayı (.pfx dosyası) seçin.
4. İçinde **parola**, sertifikanın özel anahtarı girin.
5. Tıklatın **Tamam** (onay işareti).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Aşağıda gösterilen karşıya yükleme iletisi alanında ilerlemesini izleyebilirsiniz. Karşıya yükleme tamamlandığında, sertifika tablosuna eklenir. İleti alanına iletiyi kapatmak için Tamam'ı tıklatın.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Nasıl yapılır: bir bulut hizmeti dağıtma
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**, bulut hizmeti adına tıklayın ve ardından **Pano**.
2. Ya da tıklatın **yeni bir üretim dağıtımı karşıya** veya **karşıya**.
3. İçinde **dağıtım etiketi**, yeni dağıtım - Örneğin, MyCloudServicev4 için bir ad girin.
4. İçinde **paket**, kullanın **Gözat** kullanmak için hizmet paketi dosyasının (.cspkg) seçin.
5. İçinde **yapılandırma**, kullanın **Gözat** kullanmak için hizmet yapılandırma dosyasının (.cscfg) seçin.
6. Bulut hizmeti herhangi bir rol ile yalnızca bir örnek içeriyorsa, seçin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** ilerlemek için dağıtımını etkinleştirmek için onay kutusunu.
   
    Her role en az iki örnek varsa azure yalnızca bulut hizmetine 99,95 yüzde erişimi Bakım ve hizmet güncelleştirmeleri sırasında garanti edebilir. Gerekirse, ek rol örnekleri üzerinde ekleyebileceğiniz **ölçek** bulut hizmeti dağıttıktan sonra sayfa. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).
7. Tıklatın **Tamam** (bulut hizmeti dağıtımı başlatmak için onay işaretine).
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    İleti alanında dağıtım durumunu izleyebilirsiniz. İleti gizlemek için Tamam'ı tıklatın.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Başarıyla tamamlandı, dağıtımı doğrulama
1. Tıklatın **Pano**.
   
    Hizmetin durumunu göstermesi gerekir **çalıştıran**.
2. Altında **Hızlı Bakış**, bulut hizmetiniz bir web tarayıcısında açmak için site URL'sini tıklatın.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate.md).

