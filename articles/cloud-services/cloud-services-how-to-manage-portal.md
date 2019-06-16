---
title: Bulut hizmeti ilgili genel yönetim görevleri | Microsoft Docs
description: Azure portalında cloud Services yönetmeyi öğrenin. Bu örnekler, Azure portalını kullanın.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: d3d1ae759f0f3fa5edd417da61f1fa50b5d9cde7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61433962"
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Azure portalında bulut hizmetlerini yönetme
İçinde **Cloud Services** alanında Azure portal'ın, şunları yapabilirsiniz:

* Bir hizmeti rolü veya bir dağıtım güncelleştirin.
* Aşamalı bir dağıtım üretime yükseltin.
* Kaynakları kaynak bağımlılıkları görmek ve ölçek kaynakları birlikte, bulut hizmetine bağlayabilirsiniz.
* Bir bulut hizmeti ya da bir dağıtımı silin.

Bulut hizmetinizi ölçeklendirin hakkında daha fazla bilgi için bkz. [portalda bir bulut hizmeti için otomatik ölçeklendirme yapılandırma](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Bir bulut hizmeti rolü veya dağıtımını güncelleştirme
Bulut hizmetiniz için uygulama kodu güncelleştirmeniz gerekirse kullanın **güncelleştirme** bulut hizmeti dikey. Tek bir rol veya tüm rolleri güncelleştirebilirsiniz. Güncelleştirmek için yeni hizmet paketi veya hizmet yapılandırma dosyasını karşıya yükleyebilirsiniz.

1. İçinde [Azure portalında][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım, bulut hizmeti örneği dikey penceresinde açılır.

2. Dikey pencerede seçin **güncelleştirme**.

    ![Güncelleştir düğmesi](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Dağıtım, yeni hizmet paketi dosyasının (.cspkg) ve hizmet yapılandırma dosyasının (.cscfg) ile güncelleştirin.

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. İsteğe bağlı olarak, depolama hesabı ve dağıtım etiketi güncelleştirin.

5. Herhangi bir rolü yalnızca bir rol örneği varsa, seçin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** devam etmek yükseltmeyi etkinleştirmek için onay kutusu.

    Her role en az iki rol örneği (sanal makineler) varsa, azure bulut hizmeti güncelleştirme sırasında yalnızca yüzde 99,95 hizmet kullanılabilirliği garanti edebilir. Diğer güncelleştirilirken iki rol örneği ile bir sanal makine istemci isteklerini işler.

6. Seçin **Başlat dağıtım** paketin karşıya yüklenmesi tamamlandıktan sonra güncelleştirmeyi uygulamak için onay kutusunu işaretleyin.

7. Seçin **Tamam** hizmet güncelleştirilirken başlatmak için.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Aşamalı bir üretim dağıtımına yükseltmek üzere dağıtımları değiştirme
Yeni bir yayın aşama bir bulut hizmeti dağıtma ve yeni sürümünüzü bulut hizmetini hazırlama ortamınızda test etmek karar verdiğinizde. Kullanım **takas** iki dağıtım ele alınır ve üretim yeni bir sürüme yükseltin, URL'leri geçiş yapmak için.

Dağıtımları, takas **Cloud Services** bir sayfaya veya panoya.

1. İçinde [Azure portalında][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım, bulut hizmeti örneği dikey penceresinde açılır.

2. Dikey pencerede seçin **takas**.

    ![Bulut Hizmetleri değiştirme düğmesi](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Aşağıdaki onay istemi açılır:

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Dağıtım bilgileri doğruladıktan sonra seçin **Tamam** dağıtımları değiştirme.

    Sanal IP adresleri (VIP) değiştiren tek şey, çünkü dağıtım Takas dağıtımları için hızlı bir şekilde gerçekleşir.

    Bilgi işlem maliyetlerinden tasarruf için hazırlama dağıtımını, üretim dağıtımınızı beklendiği gibi çalıştığını doğruladıktan sonra silebilirsiniz.

### <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme hakkında sık sorulan sorular

**Dağıtımları takas için Önkoşullar nelerdir?**

Başarılı dağıtım Takas için iki anahtar Önkoşullar vardır:

- Statik bir IP adresi için üretim yuvası kullanmak istiyorsanız, bir hazırlama yuvasına de ayırmanız gerekir. Aksi takdirde değiştirme başarısız olur.

- Değiştirmeyi gerçekleştirmek için önce tüm rol örneklerini çalışıyor olması gerekir. Örneklerinizin durumunu kontrol edebilirsiniz **genel bakış** Azure portal'ın dikey penceresi. Alternatif olarak, [Get-AzureRole](/powershell/module/servicemanagement/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell komutu.

Konuk işletim sistemi güncelleştirmeleri ve hizmet işlemleri de Düzeltme Dağıtım takasları başarısız olmasına neden olabileceğini unutmayın. Daha fazla bilgi için [cloud service dağıtım sorunlarını giderme](cloud-services-troubleshoot-deployment-problems.md).

**Uygulamam için kapalı kalma süresi bir takas tabi? Bunu nasıl işlemesi gerekir?**

Yalnızca bir yapılandırma değişikliği Azure yük dengeleyicide olduğu için önceki bölümde açıklandığı gibi bir dağıtım Takas genellikle hızlı çalışır. Bazı durumlarda, geçici bağlantı hataları 10 veya daha fazla saniye ve sonuç alabilir. Müşterilerinizin etkisini sınırlamak için uygulamayı düşünün [istemci yeniden deneme mantığı](../best-practices-retry-general.md).

## <a name="delete-deployments-and-a-cloud-service"></a>Dağıtımları ve bulut hizmeti Sil
Bir bulut hizmetini silebilmeniz için önce var olan her dağıtım silmeniz gerekir.

Bilgi işlem maliyetlerinden tasarruf için hazırlama dağıtımını, üretim dağıtımınızı beklendiği gibi çalıştığını doğruladıktan sonra silebilirsiniz. İşlem maliyetleri durdurulur dağıtılan rol örnekleri için faturalandırılırsınız.

Bir dağıtımı veya Bulut hizmetinizi silmek için aşağıdaki yordamı kullanın.

1. İçinde [Azure portalında][Azure portal], silmek istediğiniz bulut hizmeti seçin. Bu adım, bulut hizmeti örneği dikey penceresinde açılır.

2. Dikey pencerede seçin **Sil**.

    ![Bulut Hizmetleri Sil düğmesi](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Tüm bulut hizmeti silmek için işaretleyin **bulut hizmeti ve hizmetin dağıtımları** onay kutusu. Veya ya da tercih edebilirsiniz **Üretim dağıtımı** veya **hazırlama dağıtımı** onay kutusu.

    ![Bulut Hizmetleri Sil](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Seçin **Sil** altındaki.

5. Bulut hizmeti silmek için işaretleyin **bulut hizmet Sil'i**. Ardından, onay isteminde **Evet**.

> [!NOTE]
> Bir bulut hizmeti silindi ve ayrıntılı izleme yapılandırılmış verileri depolama hesabınızdan el ile silmelisiniz. Tabulky Metrik nerede bulunacağı hakkında daha fazla bilgi için bkz. [izleme hizmeti, buluta giriş](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Başarısız dağıtımlar hakkında daha fazla bilgi
**Genel bakış** dikey penceresinde, bir durum çubuğu üstünde bulunur. Çubuğu seçtiğinizde, yeni bir dikey pencere açılır ve hata bilgilerini görüntüler. Dağıtım hataları içermiyorsa, bilgileri dikey penceresi boş olur.

![Bulut hizmetlerine genel bakış](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırma](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmeti dağıtma](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* Yapılandırma [SSL sertifikaları](cloud-services-configure-ssl-certificate-portal.md).
