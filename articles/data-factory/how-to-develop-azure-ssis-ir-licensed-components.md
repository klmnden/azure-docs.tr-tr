---
title: Azure SSIS tümleştirmesi çalışma zamanı için ücretli veya lisanslı bileşenler geliştirme | Microsoft Docs
description: Bu makalede nasıl ISV geliştirebilir ve yükleme ücretli veya Azure SSIS tümleştirmesi çalışma zamanı için özel bileşenler lisanslı
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: e22ca4bd5b749e8752f800590938199e06abbd34
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="develop-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı için ücretli veya lisanslı özel bileşenler geliştirin

## <a name="problem---the-azure-ssis-ir-requires-a-different-approach"></a>Sorun - Azure-SSIS IR farklı bir yaklaşım gerektirir.

Azure SSIS Integration zamanının yapısı özel bileşenlerin yetersiz şirket içi yüklenmesi için kullanılan normal lisans yöntemleri olun çeşitli zorluklar gösterir.

-   Azure SSIS IR düğümleri geçici olan ve ayrılmış veya herhangi bir zamanda yayımladı. Örneğin, başlatabilir veya maliyet yönetmek veya yukarı ve aşağı çeşitli düğümü boyutları ölçeklendirmek için düğümleri durdurabilirsiniz. Sonuç olarak, MAC adresi ya da CPU kimliği gibi makine özgü bilgileri kullanarak belirli bir düğüme bir üçüncü taraf bileşen lisans bağlama artık geçerli değil.

-   Böylece düğüm sayısını daraltmak veya herhangi bir zamanda genişletmek de Azure SSIS IR giriş veya çıkış ölçeklendirebilirsiniz.

## <a name="solution---windows-environment-variables-and-ssis-system-variables-for-license-binding-and-validation"></a>Çözüm - Windows ortam değişkenlerini ve lisans bağlama ve doğrulama SSIS sistem değişkenleri

Önceki bölümde açıklanan geleneksel lisans yöntemleri sınırlamaları sonucu olarak, Windows ortam değişkenlerini ve lisans bağlama ve üçüncü taraf bileşenlerinin doğrulanması için SSIS sistem değişkenleri Azure SSIS IR sağlar. ISV'ler, küme kimliği ve Küme düğüm sayısı gibi bir Azure SSIS IR için benzersiz ve kalıcı bilgilerini edinmek için bu değişkenleri kullanabilirsiniz. Bu bilgiyle ISV'ler bir Azure SSIS IR kendi bileşen lisansını bağlayabilirsiniz *kümesi olarak*, müşterilerin başlattığınızda veya durdurduğunuzda değişmeyen bir Kimliğe sahip ölçeği aşağı, giriş veya çıkış ölçeklendirme veya Azure SSIS IR herhangi bir şekilde yeniden yapılandırın.

Aşağıdaki diyagram tipik yükleme, etkinleştirme ve lisans bağlama gösterir ve bu yeni değişkenleri kullanmak üçüncü taraf bileşenler için doğrulama akar:

![Lisanslı bileşenlerini yükleme](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Yönergeler
1. ISV'ler lisanslı bileşenleri çeşitli SKU'ları veya katmanları (örneğin, tek düğümü, 10 düğümleri ve benzeri kadar 5 düğümleri kadar) sunabilir. Müşterilerin bir ürün satın aldığınızda ISV karşılık gelen ürün anahtarı sağlar. ISV ISV Kurulum komut dosyası ve ilişkili dosyaları içeren bir Azure depolama blob kapsayıcısı da sağlayabilir. Müşteriler kendi depolama kapsayıcıya bu dosyaları kopyalayın ve bunları kendi ürün anahtarı ile değiştirin (örneğin, çalışan tarafından `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Müşteriler sonra sağlamak veya Azure SSIS IR parametre olarak, kapsayıcı SAS URI'sini ile yeniden yapılandırın. Daha fazla bilgi için bkz: [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

2. Azure SSIS IR sağlanan ya da yeniden ISV kurulum Windows ortam değişkenlerini sorgulamak için her bir düğümde çalışan `SSIS_CLUSTERID` ve `SSIS_CLUSTERNODECOUNT`. Ardından, küme kimliği ve ISV etkinleştirme sunucusuna lisanslı ürünün ürün anahtarını etkinleştirme anahtarı oluşturmak için Azure SSIS IR gönderir.

3. Etkinleştirme anahtarı aldıktan sonra ISV kurulum anahtarı yerel olarak her düğümde (örneğin, kayıt defteri) depolayabilirsiniz.

4. Müşterilerin Azure SSIS IR düğümde ISV lisanslı bileşeni kullanan bir paketi çalıştırdığınızda, paketi yerel olarak depolanan etkinleştirme anahtarı okur ve düğümün küme kimliği karşı doğrular Paket, ISV etkinleştirme sunucunun Küme düğüm sayısı Ayrıca isteğe bağlı olarak bildirebilirsiniz.

    Etkinleştirme anahtarı doğrular ve Küme düğüm sayısı raporları kodu bir örneği burada verilmiştir:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Azure SSIS Integration zamanının Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)