---
title: Azure Active Directory ile HPC paketi küme | Microsoft Docs
description: Azure Microsoft HPC Pack 2016 kümede Azure Active Directory ile tümleştirme öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/16/2017
ms.author: danlep
ms.openlocfilehash: bb0e878c4e987d111a535603cede25c639087ca7
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
ms.locfileid: "25452581"
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak Azure HPC Pack kümede yönetme
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) ile tümleştirmeyi destekleyen [Azure Active Directory](../../active-directory/index.md) (Azure AD) Azure HPC Pack kümede dağıtmak Yöneticiler için.



Aşağıdaki yüksek düzey görevler için bu makaledeki adımları izleyin: 
* HPC Pack kümenizi Azure AD kiracınıza el ile tümleştirme
* Yönetmek ve azure'da HPC Pack kümenizdeki işlerini zamanla 

HPC Pack küme çözümünü Azure AD ile tümleştirme, diğer uygulamalar ve hizmetler tümleştirmek için standart adımları izler. Bu makale, Azure AD'de temel kullanıcı yönetimi ile bildiğinizi varsayar. Daha fazla bilgi ve arka plan için bkz: [Azure Active Directory belgelerindeki](../../active-directory/index.md) ve aşağıdaki bölümü.

## <a name="benefits-of-integration"></a>Tümleştirme yararları


Azure Active Directory (Azure AD) olduğu bulut çözümleri çoklu oturum açma (SSO) erişimi sağlayan bir çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti.

HPC Pack küme Azure AD ile tümleştirilmesi, aşağıdaki hedeflere ulaşmak yardımcı olabilir:

