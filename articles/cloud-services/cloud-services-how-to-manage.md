---
title: "Genel bulut hizmeti yönetim görevleri (Klasik) | Microsoft Docs"
description: "Klasik Azure portalındaki bulut Hizmetleri yönetmeyi öğrenin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 2ee76dfcb579e53975b1f61a6590f8d78dc0961b
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

İçinde **bulut Hizmetleri** alanı Azure Klasik portal, hizmet rolü veya bir dağıtım güncelleştirmek, aşamalı bir dağıtım üretime yükseltmek, yapabilirsiniz; böylece kaynak bağımlılıkları bakın ve kaynakları birlikte ölçeği ve bir bulut hizmeti ya da dağıtım silmek bulut hizmetinize bağlamak kaynakları.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Nasıl yapılır: bir bulut hizmet rolü veya dağıtım güncelleştirme
Bulut hizmetiniz için uygulama kodu güncellemeniz gerekiyorsa kullanın **güncelleştirme** bir Pano üzerinde **bulut Hizmetleri** sayfasında veya **örnekleri** sayfası. Tek bir rol veya tüm rolleri güncelleştirebilirsiniz. Yeni hizmet paketi ve hizmet yapılandırma dosyası karşıya gerekir.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), bir Pano üzerinde **bulut Hizmetleri** sayfasında veya **örnekleri** sayfasında, **güncelleştirme**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. İçinde **dağıtım etiketi**, dağıtım (örneğin, mycloudservice4) tanımlamak için bir ad girin. Dağıtım etiketi altında bulabilirsiniz **Hızlı Başlangıç** Panoda.
3. İçinde **paket**, kullanın **Gözat** hizmet paketi dosyasının (.cspkg) yüklemek için.
4. İçinde **yapılandırma**, kullanın **Gözat** hizmet yapılandırma dosyasının (.cscfg) yüklemek için.
5. İçinde **rol**seçin **tüm** bulut hizmetindeki tüm rolleri yükseltmek istiyorsanız. Tek bir rol güncelleştirme gerçekleştirmek için güncelleştirmek istediğiniz rolü seçin. Güncelleştirme için belirli bir rol seçseniz bile hizmet yapılandırma dosyası güncelleştirmeleri tüm rolleri uygulanır.
6. Güncelleştirme rol sayısı veya herhangi bir rol boyutunu değişirse seçin **rol boyutları veya rollerin sayısı değişirse güncelleştirmeye izin ver** devam etmek güncelleştirmeyi etkinleştirmek için onay kutusunu.

    Bir rol (diğer bir deyişle, bir rol örneği barındıran bir sanal makine boyutu) ya da rol sayısı boyutunu değiştirirseniz, her rol örneği (sanal makine) görüntüsü yüklenmiş olması gerekir ve tüm yerel veriler kaybolacak unutmayın.

7. Herhangi bir hizmet rolü yalnızca bir rol örneği varsa, seçin **bir veya daha fazla rol içeriyor olsa bile bir tek örnek onay kutusu güncelleştirme** devam etmek yükseltmeyi etkinleştirmek için.

    Her role en az iki rol örnekleri (sanal makineler) varsa azure yalnızca 99,95 yüzde hizmet kullanılabilirliği bir bulut hizmet güncelleştirmesi sırasında garanti edebilir. Diğer güncelleştirilirken istemci isteklerini işlemek bir sanal makine sağlar.

8. Tıklatın **Tamam** (hizmeti güncelleştirme başlatmak için onay işaretine).

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Nasıl yapılır: takas aşamalı bir dağıtım üretime yükseltmek için dağıtımlar
Kullanım **takas** bir bulut hizmeti üretime hazırlama dağıtımı yükseltmek için. Yeni sürümde bir bulut hizmeti dağıtmak karar verdiğinizde, aşama ve müşterileriniz üretimde geçerli sürümde kullanırken yeni sürüm, bulut hizmeti hazırlama ortamınızda sınayın. Üretim yeni sürüme yükseltmek hazır olduğunuzda, kullanabileceğiniz **takas** iki dağıtım olarak ele URL'leri geçiş yapmak için.

