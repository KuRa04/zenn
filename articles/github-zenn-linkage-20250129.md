---
title: "[SpringBoot]SpringSecurity6.xで403エラーが発生した時の対応について"
type: "tech"
topics: ["SpringBoot", "SpringSecurity"]
published: true
---

### やったこと
SpringSecurity の 6.x を利用して認可・認証機能を実装。

### 発生したエラー
vscode の restClient を利用して signupAPI を叩いたところ、403 エラーが発生。

```
HTTP/1.1 403
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Length: 0
Date: Tue, 28 Jan 2025 01:59:51 GMT
Connection: close
```

### 原因
自動で`CSRF`の設定がされている。

### 解決策
`CSRF`を disabled にする。
securityFilterChain メソッド内で`http.csrf(AbstractHttpConfigurer::disable);`と設定。

```Java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .authorizeHttpRequests(authz -> authz
                        .requestMatchers("/api/auth/signup").permitAll()
                        .requestMatchers("/api/auth/login").permitAll()
                        .anyRequest().authenticated());

        http.csrf(AbstractHttpConfigurer::disable);

        return http.build();
    }
}
```

### 詰まったこと
SpringSecurity6.x から大幅に記述方法が変わったらしく、http.csrf を disabled にする方法が中々見つからなかったこと。

# 参考記事
[Spring Boot3 + SpringSecurity6 => 403 Forbidden with "requestMatchers"](https://stackoverflow.com/questions/75114615/spring-boot-3-spring-security-6-403-forbidden-with-requestmatchers)
