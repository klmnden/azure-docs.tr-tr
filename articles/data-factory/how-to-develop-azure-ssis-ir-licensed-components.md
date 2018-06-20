---
title: Azure SSIS tümleştirmesi çalışma zamanı için lisanslı bileşenlerini yükleme | Microsoft Docs
description: ISV nasıl geliştirebileceğinizi öğrenin ve yükleme ücretli veya Azure SSIS tümleştirmesi çalışma zamanı için özel bileşenler lisanslı
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 146dc8c4475a041f28d7fe7ca464dfbc104258c7
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265963"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı için ücretli veya lisanslı özel bileşenlerini yükle

Bu makalede, nasıl bir ISV geliştirmek ve Azure SSIS tümleştirmesi çalışma zamanı'nda Azure'da çalıştırılan SQL Server Integration Services (SSIS) paketleri için ücretli veya lisanslı özel bileşenlerini yüklemek açıklanmaktadır.

## <a name="the-problem"></a>Sorun

Azure SSIS Integration zamanının yapısı özel bileşenlerin yetersiz şirket içi yüklenmesi için kullanılan normal lisans yöntemleri olun çeşitli zorluklar gösterir. Sonuç olarak, Azure SSIS IR farklı bir yaklaşım gerektirir.

-   Azure SSIS IR düğümleri geçici olan ve ayrılmış veya herhangi bir zamanda yayımladı. Örneğin, başlatabilir veya maliyet yönetmek veya yukarı ve aşağı çeşitli düğümü boyutları ölçeklendirmek için düğümleri durdurabilirsiniz. Sonuç olarak, MAC adresi ya da CPU kimliği gibi makine özgü bilgileri kullanarak belirli bir düğüme bir üçüncü taraf bileşen lisans bağlama artık geçerli değil.

-   Böylece düğüm sayısını daraltmak veya herhangi bir zamanda genişletmek de Azure SSIS IR giriş veya çıkış ölçeklendirebilirsiniz.

## <a name="the-solution"></a>Çözüm

Önceki bölümde açıklanan geleneksel lisans yöntemleri sınırlamaları sonucu olarak, Azure SSIS IR yeni bir çözüm sağlar. Bu çözüm Windows ortam değişkenlerini ve SSIS sistem değişkenleri lisans bağlama ve üçüncü taraf bileşenlerinin doğrulanması için kullanır. ISV'ler, küme kimliği ve Küme düğüm sayısı gibi bir Azure SSIS IR için benzersiz ve kalıcı bilgilerini edinmek için bu değişkenleri kullanabilirsiniz. Bu bilgiyle ISV'ler sonra lisans kendi bileşeni için bir Azure SSIS IR bağlayabilirsiniz *kümesi olarak*. Bu bağlama müşteriler başlatmak veya durdurmak, yukarı veya aşağı, Ölçek içeri veya dışarı ölçeklendirme veya Azure SSIS IR herhangi bir şekilde yeniden yapılandırın değişmeyen bir Kimliğini kullanır.

Aşağıdaki diyagram tipik yükleme, etkinleştirme ve lisans bağlama gösterir ve bu yeni değişkenleri kullanmak üçüncü taraf bileşenler için doğrulama akar:

![Lisanslı bileşenlerini yükleme](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Yönergeler
1. ISV'ler lisanslı bileşenleri çeşitli SKU'ları veya katmanları (örneğin, tek düğümü, 10 düğümleri ve benzeri kadar 5 düğümleri kadar) sunabilir. Müşterilerin bir ürün satın aldığınızda ISV karşılık gelen ürün anahtarı sağlar. ISV ISV Kurulum komut dosyası ve ilişkili dosyaları içeren bir Azure depolama blob kapsayıcısı da sağlayabilir. Müşteriler kendi depolama kapsayıcıya bu dosyaları kopyalayın ve bunları kendi ürün anahtarı ile değiştirin (örneğin, çalışan tarafından `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Müşteriler sonra sağlamak veya Azure SSIS IR parametre olarak, kapsayıcı SAS URI'sini ile yeniden yapılandırın. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

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

## <a name="isv-partners"></a>ISV iş ortakları

Bu blog gönderisine - sonunda Azure SSIS IR için kendi bileşenleri ve uzantıları uyarlanan ISV iş ortaklarına listesini bulabilirsiniz [Enterprise Edition, özel kurulum ve ADF SSIS 3 taraf Genişletilebilirliğe](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Azure SSIS Integration zamanının Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)
