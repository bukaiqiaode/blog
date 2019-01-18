This snippet of code uses an encrypted data to do log-in check.

## Imports
```java
import com.alibaba.fastjson.JSON;
import com.domain.User;
import com.domain.VO_User;
import com.repository.UserRepository;
import com.service.UserLoginServiceImpl;
import com.utils.APIResult;
import com.utils.LoginType;
import com.utils.MD5Utils;
import com.utils.RSAUtils;
import com.utils.SysGlobalConstants;
import com.utils.SysErrorMessages;

import javax.servlet.http.HttpServletRequest;

import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
```

## Code
```java
	@RequestMapping(value = "login")
	public Object login(@RequestParam(value = "account", defaultValue = "test") String u_account,
			@RequestParam(value = "type", defaultValue = "1") Integer u_loginType,
			@RequestParam(value = "payload", defaultValue = "test") String payload,
			@RequestParam(value = "timestamp") Long timeStamp,
			@RequestParam(value = "pk", defaultValue = "test") String public_key,
			HttpServletRequest httpServletRequest) {
		String retVal = "success";

		User u = null;
		switch (u_loginType) {
		case LoginType.ByEmail:
			u = userRepository.findByUserEmail(u_account);
			break;
		case LoginType.ByPhone:
			u = userRepository.findByUserPhone(u_account);
			break;
		case LoginType.ByUserName:
		default:
			// default to log-in with user-name
			u = userRepository.findByUserName(u_account);
			break;
		}

		if (u == null) {
			return new APIResult(SysGlobalConstants.ERROR_GENERAL, SysErrorMessages.WrongAccountAndPassword);
		}

		// get md5(password) from system,
		String md5_pwd = u.getUserPwd();
		// encrypt (md5_pwd + timeStamp) with timeStamp
		String new_payload = MD5Utils.md5(md5_pwd + timeStamp.toString());
		if (!new_payload.equals(payload)) {
			return new APIResult(SysGlobalConstants.ERROR_GENERAL, SysErrorMessages.DataCorrupted);
		} else {
			// generate & save token for the user.
			String u_token = UUID.randomUUID().toString();
			u.setLoginToken(u_token);
			u.setLastLoginTime(new java.sql.Timestamp(System.currentTimeMillis()));
			userRepository.save(u);

			/**
			 * Return user information that could be exposed to clients. Token will be used
			 * to encrypt/decipher data.
			 */
			VO_User vo_user = new VO_User(u);
			// encrypt (user_id + token + timeStamp), using public_key
			retVal = RSAUtils.publicEncrypt(JSON.toJSONString(vo_user), public_key);
			return APIResult.createSuccess(retVal);
		}
	}
```

## Back to [index](./index.md)
