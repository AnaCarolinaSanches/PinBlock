package cryptography.utils;

import java.security.KeyFactory;
import java.security.PublicKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

public class CryptographyUtils {

    // Método para obter a chave pública a partir de uma string Base64
    public static RSAPublicKey getPublicKeyFromBase64(String base64PublicKey) throws Exception {
        byte[] decodedPublicKey = Base64.getDecoder().decode(base64PublicKey);
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedPublicKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        return (RSAPublicKey) keyFactory.generatePublic(keySpec);
    }

    // Valida o tamanho da chave pública
    public static void validateKeySize(RSAPublicKey publicKey) {
        int keySize = publicKey.getModulus().bitLength();
        if (keySize < 2048) {
            throw new IllegalArgumentException("Key size must be at least 2048");
        }
    }

    // Valida os dados de entrada
    public static void validateInputData(String value) {
        if (value == null || value.isEmpty()) {
            throw new IllegalArgumentException("Value cannot be null or empty");
        }
    }
}