* Geleneksel Active Directory etki alanı denetleyicisi HPC Pack kümeden kaldırın. Bu, bu, iş ve faturalamak dağıtım işlemi için gerekli değilse, küme koruma maliyetlerini azaltmaya yardımcı olabilir.
* Azure AD tarafından yapılmadan aşağıdaki yararları yararlanın:
    *   Çoklu oturum açma 
    *   Azure HPC paketi küme için bir yerel AD kimliği kullanma 

    ![Azure Active Directory ortamı](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Ön koşullar
* **Azure sanal makinelerinde dağıtılan HPC Pack 2016 küme** - adımları için bkz: [Azure HPC Pack 2016 kümede dağıtmak](hpcpack-2016-cluster.md). Baş düğüm DNS adını ve bu makaledeki adımları tamamlamak için bir Küme Yöneticisi kimlik bilgileri gerekir.

  > [!NOTE]
  > Azure Active Directory Tümleştirme HPC Pack HPC Pack 2016 önce sürümlerinde desteklenmez.



* **İstemci bilgisayar** -HPC Pack istemci yardımcı programları çalıştırmak için bir Windows veya Windows Server istemci bilgisayar gerekir. Yalnızca iş göndermek için HPC Pack web portalı veya REST API'yi kullanmak istiyorsanız, tercih ettiğiniz herhangi bir istemci bilgisayarı kullanabilirsiniz.

* **HPC Pack istemci yardımcı programları** -HPC Pack istemci yardımcı programları Microsoft Download Center'dan gelen kullanılabilir boş yükleme paketini kullanarak istemci bilgisayara yükleyin.


## <a name="step-1-register-the-hpc-cluster-server-with-your-azure-ad-tenant"></a>1. adım: Azure AD kiracınıza HPC küme sunucusu kaydetme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız için birden fazla Azure AD kiracısı erişmenizi, sağ üst köşedeki hesabınızda'ı tıklatın. Sonra portal oturumunuz istenen Kiracı ayarlayın. Dizindeki kaynaklara erişim izni olması gerekir. 
3. Tıklatın **Azure Active Directory** sol Hizmetleri Gezinti bölmesinde **kullanıcılar ve gruplar**ve kullanıcı hesapları zaten oluşturuldu veya yapılandırılmış olmadığından emin olun.
4. İçinde **Azure Active Directory**, tıklatın **uygulama kayıtlar** > **yeni uygulama kaydı**. Aşağıdaki bilgileri girin:
    * **Ad** -HPCPackClusterServer
    * **Uygulama türü** - seçin **Web uygulaması / API**
    * **URL oturum açma**-varsayılan örnek için temel URL`https://hpcserver`
    * **Oluştur**'a tıklayın.
5. Uygulama eklendikten sonra seçin **uygulama kayıtlar** listesi. Ardından **ayarları** > **özellikleri**. Aşağıdaki bilgileri girin:
    * Seçin **Evet** için **çoklu kiralanan**.
    * Değişiklik **uygulama kimliği URI'si** için `https://<Directory_name>/<application_name>`. Değiştir `<Directory_name`> Azure AD kiracınız, örneğin, tam adıyla `hpclocal.onmicrosoft.com`ve yerine `<application_name>` daha önce seçtiğiniz ada sahip.
6. **Kaydet** düğmesine tıklayın. Kaydetme tamamlandığında uygulama sayfasında tıklayın **bildirim**. Bildirim bularak Düzenle `appRoles` ayarlama ve aşağıdaki uygulama rolü ekleme ve ardından **kaydetmek**:

  ```json
  "appRoles": [
     {
     "allowedMemberTypes": [
         "User",
         "Application"
     ],
     "displayName": "HpcAdminMirror",
     "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
     "isEnabled": true,
     "description": "HpcAdminMirror",
     "value": "HpcAdminMirror"
     },
     {
     "allowedMemberTypes": [
         "User",
         "Application"
     ],
     "description": "HpcUsers",
     "displayName": "HpcUsers",
     "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
     "isEnabled": true,
     "value": "HpcUsers"
     }
  ],
  ```
7. İçinde **Azure Active Directory**, tıklatın **kurumsal uygulamalar** > **tüm uygulamaları**. Seçin **HPCPackClusterServer** listeden.
8. Tıklatın **özellikleri**, değiştirip **gerekli kullanıcı ataması** için **Evet**. **Kaydet** düğmesine tıklayın.
9. Tıklatın **kullanıcılar ve gruplar** > **Kullanıcı Ekle**. Bir kullanıcı seçin ve bir rol seçin ve ardından **atamak**. Kullanılabilir roller (HpcUsers veya HpcAdminMirror) birini kullanıcıya atayın. Dizinde ek kullanıcılar ile bu adımı yineleyin. Küme kullanıcılar hakkında bilgi için bkz: [küme kullanıcıları yönetme](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a>2. adım: Azure AD kiracınıza HPC küme istemci Kaydet

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız için birden fazla Azure AD kiracısı erişmenizi, sağ üst köşedeki hesabınızda'ı tıklatın. Sonra portal oturumunuz istenen Kiracı ayarlayın. Dizindeki kaynaklara erişim izni olması gerekir. 
3. İçinde **Azure Active Directory**, tıklatın **uygulama kayıtlar** > **yeni uygulama kaydı**. Aşağıdaki bilgileri girin:

    * **Ad** -HPCPackClusterClient    
    * **Uygulama türü** - seçin **yerel**
    * **Yeniden yönlendirme URI'si** - `http://hpcclient`
    * **Oluştur**'a tıklayın

4. Uygulama eklendikten sonra seçin **uygulama kayıtlar** listesi. Kopya **uygulama kimliği** değer ve kaydedin. Bu daha sonra uygulamanızın yapılandırırken gerekir.

5. Tıklatın **ayarları** > **gerekli izinleri** > **Ekle** > **bir API seçin**. Arama ve (1. adımda oluşturduğunuz) HpcPackClusterServer uygulamayı seçin.

6. İçinde **erişimi etkinleştir** sayfasında, **erişim HpcClusterServer**. Sonra da **Bitti**’ye tıklayın.


## <a name="step-3-configure-the-hpc-cluster"></a>3. adım: HPC küme yapılandırma

1. Azure HPC Pack 2016 baş düğümünde bağlayın.

2. HPC PowerShell'i başlatın.

3. Şu komutu çalıştırın:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcPackClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    Burada

    * `AADTenant`Azure AD Kiracı adı gibi belirtir`hpclocal.onmicrosoft.com`
    * `AADClientAppId`2. adımda oluşturduğunuz uygulamanın uygulama Kimliğini belirtir.

4. Baş düğüm yapılandırmasına bağlı olarak aşağıdakilerden birini yapın:

    * Bir tek baş düğüm HPC Pack kümede HpcScheduler hizmetini yeniden başlatın.

    * Birden çok baş düğümü olan bir HPC Pack kümesinde baş düğümünde HpcSchedulerStateful hizmetini yeniden başlatmak için aşağıdaki PowerShell komutlarını çalıştırın:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName "fabric:/HpcApplication/SchedulerStatefulService"

    ```

## <a name="step-4-manage-and-submit-jobs-from-the-client"></a>Adım 4: Yönetmek ve istemciden işleri gönderme

HPC Pack istemci yardımcı programları bilgisayarınıza yüklemek için Microsoft Download Center'dan gelen HPC Pack 2016 Kurulum dosyaları (tam yükleme) indirin. Yüklemeye başlamak için kurulum seçeneğini seçin **HPC Pack istemci yardımcı programları**.

Yükleme sırasında kullanılan sertifika istemci bilgisayarı hazırlamak için [HPC Küme kurulumu](hpcpack-2016-cluster.md) istemci bilgisayardaki. Ortak sertifikayı yüklemek için standart Windows sertifika yönetimi yordamları kullanın **Sertifikalar – Geçerli kullanıcı** > **güvenilen kök sertifika yetkilileri** depolar. 

Şimdi, HPC Pack komutları çalıştırmak veya HPC Pack İş Yöneticisi'ni GUI göndermek ve Azure AD hesabının kullanarak küme işlerini yönetmek için kullanın. İş gönderme seçenekleri için bkz: [bir HPC paketi için gönderme HPC işleri küme Azure'da](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> İlk olarak Azure HPC Pack kümede bağlanmaya çalıştığında, açılır pencereleri görüntülenir. Oturum açmak için Azure AD kimlik bilgilerinizi girin. Belirteç sonra önbelleğe alınır. Kimlik doğrulaması değişiklikleri veya önbellek temizlenmediği sürece Azure kümedeki sonraki bağlantılar önbelleğe alınmış belirteci kullanın.
>
  
Örneğin, yukarıdaki adımları tamamladıktan sonra bir şirket içi istemci işlerden şu şekilde sorgulayabilirsiniz:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Azure AD tümleştirmesiyle iş gönderme için kullanışlı cmdlet'leri 

### <a name="manage-the-local-token-cache"></a>Yerel belirteç önbelleği yönetme

HPC Pack 2016 yerel belirteç önbelleği yönetmek için aşağıdaki HPC PowerShell cmdlet'lerini sağlar. Bu cmdlet, işleri etkileşimsiz göndermek için faydalıdır. Aşağıdaki örneğe bakın:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a>Azure AD hesabının kullanarak işleri göndermek için kimlik bilgilerini ayarlayın 

Bazı durumlarda, işi HPC küme kullanıcıdan (çalıştıran bir etki alanı kullanıcısı olarak; baş düğümünde bir yerel kullanıcı olarak çalıştırılan bir etki alanına katılmış HPC Kümesi için bir etki alanına katılmış HPC küme) altında çalıştırmak isteyebilirsiniz.

1. Kimlik bilgilerini ayarlamak için aşağıdaki komutları kullanın:

    ```powershell
    $localUser = "<username>"

    $localUserPassword="<password>"

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Ardından işi şu şekilde gönderin. İş/görevi $localUser altında işlem düğümlerinde çalışır.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Varsa `–Credential` ile belirtilmemiş `Submit-HpcJob`, iş veya görevi yerel eşlenen kullanıcı olarak Azure AD hesabının altında çalışır. (HPC küme görevi çalıştırmak için Azure AD hesabının aynı ada sahip yerel bir kullanıcı oluşturur.)
    
3. Azure AD hesabı için genişletilmiş veri ayarlayın. Bu, bir MPI işi Azure AD hesabının kullanarak Linux düğümleri üzerinde çalışırken yararlıdır.

   * Azure AD hesabı için genişletilmiş veri kümesi

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Genişletilmiş veri kümesi ve HPC küme kullanıcı olarak çalıştırın
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