Dağıtımlarından takas **bulut Hizmetleri** sayfası veya Pano'dan.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**.
2. Bulut hizmetleri listesinde, bulut hizmeti seçmek için tıklatın.
3. Tıklatın **takas**.

    Aşağıdaki onay istemi açılır.

    ![Bulut Hizmetleri değiştirme](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Dağıtım bilgileri doğruladıktan sonra tıklatın **Evet** dağıtımlarını değiştirmek için.

    Sanal IP adresleri (VIP) değişiklikleri tek şey olduğu için dağıtımı takas dağıtımları için hızlı bir şekilde gerçekleşir.

    İşlem maliyetleri kaydetmek için yeni üretim dağıtımı beklenen şekilde çalıştığını emin olduğunuzda hazırlama ortamında dağıtım silebilirsiniz.

### <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme hakkında sık sorulan sorular

**Dağıtımları takas önkoşulları nelerdir?**

Başarılı dağıtım Takas için iki anahtar Önkoşullar şunlardır:

- Statik bir IP adresi, üretim yuvası için kullanmak istiyorsanız, bir hazırlama yuvası da ayırmanız gerekir. Aksi takdirde değiştirme başarısız olur.

- Takas gerçekleştirmeden önce tüm örneklerini rollerinizi çalıştırması gerekir. Azure Klasik portalında veya kullanarak örneklerinizi durumunu kontrol edebilirsiniz [Windows PowerShell Get-AzureRole komutta](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Konuk işletim sistemi güncelleştirmelerini ve hizmet işlemleri düzeltme başarısız dağıtım takasları ayrıca neden olabileceğini unutmayın. Bkz: [bulut hizmeti dağıtım sorunlarını giderme](cloud-services-troubleshoot-deployment-problems.md) daha fazla ayrıntı için.

**Bir takas Uygulamam için kapalı kalma süresi maliyetine neden olabilir mi? Bunu nasıl işlemesi gerekir?**

Yalnızca Azure yük dengeleyici yapılandırması değişikliği olduğundan son bölümde açıklandığı gibi bir dağıtımı takas genellikle çok hızlıdır. Bazı durumlarda, Bununla birlikte, en az on dakika sürer ve geçici bağlantı hatalarına neden. Müşterilerinize etkilerini sınırlamaktır uygulayın [istemci yeniden deneme mantığı](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Nasıl yapılır: bir bulut hizmeti için bir kaynak Bağla
Bulut hizmetinizin diğer kaynaklara bağımlılığını göstermek için bir Azure SQL Veritabanı örneğini veya depolama hesabını bulut hizmetine bağlayabilirsiniz. Bağlantı ve kaynakları üzerinde bağlantısını **bağlı kaynaklar** sayfasında ve ardından kullanımları bulut hizmetinin panosunda izleyebilirsiniz. Bağlantılı Depolama hesabına açık izleme varsa, bulut hizmet panosunda toplam istek sayısı izleyebilirsiniz.

Kullanım **bağlantı** bir yeni veya var olan SQL veritabanı örneği veya depolama hesabı bulut hizmetinize bağlamak için. Veritabanı üzerinde kullanarak bulut hizmet rolü ile birlikte sonra ölçeklendirebilirsiniz **ölçek** sayfası. (Bir depolama hesabı kullanımı arttıkça otomatik olarak ölçeklendirir.) Daha fazla bilgi için bkz: [bir bulut hizmeti ve bağlı kaynaklar ölçeklendirme](cloud-services-how-to-scale.md).

Ayrıca izleyebilir, yönetme ve veritabanında ölçeklendirme **veritabanları** Klasik Azure portalı düğümünün.

"Bu bağlamda bir kaynak bağlama" uygulamanızı kaynağa bağlama değil. Kullanarak yeni bir veritabanı oluşturursanız **bağlantı**, bağlantı dizeleri, uygulama kodu ekleyin ve ardından bulut hizmeti yükseltmeniz gerekir. Uygulamanız bir bağlantılı depolama hesabında kaynakları kullanıyorsa bağlantı dizeleri eklemeniz gerekir.

Aşağıdaki yordam, yeni bir SQL veritabanı örneği, bir bulut hizmeti için yeni bir SQL veritabanı sunucusunda dağıtılan bağlantı açıklar.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>SQL veritabanı örneğinde bir bulut hizmetine bağlamak için
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**. Ardından panoyu açmak için bulut hizmeti adına tıklayın.
2. Tıklatın **bağlı kaynakları**.

    **Bağlı kaynaklar** sayfası açılır.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Ya da tıklatın **bir kaynağı bağladıktan** veya **bağlantı**.

    **Bağlantı kaynak** Sihirbazı'nı başlatır.

    ![Bağlantı Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Tıklatın **yeni bir kaynak oluşturmak** veya **mevcut bir kaynağı bağlayın**.
5. Bağlantı için kaynak türünü seçin. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **SQL veritabanı**. (Yalnızca klasik Azure portalı, bir bulut hizmeti için bir depolama hesabı bağlama destekler.)
6. Veritabanı yapılandırmasını tamamlamak için Yardımı'ndaki yönergeleri izleyin **SQL veritabanları** Klasik Azure portalı alanı.

    İleti alanında bağlama işlemin ilerlemesini izleyebilirsiniz.

    Bağlama tamamlandığında, bulut hizmetinin panosundaki bağlantılı kaynağın durumunu izleyebilirsiniz. Bağlantılı bir SQL veritabanı ölçeklendirme hakkında daha fazla bilgi için bkz: [bir bulut hizmeti ve bağlı kaynaklar ölçeklendirme](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Bağlantılı kaynak bağlantısını kesme
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**. Ardından panoyu açmak için bulut hizmeti adına tıklayın.
2. Tıklatın **bağlı kaynaklar**ve ardından kaynağı seçin.
3. Tıklatın **bağlantısını**. Ardından **Evet** onay isteminde.

    Bir SQL veritabanı bağlantısını veritabanı veya veritabanı uygulamanın bağlantılarını üzerinde etkisi yoktur. Veritabanında yönetmeye devam edebilirsiniz **SQL veritabanları** Klasik Azure portalı alanı.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Nasıl yapılır: dağıtımları ve bulut hizmeti Sil
Bir bulut hizmeti silebilmek için önce varolan her dağıtım silmeniz gerekir.

İşlem maliyetleri kaydetmek için hazırlama dağıtımınızı üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra silebilirsiniz. Bir bulut hizmeti çalışmıyor olsa bile, rol örnekleri için faturalanan işlem maliyetleri demektir.

Bir dağıtımı veya Bulut hizmeti silmek için aşağıdaki yordamı kullanın.

1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**.
2. Bulut hizmeti seçin ve ardından **silmek**. (Pano açmadan bir bulut hizmeti seçmek için herhangi bir yere ad bulut hizmeti girişinde dışındaki tıklayın.)

    Hazırlık veya üretim dağıtımında varsa, pencerenin altındaki aşağıdaki benzer Seçenekler menüsüne görürsünüz. Bulut hizmeti silebilmek için önce var olan tüm dağıtımları silmeniz gerekir.

    ![Menü silme](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. Bir dağıtımı silmek için tıklatın **silmek Üretim dağıtımı** veya **hazırlama dağıtımı silin**. Ardından, onay isteminde **Evet**.
4. Bulut hizmetini silin planlıyorsanız, adım 3, gerekirse diğer dağıtımınızı silmek için yineleyin.
5. Bulut hizmeti silmek için tıklatın **Delete bulut hizmeti**. Ardından, onay isteminde **Evet**.

> [!NOTE]
> Ayrıntılı izleme bulut hizmetiniz için yapılandırılmışsa, bulut hizmeti sildiğinizde Azure izleme verilerini depolama hesabınızdan silmez. Verileri el ile silmeniz gerekir. Ölçümleri tabloları nerede bulacağını hakkında daha fazla bilgi için bkz "nasıl yapılır: izleme verilerini Klasik Azure portalı dışında ayrıntılı erişim" içinde [İzleyici bulut hizmetlerini](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate.md).
