---
title: Azure DevOps Services öğreticisini kullanarak CI/CD ile Azure Stream Analytics işi dağıtma
description: Bu makalede, Azure DevOps Services kullanarak CI/CD ile Stream Analytics işinin nasıl dağıtıldığı açıklanır.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 7/10/2018
ms.openlocfilehash: adacbaf718c5ef293b4ee3fa833083704aa41f5c
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297951"
---
# <a name="tutorial-deploy-an-azure-stream-analytics-job-with-cicd-using-azure-pipelines"></a>Öğretici: Azure Pipelines kullanarak CI/CD ile Azure Stream Analytics işi dağıtma
Bu öğreticide, Azure Pipelines kullanarak bir Azure Stream Analytics işi için sürekli tümleştirme ve dağıtımın nasıl ayarlanacağı açıklanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Projenize kaynak denetimi ekleme
> * Azure Pipelines’da derleme işlem hattı oluşturma
> * Azure Pipelines’da yayın işlem hattı oluşturma
> * Uygulamayı otomatik olarak dağıtma ve yükseltme

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Visual Studio](stream-analytics-tools-for-visual-studio-install.md)’yu ve **Azure geliştirme** veya **Veri Depolama ve İşleme** iş yüklerini yükleyin.
* [Visual Studio’da Stream Analytics projesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-vs) oluşturun.
* [Azure DevOps](https://visualstudio.microsoft.com/team-services/) kuruluşu oluşturun.

## <a name="configure-nuget-package-dependency"></a>NuGet paketi bağımlılığını yapılandırma
Rastgele bir makinede otomatik derleme ve otomatik dağıtım yapmak için `Microsoft.Azure.StreamAnalytics.CICD` NuGet paketini kullanmanız gerekir. Bu, Stream Analytics Visual Studio projelerinin sürekli tümleştirme ve dağıtım işlemini destekleyen MSBuild, yerel çalıştırma ve dağıtım araçlarını sağlar. Daha fazla bilgi için bkz. [Stream Analytics CI/CD araçları](stream-analytics-tools-for-visual-studio-cicd.md).

Proje dizininize **packages.config** ekleyin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
<package id="Microsoft.Azure.StreamAnalytics.CICD" version="1.0.0" targetFramework="net452" />
</packages>
```

## <a name="share-your-visual-studio-solution-to-a-new-azure-repos-git-repo"></a>Visual Studio çözümünüzü yeni bir Azure Repos Git deposunda paylaşma

Derlemeler oluşturabilmek için uygulamanızın kaynak dosyalarını Azure DevOps’daki bir projede paylaşın.  

1. Visual Studio’nun sağ alt köşesindeki durum çubuğunda **Kaynak Denetimine Ekle**’yi ve sonra da **Git**’i seçerek projeniz için yeni bir yerel Git deposu oluşturun. 

2. **Takım Gezgini**’ndeki **Eşitleme** görünümünde **Azure DevOps Services’a Gönder**’in altında yer alan **Git Deposunda Yayımla** düğmesini seçin.

   ![Git Deposunu gönderme](./media/stream-analytics-tools-visual-studio-cicd-vsts/publishgitrepo.png)

3. E-postanızı doğrulayın ve **Azure DevOps Services Etki Alanı** açılır listesinde kuruluşunuzu seçin. Deponuzun adını girin ve **Depoyu yayımla**’yı seçin.

   ![Git deposunu gönderme](./media/stream-analytics-tools-visual-studio-cicd-vsts/publishcode.png)

    Depoyu yayımlamak, kuruluşunuzda yerel depoyla aynı ada sahip yeni bir proje oluşturur. Var olan projede depoyu oluşturmak için, **Depo adının** yanındaki **Gelişmiş**’e tıklayın ve bir proje seçin. **Web üzerinde görüntüleyin**’i seçerek kodunuzu tarayıcıda görüntüleyebilirsiniz.
 
## <a name="configure-continuous-delivery-with-azure-devops"></a>Azure DevOps ile sürekli teslimi yapılandırma
Azure Pipelines derleme işlem hattı, sırayla yürütülen derleme adımlarından oluşturulmuş bir iş akışını açıklar. [Azure Pipelines derleme işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) hakkında daha fazla bilgi edinin. 

Bir Azure Pipelines yayın işlem hattı, kümeye uygulama paketi dağıtan bir iş akışını açıklar. Derleme işlem hattı ve yayın işlem hattı birlikte kullanıldığında kaynak dosyalarla başlayan ve kümenizde çalışan bir uygulamayla biten iş akışının tamamını yürütür. Azure Pipelines [yayın işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) hakkında daha fazla bilgi edinin.

### <a name="create-a-build-pipeline"></a>Derleme işlem hattı oluşturma
Bir web tarayıcısı açın ve [Azure DevOps](https://app.vsaex.visualstudio.com/)’da az önce oluşturduğunuz projeye gidin. 

1. **Derleme ve Yayınlama** sekmesi altında **Derlemeler**’i ve **+Yeni**’yi seçin.  **Azure DevOps Services Git**’i ve **Devam Et**’i seçin.
    
    ![Kaynak seçme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-source.png)

2. Boş bir işlem hattıyla başlamak için **Şablon seçin**’de **Boş İşlem**’e tıklayın.
    
    ![Derleme şablonu seçme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-template.png)

3. **Tetikleyiciler**’in altında **Sürekli tümleştirmeyi etkinleştir** tetikleyici durumunu işaretleyerek sürekli tümleştirmeyi etkinleştirin.  Derlemeyi el ile başlatmak için **Kaydet ve kuyruğa al**’ı seçin. 
    
    ![Tetikleyici durumu](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-trigger.png)

4. Derlemeler gönderme veya iade işlemleriyle de tetiklenir. Derlemenizin ilerleme durumunu denetlemek için **Derlemeler** sekmesine geçin.  Derlemenin başarıyla yürütüldüğünü doğruladıktan sonra, uygulamanızı kümeye dağıtan bir yayın işlem hattı belirlemeniz gerekir. Derleme işlem hattınızın yanındaki üç noktaya sağ tıklayın ve **Düzenle**’yi seçin.

5.  **Görevler**’de **Aracı kuyruğu** olarak "Hosted" girin.
    
    ![Aracı kuyruğunu seçme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-agent-queue.png) 

6. **1. Aşama**’da **+** öğesine tıklayın ve bir **NuGet** görevi ekleyin.
    
    ![NuGet görevi ekleme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-nuget.png)

7. **Gelişmiş**’i genişletin ve **Hedef dizini**’ne `$(Build.SourcesDirectory)\packages` ekleyin. Diğer varsayılan NuGet yapılandırması değerlerini olduğu gibi bırakın.

   ![NuGet görevini yapılandırma](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-nuget-config.png)

8. **1. Aşama**’da **+** seçeneğine tıklayın ve bir **MSBuild** görevi ekleyin.

   ![MSBuild Görevi ekleme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-msbuild-task.png)

9. **MSBuild Bağımsız Değişkenleri**’ni şöyle değiştirin:

   ```
   /p:CompilerTaskAssemblyFile="Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll"  /p:ASATargetsFilePath="$(Build.SourcesDirectory)\packages\Microsoft.Azure.StreamAnalytics.CICD.1.0.0\build\StreamAnalytics.targets"
   ```

   ![MSBuild görevini yapılandırma](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-msbuild.png)

10. **1. Aşama**’da **+** seçeneğine tıklayın ve **Azure Kaynak Grubu Dağıtımı** görevini ekleyin. 
    
    ![Azure Kaynak Grubu Dağıtımı görevi ekleme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-deploy.png)

11. **Azure Ayrıntıları**’nı genişletin ve yapılandırmayı aşağıdakilerle doldurun:
    
    |**Ayar**  |**Önerilen değer**  |
    |---------|---------|
    |Abonelik  |  Aboneliğinizi seçin.   |
    |Eylem  |  Kaynak grubu oluşturma veya güncelleştirme   |
    |Kaynak Grubu  |  Kaynak grubu adı girin.   |
    |Şablon  | [Çözüm yolunuz]\bin\Debug\Deploy\\[Projenizin adı].JobTemplate.json   |
    |Şablon parametreleri  | [Çözüm yolunuz]\bin\Debug\Deploy\\[Projenizin adı].JobTemplate.parameters.json   |
    |Şablon parametrelerini geçersiz kılma  | Metin kutusunda geçersiz kılmak için şablon parametrelerini yazın. Örneğin, –storageName fabrikam –adminUsername $(vmusername) -adminPassword $(password) –azureKeyVaultName $(fabrikamFibre). Bu özellik isteğe bağlı olsa da, anahtar parametreleriniz geçersiz kılınmamışsa derlemeniz hatalı bitecektir.    |
    
    ![Özellikleri ayarlama](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-deploy-2.png)

12. Derleme işlem hattınızı test etmek için **Kaydet ve Kuyruğa Al**’a tıklayın.
    
    ![Geçersiz kılma parametrelerini ayarlama](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-save-queue.png)

### <a name="failed-build-process"></a>Başarısız derleme işlemi
Derleme işlem hattınızın **Azure Kaynak Grubu Dağıtımı** görevinde şablon parametrelerini geçersiz kılmadıysanız, null dağıtım parametreleri nedeniyle hata alabilirsiniz. Derleme işlem hattına dönün ve hatayı gidermek için null parametreleri geçersiz kılın.

   ![Derleme işlemi başarısız oldu](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-process-failed.png)

### <a name="commit-and-push-changes-to-trigger-a-release"></a>Yayını tetiklemek için değişiklikleri işleme ve gönderme
Azure DevOps'da yapılan bazı kod değişikliklerini denetleyerek sürekli tümleştirme işlem hattının çalıştığını doğrulayın.    

Siz kodunuzu yazarken, değişiklikleriniz Visual Studio tarafından otomatik olarak izlenir. Sağ alt kısımdaki durum çubuğunda bekleyen değişiklikler simgesini seçerek, değişiklikleri yerel Git deponuza işleyin.

1. Takım Gezgini'ndeki **Değişiklikler** görünümünde, güncelleştirmenizi açıklayan bir ileti ekleyin ve değişikliklerinizi işleyin.

    ![Değişiklikleri işleme ve gönderme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-push-changes.png)

2. Yayımlanmamış değişiklikler durum çubuğu simgesini veya Takım Gezgini'nde Eşitleme görünümünü seçin. Azure DevOps’da kodunuzu güncelleştirmek için **Gönder**'i seçin.

    ![Değişiklikleri işleme ve gönderme](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-push-changes-2.png)

Değişikliklerin Azure DevOps Services’a gönderilmesi otomatik olarak derlemeyi tetikler.  Derleme işlem hattı başarıyla tamamlandığında, otomatik olarak bir yayın oluşturulur ve kümede işi güncelleştirme işlemini başlatır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu öğreticiyle oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın.  
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio için Azure Stream Analytics araçlarını kullanarak sürekli tümleştirme ve dağıtım işlemi ayarlama hakkında daha fazla bilgi edinmek için, CI/CD işlem hattını ayarlama makalesiyle devam edin:

> [!div class="nextstepaction"]
> [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
