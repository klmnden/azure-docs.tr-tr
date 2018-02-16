---
title: "Genel bulut hizmeti yönetim görevleri | Microsoft Docs"
description: "Azure portalında bulut Hizmetleri yönetmeyi öğrenin. Bu örnekler Azure Portalı'nı kullanın."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: e60bf5c82e68d49abaa44d80ac9fafba2d8265da
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Bulut Hizmetleri Azure portalında Yönet
İçinde **bulut Hizmetleri** alanı Azure portalının şunları yapabilirsiniz:

* Bir hizmet rolü veya bir dağıtım güncelleştirin.
* Aşamalı bir dağıtım üretime Yükselt.
* Kaynakları kaynak bağımlılıkları bakın ve kaynakları birlikte ölçeği, bulut hizmetine bağlayın.
* Bir bulut hizmeti veya bir dağıtımı silin.

Bulut hizmetinizi ölçeklendirme hakkında daha fazla bilgi için bkz: [portalında bulut hizmeti için otomatik olarak ölçeklendirme yapılandırma](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Bir bulut hizmet rolü veya dağıtım güncelleştirme
Bulut hizmetiniz için uygulama kodu güncellemeniz gerekiyorsa kullanın **güncelleştirme** bulut hizmeti dikey. Tek bir rol veya tüm rolleri güncelleştirebilirsiniz. Güncelleştirmek için yeni hizmet paketi ya da hizmet yapılandırma dosyasını karşıya yükleyebilirsiniz.

1. İçinde [Azure portal][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.

2. Dikey penceresinde, seçin **güncelleştirme**.

    ![Güncelleştir düğmesi](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Dağıtım, yeni hizmet paketi dosyasının (.cspkg) ve hizmet yapılandırma dosyasının (.cscfg) ile güncelleştirin.

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. İsteğe bağlı olarak, depolama hesabı ve dağıtım etiketi güncelleştirin.

5. Herhangi bir rolü yalnızca bir rol örneği varsa, seçin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** devam etmek için yükseltmeyi etkinleştirmek için onay kutusunu.

    Her role en az iki rol örnekleri (sanal makineler) varsa, azure bulut hizmeti güncelleştirme sırasında yalnızca 99,95 yüzde hizmet kullanılabilirliğini garanti edebilir. Diğer güncelleştirilirken iki rol örneği ile bir sanal makine istemci isteklerini işler.

6. Seçin **Başlat dağıtım** paketin karşıya yükleme tamamlandıktan sonra güncelleştirmeyi uygulamak için onay kutusunu.

7. Seçin **Tamam** hizmeti güncelleştirme başlamak için.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Aşamalı bir dağıtım üretime yükseltmek için dağıtımları değiştirme
Bir bulut hizmeti, aşama yeni bir sürümünü dağıtmak ve yeni sürüm, bulut hizmeti hazırlama ortamında test etmek karar verdiğinizde. Kullanım **takas** , iki dağıtım giderilmesini ve üretim yeni bir sürüme yükseltmek URL'leri geçiş yapmak için.

Dağıtımlarından takas **bulut Hizmetleri** sayfası veya Pano'dan.

1. İçinde [Azure portal][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.

2. Dikey penceresinde, seçin **takas**.

    ![Bulut Hizmetleri değiştirme düğmesi](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Aşağıdaki onay istemi açar:

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Dağıtım bilgileri doğruladıktan sonra Seç **Tamam** dağıtımlarını değiştirmek için.

    Sanal IP adresleri (VIP) değişiklikleri tek şey olduğu için dağıtımı takas dağıtımları için hızlı bir şekilde gerçekleşir.

    İşlem maliyetleri kaydetmek için üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtım silebilirsiniz.

### <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme hakkında sık sorulan sorular

**Dağıtımları takas önkoşulları nelerdir?**

Başarılı dağıtım Takas için iki anahtar Önkoşullar şunlardır:

- Statik bir IP adresi, üretim yuvası için kullanmak istiyorsanız, bir hazırlama yuvası da ayırmanız gerekir. Aksi takdirde değiştirme başarısız olur.

- Takas gerçekleştirmeden önce tüm örneklerini rollerinizi çalıştırması gerekir. Örneklerinizin durumunu denetleyebilirsiniz **genel bakış** Azure portalı dikey. Alternatif olarak, kullanabileceğiniz [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell komutu.

Konuk işletim sistemi güncelleştirmelerini ve hizmet işlemleri aynı zamanda düzeltme dağıtım takasları başarısız olmasına neden olabileceğini unutmayın. Daha fazla bilgi için bkz: [bulut hizmeti dağıtım sorunlarını giderme](cloud-services-troubleshoot-deployment-problems.md).

**Bir takas Uygulamam için kapalı kalma süresi maliyetine neden olabilir mi? Bunu nasıl işlemesi gerekir?**

Yalnızca Azure yük dengeleyici yapılandırma değişikliği olduğu için önceki bölümde açıklandığı gibi bir dağıtımı takas genellikle hızlıdır. Bazı durumlarda, geçici bağlantı hataları 10 veya daha fazla saniye ve sonuç alabilir. Müşterilerinize etkilerini sınırlamaktır uygulayın [istemci yeniden deneme mantığı](../best-practices-retry-general.md).

## <a name="delete-deployments-and-a-cloud-service"></a>Dağıtımları ve bulut hizmeti Sil
Bir bulut hizmeti silebilmek için önce varolan her dağıtım silmeniz gerekir.

İşlem maliyetleri kaydetmek için üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtım silebilirsiniz. İşlem maliyetleri durdurulur dağıtılan rol örnekleri için faturalandırılır.

Bir dağıtımı veya Bulut hizmeti silmek için aşağıdaki yordamı kullanın.

1. İçinde [Azure portal][Azure portal], silmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.

2. Dikey penceresinde, seçin **silmek**.

    ![Bulut Hizmetleri Sil düğmesi](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Tüm bulut hizmeti silinecek seçin **bulut hizmeti ve hizmetin dağıtımları** onay kutusu. Ya da seçebilirsiniz **Üretim dağıtımı** veya **hazırlama dağıtımı** onay kutusu.

    ![Cloud Services Delete](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Seçin **silmek** altındaki.

5. Bulut hizmeti silinecek seçin **Delete bulut hizmeti**. Ardından, onay isteminde **Evet**.

> [!NOTE]
> Bir bulut hizmeti silindi ve ayrıntılı izleme yapılandırılmış veri depolama hesabınızdan el ile silmelisiniz. Ölçümleri tabloları nerede bulacağını hakkında daha fazla bilgi için bkz: [izleme hizmeti bulut giriş](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Dağıtımları başarısız hakkında daha fazla bilgi
**Genel bakış** dikey durum çubuğu üstünde vardır. Çubuğu seçtiğinizde, yeni bir dikey pencere açar ve hata bilgilerini görüntüler. Dağıtım hatalarını içermiyorsa, bilgi dikey boştur.

![Bulut hizmetlerine genel bakış](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* Yapılandırma [SSL sertifikalarını](cloud-services-configure-ssl-certificate-portal.md).
