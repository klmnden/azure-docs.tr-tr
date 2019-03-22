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
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: aae3ec8ff713959c5cc2485951aba025a6f89a1e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58113291"
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

1. İçinde [Azure portalında](https://portal.azure.com)içinde oluşturduğunuz depolama hesabına gidin [doğrulama hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md).

2. Sol dikey penceredeki **Blob hizmeti**seçin **kapsayıcıları**.

3. Seçin **+ kapsayıcı** menü çubuğundan.
    1. Örneğin, kapsayıcı için bir ad sağlamanız `vaaspackages`.
    1. Kimlik doğrulaması yapılmamış istemcilerin VaaS gibi istediğiniz erişim düzeyini seçin. Her senaryoda paketleri VaaS erişim konusunda daha fazla ayrıntı için bkz. [kapsayıcı erişim düzeyi işleme](#handling-container-access-level).

### <a name="upload-package-to-storage-account"></a>Paket depolama hesabına yükleyin.

1. Doğrulamak istediğiniz paketi hazırlayın. Bu bir `.zip` dosya içerikleri açıklanan yapısı eşleşmelidir [OEM paket oluşturma](azure-stack-vaas-create-oem-package.md).

    > [!NOTE]
    > Lütfen `.zip` içeriği kökünde yerleştirilir `.zip` dosya. Pakette hiçbir alt klasör olmalıdır.

1. İçinde [Azure portalında](https://portal.azure.com), paket kapsayıcıyı seçin ve paket seçerek karşıya **karşıya** menü çubuğundaki.

1. Paketi seçin `.zip` dosyasını karşıya yükleyin. İçin varsayılan değerleri tutun **Blob türü** (diğer bir deyişle, **blok blobu**) ve **bloğu boyutunu**.

### <a name="generate-package-blob-url-for-vaas"></a>Paket blob URL'si için VaaS oluştur

Oluştururken bir **paket doğrulama** VaaS portalı akışı paketinizi içeren Azure depolama blob için bir URL sağlamanız gerekir. Bazı *etkileşimli* dahil olmak üzere testleri **aylık AzureStack güncelleştirme doğrulama** ve **OEM uzantı paketi doğrulama**, ayrıca paket BLOB'ları için bir URL gerektirir.

#### <a name="handling-container-access-level"></a>İşleme kapsayıcı erişim düzeyi

İster bir paket doğrulama iş akışı oluşturuyorsanız veya zamanlama üzerinde VaaS tarafından gerekli en düşük erişim düzeyine bağlıdır bir *etkileşimli* test edin.

Durumunda, **özel** ve **Blob** erişim düzeyi, geçici olarak VaaS vererek paket bloba erişim izni vermelidir bir [paylaşılan erişim imzası](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1?) (SAS). **Kapsayıcı** erişim düzeyi SAS URL'lerini oluşturmak için gerekli değildir, ancak kapsayıcı ve bloblarını kimliği doğrulanmamış erişime izin verir.

|Erişim düzeyi | İş akışı gereksinimi | Test gereksinimi |
|---|---------|---------|
|Özel | Paket blob başına bir SAS URL'si oluşturun ([1. seçenek](#option-1-generate-a-blob-sas-url)). | Hesap düzeyinde bir SAS URL'si oluşturun ve el ile paket blob adını ekleyin ([seçeneği 2](#option-2-construct-a-container-sas-url)). |
|Blob | Blob URL'si özelliği sağlar ([seçeneği 3](#option-3-grant-public-read-access)). | Hesap düzeyinde bir SAS URL'si oluşturun ve el ile paket blob adını ekleyin ([seçeneği 2](#option-2-construct-a-container-sas-url)). |
|Kapsayıcı | Blob URL'si özelliği sağlar ([seçeneği 3](#option-3-grant-public-read-access)). | Blob URL'si özelliği sağlar ([seçeneği 3](#option-3-grant-public-read-access)).

Paketlerinizi erişim verme seçeneklerini en az erişimden en yüksek erişim sıralanır.

#### <a name="option-1-generate-a-blob-sas-url"></a>1. seçenek: Bir blob SAS URL'si oluşturun

Depolama kapsayıcınızda erişim düzeyini ayarlamak bu seçeneği kullanırsanız **özel**, burada kapsayıcı kapsayıcıya veya bloblarına genel okuma erişimi sağlamaz.

> [!NOTE]
> Bu yöntem çalışmaz *etkileşimli* testleri. Bkz: [seçeneği 2: Bir kapsayıcı SAS URL'si oluşturmak](#option-2-construct-a-container-sas-url).

1. İçinde [Azure portalında](https://portal.azure.com/), depolama hesabınıza gidin ve paketinizi içeren .zip olarak gidin.

2. Seçin **Generate SAS** bağlam menüsünden.

3. Seçin **okuma** gelen **izinleri**.

4. Ayarlama **başlangıç zamanı** geçerli saat ve **bitiş saati** en az 48 saate **başlangıç zamanı**. Diğer iş akışları ile aynı paket oluşturacaksanız, artırmayı **bitiş saati** testinizin uzunluğu.

5. Seçin **blob SAS belirteci ve URL üretmek**.

Kullanım **Blob SAS URL'si** zaman paket sağlayan blob URL'leri portala.

#### <a name="option-2-construct-a-container-sas-url"></a>2. seçenek: Bir kapsayıcı SAS URL'si oluşturun

Depolama kapsayıcınızda erişim düzeyini ayarlamak bu seçeneği kullanırsanız **özel** ve paket blob URL'sini sağlamanız gereken bir *etkileşimli* test edin. Bu URL de iş akışı düzeyinde kullanılabilir.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Seçin **Blob** gelen **izin verilen hizmetler seçenekleri**. Diğer seçenekleri seçimini kaldırın.

1. Seçin **kapsayıcı** ve **nesne** gelen **izin verilen kaynak türleri**.

1. Seçin **okuma** ve **listesi** gelen **iznine**. Diğer seçenekleri seçimini kaldırın.

1. Seçin **başlangıç zamanı** olarak geçerli saati ve **bitiş saati** için en az 14 gün sonra **başlangıç zamanı**. Aynı paket ile diğer testler çalıştıracaksanız artırmayı **bitiş saati** testinizin uzunluğu. Sonra VaaS aracılığıyla zamanlanmış herhangi bir test **bitiş saati** başarısız olur ve yeni SAS oluşturulması gerekecek.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    Biçim şu şekilde görünmelidir: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Paket kapsayıcısı dahil etmek için oluşturulan SAS URL'sini değiştirmek `{containername}`, paket blobunuz adını `{mypackage.zip}`gibi:  `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Portala URL'leri paket sağlayan blob olduğunda bu değeri kullanın.

#### <a name="option-3-grant-public-read-access"></a>Seçenek 3: Genel okuma erişimi verin

Kimliği doğrulanmamış istemciler için tek tek bloblar veya durumunda erişim izin vermek için kabul edilebilir ise, bu seçeneği kullanın *etkileşimli* testleri, kapsayıcı.

> [!CAUTION]
> Bu seçenek, anonim salt okunur erişim için BLOB'kurmak açar.

1. Paket kapsayıcıya erişim düzeyini ayarlayın **Blob** veya **kapsayıcı** bölümündeki yönergeleri izleyerek [kapsayıcılara ve blob'laraanonimkullanıcılaraizinvermek](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

    > [!NOTE]
    > Paket URL'si sağlıyorsanız bir *etkileşimli* test vermelidir **tam genel okuma erişimini** testiyle devam etmek için kapsayıcı.

1. Paket kapsayıcıda özellikler bölmesini açmak için paket blob seçin.

1. Kopyalama **URL**. Portala URL'leri paket sağlayan blob olduğunda bu değeri kullanın.

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
    > Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

2. Sınama Aracı'nı seçin. Ekleme hakkında bilgi yerel test yürütme aracıları, bkz: [yerel aracı dağıtma](azure-stack-vaas-local-agent.md).

3. OEM uzantı paketini doğrulamayı tamamlamak için seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

4. Test parametreleri gözden geçirin ve ardından **Gönder** zamanlayarak için OEM uzantı paketi doğrulama.

    OEM uzantı paketi doğrulama, iki el ile adımlar bölünür: Azure yığını güncelleştirmesi ve OEM güncelleştirme.

   1. **Seçin** "Precheck betiği yürütmek için kullanıcı Arabiriminde Çalıştır". Tamamlanması yaklaşık 5 dakika sürer ve hiçbir eylem gerektirmez otomatikleştirilmiş bir testi budur.

   1. Precheck betik tamamlandıktan sonra el ile adımıysa: **yükleme** Azure Stack portalını kullanarak en son kullanılabilir Azure Stack güncelleştirme.

   1. **Çalıştırma** Test AzureStack damgası üzerinde. Herhangi bir hata oluşursa, kişi ve test ile devam etmeyin [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

       Test-AzureStack komut çalıştırma hakkında daha fazla bilgi için bkz: [Azure Stack doğrulama sistem durumu](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test).

   1. **Seçin** "İleri" postcheck betiği yürütülemedi. Bu otomatik test ve Azure Stack'te güncelleştirme işleminin sonunu işaretler.

   1. **Seçin** OEM güncelleştirmesi precheck betiği yürütmek için "Run".

   1. Ön denetim tamamlandıktan sonra el ile adımıysa: **yükleme** portal üzerinden OEM uzantı paketi.

   1. **Çalıştırma** Test AzureStack damgası üzerinde.

      > [!NOTE]
      > Önceki örneklerde olduğu gibi test ve kişi ile devam etmeyin [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) başarısız olması durumunda. Bu adım, bir yeniden dağıtım kaydedecek şekilde önemlidir.

   1. **Seçin** "İleri" postcheck betiği yürütülemedi. Bu, OEM güncelleştirme adım sonunu işaretler.

   1. Testin sonunda kalan tüm sorularınızı ve **seçin** "Gönder".

   1. Bu etkileşimli test sonunu işaretler.

5. Sonuç, OEM uzantı paketini doğrulama için gözden geçirin. Test başarılı oldu sonra bulut benzetimi altyapısı yürütme için zamanlayın.

Bir paket imzalama isteğini göndermek için Gönder [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) çözüm adı ve paket doğrulama adı bu çalıştırmasıyla ilişkili.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
