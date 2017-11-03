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
ms.openlocfilehash: 4650cebe18153e3b10bbec685a66a590348c99e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-cloud-services"></a>Bulut Hizmetleri yönetme
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-how-to-manage-portal.md)
> * [Klasik Azure Portalı](cloud-services-how-to-manage.md)
>
>

İçinde **bulut Hizmetleri (Klasik)** alanı Azure portal, hizmet rolü veya bir dağıtım güncelleştirmek, aşamalı bir dağıtım üretime yükseltmek, yapabilirsiniz; böylece kaynak bağımlılıkları bakın ve kaynakları birlikte ölçeği ve bir bulut hizmeti ya da dağıtım silmek bulut hizmetinize bağlamak kaynakları.

Bulut hizmetinizi ölçeklendirme hakkında daha fazla bilgi kullanılabilir [burada](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Nasıl yapılır: bir bulut hizmet rolü veya dağıtım güncelleştirme
Bulut hizmetiniz için uygulama kodu güncellemeniz gerekiyorsa kullanın **güncelleştirme** bulut hizmeti dikey. Tek bir rol veya tüm rolleri güncelleştirebilirsiniz. Güncelleştirmek için yeni hizmet paketi ya da hizmet yapılandırma dosyasını karşıya yükleyebilirsiniz.

1. İçinde [Azure portal][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.
2. Dikey penceresinde tıklayın **güncelleştirme** düğmesi.

    ![Güncelleştir düğmesi](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Dağıtım, yeni hizmet paketi dosyasının (.cspkg) ve hizmet yapılandırma dosyasının (.cscfg) ile güncelleştirin.

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **İsteğe bağlı olarak** dağıtım etiketi ve depolama hesabı güncelleştirin.
5. Herhangi bir rolü yalnızca bir rol örneği varsa, seçin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** devam etmek yükseltmeyi etkinleştirmek için.

    Her role en az iki rol örnekleri (sanal makineler) varsa azure yalnızca 99,95 yüzde hizmet kullanılabilirliği bir bulut hizmet güncelleştirmesi sırasında garanti edebilir. Diğer güncelleştirilirken iki rol örneği ile bir sanal makine istemci isteklerini işler.

6. Denetleme **Başlat dağıtım** paketin karşıya yükleme tamamlandıktan sonra uygulanan güncelleştirme sağlamak için.
7. Tıklatın **Tamam** hizmeti güncelleştirme başlamak için.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Nasıl yapılır: takas aşamalı bir dağıtım üretime yükseltmek için dağıtımlar
Bir bulut hizmeti, aşama yeni bir sürümünü dağıtmak ve yeni sürüm, bulut hizmeti hazırlama ortamında test etmek karar verdiğinizde. Kullanım **takas** , iki dağıtım giderilmesini ve üretim yeni bir sürüme yükseltmek URL'leri geçiş yapmak için.

Dağıtımlarından takas **bulut Hizmetleri** sayfası veya Pano'dan.

1. İçinde [Azure portal][Azure portal], güncelleştirmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.
2. Dikey penceresinde tıklayın **takas** düğmesi.

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Aşağıdaki onay istemi açılır.

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Dağıtım bilgileri doğruladıktan sonra tıklatın **Tamam** dağıtımlarını değiştirmek için.

    Sanal IP adresleri (VIP) değişiklikleri tek şey olduğu için dağıtımı takas dağıtımları için hızlı bir şekilde gerçekleşir.

    İşlem maliyetleri kaydetmek için üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtım silebilirsiniz.

### <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme hakkında sık sorulan sorular

**Dağıtımları takas önkoşulları nelerdir?**

Başarılı dağıtım Takas için iki anahtar Önkoşullar şunlardır:

- Statik bir IP adresi, üretim yuvası için kullanmak istiyorsanız, bir hazırlama yuvası da ayırmanız gerekir. Aksi takdirde değiştirme başarısız olur.

- Takas gerçekleştirmeden önce tüm örneklerini rollerinizi çalıştırması gerekir. Genel Bakış dikey penceresinde örneklerinizi Azure portal'ın durumunu kontrol edebilirsiniz. Alternatif olarak, kullanabileceğiniz [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell komutu.

Not Konuk işletim sistemi güncelleştirmeleri ve hizmet işlemleri düzeltme dağıtım takasları başarısız olmasına neden olabilir. Daha fazla bilgi için bkz: [bulut hizmeti dağıtım sorunlarını giderme](cloud-services-troubleshoot-deployment-problems.md).

**Bir takas Uygulamam için kapalı kalma süresi maliyetine neden olabilir mi? Bunu nasıl işlemesi gerekir?**

Yalnızca Azure yük dengeleyici yapılandırması değişikliği olduğundan son bölümde açıklandığı gibi bir dağıtımı takas genellikle hızlıdır. Bazı durumlarda, Bununla birlikte, en az on dakika sürer ve geçici bağlantı hatalarına neden. Müşterilerinize etkilerini sınırlamaktır uygulayın [istemci yeniden deneme mantığı](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Nasıl yapılır: bir bulut hizmeti için bir kaynak Bağla
Geçerli Azure Klasik portalı yaptığı gibi Azure portalındaki kaynakları birlikte bağlantı vermiyor. Bunun yerine, bulut hizmeti tarafından kullanılan aynı kaynak grubuna ek kaynaklar dağıtın.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Nasıl yapılır: dağıtımları ve bulut hizmeti Sil
Bir bulut hizmeti silebilmek için önce varolan her dağıtım silmeniz gerekir.

İşlem maliyetleri kaydetmek için üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtım silebilirsiniz. İşlem maliyetleri durdurulur dağıtılan rol örnekleri için faturalandırılır.

Bir dağıtımı veya Bulut hizmeti silmek için aşağıdaki yordamı kullanın.

1. İçinde [Azure portal][Azure portal], silmek istediğiniz bulut hizmeti seçin. Bu adım bulut hizmeti örneği dikey pencere açılır.
2. Dikey penceresinde tıklayın **silmek** düğmesi.

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Kontrol ederek tüm bulut hizmeti silebilirsiniz **bulut hizmeti ve hizmetin dağıtımları** veya seçin ya da **Üretim dağıtımı** veya **hazırlama dağıtımı**.

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Tıklatın **silmek** altındaki düğmesini.
5. Bulut hizmeti silmek için tıklatın **Delete bulut hizmeti**. Ardından, onay isteminde **Evet**.

> [!NOTE]
> Bir bulut hizmeti silindi ve ayrıntılı izleme yapılandırılmış veri depolama hesabınızdan el ile silmelisiniz. Ölçümleri tabloları nerede bulacağını hakkında daha fazla bilgi için bkz: [bu](cloud-services-how-to-monitor.md) makalesi.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Nasıl yapılır: başarısız dağıtımları hakkında daha fazla bilgi
**Genel bakış** dikey durum çubuğu üstünde vardır. Çubuğu'nu tıklatın, yeni bir dikey pencere açar ve hata bilgilerini görüntüler. Dağıtım hatalarını içermiyorsa, bilgi dikey boştur.

![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate-portal.md).
