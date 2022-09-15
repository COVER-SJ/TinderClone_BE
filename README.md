# Tinder clonecoding_BE

<hr>

## 기능 구현

1. 회원가입, 로그인, 로그아웃
2. 상대방 프로필 조회
3. 마이페이지 조회
4. 프로필 수정
5. 로그인 유저 정보 조회
6. 채팅방 전체 리스트
7. 채팅방 생성
8. 채팅방 정보
9. 채팅방입장
10. 채팅작성
11. 채팅받기
12. 채팅방나가기

<hr>

## API 명세서

# [Tinder API 명세서](https://www.notion.so/03898387badf4d749a5a4f1d271cffc6?v=77f3e78ad1d34af7b5557c6dd21485cb)


<hr>

## 와이어프레임

![KakaoTalk_20220915_200628435](https://user-images.githubusercontent.com/110081578/190389084-afe88bbe-19ee-4074-91e4-50a6dbda0bbe.jpg)
![KakaoTalk_20220915_200628435_01](https://user-images.githubusercontent.com/110081578/190389116-59e5e24a-8663-4dd9-b78d-a11b1e540283.jpg)
![KakaoTalk_20220915_200628435_02](https://user-images.githubusercontent.com/110081578/190389187-da6ff14c-8aef-4fe1-9026-85c9b2460625.jpg)




<hr>

## 트러블 슈팅

문제 : 클라이언트에서 서버로 요청했을 때 CORS 에러 발생

해결 : Spring Security에 CORS 허용 설정을 추가하였다.

```
 @Bean
   @Order(SecurityProperties.BASIC_AUTH_ORDER)
   public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     http.cors();

     http.csrf().disable()

             .exceptionHandling()
             .authenticationEntryPoint(authenticationEntryPointException)
             .accessDeniedHandler(accessDeniedHandlerException)

             .and()
             .sessionManagement()
             .sessionCreationPolicy(SessionCreationPolicy.STATELESS)

             .and()
             .authorizeRequests()
             .antMatchers("/api/signup").permitAll()
             .antMatchers("/api/login").permitAll()
             .antMatchers("/api/place/**").permitAll()
             .antMatchers("/api/comment/**").permitAll()
             .anyRequest().authenticated()

             .and()
             .apply(new JwtSecurityConfiguration(SECRET_KEY, tokenProvider, userDetailsService));

     return http.build();
   }

   @Bean
   public CorsConfigurationSource corsConfigurationSource(){
     CorsConfiguration configuration = new CorsConfiguration();
     configuration.addAllowedOrigin("http://localhost:3000");
     configuration.addAllowedHeader("*");
     configuration.addAllowedMethod("*");
     configuration.addExposedHeader("*");

     UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
     source.registerCorsConfiguration("/**", configuration);
     return source;
   }
```
