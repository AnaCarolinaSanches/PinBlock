// This file was generated by Mendix Studio Pro.
//
// WARNING: Only the following code will be retained when actions are regenerated:
// - the import list
// - the code between BEGIN USER CODE and END USER CODE
// - the code between BEGIN EXTRA CODE and END EXTRA CODE
// Other code you write will be lost the next time you deploy the project.
// Special characters, e.g., é, ö, à, etc. are supported in comments.

package cryptography.actions;

import com.mendix.systemwideinterfaces.core.IContext;
import com.mendix.webui.CustomJavaAction;
import org.apache.commons.codec.DecoderException;
import org.apache.commons.codec.binary.Hex;
import org.apache.commons.lang.StringUtils;
import cryptography.actions.JS_PinblockEncodeDecodeAction;

public class JS_PinblockEncodeDecodeAction extends CustomJavaAction<java.lang.String>
{
	private java.lang.String pin;
	private java.lang.String pan;

	public JS_PinblockEncodeDecodeAction(IContext context, java.lang.String pin, java.lang.String pan)
	{
		super(context);
		this.pin = pin;
		this.pan = pan;
	}

	@java.lang.Override
	public java.lang.String executeAction() throws Exception
	{
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
		return null;		
		// END USER CODE
	}

	/**
	 * Returns a string representation of this action
	 * @return a string representation of this action
	 */
	@java.lang.Override
	public java.lang.String toString()
	{
		return "JS_PinblockEncodeDecodeAction";
	}

	// BEGIN EXTRA CODE
	 // Método para decodificar o Pinblock (ISO 9564 formato 0)
    public static String format0Decode(String pinblock, String pan) {
        try {
            final String panPart = extractPanAccountNumberPart(pan);
            final String panData = StringUtils.leftPad(panPart, 16, '0');
            final byte[] bPan = Hex.decodeHex(panData.toCharArray());

            final byte[] bPinBlock = Hex.decodeHex(pinblock.toCharArray());

			final byte[] bPin = new byte[8];
			for(int i = 0; i < 8; i++)
			bPin[i] = (byte) (bPinBlock[i] ^ bPan[i]);

			final String pinData = Hex.encodeHexString(bPin);
			final int pinLen = Integer.parseInt(pinData.substring(0, 2)); 
			return pinData.substring(2, 2 + pinLen);
		} catch (NumberFormatException e) {
			throw new RuntimeException("Invalid PinBlock Format!", e);
		} catch (DecoderException e) {
			throw new RuntimeException("Hex decoder failed!", e);
			
		} 

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
	// END EXTRA CODE
}
