---
title: Azure Data Lake Store erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma
description: Bir Linux VM yönetilen hizmet kimliği (MSI) Azure Data Lake Store erişmek için nasıl kullanılacağını gösteren öğretici.
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
ms.openlocfilehash: 358827722e8d77cd91410fae842ad2ba99967d98
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610363"
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Azure Data Lake Store erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Azure Data Lake Store erişmek için bir Linux sanal makinesi (VM) için Yönetilen hizmet kimliği (MSI) kullanma işlemini gösterir. Yönetilen hizmet kimlikleri, Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Linux VM'de MSI etkinleştir 
> * VM erişim için bir Azure Data Lake Store
> * VM kimliğini kullanarak bir erişim belirteci alma ve bir Azure Data Lake Store erişimi için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makinesi oluşturma

Bu öğretici için yeni bir Linux VM'yi oluştururuz. Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1. Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarı** veya **parola**. Oluşturulan kimlik bilgilerini, VM'de oturum açmak izin verin.

   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. Sanal makine için boyutu seçin. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir

Bir sanal makine MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve Azure Resource Manager'daki MSI sağlar.  

1. Seçin **sanal makine** MSI etkinleştirmek istiyorsanız.
2. Sol gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**.
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.

   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların bu olduğunu denetlemek isterseniz **Linux VM**, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforLinux** listede görüntülenir.

   ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

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

VM MSI, artık tüm işlemleri dosyaları oluşturduğunuz klasöre gerçekleştirebilirsiniz.  Bu makalede Data Lake Store erişimi yönetme hakkında daha fazla bilgi okuyun için [Data Lake Store erişim denetimi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>VM MSI kullanarak bir erişim belirteci alma ve Azure Data Lake Store dosya sistemi çağırmak için kullanın

Azure Data Lake Store MSI kullanarak doğrudan erişim belirteçleri kabul edebilir destekleyen Azure AD kimlik doğrulaması, yerel olarak alınır.  Data Lake Store dosya sistemi için kimlik doğrulaması yapmak için Data Lake Store dosya sistemi uç noktası bir yetkilendirme üst bilgisi biçiminde Azure AD tarafından verilen bir erişim belirteci gönderme "taşıyıcı \<ACCESS_TOKEN_VALUE\>".  Azure AD kimlik doğrulaması için Data Lake Store desteği hakkında daha fazla bilgi edinmek için [kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

Bu öğreticide, Data Lake Store dosya sistemi REST yapmak için CURL kullanarak REST API istekleri için kimlik doğrulaması.

> [!NOTE]
> Data Lake Store dosya sistemi istemci SDK'ları henüz desteklemediği yönetilen hizmet kimliği.  Bu öğretici SDK için destek eklendiğinde güncelleştirilir.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinizin anahtarları yapılandırılıyor yardıma ihtiyacınız varsa bkz [azure'da Windows ile SSH kullanma anahtarları nasıl](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti kullanmak nasıl](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Portalda hem de Linux vm'nize gidin **genel bakış**, tıklayın **Connect**.  
2. **Connect** VM'ye SSH istemcisine sahip. 
3. Terminal penceresinde CURL, kullanarak Data Lake Store dosya sistemi için bir erişim belirteci almak için yerel MSI uç noktasına bir istek olun.  Data Lake Store için kaynak tanımlayıcı "https://datalake.azure.net/."  Kaynak tanımlayıcısı sonunda eğik çizgi dahil etmek önemlidir.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Data Lake Store için kimlik doğrulaması yapmak için kullanın ve erişim belirteci başarılı bir yanıt döndürür:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. CURL kullanarak, bir istek kök klasöründe klasörleri listelemek için Data Lake Store dosya sistemi REST uç noktanızı olun.  Bu, her şeyin doğru yapılandırıldığını denetlemek için basit bir yoludur.  Erişim belirteci değerini önceki adımda kopyalayın.  Büyük harf "B" ' % s'dizesi "Bearer" yetkilendirme üst bilgisinde sahip önemlidir.  İçinde Data Lake Store adı bulabilirsiniz **genel bakış** Azure portalında bir Data Lake Store dikey bölümü.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Başarılı bir yanıt şu şekilde görünür:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Şimdi, Data Lake Store için bir dosyayı karşıya yüklemeyi deneyebilirsiniz.  İlk olarak karşıya yüklenecek bir dosya oluşturun.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. CURL kullanarak, daha önce oluşturduğunuz klasöre dosyayı karşıya yüklemek için Data Lake Store dosya sistemi REST uç noktanızı isteği yapmak.  Karşıya yükleme, bir yeniden yönlendirme içerir ve yeniden yönlendirme, otomatik olarak CURL izler. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Başarılı bir yanıt şu şekilde görünür:

   ```bash
   HTTP/1.1 100 Continue
   HTTP/1.1 307 Temporary Redirect
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: 756f6b24-0cca-47ef-aa12-52c3b45b954c
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
       
   HTTP/1.1 100 Continue
       
   HTTP/1.1 201 Created
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: af5baa07-3c79-43af-a01a-71d63d53e6c4
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
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