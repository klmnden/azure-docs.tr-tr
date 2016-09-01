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
   ms.date="08/18/2016"
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

Aşağıdaki bağlantılar, Data Lake Store için Java SDK'ya ve Java SDK başvurusuna yönelik indirme konumunu sağlar. Bu öğretici için SDK'yı indirmeniz veya başvuru belgesini izlemeniz gerekmez. Bu bağlantılar yalnızca size bilgi vermek üzere sağlanmıştır.

* Data Lake Store için Java SDK'ya yönelik kaynak kodu [GitHub](https://github.com/Azure/azure-sdk-for-java) üzerinde mevcuttur.
* Data Lake Store için Java SDK Başvurusu, [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/) adresinde mevcuttur.

## Ön koşullar

* Java Geliştirme Seti (JDK) 8 (Java sürüm 1.8'i kullanır).
* IntelliJ veya başka bir uygun Java geliştirme ortamı. Bu isteğe bağlı olsa da önerilir. Aşağıdaki yönergelerde IntelliJ kullanılmıştır.
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Data Lake Store genel önizlemesi için **Azure aboneliğinizi etkinleştirme**. Bkz. [yönergeler](data-lake-store-get-started-portal.md#signup).
* **Azure Active Directory Uygulaması oluşturma**. Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmenin iki yolu vardır: **Etkileşimli** ve **etkileşimli olmayan**. Kimlik doğrulamasını nasıl gerçekleştirmek istediğinize bağlı olarak farklı önkoşullar mevcuttur.
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

2. **Dosya** -> **Proje Yapısı** -> **Modüller** (Proje Ayarları altında) -> **Bağımlılıklar** -> **+** -> **Kitaplık** -> **Maven’den**’i açın.

3. Aşağıdaki Maven paketlerini bulun ve projenize ekleyin:

    * com.microsoft.azure:azure-mgmt-datalake-store:1.0.0-beta1.2
    * com.microsoft.azure:azure-mgmt-datalake-store-uploader:1.0.0-beta1.2
    * com.microsoft.azure:azure-client-authentication:1.0.0-beta2

4. Sol bölmeden, **src**, **main (ana)**, **java**, **\<(package name) paket adı>** konumuna gidin ve ardından **Main.java** dosyasını açıp var olan kod bloğunu aşağıdaki kodla değiştirin. Ayrıca, **localFolderPath**, **DATA-LAKE-STORE-NAME**, **RESOURCE-GROUP-NAME** gibi kod parçacığında çağrılan parametrelerin değerlerini sağlayın ve **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** ve **SUBSCRIPTION-ID** yer tutucularını aboneliğiniz ve aboneliğinizin Azure Active Directory’si ile ilgili bilgilerle değiştirin. Bu bilgileri nasıl bulacağınız konusunda bilgi edinmek için bkz. [Azure hizmet sorumluları oluşturma kılavuzu](../resource-group-authenticate-service-principal.md).

    Bu kod, Data Lake Store hesabı oluşturma, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve son olarak hesabı silme işlemini içerir.

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreAccountManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreFileSystemManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.store.uploader.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;

        public class Main {
            final static String ADLS_ACCOUNT_NAME = <DATA-LAKE-STORE-NAME>;
            final static String RESOURCE_GROUP_NAME = "<RESOURCE-GROUP-NAME>";
            final static String LOCATION = "East US 2";
            final static String TENANT_ID = "<TENANT-ID>";
            final static String SUBSCRIPTION_ID =  "<SUBSCRIPTION-ID>";
            final static String CLIENT_ID = "<CLIENT-ID>";
            final static String CLIENT_SECRET = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

            private static DataLakeStoreAccountManagementClientImpl _adlsClient;
            private static DataLakeStoreFileSystemManagementClientImpl _adlsFileSystemClient;

            public static void main(String[] args) throws Exception {
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(CLIENT_ID, TENANT_ID, CLIENT_SECRET, null);
                SetupClients(creds);

                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");

                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", SUBSCRIPTION_ID));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.accounts().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.name());
                }
                WaitForNewline("Account(s) displayed.", "Uploading file.");

                // Upload a file to Data Lake Store: file1.csv
                UploadFile(localFolderPath + "file1.csv", "/file1.csv");
                WaitForNewline("File uploaded.", "Appending newline.");

                // Append newline to file1.csv
                AppendToFile("/file1.csv", "\r\n");
                WaitForNewline("Newline appended.", "Creating file.");

                // Create a new file in Data Lake Store: file2.csv
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("File created.", "Concatenating files.");

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
                _adlsClient.withSubscriptionId(SUBSCRIPTION_ID);
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
                adlsParameters.withLocation(LOCATION);

                _adlsClient.accounts().create(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME, adlsParameters);
            }

            // Create file
            public static void CreateFile(String path) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path);
            }

            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path, bytesContents, force);
            }

            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().append(ADLS_ACCOUNT_NAME, path, bytesContents);
            }

            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().concat(ADLS_ACCOUNT_NAME, destFilePath, srcFilePaths);
            }

            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().delete(ADLS_ACCOUNT_NAME, filePath);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().getFileStatus(ADLS_ACCOUNT_NAME, path).getBody().fileStatus();
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().listFileStatus(ADLS_ACCOUNT_NAME, directoryPath).getBody().fileStatuses().fileStatus();
            }

            // Upload file
            public static void UploadFile(String srcPath, String destPath) throws Exception {
                UploadParameters parameters = new UploadParameters(srcPath, destPath, ADLS_ACCOUNT_NAME);
                FrontEndAdapter frontend = new DataLakeStoreFrontEndAdapterImpl(ADLS_ACCOUNT_NAME, _adlsFileSystemClient);
                DataLakeStoreUploader uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.execute();
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, AdlsErrorException {
                InputStream stream = _adlsFileSystemClient.fileSystems().open(ADLS_ACCOUNT_NAME, srcPath).getBody();

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
                _adlsClient.accounts().delete(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME);
            }
        }


6. Uygulamayı çalıştırın. Uygulamayı çalıştırmak ve tamamlamak için istemleri izleyin.

## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Aug16_HO4-->


