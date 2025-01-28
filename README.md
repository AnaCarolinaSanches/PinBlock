package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import cryptography.utils.CryptographyUtils;
import java.security.interfaces.RSAPublicKey;
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

    @Override
    public java.lang.String executeAction() throws Exception {
        // BEGIN USER CODE
        try {
            // Valida e limpa a chave pública Base64
            RSAPublicKey rsaPublicKey = CryptographyUtils.getPublicKeyFromBase64(PublicKey);

            // Valida o tamanho da chave pública (mínimo de 2048 bits)
            CryptographyUtils.validateKeySize(rsaPublicKey);

            // Valida o valor a ser encriptado (não pode ser nulo ou vazio)
            CryptographyUtils.validateInputData(Value);

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
        // END USER CODE
    }
}
