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
ms.date: 1/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: 5ba290f442f4c27b510538d7c1f7b5e27467efc5
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246662"
---
# <a name="validate-oem-packages"></a>OEM paketleri doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bellenim veya sürücülerinin tamamlanan çözümü doğrulama için bir değişiklik olduğunda yeni bir OEM paketi test edebilirsiniz. Test paketiniz geçtiğinde, Microsoft tarafından imzalanır. Testiniz, Windows Server logo ve bilgisayarları testlerini geçtiğini bellenim ve sürücüleri ile güncelleştirilmiş OEM uzantı paketi içermelidir.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Karşıya yükleme veya paketleri göndermeden önce gözden [OEM paket oluşturma](azure-stack-vaas-create-oem-package.md) beklenen paket biçimi ve içeriği.

## <a name="managing-packages-for-validation"></a>Doğrulama için paketleri yönetme

Kullanırken **paket doğrulama** bir paket doğrulamak için iş akışı için bir URL sağlamak ihtiyacınız olacak bir **Azure depolama blobu**. Bu blob çözümü, dağıtım sırasında yüklenen OEM paketidir. Kurulumu sırasında oluşturduğunuz Azure depolama hesabı kullanarak blob oluşturma (bkz [doğrulama hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)).

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

#### <a name="option-1-generating-an-account-sas-url"></a>1. seçenek: Hesap SAS URL'si oluşturuluyor

1. İçinde [Azure portalında](https://portal.azure.com/), depolama hesabınıza gidin ve paketinizi içeren .zip olarak gidin

2. Seçin **Generate SAS** bağlam menüsünden

3. Seçin **okuma** gelen **izinleri**

4. Ayarlama **başlangıç zamanı** geçerli saat ve **bitiş saati** en az 48 saate **başlangıç zamanı**. Aynı paket ile diğer testler çalıştıracaksanız artırmayı **bitiş saati** testinizin uzunluğu. Sonra VaaS aracılığıyla zamanlanmış herhangi bir test **bitiş saati** başarısız olur ve yeni SAS oluşturulması gerekecek.

5. Seçin **blob SAS belirteci ve URL oluştur**

Kullanım **Blob SAS URL'si** başlatırken Yeni **paket doğrulama** VaaS Portalı'nda iş akışı.

#### <a name="option-2-using-public-read-container"></a>2. seçenek: Ortak okuma kapsayıcı kullanma

> [!CAUTION]
> Yukarı kapsayıcınızı anonim salt okunur erişim için bu seçeneği açılır.

1. GRANT **genel okuma erişimi yalnızca BLOB'lar için** bölümündeki yönergeleri izleyerek paket kapsayıcıya [kapsayıcılara ve blob'lara anonim kullanıcılara izin vermek](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

2. Özellikler bölmesinde açmak için paket blob kapsayıcısındaki paket kapsayıcısında seçin.

3. Kopyalama **URL**. Bu değer, yeni bir başlatırken kullanılacak **paket doğrulama** VaaS Portalı'nda iş akışı.

## <a name="apply-monthly-update"></a>Aylık güncelleştirme uygulama

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Paket doğrulama iş akışı oluşturma

1. Oturum [VaaS portalı](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Seçin **Başlat** üzerinde **paket doğrulama** Döşe.

    ![Paket doğrulamaları iş akışı kutucuğu](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Çözüm üzerinde dağıtım sırasında yüklenen OEM paketine Azure depolama blobu URL'si girin. Yönergeler için [paket blob URL'yi oluşturmak için VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Bir iş akışını oluşturduktan sonra ortam parametrelerini değiştirilemez.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testleri Özet sayfasına yönlendirilirsiniz.

## <a name="run-package-validation-tests"></a>Paket doğrulama testlerini çalıştırın

1. İçinde **paket doğrulama testleri özeti** sayfasında doğrulamayı tamamlamak için gereken testlerin bir listesini görürsünüz. Bu iş akışı testlerinde yaklaşık 24 saat için çalıştırın.

    Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

    > [!NOTE]
    > Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
    Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

2. Sınama Aracı'nı seçin. Ekleme hakkında bilgi yerel test yürütme aracıları, bkz: [yerel aracı dağıtma](azure-stack-vaas-local-agent.md).

3. Her biri aşağıdaki testleri için dört ve beş adımı:
    - OEM uzantı paketi doğrulama
    - Bulut benzetimi altyapısı

4. Seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

5. Test parametreleri gözden geçirin ve ardından **Gönder** test yürütme için zamanlamak için.

Tüm testler başarıyla tamamladıktan sonra VaaS çözümünüzü ve paket doğrulama adını gönder [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) paket imzalama istemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)