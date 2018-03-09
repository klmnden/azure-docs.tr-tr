---
title: "Azure Data Lake Store'a erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma"
description: "Bir Windows VM yönetilen hizmet kimliği (MSI) Azure Data Lake Store'a erişmek için nasıl kullanılacağını gösteren bir öğretici."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: skwan
ms.openlocfilehash: 14fa2794ceac5152e25878d23c2416d5e4dd7325
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Azure Data Lake Store'a erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanın

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici, bir yönetilen hizmet Kimliği'ni (MSI) bir Windows sanal makine (VM) için bir Azure Data Lake Store'a erişmek için nasıl kullanılacağını gösterir. Yönetilen hizmet kimliği Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Windows VM MSI etkinleştir 
> * Bir Azure Data Lake Store, VM erişim
> * VM kimliğini kullanarak bir erişim belirteci almak ve bir Azure Data Lake Store'a erişmek için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresindeki Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu öğretici için yeni bir Windows VM oluşturun.  Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3. Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4. Uygun seçin **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** içinde sanal makine oluşturmak, seçim **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar sayfasında, Varsayılanları tutun ve tıklatın **Tamam**.

   ![Alt görüntü metin](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir 

Bir VM MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. MSI etkinleştirme, VM için yönetilen bir kimlik oluşturmak üzere Azure söyler. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve MSI Azure Kaynak Yöneticisi'nde sağlar.

1. Seçin **sanal makine** MSI etkinleştirmek istediğiniz.  
2. Sol gezinti çubuğunda **yapılandırma**. 
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.  
   ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM uzantıları doğrulamak isterseniz, tıklatın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listede görüntülenir.

   ![Alt görüntü metin](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Azure Data Lake Store, VM erişim

Şimdi bir Azure Data Lake Store bulunan dosya ve klasörleri, VM erişim izni verebilir.  Bu adım için mevcut bir Data Lake Store kullanın veya yeni bir tane oluşturun.  Azure Portalı'nı kullanarak yeni bir Data Lake Store oluşturmak için bu izleyin [Azure Data Lake Store quickstart](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Ayrıca Azure CLI ve Azure PowerShell'de kullanın quickstarts olan [Azure Data Lake deposu belgeleri](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).

Data Lake Store, yeni bir klasör oluşturun ve okuma, yazma ve o klasördeki dosyaları yürütme VM MSI izni verin:

1. Azure portalında tıklatın **Data Lake Store** sol gezinti içinde.
2. Bu öğretici için kullanmak istediğiniz Data Lake Store'ı tıklatın.
3. Tıklatın **Veri Gezgini** komut çubuğunda.
4. Data Lake Store kök klasöründe seçilir.  Tıklatın **erişim** komut çubuğunda.
5. **Ekle**'ye tıklayın.  İçinde **seçin** alan, VM adını girin, örneğin **DevTestVM**.  Arama sonuçlarından VM seçmek için tıklayın ve ardından **seçin**.
6. Tıklatın **seçin izinleri**.  Seçin **okuma** ve **yürütme**, eklemek **bu klasör**ve eklediğiniz **erişim izni yalnızca**.  **Tamam**’a tıklayın.  İzni başarıyla eklenmesi gerekir.
7. Kapat **erişim** dikey.
8. Bu öğretici için yeni bir klasör oluşturun.  Tıklatın **yeni klasör** yeni klasör örneği için bir ad verin ve komut çubuğunda **TestFolder**.  **Tamam**’a tıklayın.
9. Oluşturduğunuz klasöre tıklayın ve ardından **erişim** komut çubuğunda.
10. 5. adım, tıklatın benzer **Ekle**, **seçin** alan VM adını girin, seçin ve tıklatın **seçin**.
11. Adım 6 için benzer **Select izinleri**seçin **okuma**, **yazma**, ve **yürütme**, eklemek **bu klasör**ve eklediğiniz **erişim izni girdisi ve varsayılan izin girdisi**.  **Tamam**’a tıklayın.  İzni başarıyla eklenmesi gerekir.

VM MSI şimdi tüm dosyaları oluşturduğunuz klasöre işlemleri yapabilirsiniz.  Data Lake Store, erişimi yönetme hakkında daha fazla bilgi okumak için bu makalede [Data Lake Store'da erişim denetimi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>VM MSI kullanarak bir erişim belirteci alın ve Azure Data Lake Store dosya sistemi çağırmak için kullanın

Azure Data Lake Store MSI kullanarak doğrudan erişim belirteçleri kabul edebilir destekler Azure AD kimlik doğrulama, yerel olarak alınır.  Data Lake Store dosya sistemi için kimlik doğrulaması yapmak için Data Lake Store dosya sistemi uç nokta "Bearer < ACCESS_TOKEN_VALUE >" biçiminde bir Authorization Üstbilgisi Azure AD tarafından verilen bir erişim belirteci gönderin.  Azure AD kimlik doğrulaması için Data Lake Store desteği hakkında daha fazla bilgi için okuma [Data Lake Store ile Azure Active Directory'yi kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory)

> [!NOTE]
> Data Lake Store dosya sistemi istemci SDK'ları henüz desteklemediği yönetilen hizmet kimliği.  Destek SDK'SININ eklendiğinde Bu öğretici güncelleştirilir.

Bu öğreticide, Data Lake Store dosya sistemi REST yapmak için PowerShell kullanarak REST API istekleri için kimlik doğrulaması. VM MSI kimlik doğrulaması için kullanılacak sanal makineden istekleri yapmanız gerekir.

1. Portalı'nda gidin **sanal makineleri**gidin ve Windows VM **genel bakış** tıklatın **Bağlan**.
2. Girin, **kullanıcıadı** ve **parola** Windows VM oluşturduğunuzda, eklediğiniz için. 
3. Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle açmak **PowerShell** uzak oturumda. 
4. PowerShell'in kullanarak `Invoke-WebRequest`, Azure Data Lake Store için bir erişim belirteci almak üzere yerel MSI uç nokta için bir istek olun.  "Https://datalake.azure.net/" Data Lake Store için kaynak tanımlayıcısıdır.  Data Lake kaynak tanımlayıcısını tam bir eşleşme yapar ve eğik önemlidir.

   ```powershell
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://datalake.azure.net/"} -Headers @{Metadata="true"}
   ```
    
   Yanıt için bir PowerShell nesnesi bir JSON nesnesinde dönüştürün. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Erişim belirteci yanıttan ayıklayın.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. PowerShell'in 'Invoke-WebRequest' kullanarak, kök klasöründe klasörleri listelemek için Data Lake Store'nın REST uç noktası için bir isteği oluşturun.  Bu, her şeyin doğru yapılandırıldığını denetlemek için basit bir yoludur.  Bu, yetkilendirme üst "Bearer" dize "B" büyük olması önemlidir.  Data Lake Store'da adını bulabilirsiniz **genel bakış** Azure Portal'daki Data Lake Store dikey bölümü.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Başarılı yanıt şuna benzer:

   ```powershell
   StatusCode        : 200
   StatusDescription : OK
   Content           : {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY", "blockSize":0,"accessTime":1507934941392, "modificationTime":1507944835699,"replication":0, "permission":"770","ow..."
   RawContent        : HTTP/1.1 200 OK
                       Pragma: no-cache
                       x-ms-request-id: b4b31e16-e968-46a1-879a-3474aa7d4528
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict-Transport-Security: ma...
   Forms             : {}
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b4b31e16-e968-46a1-879a-3474aa7d4528],
                       [x-ms-webhdfs-version, 17.04.22.00], [Status, 0x0]...}
   Images            : {}
   InputFields       : {}
   Links             : {}
   ParsedHtml        : System.__ComObject
   RawContentLength  : 556
   ```

6. Şimdi, Data Lake Store için bir dosyayı karşıya yüklemeyi deneyebilirsiniz.  İlk olarak, karşıya yüklemek için bir dosya oluşturun.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. PowerShell'in kullanarak `Invoke-WebRequest`, dosyayı karşıya yüklemeyi daha önce oluşturduğunuz klasöre Data Lake Store'nın REST uç noktası için bir istek olun.  Bu istek iki tedbirleri alır.  İlk adım, istekte ve dosyayı karşıya burada yönlendirme alın.  İkinci adımda, dosyayı gerçekten karşıya.  Bu öğreticide farklı değerler kullandıysanız uygun şekilde dosya ve klasör adını ayarlamak unutmayın. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Değerini inceleyin, `$HdfsRedirectResponse` aşağıdaki yanıtı gibi görünmelidir:

   ```powershell
   PS C:\> $HdfsRedirectResponse

   StatusCode        : 307
   StatusDescription : Temporary Redirect
   Content           : {}
   RawContent        : HTTP/1.1 307 Temporary Redirect
                       Pragma: no-cache
                       x-ms-request-id: b7ab492f-b514-4483-aada-4aa0611d12b3
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosn...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b7ab492f-b514-4483-aada-4aa0611d12b3], 
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

   Karşıya yükleme yeniden yönlendirme uç noktasına istek göndererek tamamlayın:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Başarılı yanıt görünümlü:

   ```powershell
   StatusCode        : 201
   StatusDescription : Created
   Content           : {}
   RawContent        : HTTP/1.1 201 Created
                       Pragma: no-cache
                       x-ms-request-id: 1e70f36f-ead1-4566-acfa-d0c3ec1e2307
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, 1e70f36f-ead1-4566-acfa-d0c3ec1e2307],
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

Diğer Data Lake Store dosya sistemi dosyalarına ekleme API'lerini kullanarak, dosyaları ve diğer indirin.

Tebrikler!  Bir VM MSI kullanarak Data Lake Store dosya sistemi doğruladınız.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](../active-directory/msi-overview.md).
- Yönetimi için Azure Resource Manager operations Data Lake Store kullanır.  Resource Manager kimliğini doğrulamak için bir VM MSI kullanma hakkında daha fazla bilgi için okuma [Resource Manager erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanan](https://docs.microsoft.com/azure/active-directory/msi-tutorial-linux-vm-access-arm).
- Daha fazla bilgi edinmek [Data Lake Store ile Azure Active Directory'yi kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).
- Daha fazla bilgi edinmek [REST API kullanarak Azure Data Lake Store dosya sistemi işlemleri](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-operations-rest-api) veya [WebHDFS dosya sistemi API'lerini](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Daha fazla bilgi edinmek [Data Lake Store'da erişim denetimi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.