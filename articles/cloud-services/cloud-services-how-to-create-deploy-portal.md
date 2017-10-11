---
title: "Oluşturma ve bir bulut hizmeti dağıtma | Microsoft Docs"
description: "Oluşturma ve Azure portalını kullanarak bir bulut hizmeti dağıtma hakkında bilgi edinin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: e5ce666f1d826c7901c9fd5e7fafe6171139c3ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Oluşturma ve bir bulut hizmeti dağıtma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klasik Azure Portalı](cloud-services-how-to-create-deploy.md)
>
>

Azure Portalı'nı oluşturmak ve bir bulut hizmeti dağıtmak iki yol sunar: *hızlı Oluştur* ve *özel Oluştur*.

Bu makalede Hızlı oluşturma yönteminin yeni bir bulut hizmeti oluşturabilir ve daha sonra kullanmak için nasıl kullanılacağı açıklanmaktadır **karşıya** karşıya yükleyin ve Azure bulut hizmet paketinde dağıtın. Bu yöntemi kullandığınızda, Azure portalında gittiğiniz gibi tüm gereksinimleri tamamlamak için kullanılabilir uygun bağlantılar sağlar. Oluşturduğunuzda, bulut hizmeti dağıtmak hazırsanız özel Oluştur kullanarak aynı anda hem de yapabilirsiniz.

> [!NOTE]
> Bulut hizmeti Visual Studio Team Services (VSTS) gelen yayımlamayı düşünüyorsanız, hızlı Oluştur'u kullanın ve ardından Azure hızlı başlangıç veya panoyu VSTS yayımlama Ayarla. Daha fazla bilgi için bkz: [Azure kullanarak Visual Studio Team Services tarafından sürekli teslim][TFSTutorialForCloudService], veya Yardım için bkz: **Hızlı Başlangıç** sayfası.
>
>

## <a name="concepts"></a>Kavramlar
Üç bileşeni, Azure'daki bir bulut hizmeti olarak bir uygulamayı dağıtmak için gereklidir:

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

* Veri şifreleme için Güvenli Yuva Katmanı (SSL) kullanan bir bulut hizmet dağıtmak istiyorsanız [uygulamanızı yapılandırma](cloud-services-configure-ssl-certificate-portal.md#modify) SSL için.
* Rol örneği Uzak Masaüstü bağlantılarını yapılandırmak istiyorsanız, [rollerini yapılandırma](cloud-services-role-enable-remote-desktop-new-portal.md) Uzak Masaüstü için.
* Ayrıntılı bulut hizmetiniz için izleme yapılandırmak istiyorsanız, Azure tanılama bulut hizmeti için etkinleştirin. *Minimum izleme* rol örnekleri (sanal makineler) için ana bilgisayar işletim sistemlerden topladığı performans sayaçlarını (varsayılan düzeyi izleme) kullanır. *Ayrıntılı izleme* uygulama işlem sırasında oluşan sorunları daha yakından Analizine olanak sağlamak için rol örnekleri içinde performans verilerine göre ek ölçümleri toplar. Azure Tanılama'yı etkinleştirme konusunda bilgi edinmek için bkz: [Azure tanılama etkinleştirme](cloud-services-dotnet-diagnostics.md).

Web rolleri veya çalışan rolleri dağıtımlarında bir bulut hizmeti oluşturmak için şunları yapmanız gerekir [hizmet paketi oluşturma](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Başlamadan önce
* Azure SDK'sını yüklemediyseniz tıklatın **Azure SDK Yükle** açmak için [Azure indirmeler sayfası](https://azure.microsoft.com/downloads/)ve ardından, tercih ettiğiniz kodunuzu geliştirmek dil için SDK'sını indirin. (Daha sonra bunu fırsatına sahip olacaksınız.)
* Tüm rol örneklerinin bir sertifika gerektiriyorsa, sertifikaları oluşturun. Bulut Hizmetleri bir .pfx dosyası özel bir anahtarla gerektirir. Oluşturur ve bulut hizmeti dağıtmak için Azure sertifikaları karşıya yükleyebilirsiniz.

## <a name="create-and-deploy"></a>Oluşturma ve dağıtma
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Tıklatın **yeni > işlem**, tıklatın ve sonra aşağı kaydırarak **bulut hizmeti**.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. Yeni **bulut hizmeti** dikey penceresinde için bir değer girin **DNS adı**.
4. Yeni bir **kaynak grubu** veya varolan bir tanesini seçin.
5. Bir **Konum** seçin.
6. Tıklatın **paket**. Bu açılır **bir paketi yükleme** dikey. Gerekli alanları doldurun. Rollerinizi hiçbirini tek bir örnek içeriyorsa, olun **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** seçilir.
7. Olduğundan emin olun **Başlat dağıtım** seçilir.
8. Tıklatın **Tamam** hangi kapatılacak **bir paketi yükleme** dikey.
9. Eklemek için herhangi bir sertifika yoksa tıklatın **oluşturma**.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Bir sertifikayı karşıya yüklemek
Dağıtım paketi varsa [sertifikalar kullanmak üzere yapılandırılmış](cloud-services-configure-ssl-certificate-portal.md#modify), sertifika artık karşıya yükleyebilirsiniz.

1. Seçin **sertifikaları**ve **eklemek sertifikaları** dikey penceresinde, SSL sertifika .pfx dosyasını seçin ve ardından sağlayın **parola** sertifika için
2. Tıklatın **Attach sertifika**ve ardından **Tamam** üzerinde **eklemek sertifikaları** dikey.
3. Tıklatın **oluşturma** üzerinde **bulut hizmeti** dikey. Dağıtım ulaşıldığında **hazır** durumu, sonraki adımlara devam.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Başarıyla tamamlandı, dağıtımı doğrulama
1. Bulut hizmet örneği'ı tıklatın.

    Hizmetin durumunu göstermesi gerekir **çalıştıran**.
2. Altında **Essentials**, tıklatın **Site URL'si** bulut hizmetiniz bir web tarayıcısında açın.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate-portal.md).
