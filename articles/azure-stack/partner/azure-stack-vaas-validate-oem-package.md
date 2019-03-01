---
title: Hizmet olarak Azure Stack doğrulama orijinal ekipman üreticisi (OEM) paketleri doğrulama | Microsoft Docs
description: Özgün ekipman üreticisi (OEM) bir hizmet olarak doğrulama paketlerle doğrulamak hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 02/19/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f5b884ddda292b1c523a5364d34753ccb3a5bbdf
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194455"
---
# <a name="validate-oem-packages"></a>OEM paketleri doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bellenim veya sürücülerinin tamamlanan çözümü doğrulama için bir değişiklik olduğunda yeni bir OEM paketi test edebilirsiniz. Test paketiniz geçtiğinde, Microsoft tarafından imzalanır. Testiniz, Windows Server logo ve bilgisayarları testlerini geçtiğini bellenim ve sürücüleri ile güncelleştirilmiş OEM uzantı paketi içermelidir.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Karşıya yükleme veya paketleri göndermeden önce gözden [OEM paket oluşturma](azure-stack-vaas-create-oem-package.md) beklenen paket biçimi ve içeriği.

## <a name="managing-packages-for-validation"></a>Doğrulama için paketleri yönetme

Kullanırken **paket doğrulama** bir paket doğrulamak için iş akışı için bir URL sağlamak ihtiyacınız olacak bir **Azure depolama blobu**. Bu blob test güncelleştirme işleminin bir parçası yüklü OEM paket imzalanır. Kurulumu sırasında oluşturduğunuz Azure depolama hesabı kullanarak blob oluşturma (bkz [doğrulama hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Önkoşul: Bir depolama kapsayıcısı sağlama

Depolama hesabınızda blobları paket için bir kapsayıcı oluşturun. Bu kapsayıcıdaki tüm paket doğrulama çalıştırmalarını için kullanılabilir.

1. İçinde [Azure portalı](https://portal.azure.com)içinde oluşturduğunuz depolama hesabına gidin [doğrulama hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md).
2. Sol dikey penceredeki **Blob hizmeti**, select deyiminde **kapsayıcıları**.
3. Seçin **+ kapsayıcı** menüsünde örn, kapsayıcı için bir ad sağlayın ve çubuk `vaaspackages`.

### <a name="upload-package-to-storage-account"></a>Paket depolama hesabına yükleyin.

1. Doğrulamak istediğiniz paketi hazırlayın. Birden çok dosya paketiniz varsa, içine sıkıştırın bir `.zip` dosya.
2. İçinde [Azure portalı](https://portal.azure.com), paket kapsayıcıyı seçin ve paket seçerek karşıya **karşıya** menü çubuğundaki.
3. Paketi seçin `.zip` dosyasını karşıya yükleyin. İçin varsayılan değerleri tutun **Blob türü** (yani, **blok blobu**) ve **bloğu boyutunu**.

> [!NOTE]
> Lütfen `.zip` içeriği kökünde yerleştirilir `.zip` dosya. Pakette hiçbir alt klasör olmalıdır.

### <a name="generate-package-blob-url-for-vaas"></a>Paket blob URL'si için VaaS oluştur

Oluştururken bir **paket doğrulama** VaaS portalı akışı paketinizi içeren Azure depolama blob için bir URL sağlamanız gerekir.

#### <a name="option-1-generating-a-blob-sas-url"></a>1. seçenek: SAS URL blob'u oluşturuluyor

Depolama kapsayıcısı veya blobları için genel okuma erişimini etkinleştirmek istemiyorsanız bu seçeneği kullanın.

1. İçinde [Azure portalında](https://portal.azure.com/), depolama hesabınıza gidin ve paketinizi içeren .zip olarak gidin

2. Seçin **Generate SAS** bağlam menüsünden

3. Seçin **okuma** gelen **izinleri**

4. Ayarlama **başlangıç zamanı** geçerli saat ve **bitiş saati** en az 48 saate **başlangıç zamanı**. Aynı paket ile diğer testler çalıştıracaksanız artırmayı **bitiş saati** testinizin uzunluğu. Sonra VaaS aracılığıyla zamanlanmış herhangi bir test **bitiş saati** başarısız olur ve yeni SAS oluşturulması gerekecek.

5. Seçin **blob SAS belirteci ve URL üretmek**.

Kullanım **Blob SAS URL'si** zaman paket sağlayan blob URL'leri portala.

#### <a name="option-2-grant-public-read-access"></a>2. seçenek: Genel okuma erişimi verin

> [!CAUTION]
> Bu seçenek, anonim salt okunur erişim için BLOB'kurmak açar.

1. GRANT **genel okuma erişimi yalnızca BLOB'lar için** bölümündeki yönergeleri izleyerek paket kapsayıcıya [kapsayıcılara ve blob'lara anonim kullanıcılara izin vermek](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

> [!NOTE]
> Paket URL'si sağlıyorsanız bir *etkileşimli test* sağlamanız gerekir (örneğin, aylık AzureStack güncelleştirme doğrulaması veya OEM uzantı paketi doğrulama), **tam genel okuma erişimini** için Test ile devam edin.

2. Paket kapsayıcıda özellikler bölmesini açmak için paket blob seçin.

3. Kopyalama **URL**. Portala URL'leri paket sağlayan blob olduğunda bu değeri kullanın.

## <a name="apply-monthly-update"></a>Aylık güncelleştirme uygulama

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

> [!NOTE]
> Aylık güncelleştirmeyi uyguladıktan sonra güncelleştirme başarıyla uygulandı ve iyi durumda olduğunu doğrulamak için Test-AzureStack çalıştırmanızı öneririz. AzureStack test başarısız olursa, sorunu Microsoft'a bildirin. Test geçiş ile Sorun çözülene kadar devam yok. Bu Test Azure Stack komut çalıştırma hakkında daha fazla bilgi bulunabilir [makale](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test).

## <a name="create-a-package-validation-workflow"></a>Paket doğrulama iş akışı oluşturma

1. Oturum [VaaS portalı](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Seçin **Başlat** üzerinde **paket doğrulama** Döşe.

    ![Paket doğrulamaları iş akışı kutucuğu](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Azure depolama girin blob URL'sini teste imzalı Microsoft gelen bir imza gerektiren OEM paket. Yönergeler için [paket blob URL'yi oluşturmak için VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Bir iş akışını oluşturduktan sonra ortam parametrelerini değiştirilemez.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testleri Özet sayfasına yönlendirilirsiniz.

## <a name="required-tests"></a>Gerekli testleri

Aşağıdaki testler OEM paket doğrulaması için gereklidir:

- OEM uzantı paketi doğrulama
- Bulut benzetimi altyapısı

## <a name="run-package-validation-tests"></a>Paket doğrulama testlerini çalıştırın

1. İçinde **paket doğrulama testleri özeti** sayfasında, listelenen testlerin senaryonuz için uygun bir alt çalışır.

    Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

    > [!NOTE]
    > Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
    Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

2. Sınama Aracı'nı seçin. Ekleme hakkında bilgi yerel test yürütme aracıları, bkz: [yerel aracı dağıtma](azure-stack-vaas-local-agent.md).

3. Tam OEM uzantı paketi doğrulama seçmek amacıyla **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

4. Test parametreleri gözden geçirin ve ardından **Gönder** zamanlayarak için OEM uzantı paketi doğrulama.

5. Sonuç, OEM uzantı paketini doğrulama için gözden geçirin. Test başarılı oldu sonra bulut benzetimi altyapısı yürütme için zamanlayın.

Tüm testler başarıyla tamamladıktan sonra VaaS çözümünüzü ve paket doğrulama adını gönder [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) paket imzalama istemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
