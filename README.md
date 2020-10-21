#Encriptado
import java.security.MessageDigest;
import java.util.Arrays;
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import javax.swing.JOptionPane;
import org.apache.commons.codec.binary.Base64;
public class Cifrado {
    public static void main(String[] args) {
        String secretKey="ProgramadorPilo";
        Cifrado mMain = new Cifrado();
        String cadenaAEncriptar = JOptionPane.showInputDialog("Ingresa la cadena a encriptar");
        String cadenaEncriptada=mMain.ecnode(secretKey, cadenaAEncriptar);
        JOptionPane.showMessageDialog(null, "Cadena Encriptada: "+cadenaEncriptada);
        String cadenaDesencriptada = mMain.deecnode(secretKey, cadenaEncriptada);
        JOptionPane.showMessageDialog(null, "Cadena Desencriptada: "+cadenaDesencriptada);
        System.out.println(cadenaDesencriptada);
    }
    public String ecnode (String secretKey, String cadena){
        String encriptacion="";
        try{
            MessageDigest md5 = MessageDigest.getInstance("MD5");
            byte[ ] llavePassword = md5.digest(secretKey.getBytes("utf-8"));
            byte[ ] BytesKey = Arrays.copyOf(llavePassword, 24);
            SecretKey Key = new SecretKeySpec (BytesKey,"DESede");
            Cipher cifrado = Cipher.getInstance("DESede");
            cifrado.init(Cipher.ENCRYPT_MODE, Key);
            byte[ ] plainTextBytes = cadena.getBytes("utf-8");
            byte[ ] buf = cifrado.doFinal(plainTextBytes);
            byte[ ] base64Bytes = Base64.encodeBase64(buf);
            encriptacion=new String (base64Bytes);
        }catch(Exception ex){
            JOptionPane.showMessageDialog(null, "Algo salio mal");
        }
        return encriptacion;
    }
    public String deecnode (String secretKey, String cadenaEncriptada){
        String desencriptacion = "";
        try{
            byte[ ] message = Base64.decodeBase64(cadenaEncriptada.getBytes("utf-8"));
            MessageDigest md5 = MessageDigest.getInstance("MD5");
            byte[ ] digestOfPassword = md5.digest (secretKey.getBytes("utf-8"));
            byte[ ] keyBytes = Arrays.copyOf (digestOfPassword, 24);
            SecretKey Key = new SecretKeySpec (keyBytes, "DESede");
            Cipher decipher = Cipher.getInstance("DESede");
            decipher.init(Cipher.DECRYPT_MODE, Key);
            byte[ ] plainText = decipher.doFinal(message);
            desencriptacion = new String (plainText, "UTF-8");
        }catch(Exception ex){
            JOptionPane.showMessageDialog(null, "Algo salio mal"+ex.getMessage());
        }
        return desencriptacion;
    }
}
