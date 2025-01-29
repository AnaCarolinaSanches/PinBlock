@java.lang.Override
public java.lang.String executeAction() throws Exception
{
    // BEGIN USER CODE
    
    // Remover cabeçalho e rodapé da chave pública PEM
    String publicKeyPEM = PublicKey.replace("-----BEGIN PUBLIC KEY-----", "").replace("-----END PUBLIC KEY-----", "");
    
    // Decodificar a chave pública de Base64
    byte[] decodedPublicKey = Base64.getDecoder().decode(publicKeyPEM);
    
    // Gerar chave pública RSA
    X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedPublicKey);
    KeyFactory keyFactory = KeyFactory.getInstance("RSA");
    java.security.PublicKey publicKey = keyFactory.generatePublic(keySpec);
    
    // Inicializar o cipher para criptografar
    Cipher cipher = Cipher.getInstance("RSA");
    cipher.init(Cipher.ENCRYPT_MODE, publicKey);
    
    // Criptografar o valor
    byte[] encryptedValue = cipher.doFinal(Value.getBytes());
    
    // Retornar a criptografia em Base64
    return Base64.getEncoder().encodeToString(encryptedValue);
    
    // END USER CODE
}
