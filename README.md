package cryptography.actions;

import java.security.KeyFactory;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

public class RSAKeyValidator {

    public static boolean doKeysMatch(String privateKeyPEM, String publicKeyPEM) throws Exception {
        // Remover apenas espaços em branco extras dentro das chaves
        String privateKeyContent = privateKeyPEM.replaceAll("\\s+", "");
        String publicKeyContent = publicKeyPEM.replaceAll("\\s+", "");

        // Remover cabeçalhos e rodapés da chave privada para a decodificação
        String privateKeyContentClean = privateKeyContent
                .replace("-----BEGIN PRIVATE KEY-----", "")
                .replace("-----END PRIVATE KEY-----", "");

        // Remover cabeçalhos e rodapés da chave pública para a decodificação
        String publicKeyContentClean = publicKeyContent
                .replace("-----BEGIN PUBLIC KEY-----", "")
                .replace("-----END PUBLIC KEY-----", "");

        byte[] decodedPrivateKey = Base64.getDecoder().decode(privateKeyContentClean);
        byte[] decodedPublicKey = Base64.getDecoder().decode(publicKeyContentClean);

        // Gerar as chaves com os dados decodificados
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        PKCS8EncodedKeySpec privateKeySpec = new PKCS8EncodedKeySpec(decodedPrivateKey);
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(privateKeySpec);

        X509EncodedKeySpec publicKeySpec = new X509EncodedKeySpec(decodedPublicKey);
        RSAPublicKey publicKey = (RSAPublicKey) keyFactory.generatePublic(publicKeySpec);

        // Verificar se os moduli das chaves privadas e públicas são iguais
        return privateKey.getModulus().equals(publicKey.getModulus());
    }
}
