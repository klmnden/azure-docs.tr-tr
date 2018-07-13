---
title: Azure Data Lake Store erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma
description: Bir Windows VM yönetilen hizmet kimliği (MSI) Azure Data Lake Store erişmek için nasıl kullanılacağını gösteren öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: daef85164793dd6183c41604f200864aabadf8d8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610890"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Azure Data Lake Store erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Azure Data Lake Store erişmek için bir Windows sanal makine (VM) için Yönetilen hizmet kimliği (MSI) kullanma işlemini gösterir. Yönetilen hizmet kimlikleri, Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Windows VM üzerinde MSI etkinleştir 
> * VM erişim için bir Azure Data Lake Store
> * VM kimliğini kullanarak bir erişim belirteci alma ve bir Azure Data Lake Store erişimi için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturun

Bu öğretici için yeni bir Windows VM'yi oluştururuz.  Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1. Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3. Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgilerini İşte oluşturulan sanal makineye oturum açma.
4. Uygun seçin **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** seçin, sanal makinenizi oluşturmak, **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. Sanal makine için boyutu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarları sayfasında, varsayılan değerleri koruyun ve tıklayın **Tamam**.

   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir 

VM MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. MSI etkinleştirmesine VM'niz için yönetilen bir kimlik oluşturmak için Azure'da söyler. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve Azure Resource Manager'daki MSI sağlar.

1. Seçin **sanal makine** MSI etkinleştirmek istiyorsanız.  
2. Sol gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**. 
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4. Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.  
   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM üzerinde hangi uzantıları doğrulamak isterseniz tıklayın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listesinde görünür.

   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Azure Data Lake Store için VM erişimi

