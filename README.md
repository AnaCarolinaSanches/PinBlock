C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:40: error: illegal start of expressionpackage cryptography.utils;

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

	public RSAPublicKey getPublicKeyFromBase64(String base64PublicKey) throws Exception {
	^
C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:101: error: class, interface, or enum expected
	public java.lang.String toString()
	       ^
C:\Dev\Mendix\appmaisv2-feature_APP-445-encriptacao-app-runtime\javasource\cryptography\actions\JA_Criptografar.java:104: error: class, interface, or enum expected
	}
	^
3 errors

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':compile'.
> Compilation failed; see the compiler error output for details.

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 8s

