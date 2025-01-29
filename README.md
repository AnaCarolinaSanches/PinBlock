package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.nio.charset.StandardCharsets;
import java.security.KeyFactory;
import java.security.interfaces.RSAPublicKey;
import javax.crypto.Cipher;
import java.util.Base64;

public class JA_Criptografar extends CustomJavaAction<java.lang.String>
{
    private java.lang.String PublicKey;
    private java.lang.String Value;

    public JA_Criptografar(IContext context, java.lang.String PublicKey, java.lang.String Value)
    {
        super(context);
        this.PublicKey = PublicKey;
        this.Value = Value;
    }

    @java.lang.Override
    public java.lang.String executeAction() throws Exception
    {
        // BEGIN USER CODE
        // Limpar e decodificar a chave pública
        String cleanedPublicKey = CryptographyHelper.cleanPublicKey(PublicKey);
        byte[] decodedPublicKey = Base64.getDecoder().decode(cleanedPublicKey);
        
        // Carregar a chave pública diretamente a partir da chave decodificada
        RSAPublicKey publicKey = (RSAPublicKey) KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(decodedPublicKey));

        // Inicializar o cipher para criptografar
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
        
        // Criptografar o valor
        byte[] encryptedValue = cipher.doFinal(Value.getBytes(StandardCharsets.UTF_8));
        
        // Retornar a criptografia em Base64
        return Base64.getEncoder().encodeToString(encryptedValue);
        // END USER CODE
    }

    /**
     * Returns a string representation of this action
     * @return a string representation of this action
     */
    @java.lang.Override
    public java.lang.String toString()
    {
        return "JA_Criptografar";
    }

    // BEGIN EXTRA CODE
    // END EXTRA CODE
}
