package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.*;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
import javax.crypto.BadPaddingException;
import javax.crypto.IllegalBlockSizeException;

public class JA_Criptografar extends CustomJavaAction<java.lang.String> {

    private java.lang.String PublicKey;
    private java.lang.String Value;

    public JA_Criptografar(IContext context, java.lang.String PublicKey, java.lang.String Value) {
        super(context);
        this.PublicKey = PublicKey;
        this.Value = Value;
    }

    // Método para converter a chave pública Base64 para RSAPublicKey
    public RSAPublicKey getPublicKeyFromBase64(String base64PublicKey) throws Exception {
        byte[] decodedPublicKey = Base64.getDecoder().decode(base64PublicKey);
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedPublicKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        return (RSAPublicKey) keyFactory.generatePublic(keySpec);
    }

    // Método para validar o tamanho da chave pública
    public void validateKeySize(RSAPublicKey publicKey) {
        int keySize = publicKey.getModulus().bitLength();
        if (keySize < 2048) {
            throw new IllegalArgumentException("Key size must be at least 2048");
        }
    }

    // Método para validar o valor de entrada
    public void validateInputData(String value) {
        if (value == null || value.isEmpty()) {
            throw new IllegalArgumentException("Value cannot be null or empty");
        }
    }

    @Override
    public java.lang.String executeAction() throws Exception {
        try {
            // Valida e limpa a chave pública Base64
            RSAPublicKey rsaPublicKey = getPublicKeyFromBase64(PublicKey);

            // Valida o tamanho da chave pública (mínimo de 2048 bits)
            validateKeySize(rsaPublicKey);

            // Valida o valor a ser encriptado (não pode ser nulo ou vazio)
            validateInputData(Value);

            // Cria o objeto Cipher para encriptação
            Cipher cipher = Cipher.getInstance("RSA");
            cipher.init(Cipher.ENCRYPT_MODE, rsaPublicKey);

            // Encripta o valor
            byte[] encryptedValue = cipher.doFinal(Value.getBytes());

            // Codifica o valor encriptado em Base64 e retorna
            return Base64.getEncoder().encodeToString(encryptedValue);

        } catch (IllegalArgumentException e) {
            throw new IllegalArgumentException("Erro de validação: " + e.getMessage(), e);
        } catch (BadPaddingException | IllegalBlockSizeException e) {
            throw new Exception("Erro de encriptação: " + e.getMessage(), e);
        } catch (Exception e) {
            throw new Exception("Erro inesperado: " + e.getMessage(), e);
        }
    }

    /**
     * Retorna uma representação em string desta ação
     * @return uma representação em string desta ação
     */
    @Override
    public java.lang.String toString() {
        return "JA_Criptografar";
    }

}
