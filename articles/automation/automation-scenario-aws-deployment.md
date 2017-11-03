---
title: "Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme | Microsoft Docs"
description: "Bu makalede Azure Otomasyonu bir Amazon Web hizmeti VM oluşturmayı otomatikleştirmek için nasıl kullanılacağı gösterilmektedir"
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 1d85c01a-d795-4523-8194-84fc15b53838
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: tiandert; bwren
ms.openlocfilehash: 828f9e2cc9a39e54933cd0e0db7273efa460d0c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Otomasyonu senaryo - bir AWS sanal makine sağlama
Bu makalede, Azure Automation'ı Amazon Web hizmeti (AWS) aboneliğinizde bir sanal makine sağlama ve "VM etiketleme olarak" AWS başvurduğu belirli bir adı – bu VM vermek için nasıl yararlanabilirsiniz göstermektedir.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede amaçları doğrultusunda, bir Azure Otomasyonu hesabı ve bir AWS aboneliği olması gerekir. Bir Azure Otomasyonu hesabının ayarlanması ve AWS aboneliği kimlik bilgilerinizle yapılandırması hakkında daha fazla bilgi için gözden [Amazon Web Hizmetleri ile kimlik doğrulaması yapılandırma](automation-config-aws-account.md).  Bu hesap oluşturulan veya AWS abonelik bilgilerinizi devam etmeden önce aşağıdaki adımlar bu hesapta başvurur olarak güncelleştirildi.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Hizmetleri PowerShell modülü dağıtma
Bizim VM runbook sağlama işini yapmak için AWS PowerShell modülü özelliğinden yararlanır. Modül AWS aboneliği kimlik bilgilerinizle yapılandırılmış Otomasyon hesabınızı eklemek için aşağıdaki adımları gerçekleştirin.  

