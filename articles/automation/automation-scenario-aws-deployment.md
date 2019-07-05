---
title: Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme
description: Bu makalede bir Amazon Web hizmeti VM oluşturmayı otomatikleştirmek için Azure Otomasyonu'nu nasıl yapılacağı açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 31eda5a293f7154d32c508b7b9c1bf0f9f604f2e
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476906"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure otomasyonu senaryosu - bir AWS sanal makinesi sağlama
Bu makalede, Amazon Web hizmeti (AWS) aboneliğiniz bir sanal makinesi sağlama ve bu VM – AWS "VM etiketleme olarak" başvuran olan belirli bir ad vermek için Azure Otomasyonu'nu nasıl yararlanabileceğiniz öğrenin.

## <a name="prerequisites"></a>Önkoşullar
Bu makalenin amaçları için Azure Otomasyonu hesabı ve bir AWS aboneliği olması gerekir. Bir Azure Otomasyonu hesabının ayarlanması ve AWS abonelik kimlik bilgilerinizle yapılandırması hakkında daha fazla bilgi için gözden [Amazon Web Hizmetleri ile kimlik doğrulamasını yapılandırma](automation-config-aws-account.md). Bu hesap oluşturuldu veya AWS abonelik kimlik bilgilerinizle devam etmeden önce aşağıdaki adımları Bu hesapta başvuru olarak güncelleştirildi.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Services PowerShell modülü dağıtma
VM'nizi runbook sağlama işlemini gerçekleştirmek için AWS PowerShell modülünden yararlanır. AWS abonelik kimlik bilgilerinizle yapılandırılmış Otomasyon hesabınızı eklemek için aşağıdaki adımları gerçekleştirin.  

