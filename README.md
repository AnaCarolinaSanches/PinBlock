package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.KeyFactory;
import java.security.PublicKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.X509EncodedKeySpec;
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
        // Carregar a chave pública a partir da string
        byte[] decodedKey = Base64.getDecoder().decode(PublicKey);
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        RSAPublicKey rsaPublicKey = (RSAPublicKey) keyFactory.generatePublic(keySpec);

        // Criar o objeto de cifra e configurar para encriptação
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, rsaPublicKey);

        // Encriptar o valor
        byte[] encryptedValue = cipher.doFinal(Value.getBytes());

        // Codificar o valor encriptado em Base64 para retorno
        return Base64.getEncoder().encodeToString(encryptedValue);
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
