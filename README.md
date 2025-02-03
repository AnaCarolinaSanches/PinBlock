package cryptography.actions;

import java.security.KeyFactory;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

public class RSAKeyValidator {

    /**
     * Validates that the provided private key matches the provided public key.
     *
     * @param privateKeyPEM the private key in PEM format
     * @param publicKeyPEM  the public key in PEM format
     * @return true if the keys match; false otherwise
     * @throws Exception if there is an error processing the keys
     */
    public static boolean doKeysMatch(String privateKeyPEM, String publicKeyPEM) throws Exception {
        // Remove PEM headers/footers and whitespace from the private key
        String privateKeyContent = privateKeyPEM
                .replace("-----BEGIN PRIVATE KEY-----", "")
                .replace("-----END PRIVATE KEY-----", "")
                .replaceAll("\\s+", "");
        byte[] decodedPrivateKey = Base64.getDecoder().decode(privateKeyContent);

        // Remove PEM headers/footers and whitespace from the public key
        String publicKeyContent = publicKeyPEM
                .replace("-----BEGIN PUBLIC KEY-----", "")
                .replace("-----END PUBLIC KEY-----", "")
                .replaceAll("\\s+", "");
        byte[] decodedPublicKey = Base64.getDecoder().decode(publicKeyContent);

      
        PKCS8EncodedKeySpec privateKeySpec = new PKCS8EncodedKeySpec(decodedPrivateKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(privateKeySpec);

      
        X509EncodedKeySpec publicKeySpec = new X509EncodedKeySpec(decodedPublicKey);
        RSAPublicKey publicKey = (RSAPublicKey) keyFactory.generatePublic(publicKeySpec);

       
        return privateKey.getModulus().equals(publicKey.getModulus());
    }

    // A simple test
    public static void main(String[] args) {
        try {
            String privateKeyPEM = "-----BEGIN PRIVATE KEY-----\n"
                    + "MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAK8tqGnFnlHqtg5v\n"
                    + "aHTru3feaMLkt4pEwLZ/vNdni0EBdGeFtBy77Ydbk0uBLhrdWH7BPwuFlOWrl3JU\n"
                    + "fQJL8xSTl1BZJ/Zqxuml/nr0xcorY8dnnW10GXYYMciQtldRcjpUQ/zxxeqSAzCA\n"
                    + "mHZNKKlgh9U8af4dD0MxNaLRpBEnAgMBAAECgYBi2roaDjnccj4QgVAKAukEqM6n\n"
                    + "hJgKf+fcVNNFHxpXMbH1pV7RhD9zTfsd9aUF5fjFdtnT76rpvF43V3Q/8ooWGEkO\n"
                    + "sv33ZOuhAnUiTadStSEpdKLY1TuaJeqH3IbUDu0WywNno4mpjQXBs+P8tQb7aMcX\n"
                    + "ehLRF6dXt2G85rRiWQJBANy58j+0fkfwXzCEy9uK6/UZtk8UawneyGYDK9Z9PQw0\n"
                    + "dcLgblqY2dQH2Kf6wF8CyzNwyZKvcC7u1nT/G6ITIUUCQQDLLFBxGBS/1ojPkodv\n"
                    + "t52XVMVuiIPEhmWOpzXn+vOzxbhGCJbUBrBMdtQQz9KRH70htqrbNnkCIqv/iMD0\n"
                    + "X5F7AkEAwp1Q8sp57YQK6gSsmc5LbbhV/jPKjNFZcFirdlrGUNSQYFrx8f+DUGf6\n"
                    + "p2F37E3STHDNyf/Vsgv0GwQzoRus4QJANG0f6L7tA7+JF/7YgeRgft85/tatIbYI\n"
                    + "WLIe/9hKsFXRwgiPWvDK50A2Yowt6pLFDAEFv4Ej4oAt38da+vP6JwJBAJs+rpV1\n"
                    + "4HnHeMvMdA9n0Duw1SeS/UtjO0GDl0ZNMxbuIKhjIEbmqXlXinvhJ7g5zLpIpKZR\n"
                    + "2wT6MOwTdMPBtV4=\n"
                    + "-----END PRIVATE KEY-----";

            String publicKeyPEM = "-----BEGIN PUBLIC KEY-----\n"
                    + "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCvLakacWeUeq2Dm9odOu7d95ow\n"
                    + "uS3ikTC2f7zXZ4tBAXRnhbQcu+2HW5NLgS4a3Vh+wT8LhZTlq5dyVH0CS/MUk5dQ\n"
                    + "WSf2asbpZ+evTHKK2PHZ53bXQZdhhyJELZXUXI6VEP8PHF6kgMwgaFk0oqWCH1Tx\n"
                    + "p/h0PQzE1otGkEScCAwEAAQ==\n"
                    + "-----END PUBLIC KEY-----";

            boolean keysMatch = doKeysMatch(privateKeyPEM, publicKeyPEM);
            System.out.println("Keys match: " + keysMatch);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