1. Web tarayıcınızı açın ve gidin [PowerShell Galerisi](https://www.powershellgallery.com/packages/AWSPowerShell/) tıklayın **Dağıt düğmesine Azure Otomasyonu**.<br><br> ![AWS PS modül içeri aktarma](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Azure oturum açma sayfası ve kimliklerini doğruladıktan sonra alınır, Azure portalına yönlendirilir ve şu sayfaya ile sunulan:<br><br> ![İçeri aktarma modül sayfası](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. ' E tıklayın, Otomasyon hesabı seçin **Tamam** dağıtımını başlatmak için.

   > [!NOTE]
   > Bir PowerShell modülü Azure Automation'a içeri aktarma, bu da cmdlet'ler ayıklıyor ve modül içeri aktarma ve cmdlet'ler ayıklama tamamen tamamlanana kadar bu etkinlikleri görünmez olsa da. Bu işlem birkaç dakika sürebilir.  
   > <br>

1. Azure portalında, adım 3'te başvurulan Otomasyon hesabınızı açın.
2. Tıklayın **varlıklar** kutucuğuna ve **varlıklar** bölmesinde **modülleri** Döşe.
3. Üzerinde **modülleri** sayfasında, gördüğünüz **AWSPowerShell** modül listesinde.

## <a name="create-aws-deploy-vm-runbook"></a>Oluşturma AWS VM runbook dağıtma
AWS PowerShell modülü dağıtıldıktan sonra artık bir sanal makinede bir PowerShell betiğini kullanarak AWS sağlanmasını otomatikleştirmek için runbook yazabilirsiniz. Aşağıdaki adımlar, Azure automation'da yerel PowerShell Betiği yararlanmak nasıl ekleyebileceğiniz gösterilmektedir.  

> [!NOTE]
> Ek seçenekleri ve bu betik ile ilgili daha fazla bilgi için lütfen [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-AwsVM/).
> 

1. PowerShell Betiği yeni AwsVM PowerShell Galerisi'nden bir PowerShell oturumu açın ve aşağıdakileri yazarak indirin:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portalından Otomasyon hesabınızı açın ve seçin **runbook'ları** bölümünde **süreç otomasyonu** soldaki.  
3. Gelen **runbook'ları** sayfasında **runbook Ekle**.
4. Üzerinde **runbook Ekle** bölmesinde **hızlı Oluştur** (yeni bir runbook oluşturma).
5. Üzerinde **Runbook** Özellikler bölmesi, bir ad ve bağlantı kurmak, runbook için ad kutusuna yazın **Runbook türü** açılan listesini seçin **PowerShell**ve ardından**Oluşturma**.<br><br> ![Runbook bölmesi oluşturun](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell Runbook'unu Düzenle sayfası görüntülendiğinde, kopyalayın ve PowerShell betiğini yazma tuvalinde runbook'a yapıştırın.<br><br> ![Runbook PowerShell Betiği](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Aşağıdaki örnek PowerShell Betiği ile çalışırken dikkat edin:
    > 
    > * Runbook'un varsayılan parametre değerlerini içerir. Tüm varsayılan değerleri değerlendirmek ve gerekirse güncelleştirin.
    > * Bir kimlik bilgisi varlığı değerinden farklı şekilde adlandırılmış gibi AWS kimlik bilgilerinizi, depoladığınız, **AWScred**, komut satırında 57 uygun şekilde eşleşecek şekilde güncelleştirmeniz gerekir.  
    > * PowerShell, özellikle bu örnek runbook ile AWS CLI komutları ile çalışırken, AWS bölge belirtmeniz gerekir. Aksi takdirde, cmdlet başarısız. Görünüm AWS konu [AWS bölge belirtin](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) PowerShell belge için daha fazla ayrıntı için AWS araçları.  
    >

7. AWS aboneliğinizden görüntü adlarının bir listesini almak için PowerShell ISE'yi başlatma ve AWS PowerShell modülünü içeri aktarın. AWS karşı kimlik doğrulaması değiştirerek **Get-AutomationPSCredential** ISE ortamınızda **AWScred = Get-Credential**. Bu kimlik bilgilerinizi ister ve sunabilir, **erişim anahtarı kimliği** kullanıcı adı ve **gizli erişim anahtarı** parola. Aşağıdaki örneğe bakın:  

        ```powershell
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
        ```
        
    Aşağıdaki çıktı döndürülür:<br><br>
   ![AWS görüntü alma](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Görüntü adlarının bir runbook'ta başvurulan bir Otomasyon değişkeni içinde yapıştırın **$InstanceType**. Bu örnekte, olduğundan ücretsiz AWS kullanarak katmanlı abonelik, kullandığınız **t2.micro** , runbook örneği için.  
9. Runbook'u kaydedin ve ardından tıklayın **Yayımla** runbook'u yayımlayamadı ve ardından **Evet** istendiğinde.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM runbook'u test etme
Runbook'u test etme ile devam etmeden önce size birkaç şey doğrulamanız gerekiyor. Bu avantajlar şunlardır:  

* AWS karşı kimlik doğrulaması için bir varlık adlandırılan oluşturulup **AWScred** veya komut dosyası, kimlik bilgisi varlığının adını başvurmak için güncelleştirildi.    
* Azure Otomasyonu'nda AWS PowerShell modülünü içeri aktarıldı  
* Yeni runbook oluşturuldu ve parametre değerlerini doğrulandı ve gerektiğinde güncelleştirildi  
* **Ayrıntılı kayıtları günlüğe** ve isteğe bağlı olarak **ilerleme durumu kayıtlarını günlüğe** runbook ayarı altında **günlüğe kaydetme ve izleme** ayarlanmış **üzerinde**.<br><br> ![Runbook günlüğe kaydetme ve izleme](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Runbook'u başlatmak için bu nedenle tıklatın istediğiniz **Başlat** ve ardından **Tamam** zaman Runbook'u Başlat bölmeyi açar.
2. Runbook'u Başlat bölmede sağlayan bir **VMname**. Betik daha önce yapılandırılmış bir parametre için varsayılan değerleri kabul edin. Tıklayın **Tamam** runbook işini başlatmak için.<br><br> ![New-AwsVM runbook başlatma](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölmesini kapatın.
4. İş ve görünümü çıktılarını ilerlemesini görüntüleyebileceğiniz **akışları** seçerek **tüm günlükler** kutucuğunda runbook işi sayfasında.<br><br> ![Stream çıkış](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM sağlandıktan onaylamak için AWS yönetim konsoluna oturum açmış olduğunuz değil.<br><br> ![AWS konsolunda dağıtılan VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)