1. Web tarayıcınızı açın ve gidin [PowerShell Galerisi](http://www.powershellgallery.com/packages/AWSPowerShell/) ve tıklayın **Azure Otomasyonu düğmesi Dağıt**.<br><br> ![AWS PS modülü içe aktarma](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Azure oturum açma sayfası ve kimlik doğrulaması sonra alınır, size Azure portalına yönlendirilir ve olması aşağıdaki sayfasıyla sunulan.<br><br> ![İçeri aktarma modül sayfası](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Kaynak grubundan seçin **kaynak grubu** aşağı açılan listesinde ve parametreleri bölmesinde, aşağıdaki bilgileri sağlayın:
   
   * Gelen **yeni veya var olan Otomasyon hesabı (dize)** aşağı açılan listesinde seçin **varolan**.  
   * İçinde **Otomasyon hesabı adı (dize)** kutusuna AWS aboneliğiniz için kimlik bilgilerini içeren Otomasyon hesabı tam adı yazın.  Adlı bir adanmış hesabı oluşturduysanız, örneğin, **AWSAutomation**, bu kutuya yazın sonra.
   * Uygun bölgesi seçin **Otomasyon hesabı konumu** aşağı açılan liste.
4. Gerekli bilgileri girmeyi tamamladığınızda tıklatın **oluşturma**.
   
   > [!NOTE]
   > Tutarak bir PowerShell modülü Azure Automation'a içeri, Ayrıca cmdlet'leri ayıkladıktan ve modül içeri aktarma ve cmdlet'leri ayıklama tamamen tamamlanana kadar bu etkinlikler görünmez. Bu işlem birkaç dakika sürebilir.  
   > <br>
   > 
   > 
5. Azure portalında 3. adımda başvurulan Automation hesabınızı açın.
6. Tıklayın **varlıklar** döşeme ve **varlıklar** bölmesinde, **modülleri** döşeme.
7. Üzerinde **modülleri** görürsünüz sayfa **AWSPowerShell** modül listesinde.

## <a name="create-aws-deploy-vm-runbook"></a>Oluşturma AWS VM runbook dağıtma
AWS PowerShell modülü dağıtıldıktan sonra biz şimdi bir sanal makinede bir PowerShell Betiği kullanılarak AWS sağlama otomatikleştirmek için runbook yazabilirsiniz. Aşağıdaki adımlar, Azure automation'da yerel PowerShell betik yararlanmak nasıl gösterecek.  

> [!NOTE]
> Daha fazla seçenekleri ve bu komut dosyası ile ilgili bilgi için lütfen şu adresi ziyaret [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. PowerShell komut dosyasını yeni AwsVM PowerShell Galerisi'nden bir PowerShell oturumu'ni açıp aşağıdakileri yazarak yükleyin:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portalından Automation hesabınızı açın ve seçin **Runbook'lar** bölümünün altında **işlem Otomasyonu** soldaki.  
3. Gelen **Runbook'lar** sayfasında, **runbook Ekle**.
4. Üzerinde **runbook Ekle** bölmesinde, **hızlı Oluştur** (yeni bir runbook oluşturma).
5. Üzerinde **Runbook** Özellikler bölmesinde, bir gelen ve giden runbook'unuzu için ad ad kutusuna yazın **Runbook türü** aşağı açılan listesinde seçin **PowerShell**ve ardından **Oluşturma**.<br><br> ![Runbook bölmesi oluşturun](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell Runbook'unu Düzenle sayfası görüntülendiğinde, kopyalayıp tuvale yazma runbook'a PowerShell betiğini yapıştırın.<br><br> ![Runbook PowerShell Betiği](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Lütfen aşağıdaki PowerShell komut dosyası örneği ile çalışırken dikkat edin:
    > 
    > * Runbook varsayılan parametre değerlerini içerir. Lütfen tüm varsayılan değerlerini değerlendirmek ve gerekirse güncelleştirin.
    > * Daha farklı bir kimlik bilgisi varlığı adlı gibi AWS kimlik bilgilerinizi depolanan durumunda **AWScred**, komut satırında buna göre eşleşecek şekilde 57 güncelleştirmeniz gerekir.  
    > * PowerShell, özellikle bu örnek runbook ile AWS CLI komutları ile çalışırken, AWS bölge belirtmeniz gerekir. Aksi takdirde cmdlet'leri başarısız olur.  Görünüm AWS konu [AWS bölge belirtin](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) daha ayrıntılı bilgi için PowerShell belge için AWS araçları.  
    >

7. AWS aboneliğinizden görüntü adlarının bir listesini almak için PowerShell ISE başlatın ve AWS PowerShell modülünü içeri aktarın.  Değiştirerek karşı AWS kimlik doğrulaması **Get-AutomationPSCredential** ISE ortamınızda **AWScred Get-Credential =**.  Bu kimlik bilgilerinizi ister ve bunu sağlayabilir, **erişim anahtarı kimliği** kullanıcı adı için ve **gizli erişim anahtar** parolası.  Aşağıdaki örneğe bakın:  

        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    Aşağıdaki çıkış döndürdü:<br><br>
   ![AWS görüntüleri alma](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Kopyalayıp resim adları bir runbook'ta başvurulan bir Otomasyon değişkeninde **$InstanceType**. Bu örnekte, biz olduğundan katmanlı abonelik boş AWS kullanarak, kullanacağız **t2.micro** runbook örneğimizde için.  
9. Runbook'u kaydedin ve ardından **Yayımla** runbook'u yayımlamak için ve ardından **Evet** istendiğinde.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM runbook'u test etme
Runbook testi ile ilerlemeden önce birkaç doğrulayın gerekir. Bu avantajlar şunlardır:  

* AWS karşı kimlik doğrulaması için bir varlık çağrılan oluşturuldu **AWScred** veya komut dosyası, kimlik bilgisi varlığının adını başvuracak şekilde güncelleştirilmez.    
* Azure Otomasyonu'nda AWS PowerShell modülünü içeri aktarıldı  
* Yeni bir runbook oluşturulur ve parametre değerlerini doğrulandı ve gerektiğinde güncelleştirildi  
* **Ayrıntılı kayıtları günlüğe** ve isteğe bağlı olarak **oturum ilerleme durumu kayıtlarını** runbook ayarı altında **günlüğe kaydetme ve izleme** ayarlanmış **üzerinde**.<br><br> ![Runbook günlüğü ve izleme](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Runbook'u başlatmak için bu nedenle tıklatın istediğimizi **Başlat** ve ardından **Tamam** zaman Runbook'u Başlat bölmesini açar.
2. Runbook'u Başlat bölmesinde sağlayan bir **VMname**.  Komut dosyasında daha önce yapılandırılmış bir parametre için varsayılan değerleri kabul edin.  Tıklatın **Tamam** runbook işi başlatmak için.<br><br> ![Yeni AwsVM runbook başlatın](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Yeni oluşturduğumuz runbook için bir iş bölmesi açıldı. Bu bölmesini kapatın.
4. İlerleme çıktısı iş ve görünüm durumunu görüntülemek **akışları** seçerek **tüm günlükleri** döşeme runbook işi sayfasından.<br><br> ![Akış çıkışı](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM'i sağlayan onaylamak için AWS yönetim konsolunda oturum, şu anda günlüğe kaydedilir.<br><br> ![AWS Konsolu VM dağıtılan](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

