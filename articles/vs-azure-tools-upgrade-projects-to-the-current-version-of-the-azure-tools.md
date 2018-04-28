---
title: Projeleri Azure Araçları'nın geçerli sürümüne yükseltmek nasıl | Microsoft Docs
description: Visual Studio'da bir Azure projesi Azure Araçları'nın geçerli sürümüne yükseltmeyi öğrenin
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1d64070a-078d-468a-87f4-e6715de6475f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 738c9d12beed6ba73872d520cb16bd3a6c813a35
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projeleri Visual Studio için Azure Araçları geçerli sürüme yükseltme
## <a name="overview"></a>Genel Bakış
Azure Araçları (veya 1.6 yeniyse önceki bir sürümü) sürümü geçerli yükledikten sonra Azure Araçları kullanılarak oluşturulan herhangi bir projeyle 1.6 önce sürüm (Kasım 2011) otomatik olarak yükseltilecek açmadan hemen sonra. Bu araçları 1.6 (Kasım 2011) sürümünü kullanarak projeleri oluşturulan ve hala bu sürümü yüklü değilse, eski sürümde bu projeleri açabilir ve daha sonra karar bunları yükseltilmesi gerekip gerekmediğini belirleme.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Bu yükseltme yaptığınızda, projenizin nasıl değiştiğini
Bir proje otomatik olarak yükseltilir veya yükseltmek istediğiniz belirtin, projenizin belirli derlemeleri geçerli sürümü ile çalışma şekilde değiştirilir ve bu bölümde açıklandığı gibi bazı özellikler da değiştirilir. Projenizi araçları, daha yeni sürümü ile uyumlu olması için diğer değişiklikler gerektiriyorsa, bu değişiklikleri el ile yapmalısınız.

* Web rolleri için web.config dosyasının ve çalışan rolleri app.config dosyasına Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll daha yeni sürümü başvurmak için güncelleştirilir.
* Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll ve Microsoft.WindowsAzure.ServiceRuntime.dll derlemeler yeni sürümüne yükseltilir.
* Azure proje dosyası (.ccproj) depolanan yayımlama profilleri taşınır uzantısı .azurePubXml ile ayrı bir dosyaya içinde **Yayımla** alt dizin.
* Yayımlama profili bazı özellikleri yeni ve değiştirilmiş özellikler desteklemek üzere güncelleştirilmiştir. **AllowUpgrade** değiştirilir **DeploymentReplacementMethod** aynı anda veya artımlı olarak dağıtılan bulut hizmeti güncelleştirmek için.
* Özellik **UseIISExpressByDefault** eklenir ve böylece hata ayıklama için kullanılan web sunucusu otomatik olarak Internet Information Services (IIS) IIS Express'e değişmeyeceği false olarak ayarlayın. IIS Express Araçları'nın daha yeni sürümleriyle oluşturulan projeleri için varsayılan web sunucusu olabilir.
* Azure önbelleği bir veya daha fazla projenizin rolleri barındırılıyorsa, proje yükseltildiğinde bazı özellikleri hizmet yapılandırma (.cscfg dosyası) ve hizmet tanımı (.csdef dosyası) değiştirilir. Projeyi Azure önbelleğe alma NuGet paketini kullanıyorsa, proje paketin en son sürüme yükseltilir. Web.config dosyasını açın ve istemci yapılandırması, yükseltme işlemi sırasında düzgün tutulan doğrulamanız gerekir. Azure önbelleği istemci derlemelerine başvuru NuGet paketini kullanmadan eklediyseniz, bu derlemeler güncelleştirilmez; Yeni sürümler bu başvurular el ile güncelleştirmeniz gerekir.

> [!IMPORTANT]
> F # projeleri için böylece bu derlemeler daha yeni sürümleri oldukları Azure derlemelerine başvurular el ile güncelleştirmelisiniz.
> 
> 

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Bir Azure projesi geçerli sürüme yükseltme
1. Yükseltilen proje için kullanmak istediğiniz Visual Studio yüklemesi içine Azure Araçları'nın geçerli sürümü yükleyin ve ardından yükseltmek istediğiniz projeyi açın. Projeyi Azure Araçları ile oluşturulmuşsa, yayın önce 1.6 (Kasım 2011) proje otomatik olarak geçerli sürüme yükseltilir. Proje oluşturduysanız ile Kasım 2011 yayın ve bu sürüme hala yüklü değil, bu sürümde projeyi açar.
2. Çözüm Gezgini'nde proje düğümünün kısayol menüsünü açın, seçin **özellikleri**ve ardından **uygulama** görüntülenen iletişim kutusunda.
   
    **Uygulama** sekmesi projeyle ilişkili araçları sürüm gösterir. Azure Araçları'nın geçerli sürümü görünürse, proje zaten yükseltildi. Hangi sekmesi gösterir, daha araçları daha yeni bir sürümünü yüklediyseniz bir **yükseltme** düğmesi görünür.
3. Seçin **yükseltme** proje Araçları'nın geçerli sürümüne yükseltmek için düğmesi.
4. Projeyi oluşturun ve sonra API değişikliklerden kaynaklanan hataları çözün. Kodunuzu yeni sürümün değiştirme hakkında daha fazla bilgi için belirli API belgelerine bakın.

