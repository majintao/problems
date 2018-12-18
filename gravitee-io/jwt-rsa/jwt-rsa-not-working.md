Java code validate successfully. But gwio validate fail. I am sure that the client_id is right. And i had subscribed the application to the jwt plan.

```
public class JwtUtils {

	public static String buildJwtRS256() throws Exception {
		SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.RS256;

		// 读取私钥
		String key = "MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBANDrROfXcbDeMB40\r\n" + 
				"Aa+X/JkRquSj2hXfeMAbkiArzobMEkqwDbdG76/YtSRx6Nem79yqwhUNqNg+lKfM\r\n" + 
				"RMprERDm/+/eX1Tv3YDFzpXZay0md5hZxUQKNqWDobCd7U8twKRDfIw1BfIBtPid\r\n" + 
				"L3RKmOXlJudoa+KeThIRQlT2DHrFAgMBAAECgYBs1OKEU7sqA9TVJwppyqcPpiB8\r\n" + 
				"Es8c7dkdWj94+tkPZ2dv+N5sR0u9MwrJ/XzqOlBhh6KrDP6UB6Ww87wyJiwwy+11\r\n" + 
				"R6Oz2XxPSGBFimoZPyc+dgPC/z63PPpHStXN9027IcaS47sjvtUp6HoCjLZZNht/\r\n" + 
				"oSGxSLZjLDXPm3JLgQJBAP2MAgP387fF8wa5WHqafaNGKW5KI9srwY7szCbleO0Q\r\n" + 
				"EL5eLmYVGUns5sjRFJSjl242QWvkmNlp52TgIkfj+M0CQQDS8LnGa5zeWSzUiDkP\r\n" + 
				"oiMhaJSVQtlgApfsTAA7Lx+M0TENjJcFGFKhGWDSY8AIE1DzYWIijRbC0vsc+WZZ\r\n" + 
				"zOnZAkEA1dt3/7zudv2iJPPEq3UPr94IKByk7cKUemdFMzGus9YvKULrQ/Nb5zzI\r\n" + 
				"1G12PIFXwwBEYirouclYAYADqjuhqQJAHfgDfNRHKjPjMaLU8IqpkRKJoZcoyQI1\r\n" + 
				"UWYO1lnAksIZxQIHZrro6mhvoBR58OvFoX5hceU3qaBN+vTX/MQnKQJBAKX95ga2\r\n" + 
				"NNWQ5Y1TfMEN3X0Ki+y9H64HODxaIuHW/h0W7kVssHSdNPvlEzR+S5pBaVy/6nDH\r\n" + 
				"CUXtz5Uuq3Wm3CA=";

		// 生成签名密钥
		byte[] keyBytes = (new BASE64Decoder()).decodeBuffer(key);
		PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(keyBytes);
		KeyFactory keyFactory = KeyFactory.getInstance("RSA");
		PrivateKey privateKey = keyFactory.generatePrivate(keySpec);
		JwtBuilder builder = Jwts.builder().setHeaderParam("typ", "JWT").setHeaderParam("alg", "RS256").setIssuer("marco").setSubject("token").signWith(signatureAlgorithm, privateKey);
		// jwt中需要传递的内容
		builder.claim("client_id", "bd6ed83e-5e51-440f-aed8-3e5e51540f4a");
		return builder.compact();
	}
	
	
	public static Claims parseJwtRS256(String jwt) {
		Claims claims = null;
		try {
		   // 读取公钥
		   String key = "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDQ60Tn13Gw3jAeNAGvl/yZEark\r\n" + 
		   		"o9oV33jAG5IgK86GzBJKsA23Ru+v2LUkcejXpu/cqsIVDajYPpSnzETKaxEQ5v/v\r\n" + 
		   		"3l9U792Axc6V2WstJneYWcVECjalg6Gwne1PLcCkQ3yMNQXyAbT4nS90Spjl5Sbn\r\n" + 
		   		"aGvink4SEUJU9gx6xQIDAQAB";
		   // 生成签名公钥
		   byte[] keyBytes = (new BASE64Decoder()).decodeBuffer(key);
		   X509EncodedKeySpec keySpec = new X509EncodedKeySpec(keyBytes);
		   KeyFactory keyFactory = KeyFactory.getInstance("RSA");
		   PublicKey publicKey = keyFactory.generatePublic(keySpec);
		   claims = Jwts.parser()
		           .setSigningKey(publicKey)
		           .parseClaimsJws(jwt).getBody();
		} catch (Exception e) {
		   e.printStackTrace();
		}
		return claims;
		}
	
	public static void main(String[] args) throws Exception {
		String token = buildJwtRS256();
		System.out.println(token);
		System.out.println("claims:" + parseJwtRS256(token).toString());
	}

}
```

![image](https://github.com/majintao/problems/blob/master/gravitee-io/jwt-rsa/1.png)


public key is ：
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDQ60Tn13Gw3jAeNAGvl/yZEark\r\n" + 
		   		"o9oV33jAG5IgK86GzBJKsA23Ru+v2LUkcejXpu/cqsIVDajYPpSnzETKaxEQ5v/v\r\n" + 
		   		"3l9U792Axc6V2WstJneYWcVECjalg6Gwne1PLcCkQ3yMNQXyAbT4nS90Spjl5Sbn\r\n" + 
		   		"aGvink4SEUJU9gx6xQIDAQAB
		   		


![image](https://github.com/majintao/problems/blob/master/gravitee-io/jwt-rsa/2.png)


