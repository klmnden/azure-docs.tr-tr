---
title: Azure Data Lake Store'a erişmek için Yönetilen hizmet kimliği bir Linux VM için kullan
description: Yönetilen hizmet kimliği (MSI) bir Linux VM için Azure Data Lake Store'a erişmek için nasıl kullanılacağını gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: skwan
ms.openlocfilehash: 4489f194329727160d770ab72d9cd36115f2e64d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594766"
---
# <a name="tutorial-use-managed-service-identity-for-a-linux-vm-to-access-azure-data-lake-store"></a>Öğretici: Kullanım yönetilen hizmet kimliği Azure Data Lake Store'a erişmek bir Linux VM için

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici, yönetilen hizmet kimliği Linux sanal makine (VM) için Azure Data Lake Store'a erişmek için nasıl kullanılacağını gösterir. Azure MSI oluşturduğunuz kimlikleri otomatik olarak yönetir. MSI Azure Active Directory (Azure AD) ile kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetlerin kimlik doğrulaması için kullanabilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Linux VM üzerinde MSI etkinleştirin. 
> * Azure Data Lake Store, VM erişim.
> * VM kimliğini kullanarak bir erişim belirteci almak ve Azure Data Lake Store'a erişmek için kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Seçin **yeni** Azure portalının sol üst köşedeki düğmesi.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

   ![Sanal makine oluşturmak için "Temel" bölmesi](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. İçinde **abonelik** listesinde, sanal makine için bir abonelik seçin.
5. Sanal makine oluşturulması için istediğiniz yeni bir kaynak grubu seçmek için **kaynak grubu** > **Yeni Oluştur**. İşiniz bittiğinde, seçin **Tamam**.
6. VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar bölmesinde seçin ve Varsayılanları tutun **Tamam**.

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir VM MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Yönetilen hizmet kimliği bir VM'de etkinleştirme iki şey yapar: yazmaçlar yönetilen kimliğini ve oluşturmak için Azure Active Directory ile VM VM kimliğini yapılandırır.

1. İçin **sanal makine**, MSI etkinleştirmek istediğiniz sanal makineyi seçin.
2. Sol bölmede seçin **yapılandırma**.
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**. Devre dışı bırakmak istiyorsanız seçin **Hayır**.
   !["Azure Active Directory'ye kaydetme" seçimi](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)
4. **Kaydet**’i seçin.

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Azure Data Lake Store, VM erişim

Şimdi, dosya ve klasörleri Azure Data Lake Store'da VM erişim izni verebilir. Bu adım için mevcut bir Data Lake Store örneğini kullan veya yeni bir tane oluşturun. Azure portalı kullanarak bir Data Lake Store örneği oluşturmak için izlemeniz [Azure Data Lake Store quickstart](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Ayrıca Azure CLI ve Azure PowerShell'de kullanmak quickstarts olan [Azure Data Lake deposu belgeleri](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).

Data Lake Store'da yeni bir klasör oluşturun ve okuma, yazma ve o klasördeki dosyaları yürütme MSI izni verin:

1. Azure portalında seçin **Data Lake Store** sol bölmede.
2. Kullanmak istediğiniz Data Lake Store örneği seçin.
3. Seçin **Veri Gezgini** komut çubuğunda.
4. Data Lake Store örneğinin kök klasörü seçilir. Seçin **erişim** komut çubuğunda.
5. **Add (Ekle)** seçeneğini belirleyin.  İçinde **seçin** kutusunda, VM'yi--adını girin, örneğin, **DevTestVM**. Arama sonuçlarından VM seçin ve ardından **seçin**.
6. Tıklatın **seçin izinleri**.  Seçin **okuma** ve **yürütme**, eklemek **bu klasör**ve eklediğiniz **erişim izni yalnızca**. Seçin **Tamam**.  İzni başarıyla eklenmesi gerekir.
7. Kapat **erişim** bölmesi.
8. Bu öğretici için yeni bir klasör oluşturun. Seçin **yeni klasör** yeni klasör--örnek için bir ad verin ve komut çubuğunda **TestFolder**.  Seçin **Tamam**.
9. Oluşturulan ve ardından klasörü seçin **erişim** komut çubuğunda.
10. Adım 5 ' select benzer **Ekle**. İçinde **seçin** kutusuna, VM adını girin. Arama sonuçlarından VM seçin ve ardından **seçin**.
11. Adım 6, select benzer **Select izinleri**. Seçin **okuma**, **yazma**, ve **yürütme**, eklemek **bu klasör**ve olarak eklemek **ve varsayılan bir erişim izni girişi izin girişi**. Seçin **Tamam**.  İzni başarıyla eklenmesi gerekir.

MSI şimdi tüm dosyaları oluşturduğunuz klasöre işlemleri yapabilirsiniz. Data Lake Store erişimi yönetme ile ilgili daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-and-call-the-data-lake-store-file-system"></a>Erişim belirteci almak ve Data Lake Store dosya sistemi çağırın

Azure Data Lake Store doğrudan erişim belirteçleri kabul edebilir destekler Azure AD kimlik doğrulama, MSI yerel olarak alınır. Data Lake Store dosya sistemine kimliğini doğrulamak, Data Lake Store dosya sistemi uç noktası için Azure AD tarafından verilen bir erişim belirteci gönderin. Bir yetkilendirme üst bilgisi biçiminde erişim belirtecidir "taşıyıcı \<ACCESS_TOKEN_VALUE\>".  Azure AD kimlik doğrulaması için Data Lake Store desteği hakkında daha fazla bilgi için bkz: [Data Lake Store ile Azure Active Directory'yi kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).

Bu öğreticide, Data Lake Store dosya sistemi için REST API REST istekler yapmasını cURL kullanarak doğrulayabilirsiniz.

> [!NOTE]
> Data Lake Store dosya sistemi için İstemci SDK henüz desteklemediği yönetilen hizmet kimliği.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [SSH kullanma anahtarları azure'da Windows ile](../../virtual-machines/linux/ssh-from-windows.md) veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Portalda, Linux VM'NİZDE göz atın. İçinde **genel bakış**seçin **Bağlan**.  
2. VM için tercih ettiğiniz SSH istemcisini kullanarak bağlanın. 
3. Terminal penceresinde cURL, kullanarak Data Lake Store dosya sistemi için bir erişim belirteci almak için yerel MSI uç nokta için bir isteği oluşturun. Data Lake Store kaynak tanımlayıcısıdır "https://datalake.azure.net/".  Kaynak tanımlayıcısı eğik eklemek önemlidir.
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatalake.azure.net%2F' -H Metadata:true   
   ```
    
   Başarılı yanıt Data Lake Store için kimlik doğrulaması kullanma erişim belirteci döndürür:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. CURL kullanarak, kök klasöründe klasörleri listelemek için Data Lake Store dosya sisteminin REST uç noktası için bir isteği oluşturun. Bu, her şeyin doğru şekilde yapılandırıldığını denetlemek için basit bir yoludur. Erişim belirteci değerini önceki adımdan kopyalayın. Yetkilendirme üst "Bearer" dize "B" büyük olması önemlidir Data Lake Store örneğinizi adını bulabilirsiniz **genel bakış** bölümünü **Data Lake Store** Azure portalında bölmesi.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Başarılı yanıt şöyle görünür:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Şimdi, Data Lake Store örneğinizi bir dosyayı karşıya yüklemeyi deneyebilirsiniz. İlk olarak, karşıya yüklemek için bir dosya oluşturun.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. CURL kullanarak dosyayı daha önce oluşturduğunuz klasöre yüklemek için Data Lake Store dosya sisteminizin REST uç noktası için bir isteği oluşturun. CURL yeniden yönlendirme otomatik olarak izler ve karşıya yükleme yeniden yönlendirme içerir. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Başarılı yanıt şöyle görünür:

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

Data Lake Store dosya sistemi için diğer API'lerini kullanarak dosyaları, yükleme dosyalarını ve daha fazlasını ekleyebilirsiniz.

Tebrikler! Bir Linux VM için MSI kullanarak Data Lake Store dosya sistemine kimlik doğruladınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir yönetilen hizmet kimliği Linux sanal makine için bir Azure Data Lake Store'a erişmek için nasıl kullanılacağı hakkında bilgi edindiniz. Bilgi edinmek için Azure Data Lake Store hakkında daha fazla bakın:

> [!div class="nextstepaction"]
>[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview)
