package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.KeyFactory;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
import java.nio.charset.StandardCharsets;

public class JA_Descriptografar extends CustomJavaAction<String>
{
    private String PrivateKey;
    private String EncryptedValue;

    public JA_Descriptografar(IContext context, String PrivateKey, String EncryptedValue)
    {
        super(context);
        this.PrivateKey = PrivateKey;
        this.EncryptedValue = EncryptedValue;
    }

    @Override
    public String executeAction() throws Exception
    {
        // BEGIN USER CODE

        // Remove PEM header and footer from the private key
        String privateKeyPEM = PrivateKey
                .replace("-----BEGIN PRIVATE KEY-----", "")
                .replace("-----END PRIVATE KEY-----", "");

        
        privateKeyPEM = privateKeyPEM.replaceAll("\\s+", "");

        byte[] decodedPrivateKey = Base64.getDecoder().decode(privateKeyPEM);

        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(decodedPrivateKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(keySpec);

        // Initialize Cipher for RSA decryption using OAEP padding.
        Cipher cipher = Cipher.getInstance("RSA/ECB/OAEPWithSHA-1AndMGF1Padding");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);

        byte[] encryptedValueBytes = Base64.getDecoder().decode(EncryptedValue);

        byte[] decryptedValue = cipher.doFinal(encryptedValueBytes);

        return new String(decryptedValue, StandardCharsets.UTF_8);

        // END USER CODE
    }

    @Override
    public String toString()
    {
        return "JA_Descriptografar";
    }
}
