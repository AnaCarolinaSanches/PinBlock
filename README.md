package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import java.security.KeyFactory;
import java.security.PrivateKey;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
import java.nio.charset.StandardCharsets;

public class JA_Descriptografar extends CustomJavaAction<java.lang.String>
{
    private java.lang.String PrivateKey;
    private java.lang.String EncryptedValue;

    public JA_Descriptografar(IContext context, java.lang.String PrivateKey, java.lang.String EncryptedValue)
    {
        super(context);
        this.PrivateKey = PrivateKey;
        this.EncryptedValue = EncryptedValue;
    }

    @java.lang.Override
    public java.lang.String executeAction() throws Exception
    {
        // BEGIN USER CODE

        // Remove PEM header and footer from the private key
        String privateKeyPEM = PrivateKey
            .replace("-----BEGIN PRIVATE KEY-----", "")
            .replace("-----END PRIVATE KEY-----", "");

        // Remove all whitespace (including new lines)
        privateKeyPEM = privateKeyPEM.replaceAll("\\s+", "");

        // Decode the private key from Base64
        byte[] decodedPrivateKey = Base64.getDecoder().decode(privateKeyPEM);

        // Generate RSA private key
        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(decodedPrivateKey);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(keySpec);

        // Initialize Cipher for RSA decryption (PKCS#1 padding)
        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);

        // Decode the encrypted value from Base64
        byte[] encryptedValueBytes = Base64.getDecoder().decode(EncryptedValue);

        // Decrypt the value
        byte[] decryptedValue = cipher.doFinal(encryptedValueBytes);

        // Return the decrypted value as a String
        return new String(decryptedValue, StandardCharsets.UTF_8);

        // END USER CODE
    }

    /**
     * Returns a string representation of this action
     * @return a string representation of this action
     */
    @java.lang.Override
    public java.lang.String toString()
    {
        return "JA_Descriptografar";
    }

    // BEGIN EXTRA CODE
    // END EXTRA CODE
}
