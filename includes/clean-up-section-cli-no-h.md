Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. İleride bu kaynaklara ihtiyaç duymayacağınızı düşünüyorsanız kaynakları silmek için kaynak grubunu silebilirsiniz.
 
1. Kaynak grubunun kaydetmek istediğiniz herhangi bir kaynak içermediğinden emin olmak için şu komutu çalıştırın:

  ```azurecli
  az group show --name myResourceGroup
  ```

2. Gösterilen kaynakların tamamı silmek istediğiniz kaynaklarsa şu komutu çalıştırın:
 
  ```azurecli
  az group delete --name myResourceGroup
  ```
