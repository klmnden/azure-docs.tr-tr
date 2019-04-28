---
title: Azure-SSIS tümleştirme çalışma zamanı için lisanslı bileşenler yükleme | Microsoft Docs
description: Bir ISV nasıl geliştirebileceğinizi öğrenin ve yükleme ücretli veya özel bileşenler Azure-SSIS tümleştirme çalışma zamanı için lisanslı
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 80d4fff03422beacccd3aff3cdd8cb1047d5f5af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61344654"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için özel Ücretli ya da lisanslı bileşenler yükleme

Bu makalede, bir ISV nasıl geliştirebilir ve Azure-SSIS tümleştirme çalışma zamanı içinde Azure'da çalışan SQL Server Integration Services (SSIS) paketleri için özel Ücretli ya da lisanslı bileşenler yükleme açıklanır.

## <a name="the-problem"></a>Sorun

Azure-SSIS tümleştirme çalışma zamanının yapısını özel bileşenler yetersiz şirket içi yüklemek için kullanılan tipik lisans yöntemleri olun çeşitli zorluklar teşkil etmektedir. Sonuç olarak, Azure-SSIS IR, farklı bir yaklaşım gerektirir.

-   Azure-SSIS IR düğümlerinin geçici ve ayrılan veya herhangi bir anda yayınlanmıştır. Örneğin, Başlat veya maliyet yönetme veya çeşitli düğümü boyutları ile ölçeği artırma ve azaltma düğümler durdurun. Sonuç olarak, MAC adresi ya da CPU kimliği gibi makineye özgü bilgileri kullanarak, belirli bir düğüme bir üçüncü parti bileşeniniz lisans bağlama artık geçerli değil.

-   Böylece düğüm sayısını daraltmak veya genişletmek istediğiniz zaman, Azure-SSIS IR, giriş veya çıkış ölçeklendirebilirsiniz.

## <a name="the-solution"></a>Çözüm

Önceki bölümde açıklanan geleneksel lisans yöntemleri sınırlamaları sonucu olarak, Azure-SSIS IR, yeni bir çözüm sağlar. Bu çözüm lisans bağlama ve üçüncü taraf bileşenlerinin doğrulanması için Windows ortam değişkenlerini ve SSIS sistem değişkenleri kullanır. ISV küme kimliği ve kümenin düğüm sayısı gibi bir Azure-SSIS IR için benzersiz ve kalıcı bilgileri edinmek için bu değişkenleri kullanabilirsiniz. Bu bilgiyle ISV'ler sonra lisans kendi bileşeni için Azure-SSIS IR'yi bağlayabilirsiniz *kümesi olarak*. Bu bağlama, müşterilerin başlatma veya durdurma, ölçeği artırın veya azaltın, Ölçek daraltma veya genişletme veya herhangi bir şekilde Azure-SSIS IR yeniden değişmeyen bir kimliği kullanır.

Aşağıdaki diyagram tipik bir yükleme ve etkinleştirme lisans bağlama gösterir ve bu yeni değişkenler kullanan üçüncü taraf bileşenleri için doğrulama akar:

![Lisanslı bileşenlerini yükleme](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Yönergeler
1. ISV'ler, lisanslı bileşenleri çeşitli SKU'lar ya da (örneğin, tek düğüm, 10 düğümü ve benzeri en fazla 5 düğüm kadar) katmanlar sunabilir. ISV, müşterilere bir ürün satın aldığınızda ilgili ürün anahtarını sağlar. ISV, bir ISV Kurulum komut dosyası ve ilişkili dosyaları içeren bir Azure depolama blob kapsayıcısında de sağlayabilirsiniz. Müşteriler, bu dosyaları, kendi depolama kapsayıcısına kopyalayın ve bunları kendi ürün anahtarı ile değiştirin (örneğin, çalışan tarafından `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Müşteriler daha sonra sağlama veya Azure-SSIS IR ile parametre olarak, kapsayıcı SAS URI'sini yeniden yapılandırın. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

2. Azure-SSIS IR'nin sağlanması ya da yeniden ISV kurulum Windows ortam değişkenlerini sorgulamak için her bir düğümde çalışan `SSIS_CLUSTERID` ve `SSIS_CLUSTERNODECOUNT`. Ardından küme Kimliğini ve ürün anahtarı ISV etkinleştirme sunucusuna lisanslı ürünün etkinleştirme anahtarı oluşturmak için Azure-SSIS IR gönderir.

3. Etkinleştirme anahtarı aldıktan sonra ISV kurulum anahtarı yerel olarak (örneğin, kayıt defterinde) her bir düğümde depolayabilirsiniz.

4. Müşteriler Azure-SSIS IR'yi bir düğümde ISV lisanslı bileşeni kullanan bir paketi çalıştırdığınızda, paketi yerel olarak saklanan etkinleştirme anahtarı okur ve karşı düğümün küme kimliğini doğrular Paketin ISV etkinleştirme sunucunun küme düğümü sayısını Ayrıca isteğe bağlı olarak rapor edebilirsiniz.

    Etkinleştirme anahtarı doğrular ve küme düğümü sayısını raporlar koduna ilişkin bir örnek aşağıda verilmiştir:

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

Kendi bileşenleri ve uzantıları sonunda, bu blog gönderisini - Azure-SSIS IR için uyarlanmış ISV iş ortaklarının listesini bulabilirsiniz [Enterprise Edition, özel kurulum ve ADF SSIS için 3 taraf genişletilebilirlik](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Azure-SSIS Integration Runtime'nın Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)