Artık dosya ve klasörleri bir Azure Data Lake Store, VM erişimi verebilir.  Bu adım için mevcut bir Data Lake Store kullanma veya yeni bir tane oluşturun.  Azure portalını kullanarak yeni bir Data Lake Store oluşturmak için bu izleyin [Azure Data Lake Store hızlı](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Ayrıca, Azure PowerShell ve Azure CLI kullanan hızlı başlangıçlar vardır [Azure Data Lake Store belgeleri](~/articles/data-lake-store/data-lake-store-overview.md).

Data Lake Store, yeni bir klasör oluşturun ve okuma, yazma ve dosyaları bu klasörde yürütmek için VM MSI izninizi verin:

1. Azure portalında **Data Lake Store** sol taraftaki gezinti.
2. Bu öğretici için kullanmak istediğiniz Data Lake Store tıklayın.
3. Tıklayın **Veri Gezgini** komut çubuğunda.
4. Data Lake Store kök klasörüne seçilir.  Tıklayın **erişim** komut çubuğunda.
5. **Ekle**'ye tıklayın.  İçinde **seçin** alanında, sanal makinenizin adını girin, örneğin **DevTestVM**.  Arama sonuçlarından VM'nizi seçin ve ardından tıklayın **seçin**.
6. Tıklayın **izinleri seçin**.  Seçin **okuma** ve **yürütme**, ekleme **bu klasör**, olarak ekleyin **erişim izni yalnızca**.  **Tamam**’a tıklayın.  İzin başarıyla eklenmesi gerekir.
7. Kapat **erişim** dikey penceresi.
8. Bu öğretici için yeni bir klasör oluşturun.  Tıklayın **yeni klasör** yeni klasör örneği için bir ad verin ve komut çubuğunda **TestFolder**.  **Tamam**’a tıklayın.
9. Oluşturduğunuz klasöre tıklayın ve ardından tıklayın **erişim** komut çubuğunda.
10. 5. adım için benzer **Ekle**, **seçin** alanı, sanal Makinenizin adını girin, onu seçin ve tıklayın **seçin**.
11. Benzer 6. Adım'a tıklayın **Select izinleri**seçin **okuma**, **yazma**, ve **yürütme**, ekleme **Buklasör**, olarak ekleyin **erişim izni girdisi ve varsayılan izin girdisi**.  **Tamam**’a tıklayın.  İzin başarıyla eklenmesi gerekir.

VM MSI, artık tüm işlemleri dosyaları oluşturduğunuz klasöre gerçekleştirebilirsiniz.  Bu makalede Data Lake Store erişimi yönetme hakkında daha fazla bilgi okuyun için [Data Lake Store erişim denetimi](~/articles/data-lake-store/data-lake-store-access-control.md).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>VM MSI kullanarak bir erişim belirteci alma ve Azure Data Lake Store dosya sistemi çağırmak için kullanın

Azure Data Lake Store MSI kullanarak doğrudan erişim belirteçleri kabul edebilir destekleyen Azure AD kimlik doğrulaması, yerel olarak alınır.  Data Lake Store dosya sistemi için kimlik doğrulaması yapmak için Data Lake Store dosya sistemi uç noktası "Bearer < ACCESS_TOKEN_VALUE >" biçiminde bir yetkilendirme üst bilgisi için Azure AD tarafından verilen bir erişim belirteci gönderin.  Azure AD kimlik doğrulaması için Data Lake Store desteği hakkında daha fazla bilgi edinmek için [kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

> [!NOTE]
> Data Lake Store dosya sistemi istemci SDK'ları henüz desteklemediği yönetilen hizmet kimliği.  Bu öğretici SDK için destek eklendiğinde güncelleştirilir.

Bu öğreticide, Data Lake Store dosya sistemi REST yapmak için PowerShell kullanarak REST API istekleri için kimlik doğrulaması. VM MSI kimlik doğrulaması için kullanılacak sanal makineden istekleri yapmanız gerekir.

1. Portalda gidin **sanal makineler**gidin ve Windows sanal makinenize **genel bakış** tıklayın **Connect**.
2. Girin, **kullanıcıadı** ve **parola** Windows sanal Makineyi oluştururken, eklediğiniz için. 
3. Sizin oluşturduğunuz bir **Uzak Masaüstü Bağlantısı** sanal makineyle açın **PowerShell** uzak oturumu içinde. 
4. PowerShell'in kullanarak `Invoke-WebRequest`, Azure Data Lake Store için erişim belirteci almak için yerel MSI uç noktasına bir istek oluşturun.  Data Lake Store için kaynak tanımlayıcı "https://datalake.azure.net/".  Data Lake kaynak tanımlayıcısı tam eşleşme yapar ve eğik önemlidir.

   ```powershell
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://datalake.azure.net/"} -Headers @{Metadata="true"}
   ```
    
   Yanıt, bir JSON nesnesinden bir PowerShell nesnesine dönüştürmek. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Erişim belirteci yanıttan ayıklayın.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. PowerShell'in 'Invoke-WebRequest' kullanarak, kök klasöründe klasörleri listelemek için Data Lake Store'nin REST uç noktasına bir istek olun.  Bu, her şeyin doğru yapılandırıldığını denetlemek için basit bir yoludur.  Büyük harf "B" ' % s'dizesi "Bearer" yetkilendirme üst bilgisinde sahip önemlidir.  İçinde Data Lake Store adı bulabilirsiniz **genel bakış** Azure portalında bir Data Lake Store dikey bölümü.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Başarılı bir yanıt şu şekilde görünür:

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

6. Şimdi, Data Lake Store için bir dosyayı karşıya yüklemeyi deneyebilirsiniz.  İlk olarak karşıya yüklenecek bir dosya oluşturun.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. PowerShell'in kullanarak `Invoke-WebRequest`, daha önce oluşturduğunuz klasöre dosyayı karşıya yüklemek için Data Lake Store'nin REST uç noktasına bir istek oluşturun.  Bu istek, iki adımlarında rehberlik eder.  İlk adım, bir istek oluşturun ve dosya karşıya burada yönlendirme alın.  İkinci adımda, dosyayı gerçekten karşıya.  Klasörün adını ayarlayın ve Bu öğreticide farklı değerler kullanılıyorsa uygun dosya unutmayın. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Değerini incelemek, `$HdfsRedirectResponse` şu yanıtı gibi görünmelidir:

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

   Karşıya yükleme, yeniden yönlendirme uç noktaya bir istek göndererek tamamlayın:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Başarılı yanıt görünüyor:

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

Diğer Data Lake Store dosya sistemi dosyaları eklediğiniz API'leri kullanarak, dosyalar ve daha fazlasını indirin.

Tebrikler!  VM MSI kullanarak Data Lake Store dosya sistemi kimlik doğrulaması yaptınız.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- İçin yönetim işlemleri Data Lake Store, Azure Resource Manager kullanır.  Resource Manager kimliğini doğrulamak için VM MSI kullanma hakkında daha fazla bilgi için okuma [Resource Manager'a erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma](../managed-service-identity/msi-tutorial-linux-vm-access-arm.md).
- Daha fazla bilgi edinin [kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Daha fazla bilgi edinin [Azure Data Lake Store REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) veya [WebHDFS FileSystem API'lerini](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Daha fazla bilgi edinin [Data Lake Store erişim denetimi](~/articles/data-lake-store/data-lake-store-access-control.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.