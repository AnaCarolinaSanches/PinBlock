package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import org.apache.commons.codec.DecoderException;
import org.apache.commons.codec.binary.Hex;
import org.apache.commons.lang.StringUtils;

public class JS_PinblockEncodeDecodeAction extends CustomJavaAction<java.lang.Void> {
    
    public JS_PinblockEncodeDecodeAction(IContext context) {
        super(context);
    }

    @Override
    public java.lang.Void executeAction() throws Exception {
        // BEGIN USER CODE
        
        // Exemplo de uso
        String pin = "1234";
        String pan = "1234567890123456";
        
        // Codificar o Pinblock
        String pinblockEncoded = format0Encode(pin, pan);
        System.out.println("Pinblock Encoded: " + pinblockEncoded);
        
        // Decodificar o Pinblock
        String pinDecoded = format0Decode(pinblockEncoded, pan);
        System.out.println("Pin Decoded: " + pinDecoded);

        // END USER CODE
        return null;
    }

    // Método para codificar o Pinblock (ISO 9564 formato 0)
    public static String format0Encode(String pin, String pan) {
        try {
            final String pinLenHead = StringUtils.leftPad(Integer.toString(pin.length()), 2, '0') + pin;
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

    // Método para extrair os 12 últimos dígitos do PAN
    public static String extractPanAccountNumberPart(String accountNumber) {
        String accountNumberPart = null;
        if (accountNumber.length() > 12)
            accountNumberPart = accountNumber.substring(accountNumber.length() - 13, accountNumber.length() - 1);
        else
            accountNumberPart = accountNumber;
        return accountNumberPart;
    }

    // Método para decodificar o Pinblock (ISO 9564 formato 0)
    public static String format0Decode(String pinblock, String pan) {
        try {
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());

            final byte[] bPinBlock = H
