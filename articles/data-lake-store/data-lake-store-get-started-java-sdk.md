<properties
   pageTitle="Uygulama geliştirmek için Data Lake Store Java SDK'yı kullanma | Azure"
   description="Uygulama geliştirmek için Azure Data Lake Store Java SDK'yı kullanma"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/12/2016"
   ms.author="nitinme"/>

# Java'yı kullanarak Azure Data Lake Store ile çalışmaya başlama

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Azure Data Lake hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure Data Lake Store Java SDK'nın nasıl kullanılacağını öğrenin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

## Azure Data Lake Store Java SDK

Aşağıdaki bağlantılar, Data Lake Store için Java SDK'ya ve Java SDK başvurusuna yönelik indirme konumunu sağlar. Bu öğreticide SDK'yı indirmeniz veya başvuru belgesini izlemeniz gerekmez. Bu bağlantılar yalnızca size bilgi vermek üzere sağlanmıştır.

* Data Lake Store için Java SDK'ya yönelik kaynak kodu [GitHub](https://github.com/Azure/azure-sdk-for-java) üzerinde mevcuttur.
* Data Lake Store için Java SDK Başvurusu, [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/) adresinde mevcuttur.

## Önkoşullar

* Java Geliştirme Seti (JDK) 8 (Java sürüm 1.8'i kullanır).
* IntelliJ veya başka bir uygun Java geliştirme ortamı. Bu isteğe bağlı olsa da önerilir. Aşağıdaki yönergelerde IntelliJ kullanılmıştır.
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Data Lake Store genel önizlemesi için **Azure aboneliğinizi etkinleştirme**. Bkz. [yönergeler](data-lake-store-get-started-portal.md#signup).
* **Azure Active Directory Uygulaması oluşturma**. Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmenin iki yolu vardır: **etkileşimli** ve **etkileşimli olmayan**. Kimlik doğrulamasını nasıl gerçekleştirmek istediğinize bağlı olarak farklı önkoşullar mevcuttur.
    * **Etkileşimli kimlik doğrulaması için** - Azure Active Directory'de bir **Yerel İstemci uygulaması** oluşturmanız gerekir. Uygulamayı oluşturduktan sonra uygulamayla ilgili aşağıdaki değerleri alın.
        - Uygulama için **istemci kimliği** ve **yeniden yönlendirme URI'si** bilgilerini alın
        - Yetki verilmiş izinleri ayarlayın

    * **Etkileşimli olmayan kimlik doğrulaması için** (bu makalede kullanılan) - Azure Active Directory'de bir **Web uygulaması** oluşturmanız gerekir. Uygulamayı oluşturduktan sonra uygulamayla ilgili aşağıdaki değerleri alın.
        - Uygulama için **istemci kimliği**, **gizli anahtar** ve **yeniden yönlendirme URI'si** bilgilerini alın
        - Yetki verilmiş izinleri ayarlayın
        - Azure Active Directory uygulamasını bir role atayın. Rol, Azure Active Directory uygulamasına izin vermek istediğiniz kapsam düzeyinde olabilir. Örneğin, uygulamayı abonelik düzeyinde veya kaynak grubu düzeyinde atayabilirsiniz. 

    Bu değerleri almaya, izinleri ayarlamaya ve rolleri atamaya yönelik yönergeler için bkz. [Portalı kullanarak Active Directory uygulaması ve hizmet sorumlusu oluşturma](../resource-group-create-service-principal-portal.md).

## Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?

Aşağıdaki kod parçacığı, uygulamanın kendi kimlik bilgilerini sağladığı **etkileşimli olmayan** kimlik doğrulaması için kod sağlar.

Bu öğreticinin çalışması için, uygulamaya Azure'da kaynak oluşturmak üzere izin vermeniz gerekir. Bu öğreticinin amaçları doğrultusunda, bu uygulamaya Azure aboneliğinizdeki yeni, kullanılmamış ve boş bir kaynak grubu üzerinde Katılımcı izinleri vermeniz **önemle önerilir**.

## Java uygulaması oluşturma

1. IntelliJ hizmetini açın ve **Komut Satırı Uygulaması** şablonunu kullanarak yeni bir Java projesi oluşturun. Projeyi oluşturmak için sihirbazı tamamlayın.

2. Ekranınızın sol tarafında projeye sağ tıklayın ve **Framework Desteği Ekle** seçeneğine tıklayın. **Maven**'ı seçip **Tamam**'a tıklayın.

3. Yeni oluşturulan **"pom.xml"** dosyasını açın ve **\</version>** etiketi ile **\</project>** etiketi arasına şu metin parçacığını ekleyin:

    >[AZURE.NOTE] Bu, Azure Data Lake Store SDK olanağı Maven içinde kullanılabilir olana kadar geçerli bir adımdır. SDK, Maven içinde kullanılabilir olduğunda bu makale güncelleştirilecektir. İleride bu SDK'ya yönelik olarak gerçekleştirilecek tüm güncelleştirmeler Maven üzerinden sunulacaktır.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. **Dosya**'ya, **Ayarlar**'a ve ardından **Derleme, Yürütme ve Dağıtım**'a gidin. **Derleme Araçları**'nı, **Maven**'ı ve ardından **İçeri Aktarma**'yı genişletin. **Maven projelerini otomatik olarak içeri aktar** onay kutusunu işaretleyin. **Uygula**'ya ve ardından **Tamam**'a tıklayın.

5. Sol bölmeden, **src**, **ana**, **java**, **\<paket adı>** konumuna gidin ve ardından **Main.java** dosyasını açıp var olan kod bloğunu aşağıdaki kodla değiştirin. Ayrıca, **localFolderPath**, **_adlsAccountName**, **_resourceGroupName** gibi, kod parçacığında çağrılan parametrelerin değerlerini sağlayın ve **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** ve **SUBSCRIPTION-ID** yer tutucularını değiştirin.

    Bu kod, Data Lake Store hesabı oluşturma, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve son olarak hesabı silme işlemini içerir.

        package com.company;
        
        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");
        
                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Account(s) displayed.", "Creating files.");
        
                // Create two files in Data Lake Store: file1.csv and file2.csv
                CreateFile("/file1.csv", "123,abc", true);
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("Files created.", "Concatenating files.");
        
                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");
        
                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
        
                _adlsClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }
        
            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }
        
            // Create file
            public static void CreateFile(String path) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path);
            }
        
            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().append(_adlsAccountName, path, bytesContents);
            }
        
            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().concat(_adlsAccountName, destFilePath, srcFilePaths);
            }
        
            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(_adlsAccountName, filePath);
            }
        
            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().getFileStatus(_adlsAccountName, path).getBody().getFileStatus();
            }
        
            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().listFileStatus(_adlsAccountName, directoryPath).getBody().getFileStatuses().getFileStatus();
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(_adlsAccountName, srcPath).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
            }
        }


6. Uygulamayı çalıştırın. Uygulamayı çalıştırmak ve tamamlamak için istemleri izleyin.

## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Jun16_HO2-->


