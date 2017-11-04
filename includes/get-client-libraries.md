### <a name="install-via-composer"></a>Oluşturucu yükleyin
1. Adlı bir dosya oluşturun **composer.json** projenizi kök ve aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Karşıdan  **[composer.phar] [ composer-phar]**  proje kök.
3. Bir komut istemi açın ve proje kök dizininde aşağıdaki komutu yürütün
   
    ```
    php composer.phar install
    ```

Alternatif olarak gidin [Azure Storage PHP istemci Kitaplığı] [ php-sdk-github] kaynak kodunu kopyalama github'da.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
