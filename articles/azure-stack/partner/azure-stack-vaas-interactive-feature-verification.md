---
title: Hizmet olarak Azure Stack doğrulama test etkileşimli özellik doğrulama | Microsoft Docs
description: Doğrulama bir hizmet olarak Azure Stack için etkileşimli özellik doğrulama testleri oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/07/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 20a48e67a24763af7bcce9e8831e2a1d1846d094
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594313"
---
# <a name="interactive-feature-verification-testing"></a>Etkileşimli özellik doğrulama testi  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sisteminiz için testleri istemek için etkileşimli özellik doğrulama test çerçevesi'ni kullanabilirsiniz. Bir test istediğinde Microsoft framework etkileşimli el ile yapılacak adımlar gerekli testleri hazırlamak için kullanır. Microsoft, birden fazla tek başına otomatik testleri bir araya zincirleyebilirsiniz çerçevesini kullanabilirsiniz.

Bu makalede, basit bir el ile senaryosu açıklanır. Azure stack'teki disk değiştirme test denetler. Framework, her bir adımı tanılama günlüklerini toplar. Bulduğunuz gibi sorunları hata ayıklaması yapabilirsiniz. Framework ayrıca diğer araçları veya işlemler tarafından üretilen günlükleri paylaşıma izin veren ve senaryoya geri bildirim sağlamak sağlar.

> [!Important]  
> Bu makalede, Disk kimliği gerçekleştirme adımları başvuruyor. Test geçiş iş akışını toplanan herhangi bir sonuç yeni bir çözüm doğrulama için kullanılamaz olarak yalnızca bir örnek budur.

## <a name="overview-of-interactive-testing"></a>Etkileşimli test genel bakış

Disk değiştirme için bir test sık karşılaşılan bir senaryodur. Bu örnekte, test yedi adım vardır:

1. Yeni bir **Test geçiş** iş akışı
1. Seçin **Disk kimliği Test**
1. İstendiğinde el ile adımı tamamlayın
1. Senaryo sonucunu denetleyin
1. Test sonucu Microsoft'a gönder

## <a name="create-a-new-test-pass"></a>Yeni bir test geçişi oluşturma

Kullanılabilir geçirmek varolan bir testi yoksa Lütfen için yönergeleri izleyin [test zamanlama](azure-stack-vaas-schedule-test-pass.md).

## <a name="schedule-the-test"></a>Test zamanlama

1. Seçin **Disk kimliği Test**.

    > [!Note]  
    > Test malzemeleri geliştirmeleri yapılmış gibi test sürümünü artırır. Aksi halde Microsoft belirtmiyorsa en yüksek sürümü her zaman kullanılmalıdır.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image4.png)

1. Etki alanı yönetici kullanıcı adı ve parola girmek **Düzenle**.

1. Uygun test yürütme aracısı / test başlatmaya DVM seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image5.png)

1. Seçin **Gönder** testi başlatmak için.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image6.png)

1. Önceki adımda seçtiğiniz aracısından etkileşimli test için UI erişin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image8.png)

1. İzleyin **belgeleri** ve **doğrulama** bağlantıları bu senaryoyu gerçekleştirmek nasıl Microsoft gelen yönergeleri gözden geçirin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image9.png)

1. **İleri**’yi seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image10.png)

1. Precheck betiği çalıştırmak için yönergeleri izleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image11.png)

1. Precheck betik başarıyla tamamlandıktan sonra el ile senaryoyu (Disk değiştirme) olarak başına çalıştırma **belgeleri** ve **doğrulama** gelen bağlantılar **bilgi**sekmesi.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image12.png)

    > [!Important]  
    > El ile senaryo gerçekleştirirken iletişim kutusunu kapatmayın.

1. El ile senaryo gerçekleştirme tamamladığınızda onay sonrası betiği çalıştırmak için yönergeleri izleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image13.png)

1. Başarıyla tamamlandığında el ile senaryosunun (Disk değiştirme), seçin **sonraki**.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image14.png)

    > [!Important]  
    > Pencereyi kapatırsanız yapıldığını önce test durur.

1. Test deneyimini için geri bildirim sağlayın. Bu soruları senaryo başarı oranı ve yayın kalitesini değerlendirmek Microsoft yardımcı olur.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image15.png)

1. Microsoft'a göndermek istediğiniz herhangi bir günlük dosyasının ekleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image16.png)

1. Geri bildirim gönderimine EULA'yı kabul edin.

1. Seçin **Gönder** sonuçlarını Microsoft'a gönderilecek.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
