package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import org.apache.commons.codec.DecoderException;
import org.apache.commons.codec.binary.Hex;
import org.apache.commons.lang.StringUtils;
import cryptography.actions.JS_PinblockEncodeDecodeAction;
import javax.crypto.spec.SecretKeySpec;
import javax.crypto.Cipher;
import java.nio.charset.StandardCharsets;

public class JA_GeneratePinblock extends CustomJavaAction<java.lang.String> {
    private java.lang.String key;
    private java.lang.String pin;
    private java.lang.String pan;

    public JA_GeneratePinblock(IContext context, java.lang.String key, java.lang.String pin, java.lang.String pan) {
        super(context);
        this.key = key;
        this.pin = pin;
        this.pan = pan;
    }

    @java.lang.Override
    public java.lang.String executeAction() throws Exception {
        // BEGIN USER CODE

        // Codificar o Pinblock
        String iso9564format0 = format0Encode(pin, pan);
        System.out.println("Pinblock Encoded: " + iso9564format0);
        
        // Decodificar o Pinblock
        String pinDecoded = format0Decode(iso9564format0, pan);
        System.out.println("Pin Decoded: " + pinDecoded);

        // Ajustar o tamanho da chave para 16 ou 24 bytes
        if (key.length() > 24) {
            key = key.substring(0, 24); // Truncar para 24 bytes
        } else if (key.length() < 16) {
            key = String.format("%-16s", key).replace(' ', '0'); // Preencher com zeros até 16 bytes
        }

        // Converter a chave para bytes com a codificação UTF-8
        byte[] secretKey = key.getBytes(StandardCharsets.UTF_8);

        // Verificar o tamanho da chave
        if (secretKey.length != 16 && secretKey.length != 24) {
            throw new IllegalArgumentException("A chave para Triple DES deve ter 16 ou 24 bytes.");
        }

        // Criação do SecretKeySpec para Triple DES
        SecretKeySpec secretKeySpec = new SecretKeySpec(secretKey, "DESede");

        // Criação do Cipher para criptografia
        Cipher encryptCipher = Cipher.getInstance("DESede/ECB/PKCS5Padding");
        encryptCipher.init(Cipher.ENCRYPT_MODE, secretKeySpec);

        // Converter o Pinblock para bytes e realizar a criptografia
        byte[] secretMessagesBytes = iso9564format0.getBytes(StandardCharsets.UTF_8);
        byte[] encryptedMessageBytes = encryptCipher.doFinal(secretMessagesBytes);

        // Retornar o texto criptografado em maiúsculas
        return new String(encryptedMessageBytes).toUpperCase();

        // END USER CODE
    }

    @java.lang.Override
    public java.lang.String toString() {
        return "JA_GeneratePinblock";
    }

    // BEGIN EXTRA CODE
    /**
     * Encode pinblock format 0 (ISO 9564)
     * @param pin pin 
     * @param pan primary account number (PAN/CLN/CardNumber)
     * @return pinblock in HEX format
     */
    public static String format0Encode(String pin, String pan) {
        try {
            final String pinLenHead = StringUtils.leftPad(Integer.toString(pin.length()), 2, '0')+pin;
            final String pinData = StringUtils.rightPad(pinLenHead, 16, 'F');
            final byte[] bPin = Hex.decodeHex(pinData.toCharArray());
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());

            final byte[] pinblock = new byte[8];
            for (int i = 0; i < 8; i++)
                pinblock[i] = (byte) (bPin[i] ^ bPan[i]);

            return Hex.encodeHexString(pinblock).toUpperCase();
        } catch (DecoderException e) {
            throw new RuntimeException("Hex decoder failed!", e);
        }
    }

    /**
     * @param accountNumber PAN - primary account number
     * @return extract right-most 12 digits of the primary account number (PAN)
     */
    public static String extractPanAccountNumberPart(String accountNumber) {
        String accountNumberPart = null;
        if (accountNumber.length() > 12)
            accountNumberPart = accountNumber.substring(accountNumber.length() - 13, accountNumber.length() - 1);
        else
            accountNumberPart = accountNumber;
        return accountNumberPart;
    }

    /**
     * Decode pinblock format 0 - ISO 9564
     * @param pinblock pinblock in format 0 - ISO 9564 in HEX format 
     * @param pan primary account number (PAN/CLN/CardNumber)
     * @return clean PIN
     */
    public static String format0Decode(String pinblock, String pan) {
        try {
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());
            
            final byte[] bPinBlock = Hex.decodeHex(pinblock.toCharArray());
            
            final byte[] bPin = new byte[8];
            for (int i = 0; i < 8; i++)
                bPin[i] = (byte) (bPinBlock[i] ^ bPan[i]);
            
            final String pinData = Hex.encodeHexString(bPin);
            final int pinLen = Integer.parseInt(pinData.substring(0, 2));
            return pinData.substring(2, 2 + pinLen);
        } catch (NumberFormatException e) {
            throw new RuntimeException("Invalid pinblock format!");
        } catch (DecoderException e) {
            throw new RuntimeException("Hex decoder failed!", e);
        }
    }
    // END EXTRA CODE
}
