---
title: Nasıl bir bulut hizmeti oluşturma ve dağıtma | Microsoft Docs
description: Azure portalını kullanarak bir bulut hizmeti oluşturma ve dağıtma hakkında bilgi edinin.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeconnoc
ms.openlocfilehash: a6cf2276da463f71f008c4bfb6eee4c232b18308
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61433784"
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Nasıl bir bulut hizmeti oluşturma ve dağıtma
Azure portalında bir bulut hizmeti oluşturma ve dağıtma iki yol sağlar: *Hızlı oluşturma* ve *özel Oluştur*.

Bu makalede yeni bir bulut hizmeti oluşturma ve sorgulama hızlı oluşturma yöntemini kullanmayı açıklar **karşıya** karşıya yükleme ve Azure'a bir bulut hizmeti paketi dağıtma. Bu yöntemi kullandığınızda, Azure portalında kullandıkça için tüm gereksinimi tamamlamada kullanılabilir bağlantılar sağlar. Oluşturduğunuzda bulut hizmetinizin dağıtmak hazırsanız özel Oluştur kullanarak aynı anda hem de yapabilirsiniz.

> [!NOTE]
> Bulut hizmetinizdeki Azure DevOps yayımlamayı düşünüyorsanız, hızlı Oluştur özelliğini kullanın ve ardından Azure DevOps yayımlama Azure hızlı başlangıç ya da Pano Ayarla. Daha fazla bilgi için bkz. [kullanarak Azure DevOps ile azure'a sürekli teslimi][TFSTutorialForCloudService], veya Yardım **Hızlı Başlangıç** sayfası.
>
>

## <a name="concepts"></a>Kavramlar
Bir uygulamayı Azure'daki bir bulut hizmeti olarak dağıtmanın üç bileşenler gereklidir:

* **Hizmet tanımı**  
  Bulut Hizmet tanım dosyası (.csdef) rol sayısı dahil olmak üzere hizmet modeli, tanımlar.
* **Hizmet yapılandırması**  
  Bulut hizmeti yapılandırma dosyası (.cscfg), hizmet ve bireysel rolleri, rol örneği sayısı dahil olmak üzere bulut için yapılandırma ayarlarını sağlar.
* **Hizmet paketi**  
  Hizmet Paketi (.cspkg), uygulama kodu ve yapılandırmaları ve Hizmet tanım dosyası içerir.

Bunlar ve bir paket oluşturma hakkında daha fazla bilgi [burada](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Uygulamanızı hazırlama
Bir bulut hizmeti dağıtmadan önce uygulama kodunuzu ve bir bulut hizmeti yapılandırma dosyası (.cscfg) bulut hizmeti paketi (.cspkg) oluşturmanız gerekir. Bu gerekli dağıtım dosyaları hazırlamak için Azure SDK araçları sağlar. SDK'sından yükleyebileceğiniz [Azure indirmeleri](https://azure.microsoft.com/downloads/) dilinde, tercih ettiğiniz uygulama kodunuzu geliştirmek sayfa.

Hizmet paketi dışarı aktarmadan önce üç bulut hizmeti özellikleri özel yapılandırmaları gerektirir:

* Veri şifreleme için Güvenli Yuva Katmanı (SSL) kullanan bir bulut hizmet dağıtmak istiyorsanız [uygulamanızı yapılandırma](cloud-services-configure-ssl-certificate-portal.md#modify) SSL için.
* Uzak Masaüstü bağlantılarında rol örnekleri için yapılandırmak istiyorsanız [rollerini yapılandırma](cloud-services-role-enable-remote-desktop-new-portal.md) Uzak Masaüstü için.
* Bulut hizmetiniz için izleme ayrıntılı yapılandırmak istiyorsanız, bulut hizmeti için Azure Tanılama'yı etkinleştirin. *Minimum izleme* (varsayılan düzeyi izleme) rol örneklerini (sanal makineler) için ana bilgisayar işletim sistemlerinden toplanan performans sayaçlarını kullanır. *Ayrıntılı izleme* uygulama işleme süresince ortaya çıkan sorunların daha yakından Analizine olanak sağlamak için rol örneklerini içinde performans verileri temel alan ek ölçümleri bir araya getirir. Azure tanılamayı etkinleştirme konusunda bilgi edinmek için bkz: [azure'da tanılamayı etkinleştirme](cloud-services-dotnet-diagnostics.md).

Web rolleri veya çalışan rolleri dağıtımları ile bir bulut hizmeti oluşturmak için şunları yapmanız gerekir [hizmet paketi oluşturma](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Başlamadan önce
* Azure SDK'sı yüklemediyseniz tıklayın **Azure SDK'sını Yükle** açmak için [Azure indirmeler sayfasına](https://azure.microsoft.com/downloads/)ve ardından, tercih ettiğiniz kodunuzu geliştirmek dil için SDK'sını indirin. (Bunu daha sonra yapmak için bir fırsat gerekir.)
* Tüm rol örneklerinin bir sertifika gerektiriyorsa, sertifikaları oluşturun. Bulut Hizmetleri, özel anahtarı içeren bir .pfx dosyası gerektirir. Oluşturur ve bulut hizmeti dağıtmak için Azure sertifikaları karşıya yükleyebilirsiniz.

## <a name="create-and-deploy"></a>Oluşturma ve dağıtma
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Tıklayın **kaynak Oluştur > işlem**, tıklatın ve ardından aşağı kaydırarak **bulut hizmeti**.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. Yeni **bulut hizmeti** bölmesi için bir değer girin **DNS adı**.
4. Yeni bir **kaynak grubu** veya var olan bir hesabı seçin.
5. Bir **Konum** seçin.
6. Tıklayın **paket**. Bu açılır **bir paketi karşıya** bölmesi. Gerekli alanları doldurun. Herhangi bir rol tek bir örnek içeriyorsa, olun **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** seçilir.
7. Emin olun **Başlat dağıtım** seçilir.
8. Tıklayın **Tamam** hangi kapatılır **bir paketi karşıya** bölmesi.
9. Eklemek için herhangi bir sertifika yoksa tıklayın **Oluştur**.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Sertifikayı karşıya yükleyin
Dağıtım paketinizi olduysa [sertifikaları kullanacak şekilde yapılandırılmış](cloud-services-configure-ssl-certificate-portal.md#modify), artık sertifikayı karşıya yükleyebilirsiniz.

1. Seçin **sertifikaları**ve **sertifika ekleyebilirsiniz** bölmesinde SSL sertifika .pfx dosyasını seçin ve ardından sağlayın **parola** sertifika
2. Tıklayın **iliştirme sertifika**ve ardından **Tamam** üzerinde **sertifika ekleyebilirsiniz** bölmesi.
3. Tıklayın **Oluştur** üzerinde **bulut hizmeti** bölmesi. Dağıtım zaman ulaştı **hazır** durumu, sonraki adımlara devam edebilirsiniz.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Dağıtımının başarıyla tamamlandığını doğrulayın
1. Bulut hizmeti örneğine tıklayın.

    Hizmetinin durumunu göstermesi gerekir **çalıştıran**.
2. Altında **Essentials**, tıklayın **Site URL'si** bulut hizmetinize bir web tarayıcısında açın.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: https://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırma](cloud-services-how-to-configure-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
* Yapılandırma [ssl sertifikaları](cloud-services-configure-ssl-certificate-portal.md).
